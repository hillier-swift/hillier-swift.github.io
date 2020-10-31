---
title: Automatically remove Windows 10 built in applications with MDT
categories:
  - PowerShell
tags:
  - Windows 10
  - Deployment
  - MDT
---

Windows 10 love it or hate it; it's here to stay with and with the feature updates coming a few times a year keeping that clean base image isn't easy. One of the main complaints I have got from Windows 10 enterprise is why does it have all the bloat like Xbox live or Paint 3D. I have tried a few methods to achieve my goal. All of which have lead me to the final version of the script of the bottom of this post.  
  
My first failure was running the script manually on the base image and using MDT to capture this image caused sysprep issues. If you don't remove all appx and appx provisioned packages it will throw an error. You can tell this is the case by looking at `"C:\Windows\Panther\setuperr.log"` scrolling down to the bottom and seeing the appx package mentioned, I did this a few times and once you have the list of applications right you run into the below.  
  
Running the script then capturing works when using a recovery platform like Ghost but when you deploy a new machine with MDT they return errors. You can get around this by using DISM on the operating systems source files and running the script like below but rather than -online use –Path c:\offline to the OS files. My issue with this is each time you create a new base image you have to remember to do task also this messes with the pure OS source files and these can come in handy.  
  
It's current and hopefully, the final form is using a task sequence in MDT running as an application in the deployment. It's far from perfect and it doesn't remove all the AppX applications from the Administrator account but does for all new users which as we disable the built-in Admin is good enough. This means when you have a new base image you can do what you need to like Windows updates and just update the WIM file in your task sequence and don't have to remember to remove the packages. Also, if a new package in Windows 10 version 1709 or beyond appears just add its name to the list in $appsList.

```powershell
$AppsList = "Microsoft.XboxSpeechToTextOverlay","Microsoft.XboxIdentityProvider","Microsoft.XboxGameOverlay","Microsoft.Messaging","Microsoft.Office.OneNote","Microsoft.WindowsFeedbackHub","Microsoft.OneConnect","AdobeSystemsIncorporated.AdobePhotoshopExpress","*.EclipseManager","Microsoft.BingFinance","Microsoft.BingNews","Microsoft.BingWeather","Microsoft.XboxApp","Microsoft.SkypeApp","Microsoft.MicrosoftSolitaireCollection","Microsoft.BingSports","Microsoft.ZuneMusic","Microsoft.ZuneVideo","Microsoft.Windows.Photos","Microsoft.People","Microsoft.MicrosoftOfficeHub","Microsoft.WindowsMaps","microsoft.windowscommunicationsapps","Microsoft.Getstarted","Microsoft.3DBuilder","*.Duolingo-*", "actiprosoft*", "bingTrans*", "*FreshPaint*", "*getstarted", "*office.sway", "*RemoteDesktop"  
  
ForEach ($App in $AppsList)  
{  
    $PackageFullName = (Get-AppxPackage $App).PackageFullName  
    $ProPackageFullName = (Get-AppxProvisionedPackage -online | where {$_.Displayname -eq $App}).PackageName  
    write-host $PackageFullName  
    Write-Host $ProPackageFullName  
    if ($PackageFullName)  
        {  
            Write-Host "Removing Package: $App"  
            remove-AppxPackage -package $PackageFullName  
        }  
        else  
        {  
            Write-Host "Unable to find package: $App"  
        }  
    if ($ProPackageFullName)  
        {  
            Write-Host "Removing Provisioned Package: $ProPackageFullName"  
            Remove-AppxProvisionedPackage -online -packagename $ProPackageFullName  
        }  
    else  
        {  
            Write-Host "Unable to find provisioned package: $App"  
        }  
}  
```

I hope you have found this useful, I am hoping to have a few more posts coming up about MDT using DFSR, and WDS with some config files changes to have local silos of deployments around the world.
