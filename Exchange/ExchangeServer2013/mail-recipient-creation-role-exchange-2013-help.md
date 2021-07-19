---
title: 'Mail Recipient Creation role: Exchange 2013 Help'
TOCTitle: Mail Recipient Creation role
ms:assetid: 81081845-9ebf-4888-8f9f-fe7cf2704486
ms:mtpsurl: https://technet.microsoft.com/library/Dd876915(v=EXCHG.150)
ms:contentKeyID: 49289326
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Mail Recipient Creation role

_**Applies to:** Exchange Server 2013_

The `Mail Recipient Creation` management role enables administrators to create mailboxes, mail users, mail contacts, distribution groups, and dynamic distribution groups in an organization. This role can be combined with the `Mail Recipients` role to enable the creation and management of recipients. For more information, see [Mail Recipients role](mail-recipients-role-exchange-2013-help.md).

This role doesn't enable you to mail-enable public folders. To mail-enable public folders, the `Mail Enabled Public Folders` role must be used. For more information, see [Mail Enabled Public Folders role](mail-enabled-public-folders-role-exchange-2013-help.md).

If your organization maintains a Role Based Access Control (RBAC) split permissions model where recipient creation is performed by a different group than those who perform recipient management, assign the `Mail Recipient Creation` role to the management role group that performs recipient creation, and the `Mail Recipients` role to the role group that performs recipient management.

If your organization has enabled Active Directory split permissions, all non-delegating management role assignments to this management role were removed. When Active Directory split permissions is enabled, only Active Directory administrators using Active Directory management tools can create new security principals such as users and security groups.

For more information about RBAC and Active Directory split permissions, see [Understanding split permissions](understanding-split-permissions-exchange-2013-help.md).

## Additional scope considerations

In addition to recipient scopes, the **New-Mailbox** cmdlet, which is included with this role, is also scoped using database configuration scopes. Database configuration scopes control which databases the cmdlet can create new mailboxes on. The database where you want to create a mailbox must be within the database scope. This condition applies when you specify a database using the *Database* parameter on the **New-Mailbox** cmdlet or if you allow automatic mailbox distribution to select the database for you. For more information, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

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
<tr class="even">
<td><p><a href="recipient-management-exchange-2013-help.md">Recipient Management</a></p></td>
<td><p>X</p></td>
<td><p> </p></td>
<td><p><code>Organization</code></p></td>
<td><p><code>Organization</code></p></td>
<td><p><code>OrganizationConfig</code></p></td>
<td><p><code>OrganizationConfig</code></p></td>
</tr>
</tbody>
</table>
