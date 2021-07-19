---
title: 'Disable outgoing calls on UM IP gateways: Exchange 2013 Help'
TOCTitle: Disable outgoing calls on UM IP gateways
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer:
ms.assetid: a3777cc6-37e4-4359-ada3-a962ac0ef0c3
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Disable outgoing calls on UM IP gateways in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable or disable outgoing calls for a Unified Messaging (UM) IP gateway. When you clear the **Allow outgoing calls through this UM IP gateway** option on the properties for the UM IP gateway, you configure the UM IP gateway to not accept and send outgoing calls to a Voice over IP (VoIP) gateway, IP PBX, or session border controller (SBC). Although the **Allow outgoing calls through this UM IP gateway** setting controls whether the UM IP gateway is able to initiate outgoing calls for users, it doesn't affect call transfers or incoming calls from a VoIP gateway, IP PBX, or SBC.

For additional management tasks related to UM IP gateways, see [UM IP gateway procedures](um-ip-gateway-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM IP gateways" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the EAC to disable outgoing calls for a UM IP gateway

1. In the EAC, navigate to **Unified Messaging** \> **UM IP Gateways**, select the UM IP gateway you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM IP Gateway** page, clear the check box next to **Allow outgoing calls through this UM IP gateway**.

3. Click **Save**.

## Use the Shell to disable outgoing calls for a UM IP gateway

This example disables outgoing calls on a UM IP gateway named `MyUMIPGateway`.

```powershell
Set-UMIPGateway -Identity MyUMIPGateway -OutcallsAllowed $false
```
