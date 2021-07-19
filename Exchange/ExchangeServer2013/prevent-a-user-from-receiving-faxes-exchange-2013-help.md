---
title: 'Prevent a user from receiving faxes: Exchange 2013 Help'
TOCTitle: Prevent a user from receiving faxes
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer:
ms.assetid: b5d022b9-043a-4324-87fb-074d5e2c2ca3
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Prevent a user from receiving faxes in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Prevent a Unified Messaging (UM) user from receiving faxes. Find out how to alter fax settings for new and existing UM users.

By default, when you enable a user for Unified Messaging, they will be able to receive faxes if you enable faxing and configure a fax partner's URI on the UM mailbox policy that is linked to the user. Faxing can be enabled or disabled on UM dial plans, UM mailbox policies, or the UM-enabled user's mailbox.

By default, the user's mailbox and the dial plan that is linked with the user allow incoming faxes. However, for a user to receive faxes you must first enable inbound faxing on the UM mailbox policy that's associated with the UM-enabled user and enter the fax partner's URI.

> [!NOTE]
> You can use the EAC to configure fax settings on a Unified Messaging mailbox policy. However, you must use the Shell to configure fax settings on dial plans or for individual users.

For additional management tasks related to faxing, see [Faxing procedures](faxing-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- Before you perform these procedures, confirm that the user is enabled for Unified Messaging. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the Shell to prevent a UM-enabled user from receiving faxes

This example prevents a UM-enabled user named Tony from receiving fax messages in his mailbox.

```powershell
Set-UMMailbox -Identity tony@contoso.com -FaxEnabled $false
```
