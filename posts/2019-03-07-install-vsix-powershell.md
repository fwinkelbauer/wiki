# Install a Visual Studio Extension Using Powershell

One of my previous posts outlined how to manage Visual Studio extensions
installations by creating Chocolatey packages for each and every VSIX file you
want to install. This works fine in an environment where you are already using
an existing Chocolatey repository to hold custom or internalized packages. The
following PowerShell snippet presents a way to install VSIX files without
relying on Chocolatey:

``` powershell
function Get-VsixFile {
    param(
        [Parameter(Mandatory = $true)]
        [string]$Url
    )

    $page = Invoke-WebRequest -Uri $Url

    $json = $page.AllElements |
        Where-Object Class -eq 'vss-extension' |
        Select-Object -ExpandProperty innerHtml |
        ConvertFrom-Json |
        Select-Object -ExpandProperty versions

    $vsixUrl = $json.files |
        Where-Object assetType -match '\.vsix$' |
        Select-Object -ExpandProperty source

    $file = New-TemporaryFile

    try {
        Invoke-WebRequest -Uri $vsixUrl -OutFile $file
        return $file
    }
    catch {
        Remove-Item $file -Force
        throw $_
    }
}

function Invoke-VsixInstaller2017Community {
    param(
        [Parameter(Mandatory = $true)]
        [string]$File
    )

    $psi = New-Object System.Diagnostics.ProcessStartInfo
    $psi.FileName = 'C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\VSIXInstaller.exe'
    $psi.Arguments = "/q $File"

    $process = [System.Diagnostics.Process]::Start($psi)
    $process.WaitForExit()

    if ($process.ExitCode -ne 0) {
        throw "VSIXInstaller exit code: '$($process.ExitCode)'"
    }
}

function Install-Vsix2017Community {
    param(
        [Parameter(Mandatory = $true)]
        [string]$Url
    )

    $file = Get-VsixFile -Url $Url

    try {
        Invoke-VsixInstaller2017Community -File $file
    }
    finally {
        Remove-Item $file -Force
    }
}
```

These functions can be used like this:

``` powershell
Install-Vsix2017Community -Url 'https://marketplace.visualstudio.com/items?itemName=Suchiman.SerilogAnalyzer'
```

Or like this:

``` powershell
Invoke-VsixInstaller2017Community -File 'C:\Users\MyUser\Downloads\SerilogAnalyzer.Vsix.vsix'
```

**Note:** The different editions of Visual Studio 2017 (Community, Professional,
...) are installed in different locations, which is why the above PowerShell
snippets carry "2017Community" in their names.
