---
localization_priority: Normal
description: 'Summary: Learn about permissions that are required to perform tasks to configure various components of Exchange Server 2016 or Exchange Server 2019.'
ms.topic: reference
author: msdmaguire
ms.author: dmaguire
ms.assetid: 3646a4e8-36b2-41fb-89a4-79b0963fcb11
ms.reviewer:
title: Exchange infrastructure and PowerShell permissions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Exchange infrastructure and PowerShell permissions

The permissions required to perform tasks to configure various components of Exchange Server depend on the procedure being performed or the cmdlet you want to run. See each of the sections in this topic for more information about their respective features.

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](../../../ExchangeServer2013/understanding-role-based-access-control-exchange-2013-help.md).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

   > [!NOTE]
   > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](../../../ExchangeServer2013/delegate-role-assignments-exchange-2013-help.md).

> [!NOTE]
> Some features may require that you have local administrator permissions on the server you want to manage. To manage these features, you must be a member of the Local Administrators group on that server.

## Exchange infrastructure permissions

The following table lists the permissions required to perform tasks that configure general Exchange settings.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

****

|Feature|Permissions required|
|---|---|
|Administrator audit logging|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|Exchange admin center configuration settings|[View-Only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md)|
|Exchange admin center connectivity|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Exchange server configuration settings|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Exchange Help settings|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Message categories|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Hygiene Management](../../../ExchangeServer2013/hygiene-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Help Desk](../../../ExchangeServer2013/help-desk-exchange-2013-help.md)|
|Product key|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Test system health|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|View-only administrator audit logging|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md) <br/> **Note**: You can also manually assign the View-Only Audit Logs management role to a management role group. For more information, see [View-Only Audit Logs](../../../ExchangeServer2013/view-only-audit-logs-role-exchange-2013-help.md).|
|Write to audit log|Users that are members of any role group or assigned any management role can write to the administrator audit log.|
|

## Exchange PowerShell infrastructure permissions

The following table lists the permissions required to perform tasks that configure features that control how the Exchange Management Shell runs.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

****

|Feature|Permissions required|
|---|---|
|Active Directory Domain Services server settings|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [UM Management](../../../ExchangeServer2013/um-management-exchange-2013-help.md)|
|Cmdlet extension agents|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|PowerShell virtual directories|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|PowerShell and WinRM installation|Local Server Administrator|
|Remote PowerShell|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|

## Federation and certificates permissions

The following table lists permissions required for performing tasks related to federation trusts, OAuth configuration, certificate management, and hybrid deployment configuration.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md).

****

|Feature|Permissions required|
|---|---|
|Certificate management|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Federation trusts, OAuth|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Test federation trusts, OAuth|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [View-Only Organization Management](../../../ExchangeServer2013/view-only-organization-management-exchange-2013-help.md) <br/> [Server Management](../../../ExchangeServer2013/server-management-exchange-2013-help.md)|
|Hybrid deployment configuration|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md)|
|Intra-Organization connectors|[Organization Management](../../../ExchangeServer2013/organization-management-exchange-2013-help.md) <br/> [Recipient Management](../../../ExchangeServer2013/recipient-management-exchange-2013-help.md) <br/> [Records Management](../../../ExchangeServer2013/records-management-exchange-2013-help.md)|
|