---
localization_priority: Normal
description: The permissions required to perform tasks related to mail flow vary depending on the procedure being performed or the cmdlet you want to run.
ms.topic: reference
author: msdmaguire
ms.author: dmaguire
ms.assetid: f49f4fb5-af75-43cb-900f-c5f7b8cfa143
ms.reviewer:
title: Mail flow permissions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Mail flow permissions

The permissions required to perform tasks related to mail flow vary depending on the procedure being performed or the cmdlet you want to run. For more information about transport features, see [Mail flow and the transport pipeline](../../mail-flow/mail-flow.md).

This topic lists the permissions required to manage the mail flow features in Exchange Server 2016 and Exchange Server 2019. For information about how Microsoft 365 or Office 365 permissions relate to Exchange permissions, see [About admin roles](/microsoft-365/admin/add-users/about-admin-roles).

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click a role group to see its management roles. If a feature lists more than one role group, you need to be assigned to only one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](../../../ExchangeServer2013/understanding-role-based-access-control-exchange-2013-help.md).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](../../../ExchangeServer2013/delegate-role-assignments-exchange-2013-help.md).

> [!NOTE]
> Some features that you want to manage might exist on Edge Transport servers. To manage features on Edge Transport servers, you need to become a member of the Local Administrators group on the Edge Transport server you want to manage. Edge Transport servers don't use Role Based Access Control (RBAC). Features that can be managed on Edge Transport servers have Edge Transport Local Administrator in the "Permissions required" column in the table below.

> [!NOTE]
> Some features may require that you have local administrator permissions on the server you want to manage. To manage these features, you must be a member of the Local Administrators group on that server.

## Mail flow permissions

You can use the features in the following tables to configure mail flow settings in the Front End Transport, Mailbox Transport, and Transport services on Mailbox servers, and on Edge Transport servers. The permissions that are required to configure each feature are listed.

Users who are assigned the View Only Management role group can view the configuration of the features shown in the following table. For more information, see [View Only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

 **Mailbox servers**

|**Feature**|**Permissions required**|
|:-----|:-----|
|Accepted domains|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Active Directory site and site link management|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Antispam features|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Hygiene Management](../../../ExchangeServer2013/hygiene-management-exchange-2013-help.md)|
|Antispam updates|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Hygiene Management](../../../ExchangeServer2013/hygiene-management-exchange-2013-help.md)|
|Certificate management|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Delivery Agent connectors|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|DSNs|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|EdgeSync|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Foreign connectors|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Front End Transport service|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md) <br/> [Hygiene Management](../../../ExchangeServer2013/hygiene-management-exchange-2013-help.md)|
|Journaling|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|Mailbox access|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Mailbox junk email configuration|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Mailbox Transport service|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md) <br/> [Hygiene Management](../../../ExchangeServer2013/hygiene-management-exchange-2013-help.md)|
|MailTips|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Message classifications|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|Message tracking|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Moderated transport|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md)|
|Queues|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Receive connectors|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md) <br/> [Hygiene Management](../../../ExchangeServer2013/hygiene-management-exchange-2013-help.md)|
|Remote domains|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|SafeList aggregation|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|Send connectors|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Shadow redundancy|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Testing mail flow|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Testing mail flow rule (also known as transport rule) processing|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Transport agents|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|Transport configuration|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Transport logs|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Mail flow rules (also known as transport rules)|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|Transport service|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md) <br/> [Hygiene Management](../../../ExchangeServer2013/hygiene-management-exchange-2013-help.md)|
|X.400 domains|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|

 **Edge Transport servers**

|**Feature**|**Permissions required**|
|:-----|:-----|
|Accepted domains - Edge Transport|Edge Transport server local administrator|
|Address Rewriting - Edge Transport|Edge Transport server local administrator|
|Edge Transport server|Edge Transport server local administrator|
|EdgeSync - Edge Transport|Edge Transport server local administrator|
|Queues - Edge Transport|Edge Transport server local administrator|
|Receive connectors - Edge Transport|Edge Transport server local administrator|
|Send connectors - Edge Transport|Edge Transport server local administrator|
|Transport configuration - Edge Transport|Edge Transport server local administrator|
|Transport logs - Edge Transport|Edge Transport server local administrator|
|Mail flow rules (also known as transport rules) - Edge Transport|Edge Transport server local administrator|