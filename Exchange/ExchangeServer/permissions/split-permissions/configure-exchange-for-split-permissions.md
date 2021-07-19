---
title: "Configure Exchange Server for split permissions"
ms.author: dstrome
author: dstrome
manager: serdars
ms.date:
ms.audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms:assetid: 8c74f893-a6f3-4869-8571-3bc0f662cc87
description: "Learn how to configure your organization for the split permissions model in Exchange Server 2016 and Exchange Server2019."
---

# Configure Exchange Server for split permissions

Split permissions enable two separate groups, such as Active Directory administrators and Exchange administrators, to manage their respective services, objects, and attributes. Active Directory administrators manage security principals, such as users, that provide permissions to access an Active Directory forest. Exchange administrators manage the Exchange-related attributes on Active Directory objects and Exchange-specific object creation and management.

Exchange Server 2016 and Exchange Server 2019 offer the following types of split permissions models:

- **RBAC split permissions**: Permissions to create security principals in the Active Directory domain partition are controlled by Role Based Access Control (RBAC). Only those who are members of the appropriate role groups can create security principals.

- **Active Directory split permissions**: Permissions to create security principals in the Active Directory domain partition are completely removed from any Exchange user, service, or server. No option is provided in RBAC to create security principals. Creation of security principals in Active Directory must be performed using Active Directory management tools.

The model that you choose depends on the structure and needs of your organization. Choose the procedure that follows that's applicable to the model you want to configure. We recommend that you use the RBAC split permissions model. The RBAC split permissions model provides significantly more flexibility while providing the same administration separation as Active Directory split permissions.

For more information about shared and split permissions, see [Split permissions in Exchange Server](split-permissions.md).

For more information about management role groups, management roles, and regular and delegating management role assignments, see the following topics:

- [Understanding Role Based Access Control](../../../ExchangeServer2013/understanding-role-based-access-control-exchange-2013-help.md)

- [Understanding management role groups](../../../ExchangeServer2013/understanding-management-role-groups-exchange-2013-help.md)

- [Understanding management roles](../../../ExchangeServer2013/understanding-management-roles-exchange-2013-help.md)

- [Understanding management role assignments](../../../ExchangeServer2013/understanding-management-role-assignments-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Active Directory split permissions" entry in the [Role management permissions](../feature-permissions/rbac-permissions.md) topic.

- The permissions model that you select will be applied to all Exchange 2010 or later servers in your organization.

- To download the latest version of Exchange, see [Updates for Exchange Server](../../new-features/updates.md).

- To open the Exchange Management Shell, see [Open the Exchange Management Shell](/powershell/exchange/exchange-server/open-the-exchange-management-shell).

> [!TIP]
> Having problems? Ask for help in the [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver) forums.

## Switch to RBAC split permissions

After you've switched to RBAC split permissions, only Active Directory administrators will be able to create Active Directory security principals. This means that Exchange administrators won't be able to use the following cmdlets:

- **New-Mailbox**

- **New-MailContact**

- **New-MailUser**

- **New-RemoteMailbox**

- **Remove-Mailbox**

- **Remove-MailContact**

- **Remove-MailUser**

- **Remove-RemoteMailbox**

Exchange administrators will only be able to manage the Exchange attributes on existing Active Directory security principals. However, They will be able to create and manage Exchange-specific objects, such as mail flow rules (also known as transport rules) and distribution groups. For more information, see the "RBAC Split Permissions" section in [Split permissions in Exchange Server](split-permissions.md).

To configure Exchange for split permissions, you must assign the Mail Recipient Creation role and the Security Group Creation and Membership role to a role group that contains members that are Active Directory administrators. You must then remove the assignments between those roles and any role group or universal security group (USG) that contains Exchange administrators.

To configure RBAC split permissions, do the following steps:

1. If your organization is currently configured for Active Directory split permissions, do the following steps:

   1. On the target server, open File Explorer, right-click on the Exchange ISO image file, and then select **Mount**. Note the virtual DVD drive letter that's assigned.

   2. Open a Windows Command Prompt window. For example:

     - Press the Windows key + 'R' to open the **Run** dialog, type cmd.exe, and then press **OK**.

     - Press **Start**. In the **Search** box, type **Command Prompt**, then in the list of results, select **Command Prompt**.

   3. In the Command Prompt window, run the following command to disable Active Directory split permissions:

      ```
      Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareAD /ActiveDirectorySplitPermissions:false
      ```

   4. Restart all Exchange servers in your organization or wait for the Active Directory access token to replicate to all of your Exchange servers.

2. Do the following steps in the Exchange Management Shell:

   1. Create a role group for the Active Directory administrators. In addition to creating the role group, the command creates regular role assignments between the new role group and the Mail Recipient Creation role and the Security Group Creation and Membership role.

      ```
      New-RoleGroup "Active Directory Administrators" -Roles "Mail Recipient Creation", "Security Group Creation and Membership"
      ```

      > [!NOTE]
      > If you want members of this role group to be able to create role assignments, include the Role Management role. You don't have to add this role now. However, if you ever want to assign either the Mail Recipient Creation role or Security Group Creation and Membership role to other role assignees, the Role Management role must be assigned to this new role group. The steps that follow configure the Active Directory Administrators role group as the only role group that can delegate these roles.


   2. Create delegating role assignments between the new role group and the Mail Recipient Creation role and Security Group Creation and Membership role by running the following commands:

      ```
      New-ManagementRoleAssignment -Role "Mail Recipient Creation" -SecurityGroup "Active Directory Administrators" -Delegating
      New-ManagementRoleAssignment -Role "Security Group Creation and Membership" -SecurityGroup "Active Directory Administrators" -Delegating
      ```

   3. Add members to the new role group by running the following command:

      ```
      Add-RoleGroupMember "Active Directory Administrators" -Member <user to add>
      ```

   4. Replace the delegate list on the new role group so that only members of the role group can add or remove members by running the following command:

      ```
      Set-RoleGroup "Active Directory Administrators" -ManagedBy "Active Directory Administrators"
      ```

      > [!IMPORTANT]
      > Members of the Organization Management role group, or those who are assigned the Role Management role, either directly or through another role group or USG, can bypass this delegate security check. If you want to prevent any Exchange administrator from adding himself or herself to the new role group, you must remove the role assignment between the Role Management role and any Exchange administrator and assign it to another role group.

   5. Find all of the regular and delegating role assignments to the Mail Recipient Creation role by running the following command:

      ```
      Get-ManagementRoleAssignment -Role "Mail Recipient Creation" | Format-Table Name, Role, RoleAssigneeName -Auto
      ```

    6. Remove all of the regular and delegating role assignments to the Mail Recipient Creation role that aren't associated with the new role group or any other role groups, USGs, or direct assignments you want to keep by running the following command.

       ```
       Remove-ManagementRoleAssignment <Mail Recipient Creation role assignment to remove>
       ```

       > [!NOTE]
       > If you want to remove all of the regular and delegating role assignments to the Mail Recipient Creation role on any role assignee other than the Active Directory Administrators role group, use the following command. The _WhatIf_ switch lets you see what role assignments will be removed. Remove the _WhatIf_ switch and run the command again to remove the role assignments.

       ```
       Get-ManagementRoleAssignment -Role "Mail Recipient Creation" | Where { $_.RoleAssigneeName -NE "Active Directory Administrators" } | Remove-ManagementRoleAssignment -WhatIf
       ```

    7. Find all of the regular and delegating role assignments to the Security Group Creation and Membership role by running the following command.

       ```
       Get-ManagementRoleAssignment -Role "Security Group Creation and Membership" | Format-Table Name, Role, RoleAssigneeName -Auto
       ```

    8. Remove all of the regular and delegating role assignments to the Security Group Creation and Membership role that aren't associated with the new role group or any other role groups, USGs, or direct assignments you want to keep by running the following command:

       ```
       Remove-ManagementRoleAssignment <Security Group Creation and Membership role assignment to remove>
       ```

       > [!NOTE]
       > You can use the same command in the preceding Note to remove all of the regular and delegating role assignments to the Security Group Creation and Membership role on any role assignee other than the Active Directory Administrators role group, as shown in this example.

       ```
       Get-ManagementRoleAssignment -Role "Security Group Creation and Membership" | Where { $_.RoleAssigneeName -NE "Active Directory Administrators" } | Remove-ManagementRoleAssignment -WhatIf
       ```

For detailed syntax and parameter information, see the following topics:

- [New-RoleGroup](/powershell/module/exchange/new-rolegroup)

- [New-ManagementRoleAssignment](/powershell/module/exchange/new-managementroleassignment)

- [Add-RoleGroupMember](/powershell/module/exchange/add-rolegroupmember)

- [Set-RoleGroup](/powershell/module/exchange/set-rolegroup)

- [Get-ManagementRoleAssignment](/powershell/module/exchange/get-managementroleassignment)

- [Remove-ManagementRoleAssignment](/powershell/module/exchange/remove-managementroleassignment)

## Switch to Active Directory split permissions

You can configure your Exchange organization for Active Directory split permissions. Active Directory split permissions completely remove the permissions that allow Exchange administrators and servers from creating security principals in Active Directory or modifying non-Exchange attributes on those objects. When you are done, only Active Directory administrators will be able to create Active Directory security principals. This means that Exchange administrators won't be able to use the following cmdlets:

- **Add-DistributionGroupMember**

- **New-DistributionGroup**

- **New-Mailbox**

- **New-MailContact**

- **New-MailUser**

- **New-RemoteMailbox**

- **Remove-DistributionGroup**

- **Remove-DistributionGroupMember**

- **Remove-Mailbox**

- **Remove-MailContact**

- **Remove-MailUser**

- **Remove-RemoteMailbox**

- **Update-DistributionGroupMember**

Exchange administrators and servers will only be able to manage the Exchange attributes on existing Active Directory security principals. However, they will be able to create and manage Exchange-specific objects, such as transport rules and Unified Messaging dial plans.

> [!WARNING]
> After you enable Active Directory split permissions, Exchange administrators and servers will no longer be able to create security principals in Active Directory, and they won't be able to manage distribution group membership. These tasks must be performed using Active Directory management tools with the required Active Directory permissions. Before you make this change, you should understand the impact it will have on your administration processes and third-party applications that integrate with Exchange and the RBAC permissions model. <br/><br/> For more information, see the "Active Directory split permissions" section in [Split permissions in Exchange Server](split-permissions.md).

To switch from shared or RBAC split permissions to Active Directory split permissions, do the following steps:

1. On the target server, open File Explorer, right-click on the Exchange ISO image file, and then select **Mount**. Note the virtual DVD drive letter that's assigned.

2. In a Windows Command Prompt window, run the following command to enable Active Directory split permissions:

   ```
   Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareAD /ActiveDirectorySplitPermissions:true
   ```

3. If you have multiple Active Directory domains in your organization, you must either run `Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareDomain` in each child domain that contains Exchange servers or objects or run `Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareAllDomains` from a site that has an Active Directory server from every domain.

4. Restart all Exchange servers in your organization or wait for the Active Directory access token to replicate to all of you Exchange 2013 servers.