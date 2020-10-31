---
title: Manually Adding or Removing a shared mailbox from Outlook 2016
categories:
  - User Guides
tags:
  - JFD
  - Outlook
---

Just a little post as its been while, the other day I had an AD user account that was disabled when it shouldn't. When this happens, I like to check with the admin who disabled the account for the reasons as you never know the background. 

The Event ID you are looking for is [Event 4725](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4725) if you wanted to load up the event viewer on the DC and search using the MMC snap-in.

If you want to do this in PowerShell change the date and start time on the below for your needs

```powershell
get-winevent -computername DC.Domain.local -FilterHashtable @{ LogName = "Security"; Starttime = "21/04/2019" ; ID = 4725 } | fl
```

The field SubjectUserName is the user who disabled the account.
Building this out in broader environments. If you had an automated system to disable user accounts you could use this to find the last weeks disables which bypassed this process. If you do have a lot of disables, you could pipe this to a variable and filter the data searching for the username.

---

With Outlook 2016 most shared mailboxes will automatically be added to your outlook client. If the mailbox has been around for awhile it needs a little bit of help. This is the guide I send to users to add or remove a shared mailbox.

* Select File in the upper left corner of the screen.
* Select Account Settings > Account Settings.
* Select your primary account and then double click on it
* Click More settings
* Select Advanced tab
  * To add a mailbox, Select Add and type the email address of the mailbox as it appears in the address list
  * Remove the mailbox select the mailbox you are wanting to remove
* Select OK
* Select Next
* Select Done
* Select Close
* Restart Outlook and see if the mailbox is Added/removed

> Tip - If you don't find your mailbox in the account settings contact your IT department as this a new style mailbox and you will need your permissions removed for it to be removed from Outlook.

## Extra Credit

[Adding a shared mailbox - Outlook Web Access](https://support.office.com/en-us/article/open-and-use-a-shared-mailbox-in-outlook-web-app-bc127866-42be-4de7-92ae-1ef2f787fd5c)
[Adding a shared Mailbox - Outlook for Mac](https://support.office.com/en-gb/article/open-a-shared-folder-or-mailbox-in-outlook-for-mac-6ecc39c5-5577-4a1d-b18c-bbdc92972cb2)

*This post is a "just for documentation (JFD)" post which I use for my reference, but I have made public as it may be useful. I put less time into these posts as they are my reference. If you would like more information or some help post a comment, and I can see what I can do.*
