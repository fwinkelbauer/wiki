# Creating VSIX Chocolatey Packages

This post outlines how to create a Chocolatey package for a VSIX file based on
the [Cake for Visual Studio][market_cake] extension.

We will start with the package definition:

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2015/06/nuspec.xsd">
  <metadata>
    <id>my.vs2017.cake</id>
    <title>Visual Studio 2017 Cake Extension</title>
    <version>0.2.0.0</version>
    <authors>Cake Build</authors>
    <owners>Florian Winkelbauer</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <description>Adds the Cake extension to Visual Studio 2017</description>
    <copyright>Copyright 2018</copyright>
    <tags>cake visual studio 2017</tags>
    <dependencies>
      <dependency id="chocolatey-visualstudio.extension" />
    </dependencies>
  </metadata>
</package>
```

Notice the declared dependency: [chocolatey-visualstudio.extension][choco_vsix]
allows us to install a VSIX file using PowerShell.

To create the install/uninstall scripts we will need the following information:

- The VSIX ID
- A URL to download the VSIX file
- The current version of the extension
- A file checksum

I have created a small helper function to fetch the above data. This function
requires the [checksum package][choco_checksum] to be installed:

``` powershell
function Get-VsixData {
    param($url, $checksumType)

    $page = Invoke-WebRequest -Uri $url

    $vsixId = ($page.AllElements |
        Where Class -eq 'vsixId' |
        Select -ExpandProperty innerHTML).Trim() -replace '"', ''

    $json = $page.AllElements |
        Where Class -eq 'vss-extension' |
        Select -ExpandProperty innerHtml |
        ConvertFrom-Json |
        Select -ExpandProperty versions

    $vsixVersion = $json.version

    $vsixUrl = $json.files |
        where assetType -match '\.vsix$' |
        Select -ExpandProperty source

    $file = Join-Path $env:TMP 'tmp.vsix'
    Invoke-WebRequest -Uri $vsixUrl -OutFile $file
    $checksum = checksum -f="$file" -t="$checksumType"
    Remove-Item $file

    return @{ VsixId = $vsixId; VsixVersion = $vsixVersion; VsixUrl = $vsixUrl; Checksum = $checksum; ChecksumType = $checksumType }
}
```

Here's the console output for the Cake extension:

``` plaintext
Get-VsixData 'https://marketplace.visualstudio.com/items?itemName=vs-publisher-1392591.CakeforVisualStudio' 'sha256'

Name                           Value
----                           -----
VsixId                         3cf9b016-d63f-44ee-849d-6f3efc996134
VsixVersion                    0.2.0.0
VsixUrl                        https://vs-publisher-1392591.gallerycdn.vsassets.io/extensions/vs-publisher-1392591/cakeforvisualstudio/0.2.0.0/1503436806448/229353/4/Cake.VisualStudio.vsix
Checksum                       752A01E2A40A5DED7BED6FC16930B347205030AE693C75D3D5DC76DCCE407D02
ChecksumType                   sha256
```

With this information we now can create our install logic:

``` powershell
$ErrorActionPreference = 'Stop'

$parameters = @{
    PackageName   =  $env:ChocolateyPackageName
    VsixUrl       = 'https://vs-publisher-1392591.gallerycdn.vsassets.io/extensions/vs-publisher-1392591/cakeforvisualstudio/0.2.0.0/1503436806448/229353/4/Cake.VisualStudio.vsix'
    Checksum      = '752A01E2A40A5DED7BED6FC16930B347205030AE693C75D3D5DC76DCCE407D02'
    ChecksumType  = 'sha256'
}

Install-VisualStudioVsixExtension @parameters
```

And the uninstall logic:

``` powershell
$ErrorActionPreference = 'Stop'

$parameters = @{
    PackageName   = $env:ChocolateyPackageName
    VsixId        = '3cf9b016-d63f-44ee-849d-6f3efc996134'
}

Uninstall-VisualStudioVsixExtension @parameters
```

Our final package should now look like [this][gh_box].

Creating and updating several VSIX packages could be further automated using the
above helper function in combination with tools such as the Chocolatey [Auto
Updater (AU)][gh_au]:

[market_cake]: https://marketplace.visualstudio.com/items?itemName%3Dvs-publisher-1392591.CakeforVisualStudio
[choco_vsix]: https://chocolatey.org/packages?q=chocolatey-visualstudio.extension
[choco_vs2017]: https://chocolatey.org/packages/visualstudio2017-installer
[choco_checksum]: https://chocolatey.org/packages/checksum
[gh_box]: https://github.com/fwinkelbauer/windows-box-setup/tree/0c8a2e4757f6c5f4178e3adada73a10dacab869c/nuspec/my.vs2017.cake
[gh_au]: https://github.com/majkinetor/au
