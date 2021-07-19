---
localization_priority: Normal
description: 'Summary: Learn about permissions that are required to manage high availability in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: reference
author: msdmaguire
ms.author: dmaguire
ms.assetid: 66085107-4d4d-41c3-a425-82314acd9eee
ms.reviewer:
title: High availability and site resilience permissions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# High availability and site resilience permissions

The permissions required to configure high availability vary depending on the procedure being performed or the cmdlet you want to run. For more information about high availability, see [High availability and site resilience](../../high-availability/high-availability.md).

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](../../../ExchangeServer2013/understanding-role-based-access-control-exchange-2013-help.md).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](../../../ExchangeServer2013/delegate-role-assignments-exchange-2013-help.md).

## Database availability group permissions

You can use the features in the following table to add, remove, and configure settings for database availability groups (DAGs).

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Database availability group membership|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Database Availability Groups](../../../ExchangeServer2013/database-availability-groups-role-exchange-2013-help.md)|
|Database availability group properties|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Database Availability Groups](../../../ExchangeServer2013/database-availability-groups-role-exchange-2013-help.md)|
|Database availability groups|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Database Availability Groups](../../../ExchangeServer2013/database-availability-groups-role-exchange-2013-help.md)|
|Database availability networks|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Database Availability Groups](../../../ExchangeServer2013/database-availability-groups-role-exchange-2013-help.md)|

## Mailbox database copy permissions

You can use the features in the following table to add, remove, update, and activate mailbox database copies.

|**Feature**|**Permissions required**|
|:-----|:-----|
|Database switchover|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Database Copies](../../../ExchangeServer2013/database-copies-role-exchange-2013-help.md)|
|Mailbox database copies|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Database Copies](../../../ExchangeServer2013/database-copies-role-exchange-2013-help.md)|
|Server switchover|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Database Copies](../../../ExchangeServer2013/database-copies-role-exchange-2013-help.md)|
|Update a mailbox database copy|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Database Copies](../../../ExchangeServer2013/database-copies-role-exchange-2013-help.md)|