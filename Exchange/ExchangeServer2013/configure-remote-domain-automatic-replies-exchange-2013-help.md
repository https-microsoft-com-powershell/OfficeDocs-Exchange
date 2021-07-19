---
title: 'Configure remote domain automatic replies: Exchange 2013 Help'
TOCTitle: Configure remote domain automatic replies
ms:assetid: 3d88a1fb-4b62-419a-a50d-ffd868e229d0
ms:mtpsurl: https://technet.microsoft.com/library/JJ657720(v=EXCHG.150)
ms:contentKeyID: 49300519
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure remote domain automatic replies

_**Applies to:** Exchange Server 2013_

You can use the Exchange Management Shell to configure the way emails are sent and received through remote domains. The following demonstrates how to use the Exchange Management Shell to configure the way Exchange handles automatic replies.

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes

- You can only use the Shell to perform this procedure.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Remote domain" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the Shell to configure automatic replies

You can use the **Set-RemoteDomain** cmdlet to configure the properties of a remote domain.

This example allows automatic replies to the remote domain named Contoso. This setting is disabled by default.

```powershell
Set-RemoteDomain Contoso -AutoReplyEnabled $true
```

This example allows automatic forwards to the remote domain. This setting is disabled by default.

```powershell
Set-RemoteDomain Contoso -AutoForwardEnabled $true
```
