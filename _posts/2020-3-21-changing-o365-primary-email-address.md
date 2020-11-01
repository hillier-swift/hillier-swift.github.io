---
header:   
  overlay_image: /images/Header/2020-3-21-changing-o365-primary-email-address.jpg
  overlay_filter: 0.5
  caption: "Photo credit: [**unsplash: @ventiviews**](https://unsplash.com/@ventiviews)"
title: Changing the primary email address of an Office 365 Group
excerpt: "Sometimes users change their mind and an office 365 group now needs to be a shared mailbox or they didn't like the email address they created in the first place."
categories:
  - M365
  - PowerShell
tags:
  - JFD
---

## The process

1. Connect to EXOL online (Read this post for more infomation) [Connecting to Services](https://hillier-swift.co.uk/connecting-to-services/).
2. Run the below command to add the required SMTP address as an alias. `$UPN` is the current Primary Email address and `$newUPN` is what you would like to change it too. Example below

```powershell
$UPN="Email@hillier-swift.co.uk"
$NewUPN="Email-old@hillier-swift.co.uk"

Set-UnifiedGroup -Identity "$UPN" -EmailAddresses: @{Add ="$NewUPN"}
```

3. Promote alias as a primary SMTP address,
`Set-UnifiedGroup -Identity $UPN -PrimarySmtpAddress $NewUPN`
4. If not required, you can remove first ID using below command.
`Set-UnifiedGroup -Identity $NewUPN -EmailAddresses: @{Remove=$UPN}`

Please note, For updating the Office 365 group SMTP address required Global Administrator access.

## Bonus

I was doing this to migrate an O365 group to a Shared Mailbox, the below you can use to add the UPN to a shared Mailbox.

```powershell
$SharedUPN = "SharedEmail@hillier-swift.co.uk"
Set-Mailbox $sharedUPN -EmailAddresses @{Add=$UPN}
```

*This post is a "just for documentation (JFD)" post which I use for my reference, but I have made public as it may be useful. I put less time into these posts as they are my reference. If you would like more information or some help post a comment, and I can see what I can do.*
