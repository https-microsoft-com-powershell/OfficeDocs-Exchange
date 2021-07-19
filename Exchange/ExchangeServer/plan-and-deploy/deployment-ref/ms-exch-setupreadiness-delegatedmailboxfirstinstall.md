---
localization_priority: Normal
description: Exchange Server 2016 or Exchange Server 2019 Setup can't continue because the account doesn't have permission to install the first Exchange server in the organization.
ms.topic: reference
author: msdmaguire
ms.custom:
- ms.exch.setupreadiness.DelegatedMailboxFirstInstall
ms.author: dmaguire
ms.assetid: d451581b-6161-4e95-99f1-03dac8313fae
ms.reviewer: 
title: Installation of the first Exchange server in the organization can't be delegated [DelegatedMailboxFirstInstall]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: Developer
ms.prod: exchange-server-it-pro
manager: serdars

---

# Installation of the first Exchange server in the organization can't be delegated [DelegatedMailboxFirstInstall]

Exchange Setup can't continue because this is the first Exchange server in the organization, and the first Exchange server needs to be installed by a member of the Enterprise Admins security group (to create the Exchange Organization container and configure objects in it).

**Note**: If you haven't already [extended the Active Directory schema for Exchange](../prepare-ad-and-domains.md#step-1-extend-the-active-directory-schema), you need to do one of the following steps:

- A member of the Schema Admins group can extend the Active Directory schema using another computer in the domain before you install Exchange.

- Exchange Setup can extend the schema if your account is a member of the Schema Admins group.

To resolve this issue, run Exchange setup again using an account that's a member of the Enterprise Admins security group (add the current account or use a different account).

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).
