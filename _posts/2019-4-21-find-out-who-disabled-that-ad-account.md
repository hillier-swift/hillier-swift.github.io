---
header:   
  image: /images/Header/2019-4-21-find-out-who-disabled-that-ad-account.jpg
  caption: "Photo credit: [**unsplash: @yuhao**]("https://unsplash.com/@yuhao")
title: Investigating who disabled an AD account
categories:
  - PowerShell
tags:
  - Active Directory
---

Just a little post as its been while, the other day I had an AD user account that was disabled when it shouldn't. When this happens, I like to check with the admin who disabled the account for the reasons as you never know the background.

The Event ID you are looking for is [Event 4725](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4725) if you wanted to load up the event viewer on the DC and search using the MMC snap-in.

If you want to do this in PowerShell change the date and start time on the below for your needs

```powershell
get-winevent -computername DC.Domain.local -FilterHashtable @{ LogName = "Security"; Starttime = "21/04/2019" ; ID = 4725 } | fl
```

The field SubjectUserName is the user who disabled the account.
Building this out in broader environments. If you had an automated system to disable user accounts you could use this to find the last weeks disables which bypassed this process. If you do have a lot of disables, you could pipe this to a variable and filter the data searching for the username.
