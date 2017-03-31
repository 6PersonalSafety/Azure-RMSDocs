---
# required metadata

title: Migrate AD RMS-Azure Information Protection - phase 4
description: Phase 4 of migrating from AD RMS to Azure Information Protection, covering steps 8 through 9 from Migrating from AD RMS to Azure Information Protection
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 03/31/2017
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: 8b039ad5-95a6-4c73-9c22-78c7b0e12cb7


# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Migration phase 4 - supporting services configuration

>*Applies to: Active Directory Rights Management Services, Azure Information Protection, Office 365*


Use the following information for Phase 4 of migrating from AD RMS to Azure Information Protection. These procedures cover steps 8 through 9 from [Migrating from AD RMS to Azure Information Protection](migrate-from-ad-rms-to-azure-rms.md).



## Step 8. Configure IRM integration for Exchange Online

If you have previously imported your TDP from AD RMS to Exchange Online, you must remove this TDP to avoid conflicting templates and policies after you have migrated to Azure Information Protection. To do this, use the [Remove-RMSTrustedPublishingDomain](https://technet.microsoft.com/library/jj200720%28v=exchg.150%29.aspx) cmdlet from Exchange Online.

If you chose an Azure Information Protection tenant key topology of **Microsoft managed**:

1. Use the instructions in the [Exchange Online: IRM configuration](../deploy-use/configure-office365.md#exchange-online-irm-configuration) section in the [Office 365: Configuration for clients and online services](../deploy-use/configure-office365.md) article. This section includes typical commands to run that connects to the Exchange Online service, imports the tenant key from Azure Information Protection, and enables IRM functionality for Exchange Online. After you complete these steps, you will have full Azure Rights Management protection functionality with Exchange Online.

2. In addition to the standard configuration to enable IRM for Exchange Online, run the following PowerShell commands to ensure that users will be able to read emails that were sent by using AD RMS protection.

    Substitute your own organization domain name for *\<yourcompany.domain>*.

    	$irmConfig = Get-IRMConfiguration
    	$list = $irmConfig.LicensingLocation
    	$list += "https://adrms.<yourcompany.domain>/_wmcs/licensing"
    	Set-IRMConfiguration -LicensingLocation $list
    	Set-IRMConfiguration -internallicensingenabled $false
    	Set-IRMConfiguration -internallicensingenabled $true


If you chose an Azure Information Protection tenant key topology of **customer-managed (BYOK)**:

-   You will have reduced Rights Management protection functionality with Exchange Online, as described in the [BYOK pricing and restrictions](byok-price-restrictions.md) article.


## Step 9. Configure IRM integration for Exchange Server and SharePoint Server

If you have used the Information Rights Management (IRM) functionality of Exchange Server or SharePoint Server with AD RMS, you will need to deploy the Rights Management (RMS) connector, which acts as a communications interface (a relay) between your on-premises servers and the protection service for Azure Information Protection.

This steps covers installing and configuring the connector, disabling IRM for Exchange and SharePoint, and configuring these servers to use the connector. Finally, if you have imported AD RMS data configuration files (.xml) into Azure Information Protection that were used to protect email messages, you must manually edit the registry on the Exchange Server computers to redirect all trusted publishing domain URLs to the RMS connector.

> [!NOTE]
> Before you start, check the versions of the on-premises servers that the Azure Rights Management service supports, from [On-premises servers that support Azure RMS](../get-started/requirements-servers.md).

### Install and configure the RMS connector

Use the instructions in the [Deploying the Azure Rights Management connector](../deploy-use/deploy-rms-connector.md) article, and do steps 1 though 4. Do not start step 5 yet from the connector instructions. 

### Disable IRM on Exchange Servers and remove AD RMS configuration

1.  On each Exchange server, locate the following folder and delete all the entries in that folder: **\ProgramData\Microsoft\DRM\Server\S-1-5-18**

2. From one of the Exchange servers, run the following PowerShell commands to ensure that users will be able to read emails that are protected by using Azure Rights Management.

    Before you run these commands, substitute your own Azure Rights Management service URL for *\<Your Tenant URL>*.

		$irmConfig = Get-IRMConfiguration
		$list = $irmConfig.LicensingLocation 
		$list + "<Your Tenant URL>/_wmcs/licensing"
		Set-IRMConfiguration -LicensingLocation $list

3.  Now disable IRM features for messages that are sent to internal recipients:

    ```
    Set-IRMConfiguration -InternalLicensingEnabled $false
    ```

4. Then use the same cmdlet to disable IRM in Microsoft Office Outlook Web App and in Microsoft Exchange ActiveSync:

    ```
    Set-IRMConfiguration -ClientAccessServerEnabled $false
    ```

5.  Finally, use the same cmdlet to clear any cached certificates:

    ```
    Set-IRMConfiguration -RefreshServerCertificates
    ```

6.  On each Exchange Server, now reset IIS, for example, by running a command prompt as an administrator and typing **iisreset**.

### Disable IRM on SharePoint Servers and remove AD RMS configuration

1.  Make sure that there are no documents checked out from RMS-protected libraries. If there are, they will be become inaccessible at the end of this procedure.

2.  On the SharePoint Central Administration Web site, in the **Quick Launch** section, click **Security**.

3.  On the **Security** page, in the **Information Policy** section, click **Configure information rights management**.

4.  On the **Information Rights Management** page, in the **Information Rights Management** section, select **Do not use IRM on this server**, then click **OK**.

5.  On each of the SharePoint Server computers, delete the contents of the folder \ProgramData\Microsoft\MSIPC\Server\*&lt;SID of the account running SharePoint Server&gt;*.

#### Configure Exchange and SharePoint to use the connector

1. Return to the instructions for deploying the RMS connector: [Step 5: Configuring servers to use the RMS connector](../deploy-use/configure-servers-rms-connector.md)

    If you have SharePoint Server only, go straight to [Next steps](#next-steps) to continue the migration. 

2. On each Exchange Server, manually add the registry keys in the next section for each configuration data file (.xml) that you imported, to redirect the trusted publishing domain URLs to the RMS connector. These registry entries are specific to migration and are not added by the server configuration tool for Microsoft RMS connector.

    When you make these registry edits, use the following instructions:

    -   Replace *Connector FQDN* with the name that you defined in DNS for the connector. For example, **rmsconnector.contoso.com**.

    -   Use the HTTP or HTTPS prefix for the connector URL, depending on whether you have configured the connector to use HTTP or HTTPS to communicate with your on-premises servers.

##### Registry edits for Exchange

For all Exchange servers, remove the registry values that you added for LicenseServerRedirection during preparation phase. These values were added to the following paths:

HKLM\SOFTWARE\Microsoft\ExchangeServer\v15\IRM\LicenseServerRedirection

HKLM\SOFTWARE\Microsoft\ExchangeServer\v14\IRM\LicenseServerRedirection


For Exchange 2013 and Exchange 2016 - registry edit 1:


**Registry path:**

HKLM\SOFTWARE\Microsoft\ExchangeServer\v15\IRM\LicenseServerRedirection

**Type:**

Reg_SZ

**Value:**

https://\<AD RMS Intranet Licensing URL\>/_wmcs/licensing

**Data:**

One of the following, depending on whether you are using HTTP or HTTPS from your Exchange server to the RMS connector:

- http://\<connector FQDN\>/_wmcs/licensing

- https://\<connector FQDN\>/_wmcs/licensing


---

For Exchange 2013 - registry edit 2:

**Registry path:**

HKLM\SOFTWARE\Microsoft\ExchangeServer\v15\IRM\LicenseServerRedirection 


**Type:**

Reg_SZ

**Value:**

https://\<AD RMS Extranet Licensing URL\>/_wmcs/licensing


**Data:**

One of the following, depending on whether you are using HTTP or HTTPS from your Exchange server to the RMS connector:

- http://\<connector FQDN\>/_wmcs/licensing

- https://\<connector FQDN\>/_wmcs/licensing

---

For Exchange 2010 - registry edit 1:



**Registry path:**

HKLM\SOFTWARE\Microsoft\ExchangeServer\v14\IRM\LicenseServerRedirection

**Type:**

Reg_SZ

**Value:**

https://\<AD RMS Intranet Licensing URL\>/_wmcs/licensing

**Data:**

One of the following, depending on whether you are using HTTP or HTTPS from your Exchange server to the RMS connector:

- http://\<connector FQDN\>/_wmcs/licensing

- https://\<connector Name\>/_wmcs/licensing


---

For Exchange 2010 - registry edit 2:


**Registry path:**

HKLM\SOFTWARE\Microsoft\ExchangeServer\v14\IRM\LicenseServerRedirection
 

**Type:**

Reg_SZ

**Value:**

https://\<AD RMS Extranet Licensing URL\>/_wmcs/licensing


**Data:**

One of the following, depending on whether you are using HTTP or HTTPS from your Exchange server to the RMS connector:

- http://\<connectorFQDN\>/_wmcs/licensing

- https://\<connectorFQDN\>/_wmcs/licensing

---


## Next steps
To continue the migration, go to [phase 4 -post migration tasks](migrate-from-ad-rms-phase4.md).

[!INCLUDE[Commenting house rules](../includes/houserules.md)]