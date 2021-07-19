---
title: Use mail flow rules to the SCL in messages
f1.keywords: 
  - NOCSH
ms.author: chrisda
author: chrisda
manager: dansimp
ms.date: 
audience: ITPro
ms.topic: how-to

localization_priority: Normal
search.appverid: 
  - MET150
ms.assetid: 4ccab17a-6d49-4786-aa28-92fb28893e99
ms.collection: 
  - M365-security-compliance
description: Learn how to create mail flow rules (transport rules) to identify messages and set the spam confidence level (SCL) of messages in Exchange Online Protection.
ms.custom: seo-marvel-apr2020
ms.technology: mdo
ms.prod: m365-security
---

# Use mail flow rules to set the spam confidence level (SCL) in messages in Exchange Online

In Exchange Online organizations or standalone Exchange Online Protection (EOP) organizations without Exchange Online mailboxes, anti-spam policies (also known as spam filter policies or content filter policies) scan inbound messages for spam. For more information, see [Configure anti-spam policies in EOP](/microsoft-365/security/office-365-security/configure-your-spam-filter-policies).

If you want to mark specific messages as spam before they're even scanned by spam filtering, or mark messages so they'll skip spam filtering, you can create mail flow rules (also known as transport rules) to identify the messages and set the spam confidence level (SCL). For more information about the SCL, see [Spam confidence level (SCL) in EOP](/microsoft-365/security/office-365-security/spam-confidence-levels).

## What do you need to know before you begin?

- You need to be assigned permissions in Exchange Online or Exchange Online Protection before you can do the procedures in this article. Specifically, you need the **Transport Rules** role, which is assigned to the **Organization Management**, **Compliance Management** (global admins), and **Records Management** role groups by default.

  For more information, see the following topics:

  - [Permissions in Exchange Online](../../permissions-exo/permissions-exo.md)
  - [Permissions in standalone EOP](/microsoft-365/security/office-365-security/feature-permissions-in-eop)
  - [Use the EAC modify the list of members in role groups](/microsoft-365/security/office-365-security/manage-admin-role-group-permissions-in-eop#use-the-eac-modify-the-list-of-members-in-role-groups)

- To open the EAC in Exchange Online, see [Exchange admin center in Exchange Online](../../exchange-admin-center.md). To open the EAC in standalone EOP, see [Exchange admin center in standalone EOP](/microsoft-365/security/office-365-security/exchange-admin-center-in-exchange-online-protection-eop).

- To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell). To connect to standalone EOP PowerShell, see [Connect to Exchange Online Protection PowerShell](/powershell/exchange/connect-to-exchange-online-protection-powershell).

- For more information about mail flow rules in Exchange Online and standalone EOP, see the following topics:
  - [Mail flow rules (transport rules) in Exchange Online](mail-flow-rules.md)
  - [Mail flow rule conditions and exceptions (predicates) in Exchange Online](conditions-and-exceptions.md)
  - [Mail flow rule actions in Exchange Online](mail-flow-rule-actions.md)

## Use the EAC to create a mail flow rule that sets the SCL of a message

1. In the EAC, go to **Mail flow** \> **Rules**.

2. Click **Add** ![Add icon](../../media/ITPro-EAC-AddIcon.png) and then select **Create a new rule**.

3. In the **New rule** page that opens, configure the following settings:

   - **Name**: Enter a unique, descriptive name for the rule.

   - Click **More Options**.

   - **Apply this rule if**: Select one or more conditions to identify messages. For more information, see [Mail flow rule conditions and exceptions (predicates) in Exchange Online](conditions-and-exceptions.md).

   - **Do the following**: Select **Modify the message properties** \> **set the spam confidence level (SCL)**. In the **Specify SCL** dialog that appears, configure one of the following values:

   - **Bypass spam filtering**: The messages will skip spam filtering.

     > [!CAUTION]
     > Be very careful about allowing messages to skip spam filtering. Attackers can use this vulnerability to send phishing and other malicious messages into your organization. The mail flow rules requires more than just the sender's email address or domain. For more information, see [Create safe sender lists in EOP](/microsoft-365/security/office-365-security/create-safe-sender-lists-in-office-365.md).

   - **0 to 4**: The message is sent through spam filtering for additional processing.

   - **5 or 6**: The message is marked as **Spam**. The action that you've configured for **Spam** filtering verdicts in your anti-spam policies is applied to the message (the default value is **Move message to Junk Email folder**).

   - **7 to 9**: The message is marked as **High confidence spam**. The action that you've configured for **High confidence spam** filtering verdicts in your anti-spam policies is applied to the message (the default value is **Move message to Junk Email folder**).

4. Specify any additional properties that you want for the rule. When you're finished, click **Save**.

## How do you know this worked?

To verify that this procedure is working correctly, send an email message to someone inside your organization, and verify that the action performed on the message is as expected. For example, if you **set the spam confidence level (SCL)** to **Bypass spam filtering**, then the message should be sent to the specified recipient's inbox. However, if you **set the spam confidence level (SCL)** to **9**, and the **High confidence spam** action for your applicable anti-spam policies is to move the message to the Junk Email folder, then the message should be sent to the specified recipient's Junk Email folder.
