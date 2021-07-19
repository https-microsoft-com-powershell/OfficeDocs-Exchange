---
localization_priority: Normal
description: 'Summary: Learn about the Managed Store in Exchange Server 2016 and 2019'
ms.topic: overview
author: msdmaguire
ms.author: dmaguire
ms.assetid: efdaf80b-335c-491c-8eb5-1fafd297e8a2
ms.reviewer: 
title: Managed Store in Exchange 2016 and Exchange 2019
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Managed Store in Exchange Server

The Managed Store is the name for the Information Store (also known as the Store) processes in Exchange Server 2016 and Exchange Server 2019. Introduced in Exchange Server 2013, the Managed Store uses a controller/worker process model that provides storage process isolation and faster database failover. The Managed Store also uses a static database caching mechanism that replaces the dynamic buffer algorithm in previous versions of Exchange.

The multi-process model that's used by the Managed Store consists of the following processes on the Mailbox server:

- A single store service controller process for the whole Exchange server (Microsoft.Exchange.Store.Service.exe, also known as MSExchangeIS).

- One worker process for each mounted database (Microsoft.Exchange.Store.Worker.exe). When a database is mounted, a new worker process is instantiated that services only that database. When a database is dismounted, the worker process for that database is terminated.

For example, if you have 40 mailbox databases mounted on a Mailbox server, there will be 41 processes running for the Managed Store: one for each database, and one for the store service process controller. The store process controller monitors the health of all store worker processes on the server. A forcible or unexpected termination the Microsoft.Exchange.Store.Service.exe causes an immediate failover of all active database copies on the server.

The Managed Store is also tightly integrated with the Microsoft Exchange Replication service (MSExchangeRepl.exe) and Active Manager. The controller process, worker processes, and Replication service work together to provide greater availability and reliability as described in the following list:

- **Microsoft Exchange Replication service process (MSExchangeRepl.exe)**:

  - Responsible for issuing mount and dismount operations to the Store.

  - Initiates recovery action on storage or database failures reported by the Store, the Extensible Storage Engine (ESE), and Managed Availability responders.

  - Detects unexpected database failures.

  - Provides the administrative interface for management tasks.

- **Store service process/controller (Microsoft.Exchange.Store.Service.exe)**:

  - Manages each worker process lifetime based on the mount and dismount operations received from the Replication service.

  - Handles incoming requests from the Windows Service Control Manager.

  - Logs failure items when store worker process problems detected (for example, hang or unexpected exit).

  - Terminates store worker processes in response failover event.

- **Store worker process (Microsoft.Exchange.Store.Worker.exe)**

  - Responsible for executing RPC operations for mailboxes on a database.

  - RPC endpoint instance within worker process is the database GUID.

  - Provides database cache for a database.

## Static database caching algorithm

The Managed Store uses a very simple and straightforward algorithm for determining database cache as compared to *dynamic buffer allocation* that was used in the previous versions of Exchange. The memory that's allocated for each database cache (that is, each store worker process) is based on number of local database copies and configured value of the *MaximumActiveDatabases* parameter on the **Set-MailboxServer** cmdlet (the default value is $null or blank). If the value of *MaximumActiveDatabases* is greater than number of current database copies, then the cache calculation is based on the number of database copies.

The static algorithm allocates memory for the ESE cache of each store worker process based on the amount of physical RAM that's installed in the server. This is referred to the *Max Cache Target* of the database. 25% of total server memory is allocated to the ESE cache, and is referred to as the *Server Cache Size Target*.

> [!NOTE]
> You can override the Server Cache Size Target, and therefore the amount of memory allocated to the Store for ESE cache by using `msExchESEParamCacheSizeMax` attribute of the *InformationStore* object in Active Directory (the value configured is the number of 32 KB pages to allocate across all store processes).

A static amount of this cache is allocated to active and passive copies. The store worker process will be allocated the Max Cache Target only when servicing an active database copy. Passive database copies are allocated 20 percent of the Max Cache Target. The remainder is reserved by the Store, and allocated to the worker process if the database transitions from passive to active.

Max Cache Target is calculated only at Store startup. Therefore, if you add or remove databases or database copies, you must restart the Store controller service (MSExchangeIS) so that the cache can be adjusted accordingly. If the service is not restarted, new databases will have a smaller cache size target than databases that existed before the last service startup. In this scenario, the sum of database cache size targets will likely exceed the Server Cache Size Target until MSExchangeIS is restarted.

## Example database cache calculations

Here are example database caching calculations that are based on a Mailbox server's memory and database configuration.

### Example 1

Mailbox server configuration:

- 48 GB of memory

- Two active databases and two passive databases

- *MaximumActiveDatabases* parameter: not configured

The amount of database cache is 3 GB for each active database copy worker process and 0.6 GB for each passive database copy worker process. Here's how these values are calculated:

- **Server Cache Size Target**: 25% of the amount of memory: 48 GB \* 0.25 = **12 GB**.

- **Database Max Cache Target**: Divide the Server Cache Size Target by the total number of active and passive databases: 12 GB / 4 databases = **3 GB**.

- **Memory used for passive database copies**: 20% of the Database Max Cache Target: 3 GB \* 0.20 = **0.6 GB**.

Of the 12 GB of memory that's assigned to the Server Cache Size Target:

- 7.2 GB will be in use by database worker processes.

- 4.8 GB will be reserved by the Information Store for the two passive database copies in case they become active copies. If this happens, they will use their Max Cache Target of 3 GB.

### Example 2

Mailbox server configuration:

- 48 GB of memory

- Two active databases and two passive databases

- *MaximumActiveDatabases* parameter: 2

The amount of database cache is 5 GB for each active database copy worker process and 0.2 GB for each passive database copy worker process. Here's how these values are calculated:

- **Server Cache Size Target**: 25% of the amount of memory: 48 GB \* 0.25 = **12 GB**.

- **Database Max Cache Target**: Divide the Server Cache Size Target by the sum of:

  - The number of active databases

  - 20% of the number of passive databases

  12 GB / (2A + (2P \* 0.20)) = **5 GB**

- **Memory used for passive database copies**: 20% of the Database Max Cache Target: 5 GB \* 0.20 = **1 GB**.

Out of the 12 GB of memory assigned to the Server Cache Size Target:

- 12 GB will be in use by database worker processes

- No memory will be reserved by the Information Store for the two passive database copies because they cannot become active copies (*MaximumActiveDatabases* is configured with a value of 2, and there are already 2 active database copies on the server).
