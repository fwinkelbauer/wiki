# Poor Man's PowerShell Provisioning

I'm a fan of "infrastructure as code", which is why I have scripts which help me
to setup my own computers. Instead of relying on "heavy hitters" such as Chef,
Ansible or Puppet, my Windows provisioning script rely on PowerShell and
Chocolatey. I am aware of other tools such as Boxstarter, but I deliberately
choose a more manual and bare bones approach in favor of improved error
handling.

I have created two PowerShell functions which help me to keep my provisioning
scripts simple and well-arranged:

## Step

Several PowerShell instructions can be grouped together as a step:

```powershell
function step {
    param(
        [Parameter(Mandatory = $true)]
        [string]$Name,
        [Parameter(Mandatory = $true)]
        [scriptblock]$Do,
        [scriptblock]$If = { $true }
    )

    if (& $If) {
        Write-Output '============================================================'
        Write-Output "Starting step '$Name'"
        Write-Output '------------------------------------------------------------'

        $startTime = Get-Date

        & $Do

        $endTime = Get-Date
        $duration = $endTime - $startTime

        Write-Output '------------------------------------------------------------'
        Write-Output "Finished step '$Name' ($duration)"
        Write-Output '============================================================'
    }
    else {
        Write-Output '============================================================'
        Write-Output "Skipping step '$Name'"
        Write-Output '============================================================'
    }
}
```

Which might look like this:

```powershell
step 'Configure Windows explorer' {
    $explorerKey = 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced'
    # Show file extensions in explorer
    Set-ItemProperty -Path $explorerKey -Name HideFileExt -Value 0
    # Opens explorer at 'This PC' instead of 'Quick Access'
    Set-ItemProperty -Path $explorerKey -Name LaunchTo -Value 1
}
```

Or like this:

```powershell
$configFile = 'C:\meaning.ini'

step 'Write important config file' -If {
    -not (Test-Path $configFile)
} -Do {
    Set-Content -Value 'answer = 42' -Path $configFile
}
```

## Once

Sometimes I'd like to run a block of code once, but I don't have any decent way
of checking if the block was already executed. The "once" function is a
specialized step, which uses a "checkpoint file" to give a step the "executed
just once" property:

```powershell
function once {
    param(
        [Parameter(Mandatory = $true)]
        [string]$Name,
        [Parameter(Mandatory = $true)]
        [scriptblock]$ScriptBlock,
        [System.IO.DirectoryInfo]$CheckpointDirectory = (Join-Path $env:ProgramData 'my.provision')
    )

    $checkpointFileName = $Name + '.checkpoint'

    foreach ($invalidFileNameChar in [System.IO.Path]::GetInvalidFileNameChars()) {
        $checkpointFileName = $checkpointFileName.Replace($invalidFileNameChar, '_')
    }

    $checkpointFile = Join-Path $CheckpointDirectory $checkpointFileName

    step $Name -If {
        -not (Test-Path $checkpointFile)
    } -Do {
        & $ScriptBlock

        New-Item -Path $checkpointFile -Force | Out-Null
    }
}
```

Here's an example:

```powershell
once 'Write important config file' {
    Set-Content -Value 'answer = 42' -Path 'C:\meaning.ini'
}
```

## Exec

Exec makes sure that I can monitor the exit code of a command line tool:

```powershell
function exec {
    param(
        [Parameter(Mandatory = $true)]
        [scriptblock]$ScriptBlock,
        [int[]]$ValidExitCodes = @(0)
    )

    $global:LASTEXITCODE = 0

    & $ScriptBlock

    if (-not ($global:LASTEXITCODE -in $ValidExitCodes)) {
        throw "Invalid exit code: $($global:LASTEXITCODE)"
    }
}
```

Now I can call command line tools like this:

```powershell
exec { 7z }
exec { git status }
exec -ValidExitCodes 0, 1, 2 { $global:LASTEXITCODE = 2 }
```

I can even create a wrapper function to deal with restarts if a Chocolatey
package installation returns the exit code `3010`:

``` powershell
function choco-exec {
    param(
        [Parameter(Mandatory = $true)]
        [scriptblock]$ScriptBlock,
        [switch]$ConfirmBeforeReboot
    )

    exec -ScriptBlock $ScriptBlock -ValidExitCodes @(0, 1641, 3010)

    if ($global:LASTEXITCODE -eq 3010) {
        Write-Warning "Chocolatey indicates, that a restart is necessary"
        Restart-Computer -Confirm:$ConfirmBeforeReboot
    }
}
```

Which I can call like this:

``` powershell
choco-exec { choco install git -y }
choco-exec { choco install dotnet4.7.2 -y } -ConfirmBeforeReboot
```
