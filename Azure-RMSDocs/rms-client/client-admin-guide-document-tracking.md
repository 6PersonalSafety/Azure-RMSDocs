---
# required metadata

title: Document tracking for Azure Information Protection
description: Instructions and information for admins to configure and use document tracking for Azure Information Protection.
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 02/08/2017
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: 983ecdc9-5631-48b8-8777-f4cbbb4934e8

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: eymanor
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---


# Configuring and using document tracking for Azure Information Protection

>*Applies to: Active Directory Rights Management Services, Azure Information Protection, Windows 10, Windows 8.1, Windows 8, Windows 7 with SP1*

If you have a [subscription that supports document tracking](https://www.microsoft.com/en-us/cloud-platform/azure-information-protection-features), the document tracking site is enabled by default for all users in your organization. Document tracking provides information for users and administrators about when a protected document was accessed and if necessary, you can revoke access to it.

If displaying this information is prohibited in your organization because of privacy requirements, you can disable access to the document tracking site by using the [Disable-AadrmDocumentTrackingFeature](/powershell/module/aadrm/disable-aadrmdocumenttrackingfeature) cmdlet. You can re-enable access to the site at any time, by using the [Enable-AadrmDocumentTrackingFeature](/powershell/module/aadrm/enable-aadrmdocumenttrackingfeature), and you can check whether access is currently enabled or disabled by using [Get-AadrmDocumentTrackingFeature](/powershell/module/aadrm/get-aadrmdocumenttrackingfeature). To run these cmdlets, you must have at least version **2.3.0.0** of the Azure Rights Management (AADRM) module for PowerShell. 

## Privacy controls for users and document tracking

When the document tracking site is enabled, by default, it shows information such as the email addresses of the people who attempted to access the protected documents, when these people tried to access them, and their location. Having this level of information can be very helpful to determine how the shared documents are used and whether they should be revoked if suspicious activity is seen. However, for privacy reasons, you might need to disable this user information for some or all users. 

If you have users that should not have this activity tracked, add them to a group that is stored in Azure AD, and specify this group with the [Set-AadrmDoNotTrackUserGroup](/powershell/module/aadrm/Set-AadrmDoNotTrackUserGroup) cmdlet. When you run this cmdlet, you must specify a single group. However, the group can contain nested groups. Do not include external users in these groups.

For these group members, their activity related to documents that others have shared with them is not logged to your document tracking site. When a group member opens a protected document, that action is displayed on your document tracking site but without any of their user details (user name, time, location). In addition, no email notifications are sent to the user who shared the document. For the user who shared the document, that user will be able to see that the document was accessed and can still revoke access to it.

You can use the [Clear-AadrmDoNotTrackUserGroup](/powershell/module/aadrm/Clear-AadrmDoNotTrackUserGroup) if you no longer need this option. Or to selectively remove users, remove them from the group but be aware of [group caching](../plan-design/prepare.md#group-membership-caching). You can check whether this option is currently in use by using [Get-AadrmDoNotTrackUserGroup](/powershell/module/aadrm/get-AadrmDoNotTrackUserGroup). To run the cmdlets for this group configuration, you must have at least version **2.10.0.0** of the Azure Rights Management (AADRM) module for PowerShell.

For more information about each of these cmdlets, use the links provided. For installation instructions, see [Installing Windows PowerShell for Azure Rights Management](../deploy-use/install-powershell.md). If you have previously downloaded and installed the module, check the version number by running: `(Get-Module aadrm –ListAvailable).Version`

> [!IMPORTANT]
> If you have users whose activity and location should not be tracked, add them to a group that is stored in Azure AD, and specify this group with the [Set-AadrmDoNotTrackUserGroup](/powershell/module/aadrm/Set-AadrmDoNotTrackUserGroup) cmdlet.


## Destination URLs used by the document tracking site

The following URLs are used for document tracking and must be allowed on all devices and services between the clients that run the Azure Information Protection client and the Internet. For example, add these URLs to firewalls, or to your Trusted Sites if you're using Internet Explorer with Enhanced Security.

-  `https://*.azurerms.com`

- `https://ecn.dev.virtualearth.net`

    This virtualearth.net URL is for Bing maps to display user location.

- `https://*.microsoftonline.com`

- `https://*.microsoftonline-p.com`

## Tracking and revoking documents for users

When users sign in to the document tracking site, they can track and revoke documents that they have protected by using the Azure Information Protection client or shared by using the Rights Management sharing application. When you sign in as an administrator for Azure Information Protection (global admin), you can click the Admin icon, which switches to Administrator mode so that you can see the documents that have been shared by users in your organization:

![Admin icon in the document tracking site](../media/tracking-site-admin-icon.png)

Actions that you take in Administrator mode are audited and logged in the usage log files, and you must confirm to continue. For more information about this logging, see the next section.

When you are in Administrator mode, you can then search by user or document. If you search by user, you will see all the documents that the specified user has shared. If you search by document, you will see all the users in your organization who shared that document. You can then drill into the search results to track the documents that users have shared and revoke these documents, if necessary. 

To leave the Administrator mode, click **X** next to **Exit administrator mode**:

![Exit administrator mode in the document tracking site](../media/tracking-site-exit-admin-icon.png)

For instructions how to use the document tracking site, see [Track and revoke your documents](client-track-revoke.md) from the user guide.

## Usage logging for the document tracking site

Two fields in the usage log files are applicable to document tracking: **AdminAction** and **ActingAsUser**.

**AdminAction** - This field has a value of true when an administrator uses the document tracking site in Administrator mode, for example, to revoke a document on a user's behalf or to see when it was shared. This field is empty when a user signs in to the document tracking site.

**ActingAsUser** - When the AdminAction field is true, this field contains the user name that the administrator is acting on behalf of as the searched for user or document owner. This field is empty when a user signs in to the document tracking site. 

There are also request types that log how users and administrators are using the document tracking site. For example, **RevokeAccess** is the request type when a user or an administrator on behalf of a user has revoked a document in the document tracking site. Use this request type in combination with the AdminAction field to determine whether the user revoked their own document (the AdminAction field is empty) or an administrator revoked a document on behalf of a user (the AdminAction is true).


For more information about usage logging, see [Logging and analyzing usage of the Azure Rights Management service](../deploy-use/log-analyze-usage.md)



## Next steps
Now that you've configured the document tracking site for the Azure Information Protection client, see the following for additional information that you might need to support this client:

- [Client files and usage logging](client-admin-guide-files-and-logging.md)

- [File types supported](client-admin-guide-file-types.md)

- [PowerShell commands](client-admin-guide-powershell.md)

[!INCLUDE[Commenting house rules](../includes/houserules.md)]
