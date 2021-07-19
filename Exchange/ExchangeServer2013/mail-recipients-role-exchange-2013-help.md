---
title: 'Mail Recipients role: Exchange 2013 Help'
TOCTitle: Mail Recipients role
ms:assetid: 7cf1f1c7-4f32-4446-ac04-99a7f9df2059
ms:mtpsurl: https://technet.microsoft.com/library/Dd876911(v=EXCHG.150)
ms:contentKeyID: 49289318
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Mail Recipients role

_**Applies to:** Exchange Server 2013_

The `Mail Recipients` management role enables administrators to manage existing mailboxes, mail users, and mail contacts in an organization. This role can't create these recipients. Use the `Mail Recipient Creation` role to create them.

This role type doesn't enable you to manage mail-enabled public folders or distribution groups. Use the following roles to manage these objects:

  - [Mail Enabled Public Folders role](mail-enabled-public-folders-role-exchange-2013-help.md)

  - [Distribution Groups role](distribution-groups-role-exchange-2013-help.md)

If your organization has a split permissions model where recipient creation and management are performed by different groups, assign the `Mail Recipient Creation` role to the group that performs recipient creation and the `Mail Recipients` role to the group that performs recipient management. For more information, see the following topics:

  - [Mail Recipient Creation role](mail-recipient-creation-role-exchange-2013-help.md)

  - [Mail Recipients role](mail-recipients-role-exchange-2013-help.md)

  - [Understanding split permissions](understanding-split-permissions-exchange-2013-help.md)

## Additional scope considerations

In addition to recipient scopes, the **Connect-Mailbox** and **Enable-Mailbox** cmdlets, which are included with this role, are also scoped using database configuration scopes. Database configuration scopes control which databases the cmdlets can create new mailboxes on. The database where you want to create a mailbox must be within the database scope. This condition applies when you specify a database using the *Database* parameter on either cmdlet or if you allow automatic mailbox distribution to select the database for you. For more information, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

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
