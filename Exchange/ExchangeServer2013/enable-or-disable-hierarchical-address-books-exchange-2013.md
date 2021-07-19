---
title: 'Enable or disable hierarchical address books in Exchange Server'
TOCTitle:
ms.assetid: b4c3a175-ce5e-4bfb-a4a0-92d25f3644b3
ms:mtpsurl:
ms:contentKeyID:
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable hierarchical address books in Exchange Server

_**Applies to:** Exchange Server 2013_

The hierarchical address book (HAB) allows users to look for recipients in their address book using an organizational hierarchy. For more information, see [Hierarchical address books](hierarchical-address-books-exchange-2013.md).

The cmdlets and parameters that you use to configure a HAB are described in the following table:

|**Cmdlet**|**Parameter**|**Description**|
|:-----|:-----|:-----|
|[Set-OrganizationConfig](/powershell/module/exchange/set-organizationconfig)|_HierarchicalAddressBookRoot_|Enables or disables the HAB in the organization. <br/><br/> A valid value is a distribution group or mail-enabled security group. You can't use a dynamic distribution group or an Office 35 group.|
|[Set-Group](/powershell/module/exchange/set-group)|_IsHierarchicalGroup_|Specifies whether the distribution group or mail-enabled security group is used in the hierarchy of the HAB. Valid values are `$true` or `$false` (the default value is `$false`).|
|[Set-Contact](/powershell/module/exchange/set-contact) <br/> [Set-Group](/powershell/module/exchange/set-group) <br/> [Set-User](/powershell/module/exchange/set-user)|_SeniorityIndex_ <br/> _PhoneticDisplayName_|_SeniorityIndex_: A numerical value that sorts users, contacts, or groups in descending order in the HAB (higher values are shown before lower values). <br/><br/>  _PhoneticDisplayName_: When multiple users, contacts or groups have the same _SeniorityIndex_ value or the value isn't set, the users, contacts, or groups are listed in ascending alphabetical order. If _PhoneticDisplayName_ isn't configured, the users, contacts, or groups are listed in ascending alphabetical order based on the _DisplayName_ parameter value (which is also the default sort order without the HAB).|

## What do you need to know before you begin?

- Estimated time to complete: 30 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Distribution groups" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- To open the Exchange Management Shell, see [Open the Exchange Management Shell](/powershell/exchange/open-the-exchange-management-shell).

- This topic uses the Exchange Management Shell examples to create distribution groups, but you can also use the Exchange admin center (EAC) to create and add members to distribution groups. For details, see [Manage distribution groups](../ExchangeServer/recipients/distribution-groups.md).

- After you create the HAB, you can use the EAC to manage the membership of the groups in the organizational hierarchy. However, you can only use the Exchange Management Shell to configure the _SeniorityIndex_ parameter for any new groups or users that you create.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Enable and configure a hierarchical address book

### Step 1: Create the distribution groups for the HAB structure

This example uses the following hierarchy:

- The distribution group named "Contoso,Ltd" is the top-level organization in the hierarchy (the root organization).

- Distribution groups named Corporate Office, Product Support Organization, and Sales & Marketing Organization are child organizations under Contoso,Ltd (members of the Contoso,Ltd group).

- The distribution groups named Human Resources, Accounting Group, and Administration Group are child organizations under Corporate Office (members of the Corporate Office group).

```powershell
New-DistributionGroup -Name "Contoso,Ltd" -Alias "ContosoRoot"
```

```powershell
New-DistributionGroup -Name "Corporate Office"
```

```powershell
New-DistributionGroup -Name "Product Support Organization" -Alias ProductSupport
```

```powershell
New-DistributionGroup -Name "Sales & Marketing Organization" -Alias "Sales&Marketing"
```

```powershell
New-DistributionGroup -Name "Human Resources"
```

```powershell
New-DistributionGroup -Name "Accounting Group" -Alias Accounting
```

```powershell
New-DistributionGroup -Name "Administration Group" -Alias Administration
```

**Note**: If you don't use the _Alias_ parameter when you create a distribution group, the value of the _Name_ parameter is used with spaces removed.

For detailed syntax and parameter information, see [New-DistributionGroup](/powershell/module/exchange/new-distributiongroup).

### Step 2: Use the Exchange Management Shell to specify the root organization for the HAB

This example specifies the distribution group named "Contoso,Ltd" from the previous step as the root organization for the HAB.

```powershell
Set-OrganizationConfig -HierarchicalAddressBookRoot "Contoso,Ltd"
```

### Step 3: Use the Exchange Management Shell to designate distribution groups as hierarchical groups

The following examples designate the groups that we previously created as hierarchical groups:

```powershell
Set-Group -Identity "Contoso,Ltd" -IsHierarchicalGroup $true
```

```powershell
Set-Group -Identity "Corporate Office" -IsHierarchicalGroup $true
```

```powershell
Set-Group -Identity "Product Support Organization" -IsHierarchicalGroup $true
```

```powershell
Set-Group -Identity "Sales & Marketing Organization" -IsHierarchicalGroup $true
```

```powershell
Set-Group -Identity "Human Resources" -IsHierarchicalGroup $true
```

```powershell
Set-Group -Identity "Accounting Group" -IsHierarchicalGroup $true
```

```powershell
Set-Group -Identity "Administration Group" -IsHierarchicalGroup $true
```

For detailed syntax and parameter information, see [Set-Group](/powershell/module/exchange/set-group).

### Step 4: Add the child groups as members of the appropriate groups in the hierarchy

This example adds the groups named Corporate Office, Product Support Organization, and Sales & Marketing Organization as members of Contoso,Ltd (the root organization).

```powershell
Update-DistributionGroupMember -Identity "Contoso,Ltd" -Members "Corporate Office","Product Support Organization","Sales & Marketing Organization"
```

This example adds the groups named Human Resources, Accounting Group, and Administration Group as members of Corporate Office.

```powershell
Update-DistributionGroupMember -Identity "Corporate Office" -Members "Human Resources","Accounting Group","Administration Group"
```

For detailed syntax and parameter information, see [Update-DistributionGroupMember](/powershell/module/exchange/update-distributiongroupmember).

### Step 5: Add users to the appropriate groups in the HAB

This example adds the users Amy Alberts, David Hamilton, and Rajesh M. Patel to the group named Corporate Office without affecting other existing members.

```powershell
Update-DistributionGroupMember -Identity "Corporate Office" -Members @{Add="aalberts@contoso.com","dhamilton@contoso.com","rmpatel@contoso.com"}
```

For detailed syntax and parameter information, see [Update-DistributionGroupMember](/powershell/module/exchange/update-distributiongroupmember).

### Step 6: Use the Exchange Management Shell to configure the sort order for groups in the HAB

The _SeniorityIndex_ parameter value for a group affects how the groups are sorted in the HAB (higher values are displayed first).

The following examples configure the child groups of the Corporate Office group to display in the following order:

- Human Resources

- Accounting Group

- Administration Group

```powershell
Set-Group -Identity "Human Resources" -SeniorityIndex 100
```

```powershell
Set-Group -Identity "Accounting Group" -SeniorityIndex 50
```

```powershell
Set-Group -Identity "Administration Group" -SeniorityIndex 25
```

For detailed syntax and parameter information, see [Set-Group](/powershell/module/exchange/set-group).

### Step 7: Use the Exchange Management Shell to configure the sort order for users in the HAB

The _SeniorityIndex_ parameter value for a user affects how the users are sorted in groups in the HAB (higher values are displayed first).

The following examples configure the members of the Corporate Office group to display in the following order:

- David Hamilton

- Rajesh M. Patel

- Amy Alberts

```powershell
Set-User -Identity DHamilton -SeniorityIndex 100
```

```powershell
Set-User -Identity RMPatel -SeniorityIndex 50
```

```powershell
Set-User -Identity AAlberts -SeniorityIndex 25
```

For detailed syntax and parameter information, see [Set-User](/powershell/module/exchange/set-user).

### How do you know this worked?

To verify that you've successfully enabled and configured a hierarchical address book, use any of the following steps:

- Open Outlook in a profile that's connected to a mailbox in your Exchange organization, and click **Address Book** or press Ctrl+Shift+B. The HAB is displayed on the **Organization** tab, similar to the following figure.

   ![Hierarchical Address Book dialog](images/ITPro_Mailbox_HABDisplay.gif)

- In the Exchange Management Shell, run the following commands to verify the property values:

   ```powershell
   Get-OrganizationConfig | Format-List HierarchicalAddressBookRoot
   ```

   ```powershell
   Get-Group -ResultSize unlimited | where {$_.IsHierarchicalGroup -match 'True'} | Format-Table SeniorityIndex,PhoneticDisplayName,DisplayName -Auto
   ```

   ```powershell
   Get-Group -ResultSize unlimited | Format-Table SeniorityIndex,PhoneticDisplayName,DisplayName -Auto
   ```

## Use the Exchange Management Shell to disable a hierarchical address book

To disable a HAB, you don't need to delete the groups that are associated with the HAB structure or reset the _SeniorityIndex_ values for groups or users. Disabling the HAB only prevents the HAB from being displayed in Outlook. To re-enable the HAB with the same configuration settings, you only need to [specify the root organization for the HAB](#step-2-use-the-exchange-management-shell-to-specify-the-root-organization-for-the-hab).

This example disables the hierarchical address book.

```powershell
Set-OrganizationConfig -HierarchicalAddressBookRoot $null
```

### How do you know this worked?

To verify that you've successfully disabled hierarchical address book, use any of the following steps:

- Open Outlook in a profile that's connected to a mailbox in your Exchange organization, and click **Address Book** or press Ctrl+Shift+B. Verify that the entries in the address book are displayed in alphabetical order.

- In the Exchange Management Shell, run the following command to verify that the **HierarchicalAddressBookRoot** property value is blank: