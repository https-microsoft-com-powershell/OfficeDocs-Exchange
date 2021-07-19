---
title: 'Stop the Microsoft Exchange Unified Messaging Call Router service: Exchange 2013 Help'
TOCTitle: Stop the Microsoft Exchange Unified Messaging Call Router service
ms:assetid: 79935528-1a8c-4f22-826c-8f9a60f4f6f4
ms:mtpsurl: https://technet.microsoft.com/library/JJ673535(v=EXCHG.150)
ms:contentKeyID: 49315446
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Stop the Microsoft Exchange Unified Messaging Call Router service

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can use the Services snap-in in Microsoft Management Console (MMC) or cmd.exe at a command prompt to stop the Microsoft Exchange Unified Messaging Call Router service on a Client Access server. There may be times when you need to stop this service, for example, when you have to take the Client Access server offline. When you stop the Microsoft Exchange Unified Messaging Call Router service, the Client Access server won't be able to accept and process incoming calls.

For additional management tasks related to Client Access servers, see [UM services procedures](um-services-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- To perform the following procedures, you must log on to the Client Access server by using an account that's a member of the local Administrators group.

- Verify that the Client Access server is installed, either on the same computer as the Mailbox server or on a separate computer.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the MMC Services snap-in to stop the Microsoft Exchange Unified Messaging Call Router service

1. Click **Start**, and then click **Control Panel**.

2. In Control Panel, double-click **Administrative Tools**.

3. In **Administrative Tools**, double-click **Services**.

4. In the **Services** details pane, right-click **Microsoft Exchange Unified Messaging Call Router**, and then click **Stop**.

## Use a command prompt to stop the Microsoft Exchange Unified Messaging Call Router service

1. Click **Start**, and then click **Run**.

2. In the **Open** box, type the following command, and then press Enter.

   ```powershell
   net stop MSExchangeUMCR
   ```
