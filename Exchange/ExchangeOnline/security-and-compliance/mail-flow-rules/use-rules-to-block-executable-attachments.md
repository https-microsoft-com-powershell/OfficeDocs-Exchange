---
title: Use mail flow rules to block messages with executable attachments
f1.keywords: 
  - NOCSH
ms.author: chrisda
author: chrisda
manager: dansimp
ms.date: 
audience: ITPro
ms.topic: how-to

localization_priority: Normal
ms.assetid: c4fb4a86-b772-49d0-8773-e8ee897e175d
ms.custom: 
  - seo-marvel-apr2020
description: Admins can learn how to create mail flow rules (transport rules) to block messages that contain executable attachments.
ms.technology: mdo
ms.prod: m365-security
---

# Use mail flow rules to block messages with executable attachments in Exchange Online

In Exchange Online organizations or standalone Exchange Online Protection (EOP) organizations without Exchange Online mailboxes, messages with harmful attachments are blocked by anti-malware policies, including messages with executable attachments. For more information, see [Anti-malware protection in EOP](/microsoft-365/security/office-365-security/anti-malware-protection).

To further enhance protection, you can use mail flow rules (also known as transport rules) to identify and block messages that contain executable attachments as described in this article.

For example, following a malware outbreak, a company could apply this rule with a time limit so that affected users can get back to sending attachments after a specified length of time.

## What do you need to know before you begin?

- You need to be assigned permissions in Exchange Online or Exchange Online Protection before you can do the procedures in this article. Specifically, you need the **Transport Rules** role, which is assigned to the **Organization Management**, **Compliance Management**, and **Records Management** role groups by default.

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

## Use the EAC to create a rule that blocks messages with executable attachments

1. In the EAC, go to **Mail flow** \> **Rules**.

2. Click **Add** ![Add icon](../../media/ITPro-EAC-AddIcon.png) and then select **Create a new rule**.

3. In the **New rule** page that opens, configure the following settings:

   - **Name**: Enter a unique, descriptive name for the rule.

   - Click **More Options**.

   - **Apply this rule if**: Select **Any attachment** \> **has executable content**.

   - **Do the following**: Select **Block the message** and then choose the action you want:

     - **reject the message and include an explanation**: In the **Specify reject reason** dialog that appears, enter the text you want to appear in the non-delivery report (also known as an NDR or bounce message). The default enhanced status code that's used is 5.7.1.

     - **reject the message with the enhanced status code of**:  In the **Enter enhanced status code** dialog that appears, enter the enhanced status code that you want to appear in the NDR. Valid values are 5.7.1 or a value from 5.7.900 to 5.7.999. The default rejection text is: Delivery not authorized, message refused.

     - **reject the message without notifying anyone**

4. When you're finished, click **Save**. Your attachment blocking rule is now in force.

## Use PowerShell to create a rule that blocks messages with executable attachments

Use the following syntax to create a rule to blocks messages that contain executable attachments:

```powershell
New-TransportRule -Name "<UniqueName>" -AttachmentHasExecutableContent $true [-RejectMessageEnhancedStatusCode <5.7.1 | 5.7.900 to 5.7.999>] [-RejectMessageReasonText "<Text>"] [-DeleteMessage $true]
```

**Notes**:

- If you use the _RejectMessageEnhancedStatusCode_ parameter without the _RejectMessageReasonText_ parameter, the default text is: Delivery not authorized, message refused.

- If you use the _RejectMessageReasonText_ parameter without the _RejectMessageEnhancedStatusCode_ parameter, the default code is 5.7.1.

This example creates a new rule named Block Executable Attachments that silently deletes messages that contain executable attachments.

```powershell
New-TransportRule -Name "Block Executable Attachments" -AttachmentHasExecutableContent $true -DeleteMessage $true
```

For detailed syntax and parameter information, see [New-TransportRule](/powershell/module/exchange/new-transportrule).

## How do you know this worked?

To verify that you've successfully create a mail flow rule to block messages that contain executable attachments, do any of the following steps:

- In the EAC, go to **Mail flow** \> **Rules** \> select the rule \> click **Edit** ![Edit icon](../../media/ITPro-EAC-EditIcon.png), and verify the settings.

- In PowerShell, run the following command to verify the settings:

  ```powershell
  Get-TransportRule -Identity "<Rule Name>" | Format-List Name,AttachmentHasExecutableContent,RejectMessage*,DeleteMessage
  ```
