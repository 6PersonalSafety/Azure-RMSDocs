---
# required metadata

title: Microsoft-managed - tenant key lifecycle operations | Azure Information Protection
description: Information about the lifecycle operations that are relevant if Microsoft manages your tenant key for Azure Information Protection (the default).
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 09/25/2016
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: 3c48cda6-e004-4bbd-adcf-589815c56c55

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---


# Microsoft-managed: Tenant key lifecycle operations

>*Applies to: Azure Information Protection, Office 365*

If Microsoft manages your tenant key for Azure Information Protection (the default), use the following sections for more information about the lifecycle operations that are relevant to this topology.

## Revoke your tenant key
When you cancel your subscription for Azure Information Protection, Azure Information Protection stops using your tenant key and no action is needed from you.

## Re-key your tenant key
Re-keying is also known as rolling your key. Do not re-key your tenant key unless it’s really necessary. Older clients, such as Office 2010, were not designed to handle key changes gracefully. In this scenario, you must clear the Rights Management state on computers by using Group Policy or an equivalent mechanism. However, there are some legitimate events that may force you to re-key your tenant key. For example:

-   Your company has split into two or more companies. When you re-key your tenant key, the new company will not have access to new content that your employees publish. They can access the old content if they have a copy of the old tenant key.

-   You believe the master copy of your tenant key (the copy in your possession) was compromised.

You can re-key your tenant key by [contacting Microsoft Support](../get-started/information-support.md#to-contact-microsoft-support) to open an **Azure Information Protection support case with a request to re-key your Azure Information Protection tenant key**. You must prove you are an administrator for your Azure Information Protection tenant, and understand that this process will take several days to confirm. Standard support charges apply; re-keying your tenant key is a not a free-of-charge support service.

When you re-key your tenant key, new content is protected by using the new tenant key. This happens in a phased manner, so for a period of time, some new content will continue to be protected with the old tenant key. Previously protected content stays protected to your old tenant key. To support this scenario, Azure Information Protection retains your old tenant key so that it can issue licenses for old content.

## Backup and recover your tenant key
Microsoft is responsible for backing up your tenant key and no action is required from you.

## Export your tenant key
You can export your Azure Information Protection configuration and tenant key by following the instructions in these three steps:

### Step 1: Initiate export

-   To do this, [contact Microsoft Support](../get-started/information-support.md#to-contact-microsoft-support) to open an **Azure Information Protection support case with a request for an Azure Information Protection key export**. You must prove you are an administrator for your Azure Information Protection tenant, and understand that this process will take several days to confirm. Standard support charges apply; exporting your tenant key is a not a free-of-charge support service.

### Step 2: Wait for verification

-   Microsoft verifies that your request to release your Azure Information Protection tenant key is legitimate. This process can take up to 3 weeks.

### Step 3: Receive key instructions from CSS

-   Microsoft Customer Support Services (CSS) will send you your Azure Information Protection configuration and tenant key as encrypted in a password-protected file that has a .tpd file name extension. To do this, CSS first sends you (as the person who initiated the export) a tool by email. You must run the tool from a command prompt as follows:

    ```
    AadrmTpd.exe -createkey
    ```
    This generates an RSA key pair and saves the public and private halves as files in the current folder. For example: **PublicKey-FA29D0FE-5049-4C8E-931B-96C6152B0441.txt** and **PrivateKey-FA29D0FE-5049-4C8E-931B-96C6152B0441.txt**.

    Respond to the email from CSS, attaching the file that has a name that starts with **PublicKey**. CSS will next send you a TPD file as an .xml file that is encrypted with your RSA key. Copy this file to the same folder as you ran the AadrmTpd tool originally, and run the tool again, using your file that starts with **PrivateKey** and the file from CSS. For example:

    ```
    AadrmTpd.exe -key PrivateKey-FA29D0FE-5049-4C8E-931B-96C6152B0441.txt -target TPD-77172C7B-8E21-48B7-9854-7A4CEAC474D0.xml
    ```
    The output of this command should be two files: One contains the plaintext password for the password-protected TPD, and the other is the password-protected TPD itself. For cross-referencing purposes, both should have the same GUID as the public and private key files from when you ran the AadrmTpd.exe -createkey command:

    -   Password-FA29D0FE-5049-4C8E-931B-96C6152B0441.txt

    -   ExportedTPD-FA29D0FE-5049-4C8E-931B-96C6152B0441.xml

    Backup these files and store them safely to ensure that you can continue to decrypt content that is protected with this tenant key. In addition, if you are migrating to AD RMS, you can import this TPD file (the file that starts with **ExportedTDP**) to your AD RMS server.

### Step 4: Ongoing: Protect your tenant key

-   After you receive your tenant key, keep it well-guarded, because if somebody gets access to it, they can decrypt all documents that are protected by using that key.

    If the reason for exporting your tenant key is because you no longer want to use Azure Information Protection, as a best practice, now deactivate the Azure Rights Management service from your Azure Information Protection tenant. Do not delay doing this after you receive your tenant key because this precaution will help to minimize the consequences if your tenant key is accessed by somebody who should not have it. For instructions, see [Decommissioning and deactivating Azure Rights Management](decommission-deactivate.md).

## Respond to a breach
No security system, no matter how strong, is complete without a breach response process. Your tenant key might be compromised or stolen. Even when it’s well protected well, vulnerabilities might be found in current generation HSM technology or current key lengths and algorithms.

Microsoft has a dedicated team to respond to security incidents in its products and services. As soon as there is a credible report of an incident, this team engages to investigate the scope, root cause, and mitigations. If this incident affects your assets, then Microsoft will notify your Azure Information Protection tenant administrators by email by using the address that you supplied when you subscribed.

If you have a breach, the best action that you or Microsoft can take depends on the scope of the breach; Microsoft will work with you through this process. The following table shows some typical situations and the likely response, although the exact response will depend on all the information that is revealed during the investigation.

|Incident description|Likely response|
|------------------------|-------------------|
|Your tenant key is leaked.|Re-key your tenant key. See the [Re-key your tenant key](operations-microsoft-managed-tenant-key.md#re-key-your-tenant-key) section in this article.|
|An unauthorized individual or malware got rights to use your tenant key but the key itself did not leak.|Re-keying your tenant key does not help here and requires root-cause analysis. If a process or software bug was responsible for the unauthorized individual to get access, that situation must be resolved.|
|Vulnerability discovered in the RSA algorithm, or key length, or brute-force attacks become computationally feasible.|Microsoft must update Azure Information Protection to support new algorithms and longer key lengths that are resilient, and instruct all customers to renew their tenant keys.|

[!INCLUDE[Commenting house rules](../includes/houserules.md)]

