---
header:   
  overlay_image: /images/Header/2020-5-3-azure-ad-blocking-guest-users.jpg
  overlay_filter: 0.5
  caption: "Photo credit: [**unsplash: @anikinearthwalker**](https://unsplash.com/@anikinearthwalker?utmsource=ghost&utmmedium=referral&utmcampaign=api-credit)"
title: Azure AD blocking Guest users Error 53004
excerpt: "Troubleshooting issues with Azure AD where guest users look like they require your MFA setup"
categories:
  - M365
tags:
  - Azure AD
  - Security
---

Had some fun troubleshooting this one, it makes sense once you know about it but takes a moment to troubleshoot. The long and short of it Azure AD does not show you if a guest user has a risky user flag but if you have a risk policy assigned to all users or guest, it will try to enforce policies such as MFA to those users. Due to the users having their tenancy they wouldn’t be able to fulfil this requirement causing a failed log in.

## Symptoms

Guest user to your tenancy is unable to sign in, they know of nothing wrong and all sign-ins to their home tenant are fine. They could get a message like the below when accessing your resource.

![Sign in blocked](/images/2020-5-3-azure-ad-1.png "Sign in blocked")

And in Azure AD the sign-in might look like this, in our instance, we have MFA enforced for risky sign-ins.

![Azure AD blocked event](/images/2020-5-3-azure-ad-2.png "Azure AD blocked event")

## Problem

If a guest user has a risky flag on their Home tenancy, this flag gets invisibility passed to your tenancy. If you have a risky user policy higher than the default, they might never know they had a risk when the user sign into yours, they get stopped. In this case, the issue is hidden by Azure AD logs requesting to enrol in MFA but as its not their home one, this is not possible.

## Solution

Get the guest to confirm if there is any risky sign in with their account in their tenancy. If so contact their support structure to clear this appropriability. 

* Sign in to the Azure portal Home - Microsoft Azure
* In the top search box search for ‘Azure Active Directory.’
* Select Security
* Under report select Risky users
* Select users account and select dismiss users risk.
   * After an investigation on why the risk was raised, clicking on the risk gives more detail.
   * If the option is greyed out, the signed-in user doesn’t have rights to clear risks.

Being an ICT support professional not in the guest’s home tenant, I would not advise anyone to clear a risk without confirming and understanding what they are doing. I see this issue with a lot of small companies who may not understand the impact of just clearing a risky sign in.

## Further reading

Microsoft KB on Guest protection - <https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/concept-identity-protection-b2b>
My User voice on having access to see the risky flag coming in -
<https://feedback.azure.com/forums/34192--general-feedback/suggestions/40307323-azure-identity-protection-risky-sign-in-flag-for-g>
