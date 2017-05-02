---
# required metadata

title: Prepare users and groups for Azure Information Protection
description: Check that you have the user and group accounts that you need to start classifying, labeling, and protecting your organization's documents and emails.
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 05/02/2017
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: afbca2d6-32a7-4bda-8aaf-9f93f5da5abc

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Preparing users and groups for Azure Information Protection

>*Applies to: Azure Information Protection, Office 365*

Before you deploy Azure Information Protection for your organization, make sure that you have accounts for users and groups in Azure AD for your organization's tenant.

There are different ways to create these accounts for users and groups, which include:

- You create the users in the Office 365 admin center, and the groups in the Exchange Online admin center.

- You create the users and groups in the Azure portal.

- You create the users and group by using Azure AD PowerShell and Exchange Online cmdlets.

- You create the users and groups in your on-premises Active Directory and synchronize them to Azure AD.

- You create the users and groups in another directory and synchronize them to Azure AD.

When you create users and groups by using the first three methods from this list, they are automatically created in Azure AD, and Azure Information Protection can use these accounts directly. However, many enterprise networks use an on-premises directory to create and manage users and groups. Azure Information Protection cannot use these accounts directly; you must synchronize them to Azure AD.

## How users and groups are used by Azure Information Protection

There are three scenarios for using users and groups with Azure Information Protection:

- **For assigning labels to users** when you use labeling and classification. Only administrators select these groups:
    
    - The default Azure Information Protection policy is automatically assigned to all users in your tenant's Azure AD. However, you can also assign additional labels to specified users or groups by using scoped policies.     

- **For assigning usage rights and access controls** when you use the Azure Rights Management service to protect documents and emails. Administrators and users can select these users and groups:
    
    - Usage rights determine whether a user can open a document or email and how they can use it. For example, whether they can only read it, or read and print it, or read and edit it. 
    
    - Access controls include an expiry date and whether a connection to the Internet is required for access. 

- **For configuring the Azure Rights Management service** to support specific scenarios, and therefore only administrators select these groups. Examples include configuring the following:
    
    - Super users, so that designated services or people can open encrypted content if required for eDiscovery or data recovery.
    
    - Delegated administration of the Azure Rights Management service.
    
    - Onboarding controls to support a phased deployment.

## Azure Information Protection requirements for user accounts

For all three scenarios listed in the previous section, the requirements for user accounts are the same. To authorize users, two attributes in Azure AD are used: **proxyAddresses** and **userPrincipalName**.

- The Azure AD proxyAddresses attribute stores all email addresses for an account and can be populated in different ways. For example, a user in Office 365 that has an Exchange Online mailbox will automatically have an email address that is stored in this attribute. If you assign an alternative email address for an Office 365 user, it is also saved in this attribute. It can also be populated by the email addresses that are synchronized from on-premises accounts. 
    
    Azure Information Protection can use any value in this Azure AD proxyAddresses attribute if the domain has been added to the authenticating tenant (a "verified domain" for your tenant). For more information about verifying domains:
    
    - For Azure AD: [Add a custom domain name to Azure Active Directory](Add a custom domain name to Azure Active Directory)
    
    - For office 365: [Add a domain and users to Office 365](https://support.office.com/article/Add-a-domain-and-users-to-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611) 

- The **userPrincipalName** attribute is used only when an account in your tenant doesn't have values in the Azure AD proxyAddresses attribute. For example, you create a user in the Azure portal, or create a user for Office 365 that doesn't have a mailbox.

## Azure Information Protection requirements for group accounts

For assigning labels, and for assigning usage rights and access controls:

- You can use any type of group in Azure AD that has an email address that contains a verified domain for the user's tenant. A group that has an email address is often referred to as a mail-enabled group. 
    
    For example, you can use a mail-enabled security group, a distribution group (which can be static or dynamic), and an Office 365 group. You cannot use a security group (dynamic or static) because this group type doesn't have an email address.

For configuring the Azure Rights Management service:

- You can use any type of group in Azure AD that has an email address from a verified domain in your tenant, with one exception. That exception is when you configure onboarding controls to use a group, which must be a security group in Azure AD for your tenant.
    
- You can use any group in Azure AD (with or without an email address from a verified domain in your tenant) for delegated administration of the Azure Rights Management service.

## Using accounts from Active Directory on-premises for Azure Information Protection

If you have accounts that are managed on-premises that you want to use with Azure Information Protection, you must synchronize these to Azure AD. For ease of deployment, we recommend that you use [Azure AD Connect](/azure/active-directory/connect/active-directory-aadconnect). However, you can use any directory synchronization method that achieves the same result.

When you synchronize your accounts, you do not need to synchronize all attributes. For a list of the attributes that must be synchronized, see the [Azure RMS section](/azure/active-directory/connect/active-directory-aadconnectsync-attributes-synchronized#azure-rms) from the Azure Active Directory documentation. 

From the attributes list for Azure Rights Management, you'll see that for users, the on-premises AD attributes of **mail**, **proxyAddresses**, and **userPrincipalName** are required for synchronization. Values for **mail** and **proxyAddresses** are synchronized to the Azure AD proxyAddresses attribute. For more information, see [How the proxyAddresses attribute is populated in Azure AD](https://support.microsoft.com/help/3190357/how-the-proxyaddresses-attribute-is-populated-in-azure-ad)

## Confirming your users and groups are prepared for Azure Information Protection

You can use Azure AD PowerShell to confirm that users and groups can be used by Azure Information Protection and the values that can be used to authorize them. 

For example, using the V1 PowerShell module for Azure Active Directory, [M​SOnline](/powershell/module/msonline/?view=azureadps-1.0), in a PowerShell session, first connect to the service and supply your global admin credentials:

	Connect-MsolService
    

Note: If this command doesn't work, you can run `Install-Module MSOnline` to install the MSOnline module.

Next, configure your PowerShell session so that it doesn't truncate the values:

    $Formatenumerationlimit =-1

### Confirm user accounts are ready for Azure Information Protection

To confirm the user accounts, run the following command:

	Get-Msoluser | select DisplayName, UserPrincipalName, ProxyAddresses
        
Your first check is to make sure that the users you want to use with Azure Information Protection are displayed. 

Then check whether the **ProxyAddresses** column is populated. If it is, the email values in this column can be used to authorize the user for Azure Information Protection. 

If the **ProxyAddresses** column is not populated, the value in the **UserPrincipalName** will be used to authorize the user for Azure Information Protection.

For example: 
    
|Display Name|UserPrincipalName|ProxyAddresses
|-------------------|-----------------|--------------------|
|Jagannath Reddy |jagannathreddy@contoso.com|{}|
|Ankur Roy|ankurroy@contoso.com|{SMTP:ankur.roy@contoso.com, smtp: ankur.roy@onmicrosoft.contoso.com}|

In this example:

- The user account for Jagannath Reddy will be authorized by **jagannathreddy@contoso.com**.

-  The user account for Ankur Roy can be authorized by using **ankur.roy@contoso.com** and **ankur.roy@onmicrosoft.contoso.com**, but not **ankurroy@contoso.com**.

In most cases, the value for UserPrincipalName will match one of the values in the ProxyAddresses field. This is the recommended configuration but if you cannot change your UPN to match the email address, you must take the following steps:

1. If the domain name in the UPN value is verified for your Azure AD tenant, add the UPN value as another email address in Azure AD so that the UPN value can now be used to authorize the user account for Azure Information Protection.
    
    If the domain name in the UPN value is not a verified domain for your tenant, it cannot be used with Azure Information Protection. However, the user can still be authorized as a member of a group when the group email address uses a verified domain name.

2. If the UPN is not routable (for example, **ankurroy@contoso.local**), configure alternate login ID for users and instruct them how to sign in to Office by using this alternate login. You must also set a registry key for Office.
    
    For more information, see [Configuring Alternate Login ID](/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) and [Office applications periodically prompt for credentials to SharePoint Online, OneDrive, and Lync Online](https://support.microsoft.com/help/2913639/office-applications-periodically-prompt-for-credentials-to-sharepoint-online,-onedrive,-and-lync-online).

> [!TIP]
> You can use the Export-Csv cmdlet to export the results to a spreadsheet for easier sorting and searching. 
> 
> For example: `Get-MsolGroup | select DisplayName, ProxyAddresses | Export-Csv -Path UserAccounts.csv`

### Confirm group accounts are ready for Azure Information Protection

To confirm group accounts, use the following command:
         
	Get-MsolGroup | select DisplayName, ProxyAddresses

Make sure that the groups you want to use with Azure Information Protection are displayed. For the groups displayed, the email addresses in the **ProxyAddresses** column can be used to authorize the group members for Azure Information Protection.

Then check that the groups contain the users (or other groups) that you want to use for Azure Information Protection. You can use PowerShell to do this (for example, [Get-​Msol​Group​Member](/powershell/module/msonline/Get-MsolGroupMember?view=azureadps-1.0)), or use your management portal. 

For the two service configuration scenarios that use security groups, you can use the following PowerShell command to find the object ID and display name that can be used to identify these groups. You can also use the Azure portal to find these groups and copy the values for the object ID and the display name:

	Get-MsolGroup | where {$_.GroupType -eq "Security"}

## Considerations for Azure Information Protection if email addresses change

If you change the email address of a user or group, we recommend that you add the old email address as a second email address (also known as a proxy address, alias, or alternate email address) to the user or group. When you do this, the old email address is added to the Azure AD proxyAddresses attribute. This account administration ensure business continuity for any usage rights or other configurations there were saved when the old email address was in use. 

If you cannot do this, the user or group with the new email address risks being denied access to documents and emails that were previously protected, and other misconfigurations that used the old value. In this case, you must repeat the configuration to save the new email address. 

Note that it's rare for a group to change its email address and if you assign usage rights to a group rather to than individual users, it doesn't matter if the user's email address changes. In this scenario, the usage rights are assigned to the group email address and not individual user email addresses. This is the most likely (and recommended) method for an administrator to configure usage rights that protect documents and emails. However, users might more typically assign custom permissions for individual users. Because you cannot always know whether a user account or group has been used to grant access, it's safest to always add the old email address as a second email address.

## Group membership caching by Azure Rights Management

For performance reasons, group membership is cached by the Azure Rights Management service. This means that any changes to group membership in Azure AD can take up to 3 hours to take effect when these groups are used by Azure Rights Management, and this time period is subject to change. 

Remember to factor this delay into any changes or testing that you do when you use groups for Azure Rights Management, such as assigning usage rights or configuring the Azure Rights Management service. 


## Next steps

When you have confirmed that your users and groups can be used with Azure Information Protection and you are ready to start protecting documents and emails, activate the Rights Management service to enable this data protection service. For more information, see [Activating Azure Rights Management](../deploy-use/activate-service.md).

[!INCLUDE[Commenting house rules](../includes/houserules.md)]


