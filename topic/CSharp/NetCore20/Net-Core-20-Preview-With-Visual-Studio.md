## .Net Core 2.0 On Visual Studio 2017

---

適用對象 : For 對Core 2.0 有興趣，但不想安裝Visual Studio Preview 又想用VS嘗鮮或提前修改Breaking Change的人

必須條件：Visual Studio 2017 \(any version\)

Step 1 : Install Net Core 

[點此進入GitHub下載頁面](https://github.com/dotnet/core/blob/master/release-notes/download-archives/2.0.0-preview1-download.md)

Step 2：修改.csproj 專案檔如下

```
<PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
</PropertyGroup>
<ItemGroup>
    <PackageReference Update="Microsoft.NETCore.App" Version="2.0.0-*" />
</ItemGroup>
```

這裡需要注意的是 PackageReference 是 Update 而不是 Include

表示SDK所引用的Package 需要Update Version



