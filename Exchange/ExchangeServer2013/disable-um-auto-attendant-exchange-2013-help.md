---
title: 'Disable a UM auto attendant: Exchange 2013 Help'
TOCTitle: Disable a UM auto attendant
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer:
ms.assetid: ad79f374-f68f-430b-8b9c-2c841e1c55ae
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Disable a UM auto attendant in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

By default, when a Unified Messaging (UM) auto attendant is created, its status is set to disabled. After you create the UM auto attendant, you can change its status to control whether it can answer incoming calls. For example, you might want to disable the UM auto attendant when you're recording or re-recording customized prompts and messages.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md). Also confirm that the status of the UM auto attendant is set to enabled.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the EAC to disable a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the dial plan you want to change, and on the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to disable. On the toolbar, click **Down arrow** ![Down Arrow Icon](images/ITPro_EAC_DownArrowIcon.gif)

3. On the **Warning** page, click **Yes**.

## Use the Shell to disable a UM auto attendant

This example disables a UM auto attendant named `MyUMAutoAttendant`.

```powershell
Disable-UMAutoAttendant -Identity MyUMAutoAttendant
```
