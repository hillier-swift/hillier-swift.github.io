---
title: Changing A PowerShell Window Name
categories:
  - PowerShell
tags:
  - JFD
---

So I have been finding myself having two different user context PowerShell windows open but keeping track of which one is which is a pain. Found an easy one liner to update the title of the window.

```powershell
$host.ui.RawUI.WindowTitle = “New Window Name”
```

I have taken it one stage further and put this in the PowerShell profile of the user I log on with, in my example an administrative user on my machine.  If you are unsure on how profiles work let me know in the comments and I will write up a post.

```powershell
# Sets location for PowerShell to open in I find this handy to be my repo location
set-location C:\whatEverPath
# Changes the windows title to the user context it was opened with  
$host.ui.RawUI.WindowTitle = “PowerShell: $env:USERNAME”
# Clears the screen to have a clean starting look
cls
```

This post is a "just for documentation (JFD)" post which I use for my reference, but I have made public as it may be useful. I put less time into these posts as they are my reference. If you would like more information or some help post a comment, and I can see what I can do.
