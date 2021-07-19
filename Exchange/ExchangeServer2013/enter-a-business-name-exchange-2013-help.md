---
title: 'Enter a business name: Exchange 2013 Help'
TOCTitle: Enter a business name
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer:
ms.assetid: a0e7cb24-0f55-442d-8ae2-21b177940b78
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enter a business name in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enter the name of your business in the **Business name** box on a UM auto attendant. By default, no business name is entered. If you enter a business name, a default greeting prompt with the business name will be played to callers when they call in to the Unified Messaging (UM) auto attendant.

For additional tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the EAC to configure a business name

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to set a business name, and then, on the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **General**, under **Business name**, type the name of the business.

4. Click **Save**.

## Use the Shell to configure a business name

This example sets the business name on a UM auto attendant named `MyUMAutoAttendant`.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -BusinessName "Northwind Traders"
```
