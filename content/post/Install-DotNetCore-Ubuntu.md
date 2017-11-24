---
title: "Install DotNetCore Ubuntu"
date: 2017-11-11T23:47:12-08:00
categories:
- Install
- Dot Net Core
- Ubuntu
tags:
- ubuntu 
- dot net core
- install

keywords:
- Install
- Dot Net Core
- Ubuntu
- libicu
- CORECLR
#thumbnailImage: //example.com/image.jpg
---
>Warning: Linux noob content below.

Ubuntu 16.04
While most of the install instructions for .NET core works from the microsoft website it is vastly generic for all versions of Linux distros available. Here I tried to install on a Ubuntu 16.04 LTS and faced some issues. Of course .NET Core itself is pretty new and expecting standard versioning and dependency management across distros might be challenging even for Microsoft. 

-----------

## STEP 1: install dependency 
For Ubuntu 16.04 LTS which uses libicu_55 this step is necessary 
I found this informative blog post that talks about the CLR error that you might get: https://kvaes.wordpress.com/2016/12/10/issue-with-vsts-agent-on-ubuntu-failed-to-initialize-coreclr-hresult-0x80131500/


Download `libicu_52` (ubuntu) not dev from repo: http://security.ubuntu.com/ubuntu/pool/main/i/icu/.

You may need to browse through this ftp directory to find the one right for you if using a different distro. I have used ubuntu 16.04 for this post.

```
$ wget http://security.ubuntu.com/ubuntu/pool/main/i/icu/libicu52_52.1-3ubuntu0.6_amd64.deb
```
Install it 
```
$ sudo dpkg -i libicu52_52.1-3ubuntu0.6_amd64.deb
```

Lets install Dot net Dev for Ubuntu using debian installer (some caveats here): 

Using debian installer process https://www.microsoft.com/net/core#linuxdebian  (unfortunately instructions on Ubuntu apt-get install does not work. Use the debian instead with debian instructions.) 

	
## STEP 2: Install .NET Core SDK
Before you start, please remove any previous versions of .NET Core from your system.

In order to install .NET Core 1.1 on Debian, first you need to get the prerequisites and then you download the .NET Core SDK binaries, extract them onto your system and put dotnet onto your PATH.

.NET Core 1.1 is the latest version. For long term support versions and additional downloads check the all Linux downloads section.

```
$ sudo apt-get install curl libunwind8 gettext
$ curl -sSL -o dotnet.tar.gz https://go.microsoft.com/fwlink/?linkid=848826
$ sudo mkdir -p /opt/dotnet && sudo tar zxf dotnet.tar.gz -C /opt/dotnet
$ sudo ln -s /opt/dotnet/dotnet /usr/local/bin
```


## STEP 3: Initialize some code
Let's initialize a sample Hello World application!
```
$ dotnet new console -o hwapp
$ cd hwapp
```
Error you might get is libicu_55 is not installed however libicu_52 should solve the problem
```
$ Failed to initialize CoreCLR, HRESULT: 0x80131500
```
in that case check https://shazwazza.com/post/installing-net-core-101-on-ubuntu-1610/


## STEP 4: Run the app
The first command will restore the packages specified in the project file, and the second command will run the actual sample:

```
$ dotnet restore
$ dotnet run
```
Verify the code runs on a web browser http://localhost:port


And you're ready!
You now have .NET core running on your machine!

--------
