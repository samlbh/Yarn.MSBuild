Yarn.MSBuild
============

[![Travis](https://img.shields.io/travis/natemcmaster/Yarn.MSBuild.svg?style=flat-square&label=travis)](https://travis-ci.org/natemcmaster/Yarn.MSBuild)
[![AppVeyor](https://img.shields.io/appveyor/ci/natemcmaster/yarn-msbuild.svg?style=flat-square&label=appveyor)](https://ci.appveyor.com/project/natemcmaster/yarn-msbuild)
[![NuGet](https://img.shields.io/nuget/v/Yarn.MSBuild.svg?style=flat-square)](https://nuget.org/packages/Yarn.MSBuild)


An MSBuild task for running the Yarn package manager.

See [Yarn's Official Website](https://yarnpkg.com/en/) for more information about using Yarn.

# Installation

**Package Manager Console in Visual Studio**
```
PM> Install-Package Yarn.MSBuild
```

**.NET Core Command Line**
```
dotnet add package Yarn.MSBuild
```

**In csproj**
```xml
<ItemGroup>
  <PackageReference Include="Yarn.MSBuild" Version="0.24.6" />
</ItemGroup>
```

# Usage

## Default usage

This package is designed for use with ASP.NET Core projects.

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Yarn.MSBuild" Version="0.24.6" />
  </ItemGroup>
</Project>
```

Project layout:
```
+ WebApplication.csproj
+ package.json
+ Startup.cs
- wwwroot
   + app.js
   + site.css
```

Running `dotnet build` or `msbuild.exe /t:Build` will automatically invoke `yarn install`.

### Additional options

```xml
<PropertyGroup>
  <!-- Prevent yarn from running on 'Build'. Default to 'false'-->
  <SuppressAutoYarn>true</SuppressAutoYarn>

  <!-- Change the yarn that runs on 'Build'. Defaults to 'install'. -->
  <YarnBuildCommand>run build</YarnBuildCommand>

  <!-- Change the directory in which yarn is invoked on build. Defaults to '$(MSBuildProjectDirectory)'. -->
  <YarnDir>wwwroot/</YarnDir>
</PropertyGroup>
```

## Using the task

The `Yarn` task supports the following parameters
```
[Optional]
string Command             The arguments to pass to yarn.

[Optional]
string ExecutablePath      Where to find yarn (*nix) or yarn.cmd (Windows)

[Optional]
string WorkingDirectory    The directory in which to execute the yarn command
```

```xml
<Project>
  <Target Name="RunYarnCommands">
    <!-- defaults to "install" in the current directory using the bundled version of yarn. -->
    <Yarn />

    <Yarn Command="upgrade" />

    <Yarn Command="run test" WorkingDirectory="wwwroot/" />
    <Yarn Command="run cmd" ExecutablePath="/usr/local/bin/yarn" />
  </Target>
</Project>
```

# About

This is not an official Yarn project. See [LICENSE.txt](LICENSE.txt) and the [Third Party Notice](src/Yarn.MSBuild/third_party_notice.txt) for more details.
