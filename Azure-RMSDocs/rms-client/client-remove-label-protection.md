---
# required metadata

title: Remove classification labels and protection from files and emails | Azure Information Protection
description: Instructions to remove classification labels and protection from files that have been labeled by Azure Information Protection or protected by Rights Management.
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 01/30/2017
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: 

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Remove labels and protection from files and emails that have been labeled by Azure Information Protection or protected by Rights Management

>*Applies to: Active Directory Rights Management Services, Azure Information Protection, Windows 10, Windows 8.1, Windows 8, Windows 7 with SP1*

When the [Azure Information Protection client is installed on your computer](install-client-app.md), you can remove classification labels and protection from files and emails.

When the label that you remove is configured to apply protection, this action also removes protection from the file. You might be prompted to record why you are removing the label.

> [!IMPORTANT]
> You must be the owner of the file to remove protection, or been granted permissions to remove protection (the Rights Management Extract or Full Control permission).

If you want to choose a different label or a different set of protection settings, you do not need to remove the label or protection. Instead, choose a new label and if necessary, you can define custom permissions. 

You can remove labels and protection from Office documents and emails when you are creating or editing them from within your Office desktop apps: **Word**, **Excel**, **PowerPoint**, **Outlook**. 

You can also remove labels and protection by using **File Explorer**, which supports additional file types and is a convenient way to remove labels and protection from multiple files at once.

## Using Office apps to remove labels and protection from documents and emails

On the Information Protection bar, click the **Delete Label** icon. 

If the **Delete Label** icon is not immediately available, first click the **Edit Label** icon. 

## Using File Explorer to remove labels and protection from files

When you use File Explorer, you can quickly remove labels and protection from a single file, multiple files, or a folder. When you select a folder, all the files in that folder and any subfolders it has are automatically selected. 

1.  In File Explorer, select your file, multiple files, or a folder. Right-click, and select **Classify and protect**. 

2. To remove a label: In the **Classify and protect - Azure Information Protection** dialog box, click **Remove label**. 

3. To just remove protection from a single file: In the **Classify and protect - Azure Information Protection** dialog box, clear the **Protect with custom permissions** option.
    
4. To remove protection from multiple files: In the **Classify and protect - Azure Information Protection** dialog box, click **Remove custom permissions**.

5. Click **Apply** and wait for the **Work finished** message to see the results. Then click **Close**.


## Other instructions
For how-to instructions, see the following sections from the Azure Information Protection user guide:

-   [What do you want to do?](client-user-guide.md#what-do-you-want-to-do)

[!INCLUDE[Commenting house rules](../includes/houserules.md)]