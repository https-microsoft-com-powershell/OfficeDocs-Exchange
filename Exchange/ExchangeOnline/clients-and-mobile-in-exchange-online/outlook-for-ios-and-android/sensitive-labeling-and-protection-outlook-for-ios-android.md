---
localization_priority: Normal
description: 'How to classify and/or protect messages when using Outlook for iOS and Android.'
ms.topic: article
author: msdmaguire
ms.author: jhendr
title: Sensitivity labeling and protection in Outlook for iOS and Android
ms.reviewer: smithre4
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Sensitivity labeling and protection in Outlook for iOS and Android

**Summary**: How to classify and/or protect messages when using Outlook for iOS and Android.

Protecting company or organizational data is extremely important. Outlook for iOS and Android supports two scenarios for classifying and/or protecting content:

- Sensitivity labeling
- Secure/Multipurpose Internet Mail Extension (S/MIME)

Sensitivity labeling and S/MIME in Outlook for iOS and Android are supported with Microsoft 365 or Office 365 accounts using the native Microsoft sync technology.

## Understanding sensitivity labeling

Sensitivity labeling enables organizations to classify and protect sensitive content. For more information, see [Learn about sensitivity labels](/microsoft-365/compliance/sensitivity-labels).

From a classification perspective, a sensitivity label is applied to a message and is retained throughout the message's lifecycle (assuming the label is not removed). In addition, sensitivity labels can be configured to mark content by adding a header or footer to the message body.

Sensitivity labels can also be configured to protect messages with access restrictions or encryption. Access restrictions include ensuring only users within the organization can open the message, restricting editing rights, preventing forwarding, printing, or copying the contents of the message. Encryption provides at-rest encryption and ensures only authorized users can decrypt the message.

When a sensitivity label is configured with encryption, the encryption process depends on the client platform. With Outlook for iOS and Android, encryption occurs within Exchange Online transport after the message is sent from the sender, prior to recipient delivery. Encryption does not occur within the app. For more information, see [Manage sensitivity labels in Office apps](/microsoft-365/compliance/sensitivity-labels-office-apps).

Likewise, Outlook for iOS and Android does not perform decryption of received messages, either. Exchange Online performs the decryption prior to delivering the message to Outlook for iOS and Android. For more information, see [Outlook for iOS and Android in Exchange Online: FAQ](outlook-for-ios-and-android-faq.md).

## Deploying sensitivity Labeling with Outlook for iOS and Android

For information about how to create and define sensitivity labels, as well as, publishing a label policy, see [Create and configure sensitivity labels and their policies](/microsoft-365/compliance/create-sensitivity-labels). If you are new to sensitivity labels, you might also find it useful to review [Get started with sensitivity labels](/microsoft-365/compliance/get-started-with-sensitivity-labels) for information about licensing, permissions, deployment strategies, and a list of common scenarios that support sensitivity labels.

> [!IMPORTANT]
> If your organization has previously deployed Azure Information Protection labels, you must migrate from Azure Information Protection to Microsoft Information Protection. To determine which platform is being used, see [Frequently asked questions for Azure Information Protection](/azure/information-protection/faqs#how-can-i-determine-if-my-tenant-is-on-the-unified-labeling-platform). To complete the migration, see [How to migrate Azure Information Protection labels to unified sensitivity labels](/azure/information-protection/configure-policy-migrate-labels).

## Using sensitivity labeling with Outlook for iOS and Android

For information about the end user experience, see [Apply sensitivity labels to your documents and email within Office](https://support.microsoft.com/office/2f96e7cd-d5a4-403b-8bd7-4cc636bae0f9).

## Understanding S/MIME

S/MIME provides encryption, which protects the content of email messages, and it provides digital signatures, which verify the identity of the sender of an email message. S/MIME in Outlook for iOS and Android is supported with Microsoft 365 or Office 365 accounts using the native Microsoft sync technology. For a general overview of S/MIME, see [S/MIME in Exchange Online](../../security-and-compliance/smime-exo/smime-exo.md).

## Deploying and using S/MIME with Outlook for iOS and Android

See [S/MIME for Outlook for iOS and Android](smime-outlook-for-ios-and-android.md).
