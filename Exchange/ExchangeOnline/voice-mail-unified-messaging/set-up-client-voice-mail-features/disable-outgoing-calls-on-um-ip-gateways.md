---
localization_priority: Normal
description: You can enable or disable outgoing calls for a Unified Messaging (UM) IP gateway. When you clear the Allow outgoing calls through this UM IP gateway option on the properties for the UM IP gateway, you configure the UM IP gateway to not accept and send outgoing calls to a Voice over IP (VoIP) gateway, IP PBX, or session border controller (SBC). Although the Allow outgoing calls through this UM IP gateway setting controls whether the UM IP gateway is able to initiate outgoing calls for users, it doesn't affect call transfers or incoming calls from a VoIP gateway, IP PBX, or SBC.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: a3777cc6-37e4-4359-ada3-a962ac0ef0c3
ms.reviewer: 
f1.keywords:
- NOCSH
title: Disable outgoing calls on UM IP gateways in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Disable outgoing calls on UM IP gateways in Exchange Online

You can enable or disable outgoing calls for a Unified Messaging (UM) IP gateway. When you clear the **Allow outgoing calls through this UM IP gateway** option on the properties for the UM IP gateway, you configure the UM IP gateway to not accept and send outgoing calls to a Voice over IP (VoIP) gateway, IP PBX, or session border controller (SBC). Although the **Allow outgoing calls through this UM IP gateway** setting controls whether the UM IP gateway is able to initiate outgoing calls for users, it doesn't affect call transfers or incoming calls from a VoIP gateway, IP PBX, or SBC.

For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](../../voice-mail-unified-messaging/connect-voice-mail-system/um-ip-gateway-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-ip-gateway.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the EAC to disable outgoing calls for a UM IP gateway

1. In the EAC, navigate to **Unified Messaging** \> **UM IP Gateways**, select the UM IP gateway you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM IP Gateway** page, clear the check box next to **Allow outgoing calls through this UM IP gateway**.

3. Click **Save**.

## Use Exchange Online PowerShell to disable outgoing calls for a UM IP gateway

This example disables outgoing calls on a UM IP gateway named `MyUMIPGateway`.

```PowerShell
Set-UMIPGateway -Identity MyUMIPGateway -OutcallsAllowed $false
```
