---
# required metadata

title: Classify and protect a file or email by using Azure Information Protection | Azure Information Protection
description: Instructions how to classify and protect your documents and emails.
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 02/08/2017
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: 75268245-6f14-4218-b904-202f63fb3ce6

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: eymanor
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Classify and protect a file or email by using Azure Information Protection

>*Applies to: Active Directory Rights Management Services, Azure Information Protection, Windows 10, Windows 8.1, Windows 8, Windows 7 with SP1*

The easiest way to classify and protect your documents and emails is when you are creating or editing them from within your Office desktop apps: **Word**, **Excel**, **PowerPoint**, **Outlook**. 

However, you can also classify and protect files by using **File Explorer**, which supports additional file types and is a convenient way to classify and protect multiple files at once.

## Using Office apps to classify and protect your documents and emails

Use the Azure Information Protection bar and select one of the labels that has been configured for you. 

For example, the following picture shows that the document hasn't yet been labeled because the **Sensitivity** shows **Not set**. To set a label, such as "Internal", click **Internal**. If you're not sure which label to apply to the current document or email, use the label tooltips to learn more about each label and when to apply it.

![Azure Information Protection bar example](../media/info-protect-bar-not-set-callout.png)

If a label is already applied to the document and you want to change it, you can select a different label. If the labels are not displayed on the bar, first click the **Edit Label** icon, next to the current label value.

In addition to manually selecting labels, labels can also be applied in the following ways:

- Your administrator configured a default label, which you can keep or change.

- Your administrator configured recommended prompts to select a specific label when sensitive data is detected. You can accept the recommendation (and the label is applied), or reject it (the recommended label is not applied).

### Exceptions for the Azure Information Protection bar 

Don't see this Information Protection bar in your Office apps?

- You might not have the Azure Information Protection client [installed](install-client-app.md), or the client is running in [protection-only mode](client-protection-only-mode.md).
 
Is the label that you expect to see not displayed on the bar? 

- If your administrator has recently configured a new label for you, try closing all instances of your Office app and reopening your Office app. This action checks for changes to your labels.


### Keyboard shortcuts for the Azure Information Protection bar

To access the Azure Information Protection bar by using keyboard shortcuts, use the following key combination:

- Press **Ctrl** + **Shift** + **~** 

Then, use the Tab key to select the labels and other controls on the bar (the **Hide labels** icon and **Remove label** icon), and the Enter key to select them.

## Using File Explorer to classify and protect files

When you use File Explorer, you can quickly classify and protect a single file, multiple files, or a folder. 

When you select a folder, all the files in that folder and any subfolders it has are automatically selected for the classification and protection options that you set. However, new files that you create in that folder or subfolders are not automatically configured with those options.

When you use File Explorer to classify and protect your files, if one or more of the labels appear dimmed, the files that you selected do not support classification. For these files, you can select a label only if your administrator has configured the label to apply protection. Or, you can specify your own protection settings. 

Some files are automatically excluded from classification and protection, because changing them might stop your PC from running. Although you can select these files, they are skipped as an excluded folder or file. Examples include executable files and your Windows folder.

The admin guide contains a full list of the file types supported and the files and folders that are automatically excluded: [File types supported by the Azure Information Protection client](client-admin-guide-file-types.md).


### To classify and protect a file by using File Explorer

1.  In File Explorer, select your file, multiple files, or a folder. Right-click, and select **Classify and protect**. For example:

![File Explorer right-click Classify and protect using Azure Information Protection](../media/right-click-classify-protect.png)

2. In the **Classify and protect - Azure Information Protection** dialog box, use the labels as you would do in an Office application, which sets the classification and protection as defined by your administrator. 

    - If none of the labels can be selected (they display as unavailable): The selected file does not support classification but you can protect it with custom permissions (step 3).
    
    - If you see **Company pre-defined protection** in this dialog box: The client is running in [protection-only mode](client-protection-only-mode.md). Either select a template to apply protection that your administrator has configured for you, or, select **Custom permissions** to specify your own protection settings and go to step 4.
    
3. If you want to specify your own protection settings rather than use the protection settings that your administrator might have included with your selected label, select **Protect with custom permissions**.
    
    Any custom permissions that you specify replace rather than supplement protection settings that your administrator might have defined for your chosen label.  

4. If you selected the custom permissions option, now specify the following:

    - **Select permissions**: Select the level of access that you want people to have when you protect the selected file or files.
    
    - **Select users**: Specify the people who should have the permissions you selected for your file or files. For people and groups in your organization, you can use the address book to search and select them. For people in another organization, you must specify their full email address. Make sure that you use a business email address because personal email addresses are not currently supported.
        
    - **Expire access**: Select this option only for time-sensitive files so that the people you specified will not be able to open your selected file or files after a date that you specify. You will still be able to open the original file but after midnight (your current time zone), on the day that you select, the people that you specified will not be able to open the file.

5. Click **Apply** and wait for the **Work finished** message to see the results. Then click **Close**.

The selected file or files are now classified and protected, according to your selections. In some cases (when adding protection changes the file name extension), the original file in File Explorer is replaced with a new file that has the Azure Information Protection lock icon. For example:

![Protected file with lock icon for Azure Information Protection](../media/Pfile.png)

If you change your mind about the classification and protection, or later need to modify your settings, simply repeat this process with your new settings.

The classification and protection that you specified stays with the file, even if you email the file or save it to another location. If you protected the file, you can track how people are using it and if necessary, revoke access to it. For more information, see [Track and revoke your protected documents when you use Azure Information Protection](client-track-revoke.md). 


## Other instructions
For how-to instructions, see the following sections from the Azure Information Protection user guide:

-   [What do you want to do?](client-user-guide.md#what-do-you-want-to-do)

[!INCLUDE[Commenting house rules](../includes/houserules.md)]
