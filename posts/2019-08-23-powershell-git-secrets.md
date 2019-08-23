# Storing Secrets in Git using PowerShell

Storing secrets (passwords, API keys and so on) in Git is a bad idea. We all
know that. But somehow, we still end up doing it anyway. Some people have come
up with decent solutions to this problem. They are using asymmetric encryption
to store their secrets. Notable examples include [git-crypt][git-crypt],
[git-secret][git-secret], or [blackbox][blackbox]. But all three tools have one
thing in common: they are bash tools, which leaves the Windows folks out in the
rain. I know that WSL can get around this limitation, but I wanted something
that I can use in PowerShell. So I came up with a rather stupid and simple
solution: using 7-zip to AES encrypt secrets.

Here's an example of an encryption function:

``` powershell
function Read-Password {
    [CmdletBinding()]
    param()

    $secure1 = Read-Host -Prompt 'Enter password' -AsSecureString
    $secure2 = Read-Host -Prompt 'Re-enter password' -AsSecureString

    $plain1 = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto([System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($secure1))
    $plain2 = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto([System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($secure2))

    if ($plain1 -ne $plain2) {
        throw 'The provided passwords do not match'
    }

    $secure1
}

function Compress-Secret {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory = $true)]
        [System.IO.FileInfo[]]$Files,
        [Parameter(Mandatory = $true)]
        [System.IO.FileInfo]$DestinationPath,
        [Parameter(Mandatory = $true)]
        [System.Security.SecureString]$Password
    )

    if (Test-Path $DestinationPath) {
        Remove-Item $DestinationPath -Confirm
    }

    $plain = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto([System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($Password))

    7z a $DestinationPath -p"$plain" $Files -spf | Write-Verbose

    if ($LASTEXITCODE -ne 0) {
        throw "Error while trying to create '$DestinationPath'"
    }
}
```

Which could be called like this:

``` powershell
$ErrorActionPreference = 'Stop'
Set-StrictMode -Version 'Latest'

Compress-Secret -Files (Get-Content '.secrets') -DestinationPath 'secrets.7z' -Password (Read-Password)
```

The `.secrets` file should contain a list of secret files, e.g:

``` text
config/some-api-key.conf
config/license-key.txt
another-secret.json
```

And here's the decryption function:

``` powershell
function Expand-Secret {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory = $true)]
        [System.IO.FileInfo]$SourcePath,
        [System.Security.SecureString]$Password = (Read-Host -Prompt 'Enter password' -AsSecureString)
    )

    $plain = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto([System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($Password))

    7z x $SourcePath -p"$plain" -y | Write-Verbose

    if ($LASTEXITCODE -ne 0) {
        throw "Error while trying to decrypt '$SourcePath'"
    }
}
```

Which we can call like this:

``` powershell
$ErrorActionPreference = 'Stop'
Set-StrictMode -Version 'Latest'

Expand-Secret -SourcePath 'secrets.7z'
```

The above snippets are neither the most sophisticated, nor the most secure code,
but it's a starting point.

A few final notes:

- Make sure that you add the unencrypted files to your `.gitignore`
- An attacker could read your password by watching the parameters of the 7-zip
  process while it is running. But such an attacker could most likely just
  access the unencrypted files anyway
- The file list which is passed into 7-zip should have the format `a/b/c.txt`
  (or `a\b\c.txt`) and not `.\a\b\c.txt`. 7-zip will not find your files if you
  are using the later format

[git-crypt]: https://github.com/AGWA/git-crypt
[git-secret]: https://git-secret.io/
[blackbox]: https://github.com/StackExchange/blackbox
