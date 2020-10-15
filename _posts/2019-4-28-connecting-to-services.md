---
title: Connecting to Services Script - With MFA
---

As I have started to spend more and more time using PowerShell connecting to all the necessary services when I needed was beginning to be a drag. Also, every time I wrote a script I added a section for connecting to the required services. After finding some spare time one long weekend, I created a modular script and set my self a few rules.

1. I must be able to call this from another script.
1. Must support MFA
1. I should only enter data once where I can
1. If something goes wrong, I want the script to stop.

   This is important as I haven't built in error correction ready for this script in my existing scripts. When I have fixed this up, I will take the pauses out.

1. It should not repeat its self

   Failed a little bit on this one as final connection is just a bunch of ifs.

This went quite well for Version 1.0, there are a few things to improve, but I wanted to get some to build on.

## Currently supported systems

* Exchange Online,
* MSOL
* Azure AD
* On-Premise Exchange
* On Premise Lync
* Skype for Business Online
* Teams

## Prerequisites

* Exchange Online - A guide to setting this up with MFA can be [found here](https://hillier-swift.co.uk/connecting-to-exchange-online-with-mfa/)
* Connect-MSolService [Install Microsoft Online Services Sign-In Assistant for IT Professionals](https://www.microsoft.com/en-us/download/details.aspx?id=41950) Then run `Install-Module MSOnline`
* [Azure-AD KB](https://docs.microsoft.com/en-us/office365/enterprise/powershell/connect-to-office-365-powershell#connect-with-the-microsoft-azure-active-directory-module-for-windows-powershell) Just run `Install-Module -Name AzureAD`
* Skype for Business Online Module requires [Microsoft Visual C++ 2017 x64 Minimum runtime 14.10.25008](https://aka.ms/vs/15/release/vc_redist.x64.exe) Once you have installed C++ runtime install the [Skype for Business Online Powershell Module](https://www.microsoft.com/en-us/download/details.aspx?id=39366)
* Teams - `Install-Module -Name MicrosoftTeams`
* My Script on your machine which can be pulled from my [GitHub Page](https://github.com/hillier-swift/Public/blob/master/Connect-ToServices.ps1)

## First run

Edit any values in the get-custom section at the top of the script to your environment requirements. If you don't need a part you can delete it or just leave it.

## The Code

As this is my first more extensive project that would fill up a blog post I have hosted the file on [my GitHub public repo](https://github.com/hillier-swift/Public/blob/master/Connect-ToServices.ps1). I am happy for any feedback; this is the first time I have looked at doing a more fully formed script using functions.

### Examples

```powershell
#This will connect to Exchange Online, On-Premise Exchange and Teams, prompting for an email and on Prem credentials  
Connect-toServices.ps1 -ExchangeOnline -onpremExchange -Teams
```
