---
localization_priority: Normal
description: Learn about permissions that are required to manage email address and address book features in Exchange Server 2016 or Exchange Server 2019
ms.topic: reference
author: msdmaguire
ms.author: dmaguire
ms.assetid: 1c1de09d-16ef-4424-9bfb-eb7edffbc8c2
ms.reviewer:
title: Email address and address book permissions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Email address and address book permissions

The permissions required to configure email address and address book features vary depending on the procedure being performed or the cmdlet you want to run. For more information about email addresses and address books, see [Email addresses and address books in Exchange Server](../../email-addresses-and-address-books/email-addresses-and-address-books.md).

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](../../../ExchangeServer2013/understanding-role-based-access-control-exchange-2013-help.md).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](../../../ExchangeServer2013/delegate-role-assignments-exchange-2013-help.md).

## Email address and address book permissions

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Address book policies|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Address lists|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Email address policies|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Details templates|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Global address lists|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Offline address books|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Offline address book connectivity|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|