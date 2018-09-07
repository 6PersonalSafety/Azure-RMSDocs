---
title: Microsoft Information Protection (MIP) SDK setup and configuration
description: Provides the setup and configuration prerequisites, in order to use applications built with the Microsoft Information Protection SDK.
services: information-protection
author: BryanLa
ms.service: information-protection
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: bryanla
---

# Microsoft Information Protection (MIP) SDK setup and configuration 

The MIP quickstart and tutorial articles are centered around building applications that use the MIP SDK libraries and APIs. This article shows you how to set up and configure your Office 365 subscription and client workstation, in preparation for using the SDK.

Be sure to review the following topics before getting started:

* [What is Office 365 Security and Compliance Center?](https://docs.microsoft.com/office365/securitycompliance/security-and-compliance)
* [What is Azure Information Protection?](https://docs.microsoft.com/azure/information-protection/understand-explore/what-is-information-protection)
* [How does the protection work in Azure Information Protection?](https://docs.microsoft.com/azure/information-protection/understand-explore/what-is-information-protection#how-data-is-protected)

## Sign up for an Office 365 subscription

Many of the SDK samples require access to an Office 365 subscription. If you haven't already, be sure to sign up for one of the following subscription types:

| Name | Sign-up |
|------|---------|
| Office 365 Enterprise E3 Trial (30-day free trial) | https://go.microsoft.com/fwlink/p/?LinkID=403802 |
| Office 365 Enterprise E3 or E5 | https://products.office.com/business/office-365-enterprise-e3-business-software |
| Enterprise Mobility and Security E3 or E5 | https://www.microsoft.com/cloud-platform/enterprise-mobility-security |
| Azure Information Protection Premium P1 or P2 | https://azure.microsoft.com/pricing/details/information-protection/ |

QUESTIONS:
- Should we add Microsoft 365 to the above?
- Is AIP Premium P1/P2 backed by an Azure AD tenant? Or does the user need to sign up for an Azure subscription first, and use that AAD account to get the AIP P1/P2 subscription?
- (TBD) Because the MIP SDK requires an Office 365 tenant with certain private preview features enabled, you must have an account from a properly configured preview tenant. If you need a test account, you can submit a request at https://aka.ms/mipsdkpreviewaccount. 

## Configure your client workstation

The following steps cover items you need to complete to ensure your client computer is set up and configured correctly. 

### Windows 10

1. Using Windows Update, update your machine to Windows 10 Fall Creators Update (version 1709) or later. To  verify your current version:
    - Click the Windows icon in the lower left
    - Type "About your PC" and press the "Enter" key
    - Scroll down to **Windows specifications** and look under **Version**
2. Install [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), with the following workloads and optional components:
    - **Universal Windows Platform development** Windows workload, plus the following optional components:
        - **C++ Universal Windows Platform tools**
        - **Windows 10 SDK 10.0.16299.0 SDK** or later, if not included by default
    - **Desktop development with C++** Windows workload, plus the following optional components:
        - **Windows 10 SDK 10.0.16299.0 SDK** or later, if not included by default 

        [![Visual Studio setup](media/setup-mip-client/visual-studio-install.png)](media/setup-mip-client/visual-studio-install.png#lightbox)

3. Download the SDK binaries and samples: 
    - IF we're using a GitHub Repo (TBD)
        - First create a [GitHub profile](https://github.com/join), if you don't already have one.
        - Install the latest version of [Software Freedom Conservancy's Git client tools](https://git-scm.com/download/)
        - `git clone <repo-path>`
    - ELSE: https://aka.ms/mipsdkbins (Sep 2108)
    - Info on supported platforms, system requirements (TBD)
5. Ensure "Developer Mode" is enabled on your workstation:
    - Click the Windows icon in the lower left
    - Type "Use developer features" and press the "Enter" key
    - On the **Settings** dialog, **For developers** tab, under "Use developer features", select the **Developer mode** option.
    - Close the **Settings** dialog.

## Register a client application with Azure Active Directory

As part of the Office 365 subscription provisioning process, an associated Azure AD tenant is created. The Azure AD tenant provides identity and access management for Office 365 *user accounts* and *application accounts*. Upon sign-in, accounts are represented by a *security principal*, which encapsulates the account's identity configuration for access control, etc. Security principals that represent an application account are referred to as a [*service principal*](/azure/active-directory/develop/developer-glossary#service-principal-object). 

Application accounts are created using the [Azure AD **App registrations** page](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps), in the Azure portal. To create an application account for use by the applications you'll build with the MIP SDK:

[!IMPORTANT] 
> To access the Azure AD **App registrations** page, you'll need to sign in to the portal with a user account that is a member of the [Subscription administrator](/azure/billing/billing-add-change-azure-subscription-administrator) role.
> TBD - if they need to add access to an API/perms that require admin consent, they'll also need to be global admin in the tenant, and "Grant Permissions" when adding the API/perm. It isn't clear yet if we have MIP APIs to add to perms?
> We recommend testing with a restricted account, when passing username and password commandline parameters. Be sure the account only has rights to access the necessary SCC endpoints. Cleartext passwords passed via commandline may be collected by logging systems.

1. Follow the steps in [Integrating applications with Azure Active Directory - Add an application](/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad#adding-an-application). For testing purposes, use the following values for the given properties as you go through the guide: 
    - **Application type** - TBD
    - **Sign-On URL** - TBD. You can use "https://localhost" for test purposes.
    - **Redirect URI** - TBD. You can use "https://localhost" for test purposes.

2. TBD. Now go to the [Updating an application](/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad#updating-an-application) section for details on adding permissions to the required MIP API(s).

## TBD: Define Label Taxonomy and Protection Settings?

* Provide links to SCC configuration.
* Provide details on rights required to config labels.
* Provide details on labeling taxonomy best practices.

## Next Steps

Now you're ready to begin the first quickstart!

> [!div class="nextstepaction"]
> [Authentication and initialization](quick-auth-initialization-cpp.md)
