---
localization_priority: Normal
description: 'Summary: This article describes how passwords and security work in Outlook for iOS and Android with Exchange Server 2016 or Exchange Server 2019 when using Basic authentication with the Exchange ActiveSync protocol.'
ms.topic: overview
author: msdmaguire
ms.author: dmaguire
title: Passwords and security in Outlook for iOS and Android for Exchange Server
ms.collection: exchange-server
ms.reviewer: smithre4
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Passwords and security in Outlook for iOS and Android for Exchange Server

This article describes how passwords and security work in Outlook for iOS and Android with Exchange Server when using Basic authentication with the Exchange ActiveSync protocol.

> [!IMPORTANT]
> Outlook for iOS and Android supports hybrid Modern Authentication for on-premises mailboxes which eliminates the need to leverage basic authentication. The information contained in this article only pertains to basic authentication. For more information, please see [Using hybrid Modern Authentication with Outlook for iOS and Android](./use-hybrid-modern-auth.md).

## Creating an account and protecting passwords

The first time the Outlook app for iOS and Android is run in an Exchange on-premises environment, Outlook generates a random AES-128 key. This key is known as the *device key* and is stored only on the user's device.

When a user logs onto Exchange with Basic authentication, the username, password, and a unique AES-128 device key are sent from the user's device to the Outlook cloud service over a TLS connection, where the device key is held in runtime compute memory. After verifying the password with the Exchange server, the Microsoft 365 or Office 365-based architecture uses the device key to encrypt the password, and the encrypted password is then stored in the service. The device key, meanwhile, is wiped from memory and never stored in the Microsoft 365 or Office 365-based architecture (the key is only stored on the user's device).

Next, when a user attempts to connect to Exchange to retrieve mailbox data, the device key is again passed from the device to the Microsoft 365 or Office 365-based architecture over a TLS-secured connection, where it is used to decrypt the password in runtime compute memory. Once decrypted, the password is never stored in the service or written to a local storage disk, and the device key is once again wiped from memory.

After the Microsoft 365 or Office 365-based architecture has decrypted the password at runtime, the service can then connect to the Exchange server to synchronize mail, calendar, and other mailbox data. As long as the user continues to open and use Outlook periodically, the Microsoft 365 or Office 365-based architecture will keep a copy of the user's decrypted password in memory to keep the connection to the Exchange server active.

## Compliance considerations when sending passwords

Before you enable anything that allows for the transmission of passwords from your on-premises Exchange environment, be sure to consider the possible ramifications. For example, transmitting passwords to Microsoft 365 or Office 365-based architecture might result in your inability to meet the requirements of PCI-DSS or ISO/IEC 27001.

Furthermore, if you connect and synchronize email, calendars, and other email-related data, you might run into issues of compliance with GDPR, which restricts the private information that you can transmit without owner consent. This information might be contained in and found within emails, calendar items, and so on.

## Account inactivity and flushing passwords from memory

After three days of inactivity, the Microsoft 365 or Office 365-based architecture will flush a decrypted password from memory. With the decrypted password flushed, the architecture is unable to access a user's mailbox on-premises. The encrypted password remains stored in the Microsoft 365 or Office 365-based architecture, but decrypting it again isn't possible without the device key, which is only available from the user's device.

There are three ways a user account can become inactive:

- Outlook for iOS and Android is uninstalled by the user.

- Background app refresh is disabled in the Settings options, and then a force-quit is applied to Outlook.

- No internet connection is available on the device, preventing Outlook from synchronizing with Exchange.

> [!NOTE]
> Outlook will not become inactive simply because the user does not open the app for some time, such as over a weekend or while on vacation. As long as background app refresh is enabled (which is the default setting for Outlook for iOS and Android), functions like push notifications and background synchronization of email will count as activity.

 **Flushing encrypted password and synchronized mailbox data from Microsoft 365 or Office 365**

The Microsoft 365 or Office 365-based architecture flushes, or deletes, inactive accounts on a weekly schedule. After a user account becomes inactive, the architecture will flush both the encrypted password and all of the user's synchronized mailbox content out of the service.

 **Device and service security combination**

Each user's unique device key is never stored in the Microsoft 365 or Office 365-based architecture, and a user's Exchange password is never stored on the device. This architecture means that for a malicious party to gain access to a user's password, they would need both unauthorized access to the Microsoft 365 or Office 365-based architecture and physical access to that user's device.

By enforcing PIN policies and encryption on devices in your organization, the malicious party would also have to defeat a device's encryption to get access to the device key. This would all have to take place before the user noticed that the device was compromised and could request a remote wipe for the device.

## Password security FAQ

The following are frequently asked questions regarding security design and settings for Outlook for iOS and Android when used with Basic authentication.

### Are user credentials stored in the Microsoft 365 or Office 365-based architecture if I block Outlook from accessing my Exchange Server?

If you have chosen to block Outlook for iOS and Android from accessing your on-premises Exchange servers, the initial connection will be rejected by Exchange. User credentials will not be stored by the Outlook cloud service and the credentials presented in the failed connection attempt are immediately flushed from memory.

### How is the unique device key and user password encrypted in transit to the Microsoft 365 or Office 365-based architecture?

All communication between the Outlook app and the Microsoft 365 or Office 365-based architecture is through an encrypted TLS connection. The Outlook app is capable of connecting with the Microsoft 365 or Office 365-based architecture and nothing else.

### How do I remove a user's credentials and mailbox information from the Microsoft 365 or Office 365-based architecture?

Have the user uninstall Outlook for iOS and Android on all devices. All data will be removed from the Microsoft 365 or Office 365-based architecture in approximately 3-7 days.

### The app is closed or uninstalled, but I still see it connecting to my Exchange server. How is this happening?

The Microsoft 365 or Office 365-based architecture decrypts user passwords in runtime compute memory and then uses the decrypted passwords to connect to Exchange. Since the architecture is connecting to Exchange on behalf of the device to fetch and cache mailbox data, it can continue for a short period until the service detects that Outlook is no longer requesting data.

If a user uninstalls the app from their device without first using the **Delete Account** option, the Microsoft 365 or Office 365-based architecture will stay connected to your Exchange server until the account becomes inactive, as described above in "Account inactivity and flushing passwords from memory." To stop this activity, follow Option 1 or Option 3 from the above FAQ, or block the app, as described in [Blocking Outlook for iOS and Android](manage-devices.md#blocking-outlook-for-ios-and-android).

### Is a user password less secure in Outlook for iOS and Android than when using other Exchange ActiveSync clients?

No. EAS clients generally save user credentials locally on the user's device. This means a stolen or compromised device could result in a malicious party gaining access to the user's password. With the security design of Outlook for iOS and Android, a malicious party would need unauthorized access to the Microsoft 365 or Office 365-based architecture **and** have physical access to a user's device.

### What happens if a user attempts to use Outlook for iOS and Android after their data has been deleted from the Outlook cloud service?

If a user account becomes inactive (such as by disabling background app refresh on the device or having their device disconnected from the Internet for some time), the Outlook app will reconnect to the Microsoft 365 or Office 365-based architecture the next time the app is launched, and the password encryption and email caching process will restart. This is all transparent to the user.

### Is there a way to prevent the use of Basic authentication for on-premises mailboxes with Outlook for iOS and Android?

Yes, you can deploy hybrid Modern Authentication. For more information, see [Using hybrid Modern Authentication with Outlook for iOS and Android](use-hybrid-modern-auth.md).