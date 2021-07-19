---
title: 'Configure the time zone: Exchange 2013 Help'
TOCTitle: Configure the time zone
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer:
ms.assetid: 30d769e1-3657-4622-bc9a-643c63cf46d9
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure the time zone in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

By default, the Unified Messaging (UM) auto attendant uses the time zone of the Mailbox server on which it's created. However, there are situations where you may have to change the time zone for a UM auto attendant to a different time zone. For example, if you have two UM dial plans and each dial plan represents a different time zone, you must configure one UM auto attendant to have the same time zone as the Mailbox server and the other UM auto attendant to have a time zone that differs from the Mailbox server.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the EAC to configure the time zone

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to set the time zone, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page, click **Business Hours**, and then, under **Time zone**, select the time zone from the drop-down list.

4. To save your changes, click **OK**, and then click **Save**.

## Use the Shell to configure the time zone

This example sets the time zone to the Pacific time zone on a UM auto attendant named `MyUMAutoAttendant`.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -TimeZoneName Pacific
```
