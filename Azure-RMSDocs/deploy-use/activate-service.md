---
# required metadata

title: Activating Azure Rights Management | Azure Information Protection
description: The Azure Rights Management service must be activated before your organization can start to protect documents and emails by using applications and services that support this information protection solution. 
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 02/08/2017
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: f8707e01-b239-4d1a-a1ea-0d1cf9a8d214

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Activating Azure Rights Management

>*Applies to: Azure Information Protection, Office 365*

When the Azure Rights Management service for Azure Information Protection is activated, your organization can start to protect important data by using applications and services that support this information protection solution. Administrators can also manage and monitor protected files and emails that your organization owns. This service must be enabled before you can begin to use the information rights management (IRM) features within Office, SharePoint, and Exchange, and protect any sensitive or confidential file.

If you want to learn more about the Azure Rights Management service before you activate the it—for example, what business problems it solves, some typical use cases, and how it works—see [What is Azure Rights Management?](../understand-explore/what-is-azure-rms.md)

> [!IMPORTANT]
> Before you activate [!INCLUDE[aad_rightsmanagement_2](../includes/aad_rightsmanagement_2_md.md)], make sure that your organization has a service plan that includes Azure Rights Management data protection. If not, you will not be able to activate Azure Rights Management.
>
> You must have either an [Azure Information Protection Premium plan](https://www.microsoft.com/en-us/cloud-platform/azure-information-protection-pricing) or an [Office 365 plan that includes Rights Management](http://download.microsoft.com/download/E/C/F/ECF42E71-4EC0-48FF-AA00-577AC14D5B5C/Azure_Information_Protection_licensing_datasheet_EN-US.pdf).

When the Azure Rights Management service is activated, all users in your organization can apply information protection to their files, and all users can open (consume) files that have been protected by the Azure Rights Management service. However, if you prefer, you can restrict who can apply information protection, by using onboarding controls for a phased deployment. For more information, see the [Configuring onboarding controls for a phased deployment](#configuring-onboarding-controls-for-a-phased-deployment) section in this article.

For instructions how to activate the Rights Management service from your management portal, select whether you will use the Office 365 admin center (preview or classic), or the Azure classic management portal:


- [Office 365 admin center - preview](activate-office365-preview.md)
- [Office 365 admin center - classic](activate-office365-classic.md)
- [Azure classic portal](activate-azure-classic.md)

Alternatively, you can use PowerShell to activate [!INCLUDE[aad_rightsmanagement_2](../includes/aad_rightsmanagement_2_md.md)]:

1. Install the Azure Rights Management Administration Tool, which installs the Azure Rights Management administration module. For instructions, see [Installing Windows PowerShell for Azure Rights Management](../deploy-use/install-powershell.md).

2. From a PowerShell session, run [Connect-AadrmService](https://msdn.microsoft.com/library/windowsazure/dn629415.aspx), and when prompted, provide the global administrator account details for your Azure Information Protection tenant.

3. Run [Enable-Aadrm](http://msdn.microsoft.com/library/windowsazure/dn629412.aspx), which activates the Azure Rights Management service.

## Configuring onboarding controls for a phased deployment
If you don’t want all users to be able to protect files immediately by using Azure Rights Management, you can configure user onboarding controls by using the [Set-AadrmOnboardingControlPolicy](http://msdn.microsoft.com/library/azure/dn857521.aspx) PowerShell command. You can run this command before or after you activate the Azure Rights Management service.

> [!IMPORTANT]
> To use this command, you must have at least version **2.1.0.0** of the [Azure Rights Management PowerShell module](http://go.microsoft.com/fwlink/?LinkId=257721).
>
> To check the version you have installed, run: **(Get-Module aadrm –ListAvailable).Version**

For example, if you initially want only administrators in the “IT department” group (that has an object ID of fbb99ded-32a0-45f1-b038-38b519009503) to be able to protect content for testing purposes, use the following command:

```
Set-AadrmOnboardingControlPolicy – SecurityGroupObjectId fbb99ded-32a0-45f1-b038-38b519009503
```
Note that for this configuration option, you must specify a group; you cannot specify individual users. To obtain the object ID for the group, use Azure AD PowerShell—for example, for [version 1.0](https://msdn.microsoft.com/library/azure/jj151815\(v=azure.98\).aspx) of the module, use the [Get-MsolGroup](https://msdn.microsoft.com/library/azure/dn194130\(v=azure.98\).aspx) command.

Or, if you want to ensure that only users who are correctly licensed to use Azure Information Protection can protect content:

```
Set-AadrmOnboardingControlPolicy -UseRmsUserLicense $true
```

For more information about this cmdlet and additional examples, see the [Set-AadrmOnboardingControlPolicy](https://msdn.microsoft.com/library/dn857521.aspx) help.

When you use these onboarding controls, all users in the organization can always consume protected content that has been protected by your subset of users, but they won’t be able to apply information protection themselves from client applications. For example, they won’t see in their Office clients the default templates that are automatically published when the Azure Rights Management service is activated, or custom templates that you might configure.  Server-side applications, such as Exchange, can implement their own per-user controls for Rights Management integration to achieve the same result.


## Next steps
Now that you’ve activated [!INCLUDE[aad_rightsmanagement_1](../includes/aad_rightsmanagement_1_md.md)] for your organization, use the [Azure Information Protection deployment roadmap](../plan-design/deployment-roadmap.md) to check whether there are other configuration steps that you might need to do before you roll out Azure Information Protection to users and administrators. 

For example, you might want to use [custom templates](configure-custom-templates.md) to make it easier for users to apply information protection to files, connect your on-premises servers to use [!INCLUDE[aad_rightsmanagement_1](../includes/aad_rightsmanagement_1_md.md)] by installing the [Rights Management connector](deploy-rms-connector.md), and deploy the [Azure Information Protection client](../rms-client/aip-client.md) that supports protecting all file types on all devices. 

Office services, such as Exchange Online and SharePoint Online require additional configuration before you can use their Information Rights Management (IRM) features. 
For information about how your applications work with the Rights Management service, see [How applications support the Azure Rights Management service](../understand-explore/applications-support.md).


[!INCLUDE[Commenting house rules](../includes/houserules.md)]