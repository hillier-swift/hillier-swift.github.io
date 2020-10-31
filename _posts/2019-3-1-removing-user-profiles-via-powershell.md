---
title: Removing User Profiles via Powershell
categories:
  - PowerShell
tags:
  - JFD
---

So I wanted a way to remove userprofiles with a bit more consistency than through the control panel. For a one liner just change $user varible for "*[usersFoldername]" on the line three.

```powershell
$Profilename = "Foldername" #Change FolderName for the profile you would like to remove.
$User = "*" + $Profilename
Get-WmiObject -Class Win32_UserProfile | where {$_.LocalPath.split('')[-1] -like $user} | foreach {$_.Delete()}
```

*This post is a "just for documentation (JFD)" post which I use for my reference, but I have made public as it may be useful. I put less time into these posts as they are my reference. If you would like more information or some help post a comment, and I can see what I can do.*
