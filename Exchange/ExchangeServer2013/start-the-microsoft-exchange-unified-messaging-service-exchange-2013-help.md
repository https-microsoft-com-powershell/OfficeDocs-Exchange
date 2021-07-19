---
title: 'Start the Microsoft Exchange Unified Messaging service: Exchange 2013 Help'
TOCTitle: Start the Microsoft Exchange Unified Messaging service
ms:assetid: b54008e6-172e-4435-8516-57cff740e89c
ms:mtpsurl: https://technet.microsoft.com/library/Bb124330(v=EXCHG.150)
ms:contentKeyID: 49315502
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Start the Microsoft Exchange Unified Messaging service

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can use the Services snap-in in Microsoft Management Console (MMC) or cmd.exe at a command prompt to start the Microsoft Exchange Unified Messaging service on a Mailbox server. By default, the Microsoft Exchange Unified Messaging service is started after a Mailbox server is installed. However, there may be times when you have to restart the Microsoft Exchange Unified Messaging service manually, for example, when you've taken the Mailbox server offline and have to bring it back online.

When the Microsoft Exchange Unified Messaging service is started on a Mailbox server, the Mailbox server is available to answer and process incoming UM calls.

For additional management tasks related to Mailbox servers, see [UM services procedures](um-services-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- To perform the following procedures, you must log on to the Mailbox server by using an account that's a member of the local Administrators group.

- Verify that the Mailbox server is installed, either on the same computer as the Client Access server or on a separate computer.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the MMC Services snap-in to start the Microsoft Exchange Unified Messaging service

1. Click **Start**, and then click **Control Panel**.

2. In Control Panel, double-click **Administrative Tools**.

3. In **Administrative Tools**, double-click **Services**.

4. In the **Services** details pane, right-click **Microsoft Exchange Unified Messaging**, and then click **Start**.

## Use a command prompt to start the Microsoft Exchange Unified Messaging service

1. Click **Start**, and then click **Run**.

2. In the **Open** box, type the following command, and then press Enter.

   ```powershell
   net start MSExchangeUM
   ```
