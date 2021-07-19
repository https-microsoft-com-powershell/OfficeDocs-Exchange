---
title: 'Mailbox Import Export role: Exchange 2013 Help'
TOCTitle: Mailbox Import Export role
ms:assetid: d7cdce7a-6c46-4750-b237-d1c1773e8d28
ms:mtpsurl: https://technet.microsoft.com/library/Ee633482(v=EXCHG.150)
ms:contentKeyID: 49289427
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Mailbox Import Export role

_**Applies to:** Exchange Server 2013_

The `Mailbox Import Export` management role enables administrators to import and export mailbox content and to purge unwanted content from a mailbox.

This management role is one of several built-in roles in the Role Based Access Control (RBAC) permissions model in Microsoft Exchange Server 2013. Management roles, which are assigned to one or more management role groups, management role assignment policies, users, or universal security groups (USG), act as a logical grouping of cmdlets or scripts that are combined to provide access to view or modify the configuration of Exchange 2013 components, such as mailbox databases, transport rules, and recipients. If a cmdlet or script and its parameters, together called a management role entry, are included on a role, that cmdlet or script and its parameters can be run by those assigned the role. For more information about management roles and management role entries, see [Understanding management roles](understanding-management-roles-exchange-2013-help.md).

For more information about management roles, management role groups, and other RBAC components, see [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md).

## Management role assignments

For this role to grant permissions, it must be assigned to a role assignee, which can be a role group, user, or universal security group (USG). This assignment is done using management role assignments. Role assignments link role assignees and roles together. If more than one role is assigned to a role assignee, the role assignee is granted the combination of all the permissions granted by all the assigned roles.

In addition to linking role assignees to roles, role assignments can also apply custom or built-in management scopes. Management scopes control which recipient, server and database objects can be modified by role assignees. If this role is assigned to a role assignee, but a management scope allows the role assignee only to manage certain objects based on a defined scope, the role assignee can only use the permissions granted by this role on those specific objects. The permissions provided by this role can't be applied to objects outside the scope defined on the role assignment. For more information about role assignments and scopes, see the following topics:

  - [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

  - [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

This role is assigned to one or more role groups by default. For more information, see the "Default Management Role Assignments" section later in this topic.

If you want to view a list of role groups, users, or USGs assigned to this role, use the following command.

```powershell
Get-ManagementRoleAssignment -Role "<role name>"
```

## Regular and delegating role assignments

This role can be assigned to role assignees using either regular or delegating role assignments. Regular role assignments grant the permissions provided by the role to the role assignee. Delegating role assignments grant the role assignee the ability to assign the role to other role assignees. For more information about regular and delegating role assignments, see [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md).

## Adding or removing role assignments

You can change which role assignees are assigned this role. By changing which role assignee is assigned this role, you change who is granted its permissions. You can assign this role to other built-in role groups, or you can create role groups and assign this role to them. You can also assign this role to users or USGs. However, we recommend that you limit assignment of roles to users and USGs because such assignments can greatly increase the complexity of your permissions model.

To assign this role to role assignees, the role must be assigned to a role group you're a member of, directly to you, or to a USG you're a member of, using a delegating role assignment. For more information about delegating role assignments, see the "Regular and Delegating Role Assignments" section.

You can also remove this role from built-in role groups, role groups you create, users, and USGs. However, there must always be at least one delegating role assignment between this role and a role group or USG. You can't delete the last delegating role assignment. This limitation helps prevent you from locking yourself out of the system.

> [!IMPORTANT]
> There must be at least one delegating role assignment between this role and a role group or USG. You can't remove the last delegating role assignment associated with this role if the last assignment is to a user.

For more information about how to add or remove assignments between this role and role groups, users, and USGs, see the following topics:

  - [Manage role groups](manage-role-groups-exchange-2013-help.md)

  - [Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md)

  - [Remove a role from a user or USG](remove-a-role-from-a-user-or-usg-exchange-2013-help.md)

## Changing the management scopes on role assignments

You can also change the management scopes on existing role assignments between this role and role assignees. By changing the scopes on role assignments, you control what objects can be managed using the permissions provided by this role. You have several choices when changing the scope on a role assignment. You can do one of the following:

  - Add a new custom scope using the **Set-ManagementRoleAssignment** cmdlet. For more information, see the following topics:

      - [Create a regular or exclusive scope](create-a-regular-or-exclusive-scope-exchange-2013-help.md)

      - [Change a role assignment](change-a-role-assignment-exchange-2013-help.md)

  - Add or change an organizational unit scope using the **Set-ManagementRoleAssignment** cmdlet. For more information, see [Change a role assignment](change-a-role-assignment-exchange-2013-help.md).

  - Add or change a predefined scope using the **Set-ManagementRoleAssignment** cmdlet. For more information, see [Change a role assignment](change-a-role-assignment-exchange-2013-help.md).

  - Change the recipient, server, or database scope on a custom scope associated with a role assignment using the **Set-ManagementScope** cmdlet. For more information, see [Change a role scope](change-a-role-scope-exchange-2013-help.md).

