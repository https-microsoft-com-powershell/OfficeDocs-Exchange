---
title: 'Organization Transport Settings role: Exchange 2013 Help'
TOCTitle: Organization Transport Settings role
ms:assetid: ff4d032e-1b97-4a9d-b479-9b0d61832221
ms:mtpsurl: https://technet.microsoft.com/library/Dd876961(v=EXCHG.150)
ms:contentKeyID: 49289473
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Organization Transport Settings role

_**Applies to:** Exchange Server 2013_

The `Organization Transport Settings` management role enables administrators to manage organization-wide transport settings, such as system messages, Active Directory site configuration, and transport configuration settings for the whole Exchange organization.

This role doesn't enable you to create or manage Send or Receive connectors, queues, hygiene, agents, remote and accepted domains, or rules. To create or manage each of the transport features, you must be assigned one or more of the following roles:

  - [Receive Connectors role](receive-connectors-role-exchange-2013-help.md)

  - [Send Connectors role](send-connectors-role-exchange-2013-help.md)

  - [Transport Queues role](transport-queues-role-exchange-2013-help.md)

  - [Transport Hygiene role](transport-hygiene-role-exchange-2013-help.md)

  - [Transport Agents role](transport-agents-role-exchange-2013-help.md)

  - [Remote and Accepted Domains role](remote-and-accepted-domains-role-exchange-2013-help.md)

  - [Transport Rules role](transport-rules-role-exchange-2013-help.md)

## Default management role assignments

This role has role assignments to one or more role assignees. The following table indicates whether the role assignment is regular or delegating, and also indicates the management scopes applied to each assignment. The following list describes each column:

  - **Regular assignment**: Regular role assignments enable the role assignee to access the permissions provided by the management role entries on this role.

  - **Delegating assignment**: Delegating role assignments give the role assignee the ability to assign this role to role groups, users, or USGs.

  - **Recipient read scope**: The recipient read scope determines what recipient objects the role assignee is allowed to read from Active Directory.

  - **Recipient write scope**: The recipient write scope determines what recipient objects the role assignee is allowed to modify in Active Directory.

  - **Configuration read scope**: The configuration read scope determines what configuration and server objects the role assignee is allowed to read from Active Directory.

  - **Configuration write scope**: The configuration write scope determines what organizational and server objects the role assignee is allowed to modify in Active Directory.

### Default management role assignments for this role

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<thead>
<tr class="header">
<th>Role group</th>
<th>Regular assignment</th>
<th>Delegating assignment</th>
<th>Recipient read scope</th>
<th>Recipient write scope</th>
<th>Configuration read scope</th>
<th>Configuration write scope</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
<td><p>X</p></td>
<td><p>X</p></td>
<td><p><code>Organization</code></p></td>
<td><p><code>Organization</code></p></td>
<td><p><code>OrganizationConfig</code></p></td>
<td><p><code>OrganizationConfig</code></p></td>
</tr>
</tbody>
</table>
