---
localization_priority: Normal
description: 'Summary: Learn about permissions that are required to manage Unified Messaging services and features in Exchange Server 2016.'
ms.topic: reference
author: msdmaguire
ms.author: dmaguire
ms.assetid: d326c3bc-8f33-434a-bf02-a83cc26a5498
ms.reviewer:
title: Unified Messaging permissions in Exchange 2016
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Unified Messaging permissions

The permissions required to manage Unified Messaging services and features on Exchange 2016 Mailbox servers vary depending on the procedure being performed or the cmdlet you want to run.

> [!NOTE]
> Unified Messaging is not available in Exchange 2019.

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](../../../ExchangeServer2013/understanding-role-based-access-control-exchange-2013-help.md).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](../../../ExchangeServer2013/delegate-role-assignments-exchange-2013-help.md).

## UM component permissions

You can configure settings for the UM components and features in the following table.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

|**Feature**|**Permissions required**|
|:-----|:-----|
|UM auto attendants|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Unified Messaging Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|UM call answering rules|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Unified Messaging Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|UM call data and summary reports|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Unified Messaging Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|UM Call Router service (front-end)|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Unified Messaging Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|UM dial plans|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Unified Messaging Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|UM hunt groups|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Unified Messaging Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|UM IP gateways|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Unified Messaging Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|UM mailbox policies|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Unified Messaging Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|UM mailboxes|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Unified Messaging Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|UM prompts|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Unified Messaging Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|UM service (back-end)|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|