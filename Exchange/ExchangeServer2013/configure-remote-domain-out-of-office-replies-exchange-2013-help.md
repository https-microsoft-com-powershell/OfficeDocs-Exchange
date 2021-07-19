---
title: 'Configure remote domain out of office replies: Exchange 2013 Help'
TOCTitle: Configure remote domain out of office replies
ms:assetid: 0c1e56be-7a29-4294-9762-600f9f788741
ms:mtpsurl: https://technet.microsoft.com/library/JJ657713(v=EXCHG.150)
ms:contentKeyID: 49300434
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure remote domain out of office replies

_**Applies to:** Exchange Server 2013_

You can use the Exchange Management Shell to configure the way emails are sent and received through remote domains. The following demonstrates how to use the Exchange Management Shell to configure the way Exchange handles out of office replies.

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes

- You can only use the Shell to perform this procedure.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Remote domains" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the Shell to configure out-of-office replies

You can use the **Set-RemoteDomain** cmdlet to configure the properties of a remote domain.

This example disables out-of-office messages for the remote domain named Contoso.

```powershell
Set-RemoteDomain Contoso -AllowedOOFType None
```

This example allows only external out-of-office messages.

```powershell
Set-RemoteDomain Contoso -AllowedOOFType External
```
