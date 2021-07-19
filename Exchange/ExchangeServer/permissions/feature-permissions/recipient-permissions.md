---
localization_priority: Normal
description: 'Summary: Learn about permissions that are required to manage recipients in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: reference
author: msdmaguire
ms.author: dmaguire
ms.assetid: 5b690bcb-c6df-4511-90e1-08ca91f43b37
ms.reviewer:
title: Recipients Permissions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Recipients Permissions

The permissions required to perform tasks to manage recipients vary depending on the procedure being performed or the cmdlet you want to run.

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you need to be assigned to only one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](../../../ExchangeServer2013/understanding-role-based-access-control-exchange-2013-help.md).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](../../../ExchangeServer2013/delegate-role-assignments-exchange-2013-help.md).

## Mailbox server permissions

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Calendar repair, server configuration|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Delegating Mailbox servers|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Email address policies|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Exchange Search|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [View-Only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Exchange Search - diagnostics|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [View-Only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md) <br/> Support Diagnostics role  <br/> **Note:**: The Support Diagnostics role isn't assigned to a role group. For more information, see [Add a role to a role group](../role-groups.md#add-a-role-to-a-role-group).|
|Group metrics|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Import Export|Mailbox Import Export role  <br/> **Note:**: The Mailbox Import Export role isn't assigned to a role group. For more information, see [Mailbox Import Export Role](../../../ExchangeServer2013/mailbox-import-export-role-exchange-2013-help.md).|
|Mailbox Assistants|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Mailbox moves|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Mailbox recovery|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Mailbox repair request|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Mailbox restore request|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Mailbox server configuration|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Manage Exchange Search Indexer service on a Mailbox server|Local Administrator on the Mailbox server|
|MAPI connectivity|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|OAB virtual directories|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Remove store mailbox|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|

## Calendar and sharing permissions
<a name="calendar"> </a>

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Calendar configuration|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Calendar diagnostics|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md) <br/> [Hygiene Management](../../../ExchangeServer2013/hygiene-management-exchange-2013-help.md) <br/> [Compliance Management](../../../ExchangeServer2013/compliance-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Calendar processing|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Notifications|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Organization relationships|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Sharing policies|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|

## Resource mailbox configuration permissions
<a name="calendar"> </a>

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Booking policies|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Delegation|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Resource mailbox schema configuration|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|

## Mailbox database permissions
<a name="calendar"> </a>

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Mailbox databases|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|

## Recipient provisioning permissions
<a name="calendar"> </a>

This table contains the various permissions that are required to manage recipients.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Address list, GAL|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Antispam|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Apps for Outlook|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [View-Only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Applying sharing policies|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Arbitration|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Archive connectivity|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [View-Only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Assigning offline address books|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Automatic replies|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Calendar configuration|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Calendar repair|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Contact aggregation settings|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [View-Only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md)|
|Convert mailboxes|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Disconnected mailboxes|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Distribution groups|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Dynamic distribution groups|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Email addresses|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [UM Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|Inbox rules|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Mail contacts|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Mail tips|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Mail user|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Mailbox folder permissions|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Mailbox folders|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|MAPI connectivity|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Message configuration|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Message quotas|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Moderation|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Permissions and delegation|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Archive mailboxes|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Recipient data properties|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Remote mailboxes|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Retention and legal holds|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|Send As|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Spelling configuration|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Unified Messaging (in Exchange 2016; not available in Exchange 2019)|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [UM Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|User mailboxes|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|User photos|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|

## Mailbox move and migration permissions
<a name="calendar"> </a>

The table contains the permissions that are required to move on-premises mailboxes to different domains or forests and to migrate on-premises mailboxes to and from your cloud-based organization.

|**Feature**|**Permissions required**|
|:-----|:-----|
|Mailbox moves (local or cross-forest)|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Mailbox moves (hybrid deployment)|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Migration (on-boarding and off-boarding from the cloud)|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|