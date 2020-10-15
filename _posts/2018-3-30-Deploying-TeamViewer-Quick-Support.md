---
layout: post
title: Deploying TeamViewer Quick Support
layout: single
categories:
  - PowerShell
tags:
  - TeamViewer
  - Deployment
---

I ran into an issue with TeamViewer quick support (QS) being deployed over our network. We were trying to find a way of deploying QS differently if a user was connected to the network over a VPN or was connected locally so could access DFS shares. Allowing VPN users who could be anywhere in the world to get QS from an online location rather than pulling from our single Datacenter.  
  
I came up with the below which pulls Quick Support from two different locations. First one a network share, in this case, its a DFS path. The second does a call to the TeamViewer QuickSupport link created in our TeamViewer account and downloads our custom QS. Both methods put the file in a Local folder. The detection is done via another script which I may write about another time  
  
Usage  

```powershell
Get-TeamviewerQS.ps1 -method online
```
  
Full Code  

```powershell
#region Parameter  
 Param(  
        [string]$Path = "C:\Path",  
        [parameter(Mandatory=$true)][ValidateSet("Online","DFS")][String]$Method ="Online"  
      )  
  
#endregion  
$exePath = $Path + "TeamViewerQS.exe"  
#If Teamviewer exists delete it  
If (Test-Path $Path)  
    {  
        Remove-Item $Path  
        $actionTaken += "$(Get-Date) - Get-TeamViewer.ps1 - Removed TeamViewerQS.exe" + "`r`n"  
    }  
if (test-path $Path)  
{  
$EventMessage = "$(Get-Date) - Get-TeamViewerQS.ps1 - folder exists continuing" Write-Host $EventMessage $ActionTaken += $EventMessage + "`r`n" } Else { New-Item $Path -ItemType Directory $EventMessage = "$(Get-Date) - Get-TeamViewer.ps1 - $path Created" Write-Host $EventMessage $ActionTaken += $EventMessage + "`r`n" }  
If ($Method -eq "Online")  
{  
$URI = "https://get.teamviewer.com/YourCustomURL"  
#Downloads the file and copys it to a folder  
Try {  
Import-Module BitsTransfer  
  
#$downloadLink = (Invoke-WebRequest -Uri $URI).ParsedHtml.getElementById('MasterBodyContent_btnRetry').href  
#Its basic Parsing because with standard if IE has not been run which it wouldn't be for a system account it will error  
$Link = (Invoke-WebRequest -UseBasicParsing -Uri $URI).links.outerhtml | where {$_ -like "*TeamViewerQS.exe*" }  
$regex ='\"(.*?)\"'  
  
$downloadlink = $link -split $regex  
  
Start-BitsTransfer -Source $downloadlink[1] -Destination $exePath  
$actionTaken += "$(Get-Date) - Get-TeamViewerQS.ps1 - Downloaded from $($downloadLink[1]) and Deployed to $exePath"+ "`r`n"  
}  
  
Catch {  
$EventMessage = "$(Get-Date) - Get-TeamViewerQS.ps1 - Could not Download TeamViewer Quick Support" + ". " + $_.Exception.Message  
Write-Host $EventMessage  
  
$ActionTaken += $EventMessage + "`r`n"  
}  
}  
If ($Method -eq "DFS")  
    {  
        $URI="\\Your.Domain\DFSpath\TeamViewerQS.exe"  
  
        #Downloads the file and copys it to a folder  
 Try {  
   Import-Module BitsTransfer  
  
#Gets the download link from https://get.teamviewer.com/YourLink  
  
   Start-BitsTransfer -Source $URI -Destination $Path  
   $actionTaken += "$(Get-Date) - Get-TeamViewerQS.ps1 - Downloaded from $URI and Deployed to $exepath"+ "`r`n"  
}  
  
Catch {  
   $EventMessage = "$(Get-Date) - Get-TeamViewerQS.ps1 - Could not Download TeamViewer Quick Support" + ". " + $_.Exception.Message  
   Write-Host $EventMessage  
  
   $ActionTaken += $EventMessage + "`r`n"  
  
}  
  
    }  
  
$actionTaken | Out-File $path\Deploy.log -Append
```
  
This came with some challenges you should be made aware. If you use `Invoke-Webrequest` without the `-UseBasicParsing` parameter  it will work during testing as you would have run IE at some point if only to get chrome, so you would of run the first setup. If the first run has not been completed like on a system account the command will fail. You lose some functionality with basic parsing but will gain stability.
