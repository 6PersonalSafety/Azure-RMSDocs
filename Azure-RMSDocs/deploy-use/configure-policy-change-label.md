---
# required metadata

title: How to change or customize an existing label | Azure Information Protection
description: You can change or refine any of the labels that users see on the Information Protection bar, by configuring them in the Azure Information Protection policy.
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 12/07/2016
ms.topic: article
ms.prod:
ms.service: information-protection
ms.technology: techgroup-identity
ms.assetid: e3b6d95f-334b-4d17-80a9-7d5487ab5d32

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
#ms.reviewer: eymanor
#ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# How to change or customize an existing label for Azure Information Protection

>*Applies to: Azure Information Protection*

You can change or refine any of the labels that users see on the Information Protection bar, by configuring them in the Azure Information Protection policy.

For example, you can change a label or sub-label name, tooltip, color, order, whether it applies visual markings such as a footer or watermark, whether it applies Azure Rights Management protection, and recommended or automatic classification.

To change a label, use the following instructions.


1. If you haven't already done so, in a new browser window, sign in to the [Azure portal](https://portal.azure.com) as a global admin, and then navigate to the **Azure Information Protection** blade. 
    
    For example, on the hub menu, click **More services** and start typing **Information** in the Filter box. Select **Azure Information Protection**.

2. To change a label from the global policy so that it applies to all users, select the label to change from the **Policy:Global** blade, and then make your changes on the **Label** blade, and any subsequent blades as required. To change a label from a [scoped policy](configure-policy-scope.md) so that it applies to selected users, first select that policy on the initial **Azure Information Protection** blade.

    The exception is if you want to reorder a label, which you do on the policy blade from the global policy or your selected scoped policy: Either right-click the label or select the context menu for the label, and then select the **Move up** or **Move down** options.

3. Whenever you make changes on a blade, click **Save** on that blade if you want to keep your changes.

4. To make your changes available to users, on the **Azure Information Protection** blade, click **Publish**.

> [!TIP]
>If you want to return one of the default labels to the default values, use the information in [The default Information Protection policy](configure-policy-default.md).

## Next steps

For more information about configuring the options that you can make for a label, and other settings for your Azure Information Protection policy, use the links in the [Configuring your organization's policy](configure-policy.md#configuring-your-organizations-policy) section.



