---
header:   
  image: /images/Header/2019-3-17-connecting-to-exchange-online-with-mfa.jpg
  caption: "Photo credit: [**unsplash: @heytowner**]("https://unsplash.com/@heytowner")
title: Connecting to Exchange Online with MFA
toc: true
categories:
  - PowerShell
  - M365
tags:
- Exchange
---

I run a script to manage leaver accounts for my company; for a while now to get Exchange Online to play nice I have bypassed MultiFactor for the time I need the connection. I found some free time and started to research a way of MFA and Exchange Online to play nicely together improving my workflow and increasing my security.  

> Note this as been supserded by <https://docs.microsoft.com/en-us/powershell/exchange/exchange-online-powershell-v2?view=exchange-ps>

## Prerequisites

Install the Exchange Online PowerShell 'Module' [Microsoft's KB](https://docs.microsoft.com/en-us/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell?view=exchange-ps). You can find it from the Exchange Online portal (IE or Edge).

1. Open Internet Explorer or Edge
1. Open Exchange Online Admin Center [ECP](https://outlook.office365.com/ecp) 
1. Click Hybrid on the left menu
1. Click configure under Exchange Online PowerShell Module
1. The run the one once tool opens, hence the need for IE or Edge. 

## Script use

The run once tool installs some files into your Local AppData folder but also puts a shortcut in your start menu called "Microsoft Exchange Online Powershell Module." Click on this, and a PowerShell window opens with some helpful text on how to use it. To get started run the command in the screenshot below with your UPN and depending on your configuration and device it might connect you with no credentials needed or prompt for a password and MFA token.

![PowerShell screenshot](/images/2019-3-17-Exchange-PSREmote1.png "PowerShell screenshot")

## Using the script without the start menu shortcut

All the above might be great for doing the odd ticket and random tasks, but you can't automate this or use this for testing in an ISE and who wants to click a start menu item when you're already in PowerShell.
The below line will help with it; this searches your Local App Data for the script and pops the folder into a variable.

```powershell
$CreateEXOPSSession = (Get-ChildItem -Path $Env:LOCALAPPDATA\Apps\2.0*  -Filter CreateExoPSSession.ps1 -Recurse -ErrorAction SilentlyContinue -Force | Select -Last 1).DirectoryName
```

Then you can simply call the script using the the below

```powershell
& "$CreateEXOPSSession\CreateExoPSSession.ps1" -UserPrincipalName Blah@blah.com
```

> ProTip: Watch out the first two lines of the script as it changes your location to your userprofile.

## Making it portable

*[Skip](#workflow) to the workflow without knowing know the things I tried.*

It now gets tricky just copying the files from Local App Data doesn't work, the write-host works but some of the commands don't import correctly.
These are some of the things I tried and failed

* Copy the files to a new folder and call script.
* Creating it as a module.
  * As a module, I tried importing with a global flag.
* Adding the Verbose flag to the module worked intermittently but then completely stopped working when I ran the start menu shortcut.
* Running PowerShell as an admin.

Finally, I gave up on the module idea as if it was meant to be one, Microsoft would have deployed it that way. I went back to the drawing board and tried importing the script as a module with the force flag.  This shouldn't be the issue as each test I removed the module or opened a new PS session, but it worked so there must be more logic I am missing. I have tried this on some machines, and it consistently works so sticking with the force flag.

<a name="workflow"></a>The workflow to make this portable is below.

1. Run the click Once to get the module
2. Find the install location by the commands above in "Calling script again without having to open from Start menu" section
3. Copy the whole folder to somewhere you want to keep it.
4. You can now import this using the below then the second line to connect.

    ```powershell
    Import-Module .\CreateExoPSSession.ps1 -Force -Verbose
    .\CreateExoPSSession.ps1" -UserPrincipalName Blah@blah.com
    ```

5. You can call the first line in any of your scripts
With the issues I had getting this running I have put a test in my scripts to ensure it has imported the module correctly and example of this is below.

```powershell
Try
    {
      Get-Command Connect-EXOPSSession -ErrorAction stop
    }
Catch
    {
      Write-host "Connect-EXOPSSession not found"
    }
```

## Adding a prefix

The whole reason for me looking into this script is it's a part of a larger script, including Exchange on-prem and other online services. When connecting to Exchange Online and Exchange on Prem the command get-mailbox isn't clear which environment the mailbox is from, and the two will conflict. I could take the easy route and have Get-Mailbox be Online, and Get-OnPremMailbox to be the different one. My issue with this is it's not clear enough which is which. As we now have this away from the click once installer we can edit the script without fear of it updating.

The line in the script you need to update is as of today the line 301 but do a search for $PSSessionModuleInfo, and you will find the Import-PSSession you are after. Feel free to use your Prefix of choice, but an example of mine is below.

```powershell
$PSSessionModuleInfo = Import-PSSession $PSSession -AllowClobber
```

to

```powershell
$PSSessionModuleInfo = Import-PSSession $PSSession -Prefix EOL -AllowClobber
```

## Summary

This was a longer task than I first thought and frustrating as the best practice for scripts is to provide a module, but Microsoft's Exchange team have always been a bit different. I was able to roll this into my larger script and carry on my workload without removing MFA just would have been easier if there was a consistent approach to remote PS sessions from Microsoft.
