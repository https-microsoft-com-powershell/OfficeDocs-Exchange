---
title: 'Active Directory Permissions role: Exchange 2013 Help'
TOCTitle: Active Directory Permissions role
ms:assetid: 0908b4cb-08fd-44f6-bab6-c66af64b2d1a
ms:mtpsurl: https://technet.microsoft.com/library/Dd876845(v=EXCHG.150)
ms:contentKeyID: 49289155
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Active Directory Permissions role

_**Applies to:** Exchange Server 2013_

The `Active Directory Permissions` management role enables administrators to configure Active Directory permissions in an organization. Some features that use Active Directory permissions or an access control list (ACL) include transport Receive and Send connectors, and Send As and Send on behalf of permissions for mailboxes.

> [!NOTE]
> Permissions set directly on Active Directory objects may not be enforced through Role Based Access Control (RBAC).

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
