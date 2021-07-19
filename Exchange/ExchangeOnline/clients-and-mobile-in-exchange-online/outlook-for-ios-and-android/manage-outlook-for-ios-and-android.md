---
localization_priority: Normal
description: 'Summary: This article describes best practices for managing mobile devices with Outlook for iOS and Android in Exchange Online.'
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 30e8c819-4d41-4458-a746-a9ba9a84a7c0
title: Managing Outlook for iOS and Android in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
ms.reviewer: smithre4
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Managing Outlook for iOS and Android in Exchange Online

 **Summary**: This article describes best practices for managing mobile devices with Outlook for iOS and Android in Exchange Online.

Outlook for iOS and Android provides users the fast, intuitive email and calendar experience users expect from a modern mobile app, while being the only app to provide support for the best features of Microsoft 365 and Office 365. In addition, Microsoft provides a number of utilities for managing and protecting company data on mobile devices in your Exchange Online organization.

## Options for managing devices and applications

Customers looking to manage Outlook for iOS and Android have the following options:

1. **Recommended**: The Enterprise Mobility + Security suite, which includes Microsoft Intune and Azure Active Directory conditional access.

2. Basic Mobility and Security for Microsoft 365.

3. Third-party Unified Endpoint Management solutions.

4. Mobile Device Access and Mobile Device Mailbox Policies.

> [!NOTE]
> For implementation details on each of these three options, see [Securing Outlook for iOS and Android in Exchange Online](secure-outlook-for-ios-and-android.md).

Microsoft recommends that customers use the features of the Enterprise Mobility + Security suite to protect corporate data on mobile devices, due to the advanced capabilities provided by these services.

> [!IMPORTANT]
> When the user authenticates in Outlook for iOS and Android, Exchange Online mobile device access rules (allow, block, or quarantine) are skipped if there are any Azure Active Directory conditional access policies applied to the user that include:
> - Cloud app condition: Exchange Online or Office 365
> - Device platform condition: iOS and/or Android
> - Client apps condition: Mobile apps and desktop client
> - One of the following Grant access controls: [Require device to be marked as compliant](/azure/active-directory/active-directory-conditional-access-policy-connected-applications), [Require approved client app](/azure/active-directory/active-directory-conditional-access-technical-reference) or [Require app protection policy](/azure/active-directory/conditional-access/app-protection-based-conditional-access)

> [!NOTE]
> When using mobile device cmdlets such as `Get-MobileDevice` to check the status of a device, the timestamp for Outlook for iOS and Android synchronization, indicated by the `LastSyncTime` property, may be up to 15 minutes behind the actual time of synchronization. While device synchronization does occur in real time, the returned time stamp may lag behind.

### Using Enterprise Mobility + Security

The richest and broadest protection capabilities for Microsoft 365 and Office 365 data are available when you subscribe to the Enterprise Mobility + Security suite, which includes Microsoft Intune, Azure Information Protection, and Azure Active Directory Premium features, such as conditional access.

> [!NOTE]
> While the Enterprise Mobility + Security suite subscription includes licenses for both Microsoft Intune and Azure Active Directory, customers can purchase Microsoft Intune licenses and Azure Active Directory Premium licenses separately. All users must be licensed to leverage the conditional access and Intune app protection policies discussed in this article.

Intune provides mobile application management (MAM) capabilities, as well as other conditional access and device management capabilities. With Intune app protection policies, you can restrict actions such as cut, copy, paste, and "save as" of corporate data between Intune-managed apps and apps that are not managed by Intune. More information is available in [How to create and assign app protection policies](/intune/app-protection-policies). Additionally, the Intune-managed Outlook apps include a new multi-identity management feature that enables users to access both their personal and work email accounts in the same Outlook app while only applying the Intune app protection policies to the user's work account. This provides a much more seamless user experience.

Conditional access is a capability of Azure Active Directory that enables you to enforce controls on the access to apps in your environment based on specific conditions from a central location. By using conditional access policies, you can apply the right access controls under the required conditions. Azure Active Directory conditional access provides you with added security when such security is needed, and it stays out of your users' way when it isn't.

Key features of the Enterprise Mobility + Security suite with Outlook for iOS and Android:

- **Conditional access**. Azure Active Directory ensures that Exchange Online email can be accessed only when the conditional access requirements are met. For more information on device enrollment, see [Conditional access in Azure Active Directory](/azure/active-directory/active-directory-conditional-access-azure-portal).

- **Intune app protection**. Outlook for iOS and Android allows you to protect your corporate data with Intune app protection policies. This is a great option for "bring your own device" (BYOD) scenarios where you want to keep corporate data safe without managing a users' devices. For more information on Intune app protection policies, see [Protect app data using mobile app management policies with Microsoft Intune](/intune/deploy-use/protect-app-data-using-mobile-app-management-policies-with-microsoft-intune).

- **Device enrollment**. Intune lets you manage your workforce's devices and apps, and how they access your company data. In this model, Outlook for iOS and Android ensures that Exchange Online email can be accessed only on phones and tablets that are managed by your company and are compliant with your organization's policy. When users log on to the Outlook app on an unmanaged mobile device, Outlook prompts users to enroll the device in Intune by leveraging the Azure conditional access policy, and then validates that the device meets organizational standards of device compliance.

- **Device management and reporting**. The enrollment process allows organizations to set and manage security policies that, for example, enforce device-level PIN lock, require data encryption, and block compromised devices in order to prevent untrusted devices from accessing corporate email and data. Each enrolled device appears in the Microsoft 365 admin center, and reporting is available to provide details on the devices that access your corporate data.

- **Selective wipe**. Microsoft Intune can remove email data from Outlook for iOS and Android, while leaving any personal email accounts intact (whether the device is enrolled or not). This is an increasingly important requirement as more businesses adopt a "bring your own device" approach to phones and tablets.

### Using Basic Mobility and Security for Microsoft 365

Basic Mobility and Security for Microsoft 365 provides device management capabilities at no additional cost. Microsoft Intune powers these basic capabilities, providing a core set of controls in the Microsoft 365 admin center for organizations that need the basics.

Because this is a device management solution, there is no native capability to control which apps can be used, even after a device is enrolled. If you want to limit access to Outlook for iOS and Android, you will need to obtain Azure Active Directory Premium licenses and leverage conditional access policies.

Outlook for iOS and Android fully supports the capabilities provided by Basic Mobility and Security for Microsoft 365.

For detailed information, see the following resources:

- [Overview of Basic Mobility and Security for Microsoft 365](https://support.microsoft.com/office/faa7d8e5-645d-4d59-839c-c8d4c1869e4a).

- [Manage settings and features on your devices with Microsoft Intune policies](/intune/deploy-use/manage-settings-and-features-on-your-devices-with-microsoft-intune-policies)

- Instructions for your end-users to enroll a device in Basic Mobility and Security: [Enroll your mobile device using Basic Mobility and Security](https://support.microsoft.com/office/c8ac722d-dcaf-4135-8345-3e6327f5d3c5)

### Using Third-Party Unified Endpoint Management Solutions

Third-party unified endpoint management providers can deploy the Outlook for iOS and Android the same way they would deploy any iOS or Android app, using their existing tools. They can also apply device management controls like device PIN, device encryption, device wipe, and more, all of which are important for a secure email experience, but are also completely independent of Outlook for iOS and Android.

Third-party providers can also deploy certain app configuration settings, like account setup, organization allowed accounts mode, and general app configuration settings, to Outlook for iOS and Android; for more information, please see [Deploying Outlook for iOS and Android app configuration settings](./outlook-for-ios-and-android-configuration-with-microsoft-intune.md).

In order to manage and protect corporate data within the app (such as restricting actions with corporate data like cut, copy, paste, and "save as"), customers will need to use Microsoft's Enterprise Mobility + Security suite.

### Using Mobile Device Access and Mobile Device Mailbox Policies

Microsoft recommends that customers use either the Enterprise Mobility + Security suite or the built-in Basic Mobility and Security for Microsoft 365 to manage company data on mobile devices, due to the advanced capabilities provided by those services. Outlook for iOS and Android does support mobile device access and mobile device mailbox policies (formerly known as Exchange Active Sync policies), which are available through the Exchange admin center.

Outlook for iOS and Android supports the following Exchange mobile device mailbox policy settings:

- Device encryption enabled

- Min password length (only on Android)

- Password enabled

- Allow Bluetooth (used to manage the Outlook for Android wearable app when Intune App Protection Policies are not in use)

  - When AllowBluetooth is enabled (default behavior) or configured for HandsfreeOnly, wearable synchronization between Outlook on the Android device and Outlook on the wearable is allowed for the work or school account.

  - When AllowBluetooth is disabled, Outlook for Android will disable synchronization between Outlook on the Android device and Outlook on the wearable for the specified work or school account (and delete any data previously synced for the account). Disabling the synchronization is controlled entirely within Outlook itself; Bluetooth is not disabled on the device or wearable nor is any other wearable app affected.

For information on how to create or modify an existing mobile device mailbox policy, see [Mobile device mailbox policies in Exchange Online](../../clients-and-mobile-in-exchange-online/exchange-activesync/mobile-device-mailbox-policies.md).

Exchange administrators can also initiate a remote device wipe against Outlook for iOS and Android using Exchange admin center. Upon receiving the remote wipe request, the app will remove the Outlook profile and all data associated with it.

> [!NOTE]
> Outlook for iOS and Android only supports the **Wipe Data** remote wipe command and does not support **Account Only Remote Wipe Device** as defined in the Exchange admin center. For more information on how to perform a remote wipe, see [Perform a remote wipe on a mobile phone](../../../ExchangeServer/clients/exchange-activesync/remote-wipe.md).

For more about Microsoft Intune see [Documentation for Microsoft Intune](/intune/).