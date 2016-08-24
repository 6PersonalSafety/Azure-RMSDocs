---
# required metadata

title: What Problem Does Azure RMS Solve | Azure RMS
description: Use the following table to identify business requirements or problems that your organization might have, and how Azure' RMS can address these.
author: cabailey
manager: mbaldwin
ms.date: 07/13/2016
ms.topic: article
ms.prod:
ms.service: rights-management
ms.technology: techgroup-identity
ms.assetid: b551c62d-5ac6-4359-85b3-90693e77b37f

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---


# What problems does Azure RMS solve?

>*Applies to: Azure Rights Management, Office 365*

Use the following table to identify business requirements or problems that your organization might have, and how Azure RMS can address these.

|Requirement or problem|Solved by Azure RMS|
|--------------------------|-----------------------|
|Protect all file types|√ In previous implementation of Rights Management, only Office files could be protected, using native protection. Now, [generic protection](../rms-client/sharing-app-dialog-box.md#what-s-the-difference-between-generic-protection-and-built-in-native-protection) means that all file types are supported.|
|Protect files anywhere|√ When a file is saved to a location ([protect in-place](../rms-client/sharing-app-protect-in-place.md)), the protection stays with the file, even if it is copied to storage that is not under the control of IT, such as a cloud storage service.|
|Share files securely by email|√ When a file is shared by email ([share protected](../rms-client/sharing-app-protect-by-email.md)), the file is protected as an attachment to an email message, with instructions how to open the protected attachment. The email text is not encrypted, so the recipient can always read these instructions. However, because the attached document is protected, only authorized users will be able to open it, even if the email or document is forwarded to other people.|
|Auditing and monitoring|√ You can [audit and monitor usage](../deploy-use/log-analyze-usage.md) of your protected files, even after these files leave your organization’s boundaries.<br /><br />For example, you work for Contoso, Ltd. You are working on a joint project with 3 people from Fabrikam, Inc. You email these 3 people a document that you protect and restrict to read-only. Azure RMS auditing can provide the following information:<br /><br />- Whether the people you specified in Fabrikam opened the document, and when.<br /><br />- Whether other people that you didn’t specify attempted (and failed) to open the document—perhaps because it was forwarded or saved to a shared location that others could access.<br /><br />- Whether any of the specified people tried (and failed) to print or change the document.|
|Support for all commonly used devices, not just Windows computers|√ [Supported devices](../get-started/requirements-client-devices.md) include:<br /><br />- Windows computers and phones<br /><br />- Mac computers<br /><br />- iOS tablets and phones<br /><br />- Android tablets and phones|
|Support for business-to-business collaboration|√ Because Azure RMS is a cloud service, there’s no need to explicitly configure trusts with other organizations before you can share protected content with them. If they already have an Office 365 or an Azure AD directory, collaboration across organizations is automatically supported. If they do not, users can sign up for the free [RMS for individuals](rms-for-individuals.md) subscription.|
|Support for on-premises services, as well as Office 365|√  In addition to working [seamlessly with Office 365](office-apps-services-support.md), you can also use Azure RMS with the following on-premises services when you deploy the [RMS connector](../deploy-use/deploy-rms-connector.md):<br /><br />- Exchange Server<br /><br />- SharePoint Server<br /><br />- Windows Server running File Classification Infrastructure|
|Easy activation|√ [Activating the Rights Management service](../deploy-use/activate-service.md) for users requires just a couple of clicks in the Azure classic portal.|
|Ability to scale across your organization, as needed|√ Because Azure RMS runs as a cloud service with the Azure elasticity to scale up and out, you don’t have to provision or deploy additional on-premises servers.|
|Ability to create simple and flexible policies|√ [Customized rights policy templates](../deploy-use/configure-custom-templates.md) provide a quick and easy solution for administrators to apply policies, and for users to apply the correct level of protection for each document and restrict access to people inside your organization.<br /><br />For example, for a company-wide strategy paper to be shared with all employees, you could apply a read-only policy to all internal employees. Then, for a more sensitive document, such as a financial report, you could restrict access to executives only.|
|Broad application support|√ Azure RMS has tight integration with Microsoft Office applications and services, and extends support for other applications by using the RMS sharing application.<br /><br />√ The [Microsoft Rights Management SDK](../develop/developers-guide.md#software-development-kits) provides your internal developers and software vendors with APIs to write custom applications that support Azure RMS.<br /><br />For more information, see [Other applications that support the RMS APIs](api-support.md).|
|IT must maintain control of data|√ Organizations can choose to manage their own tenant key and use the “[Bring Your Own Key](../plan-design/plan-implement-tenant-key.md)” (BYOK) solution and store their tenant key in Hardware Security Modules (HSMs).<br /><br />√ Support for auditing and [usage logging](../deploy-use/log-analyze-usage.md) so that you can analyze for business insights, monitor for abuse, and (if you have an information leak) perform forensic analysis.<br /><br />√ Delegated access by using the [super user feature](../deploy-use/configure-super-users.md) ensures that IT can always access protected content, even if a document was protected by an employee who then leaves the organization. In comparison, peer-to-peer encryption solutions risk losing access to company data.<br /><br />√ Synchronize [just the directory attributes that Azure RMS needs](/active-directory/active-directory-aadconnectsync-attributes-synchronized#azure-rms) to support a common identity for your on-premises Active Directory accounts, by using a [directory synchronization tool](/active-directory/active-directory-hybrid-identity-design-considerations-tools-comparison), such as Azure AD Connect.<br /><br />√ Enable single-sign on without replicating passwords to the cloud, by using AD FS.<br /><br />√ Organizations always have the choice to stop using Azure RMS without losing access to content that was previously protected by Azure RMS. For information about decommissioning options, see [Decommissioning and deactivating Azure Rights Management](../deploy-use/decommission-deactivate.md). In addition, organizations who have deployed Active Directory Rights Management Services (AD RMS) can [migrate to Azure RMS](../plan-design/migrate-from-ad-rms-to-azure-rms.md) without losing access to data that was previously protected by AD RMS.|
> [!TIP]
> If you are familiar with the on-premises version of Rights Management, Active Directory Rights Management Services (AD RMS), you might be interested in the comparison table from [Comparing Azure Rights Management and AD RMS](compare-azure-rms-ad-rms.md).

## Security, compliance, and regulatory requirements
Azure RMS supports the following security, compliance and regulatory requirements:

√ Use of industry-standard cryptography and supports FIPS 140-2. For more information, see the [Cryptographic controls used by Azure RMS: Algorithms and key lengths](how-does-it-work.md#cryptographic-controls-used-by-azure-rms-algorithms-and-key-lengths) information.

√ Support for Thales Hardware Security Modules (HSMs) to store your tenant key in Microsoft Azure data centers. Azure RMS uses separate security worlds for its data centers in North America, EMEA (Europe, Middle East and Africa), and Asia, so your keys can be used only in your region.

√ Certified for the following:

-   ISO/IEC 27001:2013 (includes [ISO/IEC 27018](http://azure.microsoft.com/blog/2015/02/16/azure-first-cloud-computing-platform-to-conform-to-isoiec-27018-only-international-set-of-privacy-controls-in-the-cloud/))

-   SOC 2 SSAE 16/ISAE 3402 attestations

-   HIPAA BAA

-   EU Model Clause

-   FedRAMP as part of Azure Active Directory in Office 365 certification, issued FedRAMP Agency Authority to Operate by HHS

-   PCI DSS Level 1

For more information about these external certifications, see the [Azure Trust Center](http://azure.microsoft.com/support/trust-center/compliance/).

## Next steps

To see what Azure RMS looks like, for administrators and for users, see [Azure RMS in action](what-admins-users-see.md).

If you're interested in more technical information about Azure RMS works, see [How does Azure RMS work?](how-does-it-work.md) 