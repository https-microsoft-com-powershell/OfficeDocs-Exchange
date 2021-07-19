---
title: "Configure Exchange Server for shared permissions"
ms.author: dstrome
author: dstrome
manager: serdars
ms.date:
ms.audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms:assetid: 7d119977-b420-4b66-acf0-0a978b188cdd
description: "Learn how to leave the split permissions model and configure your organization back to the shared permissions model in Exchange Server 2016 and Exchange Server 2019."
---

# Configure Exchange Server for shared permissions

If you've never configured your organization for split permissions, you don't need to perform this procedure. Exchange Server 2016 and Exchange Server 2019 are configured for shared permissions by default.

Shared permissions enable you, as an Exchange administrator, to create Active Directory security principals, such as users, and then configure them as Exchange recipients. Unlike split permissions, which separate management tasks between groups of Exchange administrators and Active Directory administrators, there's no separation of tasks with shared permissions.

For more information about shared and split permissions, see [Split permissions in Exchange Server](split-permissions.md).

You can configure your Exchange organization for shared permissions if you've previously set your organization for split permissions. The procedure to switch to shared permissions is different depending on whether you're currently using Role Based Access Control (RBAC) split permissions or Active Directory split permissions. Choose the procedure that follows that's applicable to your current configuration. If the following are true, your organization is using Active Directory split permissions:

- The Microsoft Exchange Protected Groups organizational unit (OU) exists.

- The Exchange Windows Permissions security group is located in the Microsoft Exchange Protected Groups OU.

- The Exchange Trusted Subsystem security group is a member of the Exchange Windows Permissions security group.

- There are no regular management role assignments to the Mail Recipient Creation role or Security Group Creation and Membership role.

For more information about management role groups, management roles, and regular and delegating management role assignments, see the following topics:

- [Understanding Role Based Access Control](../../../ExchangeServer2013/understanding-role-based-access-control-exchange-2013-help.md)

- [Understanding management role groups](../../../ExchangeServer2013/understanding-management-role-groups-exchange-2013-help.md)

- [Understanding management roles](../../../ExchangeServer2013/understanding-management-roles-exchange-2013-help.md)

- [Understanding management role assignments](../../../ExchangeServer2013/understanding-management-role-assignments-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- The Exchange organization must currently be configured for RBAC or Active Directory split permissions.

- The permissions model that you select will be applied to all Exchange 2010 or later servers in your organization.

- You must have permissions to delegate the Mail Recipient Creation management role and the Security Group Creation and Membership management role to the Organization Management management role group or another role group that's assigned the Mail Recipients role.

- To download the latest version of Exchange on the target computer, see [Updates for Exchange Server](../../new-features/updates.md).

- To open the Exchange Management Shell, see [Open the Exchange Management Shell](/powershell/exchange/open-the-exchange-management-shell).

> [!TIP]
> Having problems? Ask for help in the [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver) forums.

## Switch from RBAC split permissions to shared permissions

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" entry in the [Role management permissions](../feature-permissions/rbac-permissions.md) topic.

To switch from RBAC split permissions to Exchange shared permissions, you must assign the Mail Recipient Creation role and the Security Group Creation and Membership role to a role group that's also assigned the Mail Recipients role and has Exchange administrators as members. In the default shared permissions configuration, the Organization Management role group contains each of these roles. Because of this, the Organization Management role group is in this procedure.

## Configure shared permissions

To configure shared permissions on the Organization Management role group, do the following steps using an account that has permissions to delegate role assignments for the Mail Recipient Creation role and the Security Group Creation and Membership role:

1. Add delegating role assignments for the Mail Recipient Creation role and Security Group Creation and Membership role to the Organization Management role group using the following commands.

   ```powershell
   New-ManagementRoleAssignment -Role "Mail Recipient Creation" -SecurityGroup "Organization Management" -Delegating
   New-ManagementRoleAssignment -Role "Security Group Creation and Membership" -SecurityGroup "Organization Management" -Delegating
   ```

    > [!NOTE]
    > The role group (in this procedure, the Active Directory Administrators role group) that has delegating role assignments for the Mail Recipient Creation role and Security Group Creation and Membership role must be assigned the Role Management role to run the <STRONG>New-ManagementRoleAssignment</STRONG> cmdlet. The role assignee that can delegate the Role Management role must assign that role to the Active Directory Administrators role group.

2. Add regular role assignments for the Mail Recipient Creation role to the Organization Management and Recipient Management role groups using the following commands.

   ```powershell
   New-ManagementRoleAssignment -Role "Mail Recipient Creation" -SecurityGroup "Organization Management"
   New-ManagementRoleAssignment -Role "Security Group Creation and Membership" -SecurityGroup "Recipient Management"
   ```

3. Add a regular role assignment for the Security Group Creation and Membership role to the Organization Management role group using the following command.

   ```powershell
    New-ManagementRoleAssignment -Role "Security Group Creation and Membership" -SecurityGroup "Organization Management"
   ```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](/powershell/module/exchange/new-managementroleassignment).

## Remove permissions from Active Directory administrators (Optional)

You can optionally remove the permissions granted to Active Directory administrators if you no longer want them to be able to create or manage Active Directory objects using the Exchange management tools. If you want to remove permissions from Active Directory administrators, perform this procedure.

> [!NOTE]
> Although you can remove permissions for Active Directory administrators to manage Active Directory objects using the Exchange management tools, Active Directory administrators can continue to manage Active Directory objects using Active Directory management tools, if their Active Directory permissions allow it. They won't, however, be able to manage Exchange-specific attributes on Active Directory objects. For more information, see [Split permissions in Exchange Server](split-permissions.md).

To remove Exchange-related split permissions from Active Directory administrators, do the following steps:

1. Remove the regular and delegating role assignments that assign the Mail Recipient Creation role to the role group or universal security group (USG) that contains the Active Directory administrators as members using the following command. This command uses the Active Directory Administrators role group as an example. The *WhatIf* switch lets you see what role assignments will be removed. Remove the *WhatIf* switch, and run the command again to remove the role assignments.

   ```powershell
   Get-ManagementRoleAssignment -Role "Mail Recipient Creation" | Where { $_.RoleAssigneeName -EQ "Active Directory Administrators" } | Remove-ManagementRoleAssignment -WhatIf
   ```

2. Remove the regular and delegating role assignments that assign the Security Group Creation and Membership role to the role group or USG that contains the Active Directory administrators as members using the following command. This command uses the Active Directory Administrators role group as an example. The *WhatIf* switch lets you see what role assignments will be removed. Remove the *WhatIf* switch, and run the command again to remove the role assignments.

   ```powershell
   Get-ManagementRoleAssignment -Role "Security Group Creation and Membership" | Where { $_.RoleAssigneeName -EQ "Active Directory Administrators" } | Remove-ManagementRoleAssignment -WhatIf
   ```

3. Optional. If you want to remove all Exchange permissions from the Active Directory administrators, you can remove the role group or USG in which they're members. For more information about how to remove a role group, see [Manage role groups](../../../ExchangeServer2013/manage-role-groups-exchange-2013-help.md).

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](/powershell/module/exchange/get-managementroleassignment) or [Remove-ManagementRoleAssignment](/powershell/module/exchange/remove-managementroleassignment).

## Switch from Active Directory split permissions to shared permissions

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Active Directory split permissions" entry in the [Role management permissions](../feature-permissions/rbac-permissions.md) topic.

To switch from Active Directory split permissions to Exchange shared permissions, you must rerun Exchange Setup to disable Active Directory split permissions in the Exchange organization, and then create role assignments between a role group and the Mail Recipient Creation role and Security Group Creation and Membership role. In the default shared permissions configuration, the Organization Management role group contains each of these roles. Because of this, the Organization Management role group is in this procedure.

> [!IMPORTANT]
> The Setup.exe command in this procedure makes changes to Active Directory. You must use an account that has the permissions required to make these changes. This account might not be the same account that has permissions to create role assignments using the **New-ManagementRoleAssignment** cmdlet. Use the account, or accounts, with the permissions necessary to successfully complete each step in this procedure.

To switch from Active Directory split permissions to shared permissions, do the following steps:

1. On the target server, open File Explorer, right-click on the Exchange ISO image file that you downloaded, and then select **Mount**. Note the virtual DVD drive letter that's assigned.
  
2. Open a Windows Command Prompt window. For example:

   - Press the Windows key + 'R' to open the **Run** dialog, type cmd.exe, and then press **OK**.

   - Press **Start**. In the **Search** box, type **Command Prompt**, then in the list of results, select **Command Prompt**.

3. In the Command Prompt window, run the following command:

   ```powershell
   Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareAD /ActiveDirectorySplitPermissions:false
   ```

4. In the Exchange Management Shell, run the following commands to add regular role assignments between the Mail Recipient Creation role and Security Group Creation and Management role and the Organization Management and Recipient Management role groups.

   ```powershell
   New-ManagementRoleAssignment "Mail Recipient Creation_Organization Management" -Role "Mail Recipient Creation" -SecurityGroup "Organization Management"
   New-ManagementRoleAssignment "Security Group Creation and Membership_Org Management" -Role "Security Group Creation and Membership" -SecurityGroup "Organization Management"
   New-ManagementRoleAssignment "Mail Recipient Creation_Recipient Management" -Role "Mail Recipient Creation" -SecurityGroup "Recipient Management"
   ```

5. Restart all Exchange servers in your organization.

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](/powershell/module/exchange/new-managementroleassignment).