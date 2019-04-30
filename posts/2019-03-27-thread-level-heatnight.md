# Thread Level Heatnight

In the past I've had a few occasions where I had to create a [WiX][wix]
installer for a C# project. But independent of whatever Google search query I
tried, I simply could not figure out how to use `heat.exe` to auto-generate the
content of my `.msi` files. Luckily, because of this lovely [StackOverflow
post][stack], this time was different. I'll cite the steps mentioned in this
post so that I'll hopefully never forget them:

> Define the Harvestpath for Heat:
>
> ```
> <PropertyGroup>
>   <DefineConstants>HarvestPath=..\Deploy</DefineConstants>
> </PropertyGroup>
> ```
>
> Heat will create a .wxs file. So we need to add this file to the compile ItemGroup:
>
> ```
> <ItemGroup>
>   <Compile Include="Product.wxs" /> <!-- This will be your default one -->
>   <Compile Include="HeatGeneratedFileList.wxs" /> <!-- This is the Heat created one -->
> </ItemGroup>
> ```
>
> Then execute Heat in the BeforeBuild build target:
>
> ```
> <Target Name="BeforeBuild">
>   <HeatDirectory Directory="..\Deploy"
>     PreprocessorVariable="var.HarvestPath"
>     OutputFile="HeatGeneratedFileList.wxs"
>     ComponentGroupName="HeatGenerated"
>     DirectoryRefId="INSTALLFOLDER"
>     AutogenerateGuids="true"
>     ToolPath="$(WixToolPath)"
>     SuppressFragments="true"
>     SuppressRegistry="true"
>     SuppressRootDirectory="true" />
> </Target>
> ```
>
> This will generate the HeatGeneratedFileList.wxs every time the WIX installer is built. The directory ..\Deploy has to be set to the directory of the files to include. The only thing we have to do to include these files in our installer is to edit the main .wxs file (like Product.wxs in this example). Heat will create a ComponentGroup with the given name from above. This component needs to be referenced in the Feature section of the Product.wxs:
>
> ```
> <Feature Id="ProductFeature" Title="DiBA Tool" Level="1">
>   <...>
>   <ComponentGroupRef Id="HeatGenerated" />
> </Feature>
> ```

The above steps worked perfectly, but I had to write some additional code so
that I could extract the version information of my "main `.exe` file" (which is
contained in `HeatGeneratedFileList.wxs`). Each file in the heat generated list
has an ID, which in my case looked like this:
`fil672A180CB079EF052CD394C3B527E0A9`. This ID can be used to extract and use
the version information (e.g. v1.2.3.4) in my `Product.wxs` file:

``` xml
<?define ProductVersion=!(bind.FileVersion.fil672A181CB6794D058CDE94C6B527E0F9) ?>
```

``` xml
<Product Id="*" Name="MyApp" Language="1033" Version="$(var.ProductVersion)" Manufacturer="MyCompany" UpgradeCode="ffc26465-f32c-4848-acbf-e896c4095236">
```

[wix]: http://wixtoolset.org/
[stack]: https://stackoverflow.com/questions/36756311/include-all-files-in-bin-folder-in-wix-installer
