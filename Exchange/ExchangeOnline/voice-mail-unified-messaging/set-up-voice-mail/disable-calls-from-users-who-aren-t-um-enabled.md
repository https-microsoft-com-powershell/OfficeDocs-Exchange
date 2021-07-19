---
localization_priority: Normal
description: You can enable or disable calls from users who aren't enabled for Unified Messaging (UM). By default, Unified Messaging allows incoming calls from unauthenticated callers through an auto attendant to be transferred to UM-enabled users. With this setting enabled, users from outside an organization can transfer calls to UM-enabled users.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 272ff4ab-b4d9-4647-98e2-7c171f9dfc3f
ms.reviewer: 
f1.keywords:
- NOCSH
title: Disable calls from users who aren't UM-enabled in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Disable calls from users who aren't UM-enabled in Exchange Online

You can enable or disable calls from users who aren't enabled for Unified Messaging (UM). By default, Unified Messaging allows incoming calls from unauthenticated callers through an auto attendant to be transferred to UM-enabled users. With this setting enabled, users from outside an organization can transfer calls to UM-enabled users.

If this setting has been disabled for a UM-enabled user, the user's mailbox can still be located using a directory search. However, if an external caller tries to transfer to the user, the system says, "I'm sorry, I am unable to transfer the call to this user." The caller is then transferred to the operator, if an operator has been configured on the auto attendant. If no operator has been configured on the auto attendant, the call is transferred to a dial plan operator, if one has been configured. If no operator extension has been configured on the speech-enabled auto attendant, the dual tone multi-frequency (DTMF) fallback auto attendant, or the dial plan, the system responds by saying, "Sorry. Neither the operator nor the touchtone service are available."

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy.md).

- Before you perform this procedure, confirm that the user's mailbox has been UM-enabled. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use Exchange Online PowerShell to disable calls from users who aren't UM-enabled

This example prevents Tony Smith from receiving voice calls from callers who aren't UM-enabled.

```PowerShell
Set UMMailbox -Identity tony@contoso.com -AllowUMCallsFromNonUsers None
```
