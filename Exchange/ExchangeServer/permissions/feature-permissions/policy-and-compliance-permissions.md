---
localization_priority: Normal
description: 'Summary: Learn about permissions that are required to manage policy and compliance features in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: reference
author: msdmaguire
ms.author: dmaguire
ms.assetid: ec4d3b9f-b85a-4cb9-95f5-6fc149c3899b
ms.reviewer:
title: Messaging policy and compliance permissions in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Messaging policy and compliance permissions in Exchange Server

The permissions required to configure messaging policy and compliance vary depending on the procedure being performed or the cmdlet you want to run. For more information about messaging policy and compliance, see [Messaging policy and compliance in Exchange Server](../../policy-and-compliance/policy-and-compliance.md).

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](../../../ExchangeServer2013/understanding-role-based-access-control-exchange-2013-help.md).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](../../../ExchangeServer2013/delegate-role-assignments-exchange-2013-help.md).

## Messaging policy and compliance permissions

You can use the features in the following table to configure messaging policy and compliance features. The role groups that are required to configure each feature are listed.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-Only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Data loss prevention (DLP)|[Compliance Management](../../../ExchangeServer2013/compliance-management-exchange-2013-help.md)|
|Delete mailbox content (using the [Search-Mailbox](/powershell/module/exchange/search-mailbox) cmdlet with the _DeleteContent_ switch)|[Discovery Management](../../../ExchangeServer2013/discovery-management-exchange-2013-help.md) **and** <br/> [Mailbox Import Export Role](../../../ExchangeServer2013/mailbox-import-export-role-exchange-2013-help.md) <br/> **Note**: By default, the Mailbox Import Export role isn't assigned to any role group. You can assign a management role to a built-in or custom role group, a user, or a universal security group. Assigning a role to a role group is recommended. For more information, see [Add a role to a role group](../role-groups.md#add-a-role-to-a-role-group).|
|Discovery mailboxes - Create|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Information Rights Management (IRM) configuration|[Compliance Management](../../../ExchangeServer2013/compliance-management-exchange-2013-help.md) <br/> [Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|In-Place Archive|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|In-Place Archive - Test connectivity|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|In-Place eDiscovery|[Discovery Management](../../../ExchangeServer2013/discovery-management-exchange-2013-help.md) <br/> **Note**: By default, the Discovery Management role group doesn't have any members. No users, including administrators, have the required permissions to search mailboxes. For more information, see [Assign eDiscovery permissions in Exchange Server](../../policy-and-compliance/ediscovery/assign-permissions.md).|
|In-Place Hold|[Discovery Management](../../../ExchangeServer2013/discovery-management-exchange-2013-help.md) <br/> [Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> **Notes**:  <br/> • To create a query-based In-Place Hold, a user requires both the Mailbox Search and Legal Hold roles to be assigned directly or via membership in a role group that has both roles assigned. To create an In-Place Hold without using a query, which places all mailbox items on hold, you must have the Legal Hold role assigned. The Discovery Management role group is assigned both roles.  <br/> • The Organization Management role group is assigned the Legal Hold role. Members of the Organization Management role group can place an In-Place Hold on all items in a mailbox, but can't create a query-based In-Place Hold.|
|Journaling|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|Litigation Hold|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Mailbox audit logging|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|Message classifications|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Messaging records management|[Compliance Management](../../../ExchangeServer2013/compliance-management-exchange-2013-help.md) <br/> [Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|Retention policies - Apply|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|Retention policies - Create|See the entry for Messaging records management|
|Mail flow rules (also known as transport rules)|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|