---
title: 'Start and stop the IMAP4 services: Exchange 2013 Help'
TOCTitle: Start and stop the IMAP4 services
ms:assetid: a52db4bd-69a6-47b2-acf3-d9d8571c7a87
ms:mtpsurl: https://technet.microsoft.com/library/Bb124022(v=EXCHG.150)
ms:contentKeyID: 49315251
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Start and stop the IMAP4 services

_**Applies to:** Exchange Server 2013_

By default, the two IMAP4 services, the Microsoft Exchange IMAP4 service and the Microsoft Exchange IMAP4 Backend service, aren't started on computers running Microsoft Exchange Server 2013. You must start these two services to allow your email clients to connect to Exchange using IMAP4. When these services are running, Exchange 2013 accepts unsecured IMAP4 client communications on port 143 and over port 993 using Secure Sockets Layer (SSL).

The Microsoft Exchange IMAP4 service runs on Exchange 2013 computers that are running the Client Access server role. The Microsoft Exchange IMAP4 Backend service runs on the Exchange 2013 computer that's running the Mailbox server role. In environments where the Client Access and Mailbox server roles are running on the same computer, you manage both services on the same computer.

For additional information related to POP3 and IMAP4, see [POP3 and IMAP4 in Exchange Server 2013](pop3-and-imap4-in-exchange-server-2013-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "POP3 and IMAP4 Permissions" section in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the Microsoft Management Console Services snap-in to start or stop the IMAP4 services

To start the IMAP4 services:

1. On the computer running the Client Access server role, click **Start**, point to **Programs**, point to **Administrative Tools**, and then click **Services**. Right-click **Microsoft Exchange IMAP4**, and then click **Start**.

2. On the computer running the Mailbox server role, click **Start**, point to **Programs**, point to **Administrative Tools**, and then click **Services**. Right-click **Microsoft Exchange IMAP4 Backend**, and then click **Start**.

To stop the IMAP4 services:

1. On the computer running the Client Access server role, click **Start**, point to **Programs**, point to **Administrative Tools**, and then click **Services**. Right-click **Microsoft Exchange IMAP4**, and then click **Stop**.

2. On the computer running the Mailbox server role, click **Start**, point to **Programs**, point to **Administrative Tools**, and then click **Services**. Right-click **Microsoft Exchange IMAP4 Backend**, and then click **Stop**.

## Use the Shell to start or stop the IMAP4 services

To start the IMAP4 services:

1. On the computer running the Client Access server role, from the Shell, run the following command to start the Microsoft Exchange IMAP4 service.

   ```powershell
   Start-service msExchangeIMAP4
   ```

2. On the computer running the Mailbox server role, from the Shell, run the following command to start the Microsoft Exchange IMAP4 Backend service.

   ```powershell
   Start-service msExchangeIMAP4BE
   ```

To stop the IMAP4 services:

1. On the computer running the Client Access server role, from the Shell, run the following command to stop the Microsoft Exchange IMAP4 service.

   ```powershell
    Stop-service msExchangeIMAP4
   ```

2. On the computer running the Mailbox server role, from the Shell, run the following command to stop the Microsoft Exchange IMAP4 Backend service.

   ```powershell
   Stop-service msExchangeIMAP4BE
   ```

## Use net start to start or stop the IMAP4 services

To start the IMAP4 services:

1. On the computer running the Client Access server role, at the command prompt, run the following command to start the Microsoft Exchange IMAP4 service.

   ```powershell
   net start msExchangeIMAP4
   ```

2. On the computer running the Mailbox server role, at the command prompt, run the following command to start the Microsoft Exchange IMAP4 Backend service.

   ```powershell
   net start msExchangeIMAP4BE
   ```

To stop the IMAP4 services:

1. On the computer running the Client Access server role, at the command prompt, run the following command to stop the Microsoft Exchange IMAP4 service.

   ```powershell
   Net Stop MSExchangeIMAP4
   ```

2. On the computer running the Mailbox server role, at the command prompt, run the following command to stop the Microsoft Exchange IMAP4 Backend service.

   ```powershell
   Net Stop MSExchangeIMAP4BE
   ```

## How do you know this worked?

1. On the Exchange Client Access server, open Windows Task Manager. On the **Services** tab, the status for **MSExchangeIMAP4** will show as **Running** if the Microsoft Exchange IMAP4 service is running.

2. On the Exchange Mailbox server, open Windows Task Manager. On the **Services** tab, the status for **MSExchangeIMAP4BE** will show as **Running** if the Microsoft Exchange IMAP4 Backend service is running.
