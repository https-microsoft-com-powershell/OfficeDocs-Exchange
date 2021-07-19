---
localization_priority: Normal
description: When you enable a user for UM and link them to a telephone extension dial plan, an EUM proxy address is created for the user that contains the user's extension number. You must define at least one extension number for UM to use so voice mail can be sent to the user's mailbox. The extension number is also used when the user calls in to an Outlook Voice Access number.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 1a73c9c8-cb50-4bd7-a101-dadd20e28031
ms.reviewer: 
f1.keywords:
- NOCSH
title: Add an extension number in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Add an extension number in Exchange Online

When you enable a user for UM and link them to a telephone extension dial plan, an EUM proxy address is created for the user that contains the user's extension number. You must define at least one extension number for UM to use so voice mail can be sent to the user's mailbox. The extension number is also used when the user calls in to an Outlook Voice Access number.

The primary extension number you added when the user was enabled for UM will be listed as the primary EUM proxy address. If the primary extension number was removed, the first EUM proxy address you add that contains the user's extension number will become the primary EUM proxy address. Any additional extension numbers you add will be listed as secondary EUM proxy addresses. When additional secondary extension numbers are added, callers can leave voice mail for the user at all extension numbers that have been set. All the voice messages will be delivered to the same user's mailbox.

You can use the EAC or Exchange Online PowerShell to add a primary or a secondary extension number for a user. You can use the **Email Address** page on the user's mailbox in the EAC to add a primary or secondary extension number. You can't use the **UM Mailbox** page in the EAC to add a primary extension number, but you can use that page to add secondary extension numbers.

You can view the primary and secondary extension numbers for a user by using the **Get-UMMailbox** cmdlet or the **Get-Mailbox** cmdlet in Exchange Online PowerShell.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a telephone extension UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy.md).

- Before you perform these procedures, confirm that the user's mailbox has been enabled for UM and linked to a telephone extension dial plan. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail.md).

- Before you perform these procedures, confirm that the extension number that will be assigned to the user contains the correct number of digits set on the UM dial plan.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the EAC to add a secondary extension number

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the mailbox to which you want to add an extension number.

3. In the details pane, **Phone and Voice Features**, under **Unified Messaging**, click **View details**.

4. On the **UM Mailbox** page, click **Other Extensions**, and then click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

5. On the **Other extensions** page, next to the **UM dial plan** box, click **Browse** and locate the dial plan for the user.

6. On the **Other extensions** page, in the **Extension number** box, type the extension number, and then click **OK**.

7. Click **Save**.

## Use the EAC to add a primary or secondary extension number

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the mailbox to which you want to add an extension number, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **User Mailbox** page, under **Email address**, click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

4. On the **New email address** page, select **EUM** and, in the **Address/Extension** box, enter the extension number for the user.

5. On the **New email address** page, under **Dial plan**, click **Browse** to select the telephone extension dial plan, and then click **OK**.

6. Click **Save**.

## Use Exchange Online PowerShell to add an extension number

This example adds an extension number 22222 for Tony Smith, a UM-enabled user.

> [!NOTE]
> Before you add an extension number using Exchange Online PowerShell, you need to determine the position of the EUM proxy address that you want to add. To determine the position, use the **$mbx.EmailAddresses** command. The first proxy address in the list will be 0.

```PowerShell
$mbx=Get-Mailbox tony.smith
$mbx.EmailAddresses +="eum:22222;phone-context=MyDialPlan.contoso.com"
Set-Mailbox tony.smith -EmailAddresses $mbx.EmailAddresses
```
