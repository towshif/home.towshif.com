---
title: "Deploy DotNetCore SelfContained App Without Installing Runtime on Ubuntu Part1"
date: 2017-11-12T00:13:02-08:00

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
<!--more-->
---------
>Warning: Linux noob content below.

# Part 1: Using Visual Studio 2015 which has a different tooling than 2017

How to publish dotnet core apps to ubuntu without installing dotnet runtime: self contained apps development?
I started exploring this option driven by a specific need. I have a bunch of applications and classes already available on different C# .NET projects scattered all over the place. Many of these libraries include complex neumerical functions, database transactions classes and image processing codes all written in C#. My current need is to develop a web API to access these functions in a scalable way. I do not really need complex user interactions, rather its a data access and analysis portal. Users access and visualize data through a portal and with time I want to add more operations to the same. I really did not want to reinvent the wheel and start a massive migration to java pr python based web integration. C# dot net core provided a good alternative to scale existing C# application and add web functionalities.

## So Why Dot net core?
All the reason for why not .NET and Windows server based application. I was never a fan of Win Servers running .NET and IIS. Many reasons: expensive, license, open sourcing, familiarity to the sytem itself. I am not a core web deveoper - i do this more like a hobby. So I tend to go this small cleap solutions with running a small linux instance on a machine I own with dot net core on it.

## Why Self Contained App?
I bought a few cheap thin clients specifically for this purpose with pretty low end cpu and memory. I managed to install Lubuntu (a very light version of Ubuntu) and has only 1 Gb of drive space left to do anything. At this point I was just stretching my options to squeeze the juice out of this machine with minimal installs. Net core runtime itself will cost you 300-400 Mb of install space added by versioning and package itself. If you are not really developing on that machine - installing runtime is an overkill. Fortunately dontet core can be run as standalone app with linux executables and dependecies compiled and packaged to run. While this package may end being bulky after publishing but nevertheless its is easily transferable across platforms of linux making it very easy to maintain. I used Windows and Visual Studio to build and publish the binaries for linux and copied the compiled package to my 'micro - thin client' and could successfully able to run the dot net core application. Here is how I did it. I dont think this post has anything new which you will not find over the internet or .core website. It is just my compilation of what worked for me.

I started off with reading this post:
https://www.hanselman.com/blog/SelfcontainedNETCoreApplications.aspx

With MS Visual Studio 2015 which I primarily use for all dot net core development I created my app using the following steps. Major part of the building and compilation has nothing to do with the following settings. These steps are majorly for building platform specific binaries.

## Step 1:
open and modify project.json

1. Remove "type"="platform" from this dependency.
```
"frameworks": {
    "netcoreapp1.0": {
        "dependencies": {
            "Microsoft.NETCore.App": {
                "version": "1.0.1"
            }
        }
    }
}
```

2. Add runtime ids to the json
 ```
"runtimes": {
        "win7-x64": {},
        "ubuntu.14.04-x64": {},
        "ubuntu.16.04-x64": {}
},
```
after saving this studion will automatically run dotnet restore



## Step 2: build with CLI
use commandline to build the app: do not use VS STUDIO to build/publish self contained apps. There is a tooling bug.

```
$ dotnet build -r ubuntu.14.04-x64
others:
dotnet build -r win10-x64
dotnet build -r osx.10.10-x64
dotnet build -r ubuntu.16.04-x64
```

## Step 3: Publish
```
$ dotnet publish -c release -r ubuntu.14.04-x64
```
for other platform:
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

You should be good to go.
Check out my next post on how to do the same with Visual Studion 2017 which has better tooling and bug fixes compared to VS 2015.