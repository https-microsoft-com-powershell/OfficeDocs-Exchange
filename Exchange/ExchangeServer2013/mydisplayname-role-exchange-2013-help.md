---
title: 'MyDisplayName role: Exchange 2013 Help'
TOCTitle: MyDisplayName role
ms:assetid: d78d0032-29af-476b-82e3-69364ca7f12f
ms:mtpsurl: https://technet.microsoft.com/library/Ff461938(v=EXCHG.150)
ms:contentKeyID: 49289426
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# MyDisplayName role

_**Applies to:** Exchange Server 2013_

The `MyDisplayName` management role enables individual users to view and modify their display name. This is a custom role created from the [MyProfileInformation role](myprofileinformation-role-exchange-2013-help.md) parent role.

This role is a specific type of role called a custom role. A custom role is one that's created, or derived, from a parent role. It contains a subset of management role entries that exist on the parent role. This role is provided to enable you to control, with a greater level of granularity, the information you allow end users to modify on their own mailboxes. Unlike other built-in roles, custom roles, including this one, can be deleted. If you won't use this role, it can be deleted.

For more information about built-in and custom management roles and management role entries, see [Understanding management roles](understanding-management-roles-exchange-2013-help.md).

For more information about management roles, management role groups, and other RBAC components, see [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md).

## Default management role assignments

This role doesn't have any default role assignments. It's provided in case you want control, at a more granular level, of what end-user information you allow your users to modify. For more information about assigning this role to a role assignment policy, see the "Adding or Removing Role Assignments" section.
