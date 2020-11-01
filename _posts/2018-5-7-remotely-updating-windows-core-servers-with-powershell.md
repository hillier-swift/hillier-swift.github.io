---
header:   
  image: /images/Header/2018-5-7-Remotly-Updating-Core.jpg
  caption: "Photo credit: [**Unsplash: Pietro Jeng**](https://unsplash.com/photos/n6B49lTx7NM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText")
title: Remotely Updating Windows Core Servers with Powershell
categories:
  - PowerShell
tags:
  - Server
---

I work with servers around the globe, and one of the things I was trying to do was update multiple of our core servers remotely. The problem with RDP connections if you make them too small you can't read the screen, with Powershell I can work on any number of servers, and it will all fit on one screen using a number of Powershell windows. You are able to do all this with modules like [this](https://gallery.technet.microsoft.com/scriptcenter/2d191bcd-3308-4edd-9de2-88dff796b0bc). I always like to start small and work my way up to running larger modules; it gives me a better understanding of what's happening. My current method allows me to act as if I was running on the machine which is more management friendly. It does require [psexec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec) as it calls on the built-in script.  
  
My current process is run two Powershell windows; one with a remote PSSession to shut down any VM's and any checking of the status of the server, a second with the below. Which if you have updated core server before is the script that asks which updates you would like to install which I found from scrolling through the VB scripts on the server.

```batch
.\psexec.exe \\Hostname -s cscript c:\windows\system32\en-us\WUA_SearchDownloadInstall.vbs  
```

Another reason for the two PowerShell windows is the only bug I have hit is once you download and install the options the option to reboot doesn't show. All I have to do is open the other PowerShell window and do  
  
```powershell
restart-computer -force
```

Or hit the below to stop the psexec process and you can enter your PS remote session and do it from there if you prefer one window per server.  
  
```batch
Ctrl + Pause break
```

None of this provides automation but does improve the workflow and keeps that manual control if you need it.  
  