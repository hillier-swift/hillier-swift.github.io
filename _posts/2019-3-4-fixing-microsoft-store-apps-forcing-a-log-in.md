---
layout: post
title: Fixing Microsoft Store apps forcing a log in
---

I was having an issue with a user where they were trying to download applications from the Microsoft store. The default behaviour is it asks you to sign in with a Microsoft account but gives you the option of No,thanks. This users machine would not allow you to skip and forced you to login, even after logging on with a O365 account, applications would not download. After trying the usual; reboots, signing in and out of the Microsoft account and removing the O365 account from the profile I found the below to work.

First - make sure Microsoft Store is closed.

* On the users machine open a command prompt and type `whoami /user` to get the affected users SID
* Delete the below reg key *(the whole folder)* replacing [SID] the user's SID `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Appx\AppxAllUserStore\[SID]`
* From command prompt run `WSReset.exe` to clear the Windows Store Cache
* This will force open Microsoft Store go to download a free application tap `Get`
* You should be asked a question Use across your devices, tap `No, thanks`
* Wait a few moments and it should download.
