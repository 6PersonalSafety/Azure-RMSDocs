---
# required metadata

title: Protection-only mode for the Azure Information Protection client
description: Information for users who run the Azure Information Protection client in protection-only mode. 
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 01/30/2017
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: 16042717-0d7a-41f5-87e3-12826fda35df


# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: eymanor
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Protection-only mode for the Azure Information Protection client

When you run the Azure Information Protection client without an Azure Information Protection policy, it displays in **protection-only** mode. This happens in the following scenarios:

- Your organization does not have a subscription for Azure Information Protection (for classification and protection of data) but has a subscription for the Azure Rights Management service (for data protection). 
    - This is a supported scenario and you can use the Azure Information Protection client to protect files and view protected files.

- Your organization has a subscription for Azure Information Protection but you cannot download the Azure Information Protection policy. 
    - This can happen because of a misconfiguration or because your sign in is not successful. Contact your help desk or administrator but in the meantime, you might be able to use the Azure Information Protection client to protect files and view protected files.

## Limitations for protection-only mode

- In Office apps, the Azure Information Protection bar does not display. When you click **Protect** > **Show Bar**, this menu option is unavailable.

- When you use the **Classify and protect - Azure Information Protection** dialog box with File Explorer, you do not see labels for classification. Instead, you see an option to select Rights Management templates. 

## Supported tasks for protection-only mode

- Protect (and unprotect) documents and emails from within your Office apps, by using the Office Information Rights Management (IRM) feature: For example: Click **File** > **Info** > **Protect Document** > **Restrict Access**. For more information, see [Using information protection with Office 365, Office 2016, or Office 2013](../deploy-use/help-users.md).

- Protect (and unprotect) files by using Windows File Explorer: Right-click the file, files, or folder > **Classify and protect**. To apply protection that has been configured by your administrator, in the **Classify and protect - Azure Information Protection** dialog box, click **Select template** and choose one of the available templates.

- View protected files by using the Azure Information Protection Viewer.

- Access the document tracking site from your Office apps. However, you must have a valid subscription to track and revoke documents from this site.

[!INCLUDE[Commenting house rules](../includes/houserules.md)]  
