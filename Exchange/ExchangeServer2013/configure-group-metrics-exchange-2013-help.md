---
title: 'Configure group metrics: Exchange 2013 Help'
TOCTitle: Configure group metrics
ms:assetid: 76ccd6a7-e2ec-42f4-9ab3-e8cc257ac896
ms:mtpsurl: https://technet.microsoft.com/library/JJ649327(v=EXCHG.150)
ms:contentKeyID: 49318502
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure group metrics

_**Applies to:** Exchange Server 2013_

MailTips that provide information about the size of distribution groups and dynamic distribution groups rely on group metrics data. Group metrics data is generated on designated Mailbox servers. For more information about group metrics, see [Group metrics and MailTips](group-metrics-and-mailtips-exchange-2013-help.md).

You can enable or disable group metrics generation on a Mailbox server.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Group metrics" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- You can only use the Shell to perform this procedure.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the Shell to enable or disable group metrics generation

> [!NOTE]
> By default, group metrics data is generated on any server responsible for generating the offline address book (OAB). These examples are only necessary for organizations that don't use OABs.

To enable or disable group metrics generation on a Mailbox server, run the following command:

```powershell
Set-MailboxServer <ServerIdentity> -ForceGroupMetricsGeneration <$true | $false>
```

This example enables group metrics generation on a Mailbox server named MBX1.

```powershell
Set-MailboxServer MBX1 -ForceGroupMetricsGeneration $true
```

## How do you know this worked?

To verify that you have successfully enabled or disabled group metrics generation in an organization that doesn't use OABs, do the following:

1. Run the following command:

    ```powershell
    Get-MailboxServer <ServerIdentity> | Format-List ForceGroupMetricsGeneration
    ```

2. Verify the setting displayed is the setting you configured.
