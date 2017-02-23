---
# required metadata

title: Operations for your Azure Information Protection tenant key
description: Identify the different levels of control and responsibility that you have for your Azure Information Protection tenant key.
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 02/23/2017
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: 1284d0ee-0a72-45ba-a64c-3dcb25846c3d

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Operations for your Azure Information Protection tenant key

>*Applies to: Azure Information Protection, Office 365*

Depending on your tenant key topology (Microsoft-managed or customer-managed), you have different levels of control and responsibility for your Azure Information Protection tenant key after it is implemented.

When you manage your own tenant key in Azure Key Vault, this is often referred to as bring your own key (BYOK). For more information about this scenario and how to choose between the two tenant key topologies, see [Planning and implementing your Azure Rights Management tenant key](../plan-design/plan-implement-tenant-key.md).

The following table identifies which operations you can do, depending on the topology that you’ve chosen for your Azure Information Protection tenant key.

|Lifecycle operation|Microsoft-managed (default)|Customer-managed (BYOK)|
|-----------------------|-------------------------------|---------------------------|
|Revoke your tenant key|No (automatic)|Yes|
|Re-key your tenant key|Yes|Yes|
|Backup and recover your tenant key|No|Yes|
|Export your tenant key|Yes|No|
|Respond to a breach|Yes|Yes|

After you have identified which topology you have implemented, select one of the following for more information about these operations for your Azure Information Protection tenant key:


- [Microsoft-managed tenant key](operations-microsoft-managed-tenant-key.md)
- [Customer-managed tenant key](operations-customer-managed-tenant-key.md)

[!INCLUDE[Commenting house rules](../includes/houserules.md)]
