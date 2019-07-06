# Creating a Self-Updating Chocolatey Package

More and more applications ship with an automatic update mechanisms, which
downloads new versions of the application as soon as they are available. A
command line option such as `myapp.exe --update` looks nice, but the update
process is tricky, as we cannot simply replace the application while its
running.

A very minimalistic approach, which can work for command line tools, relies on
the Chocolatey package manager. Here's an example:

**Package structure:**

- `myapp.nuspec`
- `tools\`
  - `chocolateyInstall.ps1`
  - `chocolateyUninstall.ps1`
  - `myapp.zip` (which contains a `myapp.exe` file)

**chocolateyInstall.ps1:**

``` powershell
$ErrorActionPreference = 'Stop'

$toolsDir = (Split-Path -parent $MyInvocation.MyCommand.Definition)
$zipFile = Join-Path $toolsDir 'myapp.zip'
$batShimFile = Join-Path $env:ChocolateyInstall 'bin\myapp.bat'
$ps1ShimFile = Join-Path $env:ChocolateyInstall 'bin\myapp.ps1'

$appDir = "C:\SomeFolder\$($env:ChocolateyPackageName)\$($env:ChocolateyPackageVersion)"
$appFile = Join-Path $appDir 'myapp.exe'

Get-ChocolateyUnzip -FileFullPath $zipFile -Destination $appDir
Set-Content -Path $batShimFile -Value "@echo off`ncall `"$appFile`" %*" -Force
Set-Content -Path $ps1ShimFile -Value "& '$appFile' `$args" -Force
```

**chocolateyUninstall.ps1:**

``` powershell
$ErrorActionPreference = 'Stop'

$batShimFile = Join-Path $env:ChocolateyInstall 'bin\myapp.bat'

if (Test-Path $batShimFile) {
    Remove-Item $batShimFile -Force
}

$ps1ShimFile = Join-Path $env:ChocolateyInstall 'bin\myapp.ps1'

if (Test-Path $ps1ShimFile) {
    Remove-Item $ps1ShimFile -Force
}

$appDir = "C:\SomeFolder\$($env:ChocolateyPackageName)"

if (Test-Path $appDir) {
    Remove-Item $appDir -Recurse -Force
}
```

The technique works like this:

- Calling `choco install myapp` installs the `myapp.zip` file to the Chocolatey
  lib directory (`$env:ChocolateyInstall\lib\myapp\tools`) and extracts the
  archive to `C:\SomeFolder\myapp\1.0.0\`
- The install script creates a batch and a Powershell file, which point to
  `C:\SomeFolder\myapp\1.0.0\myapp.exe`
- The application can now be called in PowerShell and cmd by typing `myapp`
- We can now update the application using `choco upgrade myapp`, even while it
  is running. The batch file will point to the new version (e.g.
  `C:\SomeFolder\myapp\2.0.0\myapp.exe`), which will work for any future calls
  to `myapp`
