---
title: 'Remove a database availability group: Exchange 2013 Help'
TOCTitle: Remove a database availability group
ms:assetid: 071296e9-31b0-40f4-9a02-177d97486ebd
ms:mtpsurl: https://technet.microsoft.com/library/Dd335069(v=EXCHG.150)
ms:contentKeyID: 48384792
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove a database availability group

_**Applies to:** Exchange Server 2013_

Removing a database availability group (DAG) is a quick and easy task. You can use the EAC or the Shell to remove a DAG.

Looking for other management tasks related to DAGs? Check out [Managing database availability groups](managing-database-availability-groups-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](high-availability-and-site-resilience-permissions-exchange-2013-help.md) topic.

- Before you can remove a DAG, the DAG must be empty. If the DAG you want to remove contains any Mailbox servers, you must first remove the servers from the DAG. For detailed steps about how to remove a Mailbox server from a DAG, see [Manage database availability group membership](manage-database-availability-group-membership-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the EAC to remove a database availability group

1. Navigate to **Servers** \> **Database availability groups**.

2. Select the DAG you want to remove and click **Delete** ![Delete icon](images/Dd298078.14f639f6-61e8-4418-bbfb-0db14de9d2f5(EXCHG.150).gif "Delete icon").

3. Click **Yes** to confirm the warning and remove the DAG.

## Use the Shell to remove a database availability group

This example removes the DAG DAG1.

```powershell
Remove-DatabaseAvailabilityGroup -Identity DAG1
```

## How do you know this worked?

To verify that you've successfully removed the DAG, do one of the following:

- In the EAC, go to **Servers** \> **Database Availability Groups**, and see if the DAG is still displayed.

- In the Shell, run the following command to see if the DAG still exists:

  ```powershell
  Get-DatabaseAvailabilityGroup <DAGName>
  ```

  If the DAG was successfully deleted, the preceding command will produce an error message indicating the object could not be found.
