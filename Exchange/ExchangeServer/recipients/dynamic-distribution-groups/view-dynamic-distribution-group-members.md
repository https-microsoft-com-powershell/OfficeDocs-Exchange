---
localization_priority: Normal
description: 'Summary: Learn how to use the Exchange Management Shell to view dynamic distribution group membership.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 40b100c6-864e-4c82-9f98-08dd5c83e378
ms.reviewer:
title: View members of a dynamic distribution group
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# View members of a dynamic distribution group

Dynamic distribution groups are distribution groups whose membership is based on specific recipient filters rather than a defined set of recipients. For more information, see [Manage dynamic distribution groups](dynamic-distribution-groups.md).

You can't use the Exchange admin center (EAC) to view the members of a dynamic distribution group. You can only use the Exchange Management Shell.

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Dynamic distribution groups" entry in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver), [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange), or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the Exchange Management Shell to view the members of a dynamic distribution group
<a name="Shell"> </a>

This example returns the list of members for the dynamic distribution group named Full Time Employees. The first command stores the dynamic distribution group object in the variable `$FTE`. The second command uses the **Get-Recipient** cmdlet to list the recipients that match the criteria defined for the dynamic distribution group.

```PowerShell
$group = "Full Time Employees"
```

```PowerShell
Get-Recipient -RecipientPreviewFilter (Get-DynamicDistributionGroup $group).RecipientFilter
```

For detailed syntax and parameter information, see [Get-DynamicDistributionGroup](/powershell/module/exchange/get-dynamicdistributiongroup) and [Get-Recipient](/powershell/module/exchange/get-recipient).