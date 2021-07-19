---
title: 'Enable common PIN patterns for voice mail: Exchange 2013 Help'
TOCTitle: Enable common PIN patterns for voice mail
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer:
ms.assetid: 9940a8c2-f576-4089-ab96-8b318ad3da0f
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable common PIN patterns for voice mail in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable or disable common Unified Messaging (UM) PIN patterns for Outlook Voice Access users. If you enable or disable the common PIN patterns setting on a UM mailbox policy, the setting will apply to all UM-enabled users associated with the UM mailbox policy. By default, UM-enabled users can't use common patterns when they create a PIN.

You can configure several PIN-related settings on a UM mailbox policy. The **Allow Common PIN Patterns** setting is used to allow or prevent the use of common number patterns when users create a PIN. By default, this setting is disabled and prevents users from using the following number patterns:

- **Sequential numbers**: These are PIN values that include only consecutive numbers. Examples of consecutive numbers for a PIN are 1234 and 65432.

- **Repeated numbers**: These are PIN values that include only repeated numbers. Examples of repeated numbers are 11111 and 22222.

- **Suffix of mailbox extension**: These are PIN values that include the suffix of a user's mailbox extension. For example, if a user's mailbox extension is 36697, the user's PIN cannot be 3669712.

> [!NOTE]
> If the **Allow Common PIN Patterns** setting is enabled, only the suffix of the mailbox extension will be rejected.

For additional tasks related to Outlook Voice Access PIN security, see [PIN security procedures](pin-security-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the EAC to enable common PIN patterns

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then on the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then on the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page, under **PIN polices** select the check box next to **Allow common PIN patterns**.

4. Click **Save**.

## Use the Shell to enable common PIN patterns

This example allows users associated with the UM mailbox policy named `MyUMMailboxPolicy` to use PINs that contain common patterns.

```powershell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -AllowCommonPatterns $true
```
