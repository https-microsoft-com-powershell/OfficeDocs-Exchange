---
localization_priority: Normal
description: 'Summary: This article contains architectural and security information for administrators about Outlook for iOS and Android in an Exchange Server 2016 or Exchange Server 2019 on-premises environment when the app uses Basic authentication.'
ms.topic: conceptual
author: msdmaguire
ms.author: dmaguire
title: Using Basic authentication with Outlook for iOS and Android
ms.collection: exchange-server
ms.reviewer: smithre4
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Using Basic authentication with Outlook for iOS and Android

The Outlook app for iOS and Android is designed to bring together email, calendar, contacts, and other files, enabling users in your organization to do more from their mobile devices. This article provides an overview of the architecture and the storage design of the app, so that Exchange administrators can deploy and maintain Outlook for iOS and Android in their Exchange organizations.

>[!NOTE]
> This article is about using the app in an Exchange 2010, Exchange 2013, Exchange 2016 or Exchange 2019 environment where hybrid modern authentication is **not** enabled. For more information about using hybrid Modern Authentication for on-premises mailboxes with the app, see [Using hybrid Modern Authentication with Outlook for iOS and Android](use-hybrid-modern-auth.md). For information about using the app with Exchange Online, see [Outlook for iOS and Android in Exchange Online](../../../ExchangeOnline/clients-and-mobile-in-exchange-online/outlook-for-ios-and-android/outlook-for-ios-and-android.md).

## Outlook for iOS and Android architecture

Outlook for iOS and Android is a cloud-backed application. This means your experience consists of a locally installed app powered by a secure and scalable service running in the Microsoft Cloud.

For Exchange Server mailboxes, Outlook for iOS and Android's architecture is built directly into the Microsoft Cloud, providing customers the additional benefits of security, privacy, built-in compliance, and transparent operations that Microsoft commits to in the [Microsoft Trust Center](https://www.microsoft.com/trust-center). 

The following environments will take advantage of this Microsoft 365 or Office 365-based architecture:

- In Exchange Server 2010 environments

- When a hybrid relationship between Exchange 2013, 2016, or 2019 on-premises and Microsoft 365 or Office 365 hasn't been configured

- When hybrid Modern Authentication hasn't been enabled between Exchange 2013, 2016, or 2019 on-premises and Microsoft 365 or Office 365

![Basic authentication in Outlook for iOS and Android](../../media/outlook_mobile_basic_auth.png)

Within the Microsoft 365 or Office 365-based architecture, Outlook for iOS and Android utilizes the native Microsoft sync technology for data synchronization that is protected by TLS-secured connections end-to-end, between Microsoft 365 or Office 365 and the app.

The Exchange ActiveSync (EAS) connection between Exchange Online and the on-premises environment enables synchronization of the users' on-premises data and includes four weeks of email, all calendar data, all contact data, and out-of-office status. The region in which this data is synchronized into depends on the IP address in use by the mobile device at the time synchronization is set up. If you have a hybrid setup with an Exchange Online tenant, the on-premises data is not synchronized into your tenant; instead, the data is synchronized into Outlook.com in the same way as if you had an Exchange Server without hybrid. If you want to control and manage your on-premises data from within your tenant, you need to enable [hybrid Modern Authentication with Outlook for iOS and Android](./use-hybrid-modern-auth.md).

Data synchronization between the Exchange on-premises environment and Exchange Online happens independent of user behavior. This ensures that new messages are delivered to the devices quickly. For more information on how the user authentication model enables data synchronization independently of user behavior, see [Passwords and security in Outlook for iOS and Android for Exchange Server](./passwords-and-security.md).

Processing information in the Microsoft Cloud enables advanced features and capabilities, such as the categorization of email for the Focused Inbox, customized experience for travel and calendar, and improved search speed. Relying on the cloud for intensive processing and minimizing the resources required from users' devices enhances the app's performance and stability. Lastly, it allows Outlook to build features that work across all email accounts, regardless of the technological capabilities of the underlying servers (such as different versions of Exchange Server, Microsoft 365, or Office 365).

>[!IMPORTANT]
> On-premises mailboxes using basic authentication with Outlook for iOS and Android do not support Enterprise Mobility + Security features such as Azure Active Directory conditional access and Intune app protection policies. For support with these technologies, see [Using hybrid Modern Authentication with Outlook for iOS and Android](use-hybrid-modern-auth.md).

## Data security, access, and auditing controls

With on-premises data being synchronized with Exchange Online, customers have questions about how the data is protected in Exchange Online. The white paper [Encryption in the Microsoft Cloud](/microsoft-365/compliance/office-365-encryption-in-the-microsoft-cloud-overview) discusses how BitLocker is used for volume-level encryption.

By default, Microsoft engineers have zero standing administrative privileges and zero standing access to customer content in Microsoft 365 and Office 365. The white paper [Administrative Access Controls in Microsoft 365](/compliance/assurance/assurance-administrative-access-controls-overview) discusses personnel screening, background checks, Lockbox and Customer Lockbox, and more.

[ISO Audited Controls on Service Assurance](https://sip.protection.office.com/) documentation provides the status of audited controls from global information security standards and regulations that Microsoft 365 and Office 365 have implemented.

## Connectivity Requirements
Microsoft recommends that the on-premises endpoints for AutoDiscover and ActiveSync protocols be opened and accessible from the Internet without any restrictions. In certain situations that may not be possible. If you must place restrictions on your on-premises firewall or gateway edge devices, Microsoft recommends filtering based on FQDN endpoints. If FQDN endpoints cannot be used, then filter on IP addresses. Make sure the following IP subnets and FQDNs are included on your allow list:

- All Exchange Online FQDNs and IP subnet ranges as defined in [Microsoft 365 and Office 365 URLs and IP address ranges](/office365/enterprise/urls-and-ip-address-ranges).

- The AutoDetect FQDNs and IP subnet ranges defined in [Additional Microsoft 365 or Office 365 IP Addresses and URLs not included in the web services](/office365/enterprise/additional-office365-ip-addresses-and-urls). This is required because the AutoDetect service establishes connections to the on-premises infrastructure.

- All Outlook iOS and Android and Office mobile app FQDNs as defined in [Microsoft 365 or Office 365 URLs and IP address ranges](/office365/enterprise/urls-and-ip-address-ranges).

## Client features that aren't supported
The following features aren't support for on-premises mailboxes using basic authentication with Outlook for iOS and Android:

- Draft folder and Draft messages synchronization

- Shared calendar access and Delegate calendar access

- Shared and delegate mailbox data access

- Cortana Time to Leave / Travel Time

- Calendar attachments

- Rich meeting locations

- Task management with Microsoft To-Do

- Favorite People with Notifications

- Add-ins

- Interesting Calendars

- Avatar support

- Play My Emails

- S/MIME

- Sensitivity labeling

- Discover Feed

- Privacy settings