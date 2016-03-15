iOS and OS X setup
=======================================================

iOS and OS X applications can use the Microsoft Rights Management SDK 4.2 to enable integrated information protection in their application by using the Azure Active Directory Rights Management (AAD RM).

This topic will guide you through setting up your environment for creating your own new apps.

**Note**  This SDK does not support iPod Touch.

 

-   [Prerequisites](#prerequisites)
-   [Optional](#optional)
-   [Configuring your development environment](#configuring_your_development_environment)
-   [See Also](#see_also)

<span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Prerequisites
-------------------------------------------------------------------------------------------------------------

We recommend the following software on your development system:

-   OS X is required for all iOS development.
-   Xcode version 6.0 and later

    Xcode is available through the [Mac App Store](https://developer.apple.com/technologies/mac/).

-   The MS RMS SDK 4.2 package for iOS and OS X. For more information see, [Get started](get_started.md).

    This SDK can be used to develop for iOS 7.0 and OS X 10.8 and later.

-   Authentication library: We recommend that you use the [Azure AD Authentication Library (ADAL)](https://msdn.microsoft.com/en-us/library/jj573266.aspx). However, other authentication libraries that support OAuth 2.0 can be used as well.

    For more information see, [ADAL for iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios) or [ADAL for OS X](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios/tree/OSXUniversal)

Read the [What's new](release_notes.md) topic for information about API updates, release notes, and frequently asked questions (FAQ).

<span id="Optional"></span><span id="optional"></span><span id="OPTIONAL"></span>Optional
-----------------------------------------------------------------------------------------

Our UI library provides re-usable UI for consumption and protection operations for developers who don’t want to create their own custom UI - [UI Library and Sample app for iOS](https://github.com/AzureAD/rms-sdk-ui-for-ios).

<span id="Configuring_your_development_environment"></span><span id="configuring_your_development_environment"></span><span id="CONFIGURING_YOUR_DEVELOPMENT_ENVIRONMENT"></span>Configuring your development environment
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-   To create a new project, on the **File** menu, click **New**, and then click **Project**.
-   Select **Single View Application**.

    ![](IMAGES/IOS-PROJECT.png)

-   Enter a name and identifier for your new project.

    ![](IMAGES/IOS-PROJECT-OPTIONS.png)

-   Click **Next** and select the location for your project.
-   To add the **MSRightsManagement** framework for iOS Frameworks, drag the .framework folder from the SDK installation folder into the **Frameworks** section of your **Project Navigator**.

    ![](IMAGES/IOS-ADD-DEPENDENCIES-01A.png)

-   Select **Create groups for any added folders** option button and clear the **Copy items into destination group's folder (if needed)** check box.

    This action maintains the reference to the SDK installation folder instead of creating a copy.

    ![](IMAGES/IOS-CREATE-GROUPS.png)

-   To add the MS RMS SDK 4.2 for the resource bundle, drag the MSRightsManagementResources.bundle file from the MSRightsManagement.framework/Resources folder into the **Frameworks** section of your Project Navigator.

    ![](IMAGES/IOS-ADD-RESOURCE-BUNDLE-02A.png)

-   As you did when you copied the Framework, select **Create groups for any added folders** option button and clear the **Copy items into destination group's folder (if needed)** check box.
-   The SDK relies on other frameworks including: **CoreData**, **MessageUI**, **SystemConfiguration**, **Libresolv** and **Security**. To add these frameworks, navigate to the **Linked Frameworks and Libraries** section of the target's **Summary** pane, and expand that section to add them.

    The **UIKit** and **Foundation** frameworks are required and generally present by default.

    ![](IMAGES/IOS-ADD-LIBRARIES.png)

-   Add the **-ObjC** flag to **Other Linker Flags** in your target **Build Settings**.

    ![](IMAGES/IOS-LINKER-FLAGS.png)

-   Now your **Project Navigator** should look something like this tree.

    ![](IMAGES/IOS-VERIFY-SETUP-01A.png)

-   You are now ready to create your own new iOS/OS X apps.

<span id="See_Also"></span><span id="see_also"></span><span id="SEE_ALSO"></span>See Also
-----------------------------------------------------------------------------------------

[Get started](get_started.md)

[What's new](release_notes.md)

[Developer terms and concepts](core_concepts.md)

[iOS / OS X API Reference](xref:iOS)

 

 


