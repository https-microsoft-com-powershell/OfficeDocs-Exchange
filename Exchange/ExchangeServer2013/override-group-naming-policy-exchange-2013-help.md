---
title: 'Override the distribution group naming policy: Exchange 2013 Help'
TOCTitle: Override the distribution group naming policy
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer:
ms.assetid: 9eb23fc9-3f59-4d09-9077-85c89a051ee0
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Override the distribution group naming policy in Exchange 2013

_**Applies to:** Exchange Server 2013_

The group naming policy for distribution groups is applied only to groups created by users. When you or other administrators use the Exchange admin center (EAC) to create distribution groups, the group naming policy is ignored and not applied to the group name.

However, if you use the Exchange Management Shell to create or rename a distribution group, the group naming policy is applied to groups created by administrators unless you use the _IgnoreNamingPolicy_ parameter to override the group naming policy.

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Distribution Groups" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the Shell to override the group naming policy when you create a new group

To override the group naming policy, run the following command.

```powershell
New-DistributionGroup -Name <Group Name> -IgnoreNamingPolicy
```

For example, if the group naming policy for your organization is DG_\<Group Name\>_Users, run the following command to create a group named All Administrators.

```powershell
New-DistributionGroup -Name "All Administrators" -IgnoreNamingPolicy
```

When Microsoft Exchange creates this group, it uses All Administrators for both the _Name_ and _DisplayName_ parameters.

## Use the Shell to override the group naming policy when you rename a group

To override the group naming policy when you rename an existing group with the Shell, run the following command.

```powershell
Set-DistributionGroup -Identity <Old Group Name> -Name <New Group Name> -DisplayName <New Group Name> -IgnoreNamingPolicy
```

For example, let's say you created a group naming policy late one night and the next morning you realized you misspelled the text string in the prefix. The next morning, you see that a new group has already been created with the misspelled prefix. You can fix the group naming policy in the EAC, but you have to use the Shell to rename the group with the misspelled name. Run the following command.

```powershell
Set-DistributionGroup -Identity "Goverment_Contracts_NWRegion" -Name "Government_ContractEstimates_NWRegion" -DisplayName "Government_ContractEstimates_NWRegion" -IgnoreNamingPolicy
```

> [!IMPORTANT]
> Be sure to include the _DisplayName_ parameter when you rename a group. If you don't, the old name is still displayed in the shared address book on the To:, Cc:, and From: lines in email messages.

## How do you know this worked?

To verify that you've successfully created or renamed a distribution group that ignores the group naming policy, run the following commands.

```powershell
Get-DistributionGroup <Name> | Format-List DisplayName
```

```powershell
Get-OrganizationConfig | Format-List DistributionGroupNamingPolicy

```

If the format of the display name for the group is different than the one enforced by your organization's group naming policy, it worked.
