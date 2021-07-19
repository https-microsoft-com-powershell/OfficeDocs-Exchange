---
localization_priority: Normal
description: Admins can learn about scenarios for mail flow rules in Exchange Online.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: ca8b52ef-0af7-4d3d-96df-13df297585e0
ms.reviewer: 
f1.keywords:
- NOCSH
title: Mail flow rule procedures in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Mail flow rule procedures in Exchange Online

In Exchange Online organizations or standalone Exchange Online Protection (EOP) organizations without Exchange Online mailboxes, you can use mail flow rules (also known as transport rules) to meet the scenarios as described in this article.

To learn about concepts and objectives for mail flow rules, see [Mail flow rules (transport rules) in Exchange Online](mail-flow-rules.md).

## Mail flow rule procedures for anti-spam features in Exchange Online and standalone EOP

[Use mail flow rules for attachment blocking scenarios](common-attachment-blocking-scenarios.md): Learn how to use mail flow rules to block all attachments.

[Use mail flow rules to block messages with executable attachments](use-rules-to-block-executable-attachments.md): Learn how to use mail flow rules to block messages that contain executable attachments.

[Use mail flow rules to inspect message attachments](inspect-message-attachments.md): Learn how to use mail flow rule conditions that allow you to inspect the content of message attachments.

[Use mail flow rules to set the spam confidence level (SCL) in messages](use-rules-to-set-scl.md): Learn how to use mail flow rules to mark specific messages as spam before they're even scanned by spam filtering, or mark messages so they'll skip spam filtering.

[Use mail flow rules to filter bulk email](use-rules-to-filter-bulk-mail.md): Examples describing how to mark messages that contain specific bulk indicator content as spam.

[Use mail flow rules to see what users are reporting to Microsoft](use-rules-to-see-what-users-are-reporting-to-microsoft.md): Receive copies of messages that users report as junk, not junk or phishing to Microsoft.

## Mail flow rule procedures for other features in Exchange Online and standalone EOP

[Organization-wide message disclaimers, signatures, footers, or headers](disclaimers-signatures-footers-or-headers.md): Learn how to set up a legal disclaimer, email disclaimer, consistent signature, email header, or email footer by using mail flow rules.

[Use mail flow rules so messages can bypass Clutter](use-rules-to-bypass-clutter.md): Information to help you make sure messages are sent to an inbox instead of the **Clutter** folder.

[Use mail flow rules to route email based on a list of words, phrases, or patterns](use-rules-to-route-email.md): Information to help you comply with your organization's email policies.

### Mail flow rule procedures for features in Exchange Online only

[Use mail flow rules for message approval scenarios in Exchange Online](common-message-approval-scenarios.md): Use mail flow rules instead of enabling moderation on recipients to meet message approval scenarios.

[Use mail flow rules to automatically add meetings to calendars in Exchange Online](use-rules-to-add-meetings.md): Use the Direct to Calendar feature in Exchange Online to add meetings directly to calendars in Exchange Online.

[Define rules to encrypt email messages in Exchange Online](/microsoft-365/compliance/define-mail-flow-rules-to-encrypt-email): Learn how to use mail flow rules to encrypt messages using Office 365 Message Encryption (OME).

### For more information

[Mail flow rules (transport rules) in Exchange Online](mail-flow-rules.md)

[Manage mail flow rules in Exchange Online](manage-mail-flow-rules.md)

[Best practices for configuring mail flow rules in Exchange Online](configuration-best-practices.md)

[Test mail flow rules in Exchange Online](test-mail-flow-rules.md)

[Use mail protection reports to view data about malware, spam, and rule detections](../../monitoring/use-mail-protection-reports.md)
