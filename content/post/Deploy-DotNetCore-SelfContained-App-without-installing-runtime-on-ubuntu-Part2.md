---
title: "Deploy DotNetCore SelfContained App Without Installing Runtime on Ubuntu Part2"
date: 2017-11-12T00:53:10-08:00
categories:
- Dot Net Core
- Ubuntu
tags:
- ubuntu
- dot net core

keywords:
- Install
- Dot Net Core
- Ubuntu
- self contained
- cross platform
#thumbnailImage: //example.com/image.jpg
---
>Warning: Linux noob content below.


# Part 2: Using Visual Studio 2017 with better tooling and bugfixes.
How to publish dotnet core apps to ubuntu without installing dotnet runtime: self contained apps development

As a continuation of the previous post I tried to do the same with VS 2017 which supposedly has better tooling for Dot Net Core. I did also see the cspoj and xml structures were altered ( maybe for good - I am too noob to comment); but long story short, I had to dig around to get it working and compile the correct platform specific runtimes binaries.
As mentioned above due to changes on csproj and json project structures I follows some instruction from this document:

https://docs.microsoft.com/en-us/dotnet/core/tools/project-json-to-csproj

## Step 1: Change VS project setting
Open and modify *.csproj instead of project.json which does not exist. Part of the change in tooling for 2017

Add runtime ids to the csproj / project.json

XML: *.csproj
```
<PropertyGroup>
  <RuntimeIdentifiers>win7-x64;osx.10.11-x64;ubuntu.16.04-x64</RuntimeIdentifiers>
</PropertyGroup>
```
JSON equivalent: project.json
```
{
  "runtimes": {
    "win7-x64": {},
    "osx.10.11-x64": {},
    "ubuntu.16.04-x64": {}
  }
}
```
after saving this studio will automatically run dotnet restore and update packages; however, if not run from project root/ dir, you can manually do the same to ensure restore.
```
$ dotnet restore
```


## Step 2: Build with CLI
use commandline to build the app: do not use VS STUDIO to build/publish self contained apps. There is a tooling bug in 2015 but aparrantly fixed on 2017. But even on 2017 I am using commandline to do the same. Commands are pretty neat ( kudos to microsoft this time...)

```
$ dotnet build -r ubuntu.14.04-x64
others:
dotnet build -r win10-x64
dotnet build -r osx.10.10-x64
dotnet build -r ubuntu.16.04-x64
```

## Step 3: Publish

In project.json, defining a runtimes section means the app was standalone during build and publish. In MSBuild, all projects are portable during build, but can be published as standalone.

**For *.csproj (VS 2017)**

Use CLI commands for publishing standalone for any target framework
Note: target framework must match build framework and then restored.
```
$ dotnet publish --framework netcoreapp1.0 --runtime ubuntu.16.04-x64
```
------------
**For project.json ( VS 2015)  use this :**
```
$ dotnet publish -c release -r ubuntu.14.04-x64
```
OR more specifically
```
dotnet publish --framework netcoreapp1.0 --runtime ubuntu.16.04-x64
```

other platforms as usual:
```
dotnet publish -c release -r win10-x64
dotnet publish -c release -r osx.10.10-x64
dotnet publish -c release -r ubuntu.16.04-x64
```

Copy publish folder to the ubuntu machine and find the executable
./executable
Eg.
```
$ towshif@appsknowhow:~/Code/dotnet-core/publish$ ./2015netCoreWebApplication2
```
ERRORS : You may end up getting a dependency error like this:
```
Failed to load /home/towshif/Code/dotnet-core/publish/libcoreclr.so, error: libunwind.so.8: cannot open shared
Failed to bind to CoreCLR at '/home/towshif/Code/dotnet-core/publish/libcoreclr.so'
```

In that case install libunwind8 and run again
```
$ sudo apt-get install libunwind8
```

You should be good to go. This post might have some repeated content as Part1. One of the reason is because Part 2 is an advanced version (superset) of Part 1 which only covers publishhing with Visual Studio 2015.