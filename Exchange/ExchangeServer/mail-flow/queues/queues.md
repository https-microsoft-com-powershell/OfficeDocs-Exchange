---
localization_priority: Normal
description: Learn about queues and messages in queues in Exchange 2016 and Exchange 2019
ms.topic: overview
author: msdmaguire
ms.author: dmaguire
ms.assetid: e7ad0ba5-3789-4a2b-9825-6bb1b321609c
ms.reviewer:
title: Queues and messages in queues in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Queues and messages in queues in Exchange Server

A *queue* is a temporary holding location for messages that are waiting to enter the next stage of processing or delivery to a destination. Each queue represents a logical set of messages that the Exchange server processes in a specific order. In Exchange 2016 and Exchange 2019, queues hold messages before, during, and after delivery. Queues exist in the Transport service on Mailbox servers and on Edge Transport servers. Mailbox servers and Edge Transport servers are called *transport servers* throughout this topic.

Like all previous versions of Exchange, a single Extensible Storage Engine (ESE) database is used for queue storage.

You can manage queues and messages in queues by using the Exchange Management Shell and Queue Viewer in the Exchange Toolbox. You can use these interfaces to view the status and contents of queues and detailed message properties. You can also perform actions that modify queues or the messages in queues. For more information, see [Procedures for queues](queue-procedures.md) and [Procedures for messages in queues](message-procedures.md).

## Types of queues

The following types of queues are used in Exchange 2016 and Exchange 2019, which are the same as Exchange 2013:

<br><br>

****

|Queue|Server role|Description|
|---|---|---|
|Delivery queues|Mailbox servers and Edge Transport servers|Holds messages that are being delivered to all internal and external destinations. <p> Delivery queues are dynamically created when they're required, and are automatically deleted when the queue is empty and the expiration time has passed. The queue expiration time is controlled by the _QueueMaxIdleTime_ parameter on the **Set-TransportService** cmdlet. The default value is three minutes. <p> On Edge Transport servers, there's a queue for every unique destination SMTP domain or smart host. <p> On Mailbox servers, there's a queue for every unique destination as indicated by the **NextHopSolutionKey** property. For more information, see the [NextHopSolutionKey](#nexthopsolutionkey) section later in this topic. <p> All messages are transmitted between Exchange 2016 and Exchange 2013 servers by using SMTP. Non-SMTP destinations also use delivery queues if the destination is serviced by a Delivery Agent connector. For more information, see [Delivery Agents and Delivery Agent Connectors](../../../ExchangeServer2013/delivery-agents-and-delivery-agent-connectors-exchange-2013-help.md).|
|Poison message queue|Mailbox servers and Edge Transport servers|Isolates messages that contain errors and are determined to be harmful to Exchange after a server or service failure. The messages may be genuinely harmful in their content and format, or the messages might have been the victims of a poorly written transport agent or a software bug that crashed the Exchange server while it was processing the otherwise valid messages. <p> The poison message queue is typically empty. If the poison message queue contains no messages, then it doesn't appear in the queue management tools. Messages in the poison message queue are never automatically resumed or expired. Messages remain in the poison message queue until they're manually resumed or removed by an administrator. <p> Every Mailbox server or Edge Transport server has only one poison message queue.|
|Shadow queues|Mailbox servers|Shadow queues hold redundant copies of messages while the messages are in transit. For more information, see [Shadow redundancy in Exchange Server](../../mail-flow/transport-high-availability/shadow-redundancy.md).|
|Submission queue|Mailbox servers and Edge Transport servers|Holds messages that have been accepted by the Transport service, but haven't been processed. Messages in the Submission queue are either waiting to be processed, or are actively being processed. <p> On Mailbox servers, messages are received by a Receive connector, the Pickup or Replay directories, or the Mailbox Transport Submission service. On Edge Transport servers, messages are typically received by a Receive connector, but the Pickup and Replay directories are also available. <p> The categorizer retrieves messages from this queue and, among other things, determines the location of the recipient and the route to that location. After categorization, the message is moved to a delivery queue or to the Unreachable queue. For more information about the categorizer and the transport pipeline, see [Mail flow and the transport pipeline](../../mail-flow/mail-flow.md). <p> Every Mailbox server or Edge Transport server has only one Submission queue.|
|Unreachable queue|Mailbox servers and Edge Transport servers|Contains messages that can't be routed to their destinations. Typically, an unreachable destination is caused by configuration changes that have modified the routing path for delivery. Regardless of destination, all messages that have unreachable recipients reside in this queue. <p> Every Mailbox server or Edge Transport server has only one Unreachable queue.|
|

## Queue database files

All the different queues are stored in a single ESE database. By default, this queue database is located on the transport server at `%ExchangeInstallPath%TransportRoles\data\Queue`.

Like any ESE database, the queue database uses log files to accept, track, and maintain data. To enhance performance, all message transactions are written first to log files and memory, and then to the database file. The checkpoint file tracks the transaction log entries that have been committed to the database. During an ordinary shutdown of the Microsoft Exchange Transport service, uncommitted database changes that are found in the transaction logs are committed to the database.

Circular logging is used for the queue database. This means that transaction logs that are older than the current checkpoint are immediately and automatically deleted. Therefore, the transaction logs can't be replayed for queue database recovery from backup.

The following table lists the files that constitute the queue database.

<br><br>

****

|File|Description|
|---|---|
|Mail.que|This queue database file stores all the queued messages.|
|Tmp.edb|This temporary database file is used to verify the queue database schema on startup.|
|Trn\*.log|Transaction logs record all changes to the queue database. Changes to the database are first written to the transaction log and then committed to the database. Trn.log is the current active transaction log file. Trntmp.log is the next provisioned transaction log file that's created in advance. If the existing Trn.log transaction log file reaches its maximum size, Trn.log is renamed to Trn _nnnn_.log, where _nnnn_ is a sequence number. Trntmp.log is then renamed Trn.log and becomes the current active transaction log file.|
|Trn.chk|This checkpoint file tracks the transaction log entries that have been committed to the database. This file is always in the same location as the mail.que file.|
|Trnres00001.jrs <p> Trnres00002.jrs|These reserve transaction log files act as placeholders. They're only used when the hard disk that contains the transaction log runs out of space to stop the queue database cleanly.|
|

Exchange uses *generation tables* for storage and clean-up of messages in the queue database. Instead of processing and deleting individual message records from one large table, the queue database stores messages in time-based tables, and only deletes the entire table after all the messages in the table have been successfully processed. For example, consider the following example:

- All messages queued from 1:00 PM to 2:00 PM, regardless of the queue or destination, are stored in the `1p-2p_msgs` table.

- At 2:00 PM, new messages are stored in the `2p-3p_msgs` table.

- At 4:00 PM, a new table named `4p-5p_msgs` is created. The entire `1p-2p_msgs` table is deleted, but only if all messages in the table have been successfully processed.

This approach of deleting entire messages tables instead of individual messages helps improves the I/O performance of the drive that holds the queue database.

### Options for configuring the queue database

You configure the queue database by adding or modifying keys in the `%ExchangeInstallPath%Bin\EdgeTransport.exe.config` XML application configuration file. This file is associated with the Microsoft Exchange Transport service. Changes you make to the EdgeTransport.exe.config file take effect after you restart the Microsoft Exchange Transport service.

> [!NOTE]
> Any customized per-server Exchange or Internet Information Server settings you make in exExchangeNoVersion XML application configuration files (for example, web.config files or the EdgeTransport.exe.config file) will be overwritten when you install an exExchangeNoVersion Cumulative Update (CU). Make sure that you save this information so that you can easily re-configure your server after the install. You must re-configure these settings after you install an exExchangeNoVersion CU.

The `<appSettings>` section of the EdgeTransport.exe.config file is where you can add new keys or modify existing keys. If a specific key doesn't exist, you can add it manually to change its value.

The keys for the queue database that are available in the EdgeTransport.exe.config file are described in the following table.

<br><br>

****

|Key|Default value|Description|
|---|---|---|
|_QueueDatabaseBatchSize_|40|Specifies the number of database I/O operations that can be grouped together before they're executed. <p> By default, this key doesn't exist in the EdgeTransport.exe.config file.|
|_QueueDatabaseBatchTimeout_|100|Specifies the maximum time in milliseconds that the database will wait for multiple database I/O operations to group before it executes them. The database I/O operations are executed without waiting for any more if the following conditions are true: <ul><li>The number of database I/O operations that's specified by the _QueueDatabaseBatchSize_ key hasn't been reached.</li><li>The time specified by the _QueueDatabaseBatchTimeout_ key has passed.</li></ul> <p> By default, this key doesn't exist in the EdgeTransport.exe.config file.|
|_QueueDatabaseMaxConnections_|4|Specifies the number of ESE database connections that can be open.|
|_QueueDatabaseLoggingBufferSize_|5MB|Specifies the memory that's used to cache the transaction records before they're written to the transaction log file.|
|_QueueDatabaseLoggingFileSize_|5MB|Specifies the maximum size of a transaction log file. When the maximum log file size is reached, a new log file is opened.|
|_QueueDatabaseLoggingPath_|`%ExchangeInstallPath%TransportRoles\data\Queue`|Specifies the default directory for the queue database log files. For instructions on how to change the location of the queue database, see [Change the location of the queue database](relocate-queue-database.md).|
|_QueueDatabaseMaxBackgroundCleanupTasks_|32|Specifies the maximum number of background cleanup work items that can be queued to the database engine thread pool at any time.|
|_QueueDatabaseOnlineDefragEnabled_|True|Enables or disables scheduled online defragmentation of the mail queue database. <p> By default, this key doesn't exist in the EdgeTransport.exe.config file.|
|_QueueDatabaseOnlineDefragSchedule_|`1:00:00` or 1:00 A.M.|Specifies the time of day in 24 hour format to start the online defragmentation of the mail queue database. To specify a value, enter the value as a time span: _hh:mm:ss_, where _h_ = hours, _m_ = minutes, and _s_ = seconds.|
|_QueueDatabaseOnlineDefragTimeToRun_|`3:00:00` or 3 hours|Specifies the length of time the online defragmentation task is allowed to run. Even if the defragmentation task doesn't finish in the time specified, the queue database is left in a consistent state. To specify a value, enter the value as a time span: _hh:mm:ss_, where _h_ = hours, _m_ = minutes, and _s_ = seconds.|
|_QueueDatabasePath_|`%ExchangeInstallPath%TransportRoles\data\Queue`|Specifies the default directory for the queue database files. For instructions on how to change the location of the queue database, see [Change the location of the queue database](relocate-queue-database.md).|
|

## Queue properties

A queue has many properties that describe the purpose and status of the queue. Some queue properties are applied to the queue when the queue is created, and don't change. Other properties contain status, size, time, or other indicators that are updated frequently.

### NextHopSolutionKey

The routing component of the categorizer in the Microsoft Exchange Transport service selects the destination for a message, and this destination is used to create the delivery queue. The destination is stamped on every recipient as the **NextHopSolutionKey** property. Every unique value of the **NextHopSolutionKey** property corresponds to a separate delivery queue.

The **NextHopSolutionKey** property contains the following fields:

- **DeliveryType**: Represents the results of the categorization of the message, and how the Transport service intends to transmit the message to the next hop, which could be the ultimate destination of the message, or an intermediate hop along the way. The Transport service uses a predefined list of values for **DeliveryType**.

  Based on the value of **DeliveryType**, the **NextHopCategory** property is added to the queue:

  - The value `External` indicates the next hop for the queue is outside the Exchange organization.
  - The value `Internal` indicates the next hop for the queue is inside the Exchange organization.

    Note that a message for an external recipient may require one or more internal hops before the message is delivered externally.

- **NextHopDomain**: Uses specific values based on the value of the **DeliveryType** field. For delivery queues, the value of this field is effectively the name of the queue.

     The value of **NextHopDomain** isn't always a domain name. For example, the value could be the name of the target Active Directory site or database availability group (DAG). Think of this field as the *next hop name*.

- **NextHopConnector**: Uses specific values based on the value of the **DeliveryType** field. The value is always expressed as a GUID. If this field isn't used, the value is a GUID with all zeroes.

     The value of **NextHopConnector** isn't always the GUID of a connector. For example, the value could be the GUID of the target Active Directory site or DAG. Think of this field as the *next hop GUID*.

The values of **DeliveryType**, **NextHopCategory**, **NextHopDomain** and **NextHopConnector** are described in the following table.

<br><br>

****

|Delivery Type in Queue Viewer|DeliveryType in the Exchange Management Shell|Description|NextHopCategory|NextHopDomain|NextHopConnector|
|---|---|---|---|---|---|
|**Delivery Agent**|`DeliveryAgent`|The queue holds messages for delivery to recipients in a non-SMTP address space that's serviced by a delivery agent and a Delivery Agent connector. The connector has the local Mailbox server configured as a source server. For more information, see [Delivery Agents and Delivery Agent Connectors](../../../ExchangeServer2013/delivery-agents-and-delivery-agent-connectors-exchange-2013-help.md).|External|This value is the destination address space that's configured on the Delivery Agent connector. For example, `MOBILE`.|This value is the GUID of the Delivery Agent connector. For example, `4520e633-d83d-411a-bbe4-6a84648674ee`.|
|**DnsConnectorDelivery**|`DnsConnectorDelivery`|The queue holds messages for delivery to recipients in an SMTP domain. The Send connector that services the domain has the local transport server configured as source server, and the Send connector is configured to use DNS routing.|External|This value is the destination address space that's configured on the Send connector. For example, `contoso.com`.|This value is the GUID of the Send connector. For example, `4520e633-d83d-411a-bbe4-6a84648674ee`.|
|**Heartbeat**|`Heartbeat`|This value is reserved for internal Microsoft use. For more information about heartbeat, see [Shadow redundancy in Exchange Server](../../mail-flow/transport-high-availability/shadow-redundancy.md).|n/a|n/a|n/a|
|**MapiDelivery**|`MapiDelivery`|**Note**: This value isn't used by Exchange 2013 or later. It's included for backwards compatibility with Exchange 2010. <p> The queue holds messages for delivery by an Exchange 2010 Hub Transport server to a mailbox on an Exchange 2010 Mailbox server in the local Active Directory site.|n/a|n/a|n/a|
|**NonSmtpGatewayDelivery**|`NonSmtpGatewayDelivery`|The queue holds messages for delivery to recipients in a non-SMTP address space that's serviced by a Foreign connector. The connector has the local Mailbox server configured as a source server. For more information, see [Foreign Connectors](../../../ExchangeServer2013/foreign-connectors-exchange-2013-help.md).|External|This value is the destination address space that's configured on the Foreign connector. For example, `FAX`.|This value is the GUID of the Foreign connector. For example, `4520e633-d83d-411a-bbe4-6a84648674ee`.|
|**Shadow Redundancy**|`ShadowRedundancy`|The queue holds messages in a shadow queue. A shadow queue holds redundant copies messages in transit in case the primary messages aren't successfully delivered. For more information, see [Shadow redundancy in Exchange Server](../../mail-flow/transport-high-availability/shadow-redundancy.md).|Internal|This value is the FQDN of the primary transport server for which the shadow queue is holding redundant copies of the primary messages. For example, `mailbox01.contoso.com`.|This value is `00000000-0000-0000-0000-000000000000`.|
|**SmartHostConnectorDelivery**|`SmartHostConnectorDelivery`|The queue holds messages for delivery to recipients in an SMTP domain. The Send connector that services the domain has the local transport server configured as source server, and the Send connector is configured to use smart host routing.|External|This value is the list of smart hosts that are configured on the Send connector. Smart hosts can be configured as FQDNs, IP addresses or both. The values can be one of the following: <p> **FQDN**: The syntax is `<FQDN1,FQDN2,...>`. For example, `smarthost01.contoso.com` or `smarthost01.contoso.com,smarthost02.fabrikam.com`. <p> **IP address**: The syntax is `<[IPAddress1],[IPAddress2],...>`. For example, `[10.10.10.100]` or `[10.10.10.100],[10.10.10.101]`. <p> **FQDN and IP address**: The syntax is `<[IPAddress1],FQDN1,...>`, and depends on how the smart hosts are listed on the Send connector. For example, `[172.17.17.7],relay.tailspintoys.com` or `mail.contoso.com,[192.168.1.50]`.|This value is the GUID of the Send connector. For example, `4520e633-d83d-411a-bbe4-6a84648674ee`.|
|**SMTP Delivery to Ex Online**|`SmtpDeliveryToExo`|This value isn't used in on-premises Exchange.|n/a|n/a|n/a|
|**SMTP Delivery to Mailbox**|`SmtpDeliveryToMailbox`| The queue holds messages for delivery to Exchange 2013 or later mailbox recipients. The destination mailbox database is in one of the following locations: <ul><li>The local Exchange 2013 or later Mailbox server.</li><li>An Exchange 2019 Mailbox server in the same Exchange 2019 DAG.</li><li>An Exchange 2016 Mailbox server in the same Exchange 2016 DAG.</li><li>An Exchange 2013 Mailbox server in the same Exchange 2013 DAG.</li><li>An Exchange 2013 or later Mailbox server in the same Active Directory site in non-DAG environments.</li></ul>|Internal|This value is the name of the destination mailbox database. For example, `Mailbox Database 0471695037`.|This value is the GUID of the target mailbox database. For example, `6dcb5a1e-0a88-4fc9-b8f9-634c34b1a123`.|
|**SMTP Relay to Send Connector Source Servers**|`SmtpRelayToConnectorSourceServers`|The queue holds messages for delivery to an SMTP or non-SMTP address space that's serviced by a Send connector, Delivery Agent connector, or Foreign connector. The connector has a remote transport server configured as a source server. <p> The remote transport server could be an Exchange 2013 or later Mailbox server or an Exchange 2010 Hub Transport server. <p> The remote transport server could be located in the local Active Directory site, or in a remote Active Directory site.|Internal|This value is the name of the destination Send connector, Delivery Agent connector, or Foreign connector. For example, `Contoso.com Send Connector`.|This value is the GUID of the destination Send connector, Delivery Agent connector, or Foreign connector. For example, `4520e633-d83d-411a-bbe4-6a84648674ee`.|
|**SMTP Relay to Database Availability Group**|`SmtpRelayToDag`|The queue holds messages for delivery to Exchange 2013 or later mailbox recipients, where the destination mailbox database is located in a remote DAG. <p> The remote DAG could be located in the local Active Directory site, or in a remote Active Directory site.|Internal|This value is the name of the destination DAG. For example, `DAG1`.|This value is the GUID of the destination DAG. For example, `6dcb5a1e-0a88-4fc9-b8f9-634c34b1a123`|
|**SMTP Relay to Mailbox Delivery Group**|`SmtpRelayToMailboxDeliveryGroup`|The queue holds messages for delivery to legacy mailbox recipients, where the destination mailbox is on an Exchange 2010 Mailbox server. The message is related to an Exchange 2010 Hub Transport server. <p> The destination Exchange 2010 Hub Transport server could be in the local Active Directory site, or a remote Active Directory site.|Internal|The queue name uses the syntax: `Site:<ADSiteName>;Version:<ExchangeVersion>`, where _\<ADSiteName\>_ is the name of the destination Active Directory site, and _\<ExchangeVersion\>_ is the version of Exchange 2010 on the Mailbox server.|This value is blank.|
|**SMTP Relay to Remote Active Directory Site**|`SmtpRelayToRemoteActiveDirectorySite`| The queue holds messages for delivery to a remote destination, and the routing topology requires the message to be routed through a specific Active Directory site. The site is an intermediate hop on the way to the final destination. This situation occurs under the following circumstances: <p> The message needs to be routed through a hub site. <p> The message requires delivery through a Send connector that's configured on an Edge Transport server that's subscribed to a remote Active Directory site.|Internal|This value is the target Active Directory site name. For example, `NorthAmericaSite`.|This value is the GUID of the target Active Directory site. For example, `bfd6c3df-5b65-8bfb-53f1f2c0d55c`.|
|**SMTP Relay to specified remote forest**|`SmtpRelayToRemoteForest`|This value isn't used in on-premises Exchange|n/a|n/a|n/a|
|**SMTP Relay to Specified Exchange Servers**|`SmtpRelayToServers`|The queue holds messages for delivery to a distribution group that's configured for a specific expansion server. The expansion server could be an Exchange 2013 or later Mailbox server or an Exchange 2010 Hub Transport server. <p> The expansion server could be located in the local Active Directory site, or in a remote Active Directory site.|Internal|This value is the FQDN of the target expansion server. For example, `mailbox01.contoso.com`.|This value is `0000000-0000-0000-0000-000000000000`.|
|**SmtpRelayToTiRg**|`SmtpRelayToTiRg`|**Note**: This value isn't used by Exchange 2013 or later. It's included for backwards compatibility with Exchange 2010. <p> The queue holds messages for delivery by an Exchange 2010 Hub Transport server to an Exchange 2003 routing group.|n/a|n/a|n/a|
|**Smtp Relay in Active Directory Site**|`SmtpRelayWithinAdSite`|**Note**: This value isn't used by Exchange 2013 or later. It's included for backwards compatibility with Exchange 2010. <p> The queue holds messages for delivery by an Exchange 2010 Hub Transport server to another Hub Transport server in the same Active Directory site.|n/a|n/a|n/a|
|**SMTP Relay in Active Directory Site to Edge Transport Server**|`SmtpRelayWithinAdSiteToEdge`|The queue holds messages for delivery to an external SMTP domain that's serviced by a Send connector that's configured on an Edge Transport server. The Edge Transport server is subscribed to the local Active Directory site.|Internal|This value is the name of the Send connector that sends outbound Internet mail from the Edge Transport server to the Internet. This Send connector is automatically created by the Edge subscription, and is named EdgeSync - _\<ADSiteName\>_ to Internet.|This value is the GUID of the Send connector. For example, `4520e633-d83d-411a-bbe4-6a84648674ee`.|
|**Undefined**|`Undefined`|This value is used only on the Submission queue and the poison message queue.|Internal|For the Submission queue, this value is `Submisssion`. For the poison message queue, this value is `Poison Message`.|This value is `00000000-0000-0000-0000-000000000000`.|
|**Unreachable**|`Unreachable`|This value is used only on the Unreachable queue.|Internal|This value is `Unreachable Domain`.|This value is `00000000-0000-0000-0000-000000000000`.|

### IncomingRate, OutgoingRate, and Velocity

Exchange measures the rate of messages entering and leaving a queue and stores these values in queue properties. You can use these rates as an indicator of queue and transport server health. The properties are described in the following table:

<br><br>

****

|Property|Description|
|---|---|
|**IncomingRate**|The rate that messages are entering the queue. The rate is the number of messages per second averaged over the last minute.|
|**OutgoingRate**|The rate that messages are leaving the queue. The rate is the number of messages per second averaged over the last minute.|
|**Velocity**|The drain rate of the queue, calculated by subtracting the value of **IncomingRate** from the value of **OutgoingRate**. <p> If the value is greater than 0, messages are leaving the queue faster than they are entering the queue. <p> If the value equals 0, messages are leaving the queue as fast as they are entering the queue. This is also the value you'll see when the queue is inactive. <p> If the value is less than 0, messages are entering the queue faster than they are leaving the queue. <p> The **Velocity** value is displayed in the results of **Get-Queue**.|
|

At a basic level, a positive value of **Velocity** indicates a healthy queue that's efficiently draining, and a negative value of **Velocity** indicates a queue that isn't efficiently draining. However, you also need to consider the values of **IncomingRate**, **OutgoingRate**, and **MessageCount**, as well as the magnitude of **Velocity**.

For example, consider a queue that has the following property values.

- **Velocity**: -50
- **MessageCount**: 1000
- **OutgoingRate**: 10
- **IncomingRate**: 60

Based on the property values for this queue, the negative value for **Velocity** clearly indicates that the queue isn't draining properly.

Now consider a queue that has the following property values.

- **Velocity**: -0.85
- **MessageCount**: 2
- **OutgoingRate**: 0.15
- **IncomingRate**: 1

Although the value for **Velocity** is negative, it's very close to zero, and the values of the other properties are also very small. Therefore, a negative **Velocity** value for this queue doesn't indicate a problem with the queue.

### Queue status

The current status of a queue is stored in the **Status** property of the queue. A queue can have one of the status values that's described in the following table:

<br><br>

****

|Queue status|Description|
|---|---|
|Active|The queue is actively transmitting messages.|
|Connecting|The queue is in the process of connecting to the next hop.|
|Ready|The queue recently transmitted messages, but the queue is now empty.|
|Retry|The last automatic or manual connection attempt failed, and the queue is waiting to retry the connection.|
|Suspended| The queue has been manually suspended by an administrator to prevent message delivery. New messages can enter the queue, and messages that are in the act of being transmitted to the next hop will finish delivery and leave the queue. Otherwise, messages won't leave the queue until the queue is manually resumed by an administrator. <p> **Notes:** <p> You can suspend the following queues: <ul><li>Delivery queues that have any status.</li><li>The Unreachable queue. When you suspend this queue, messages are no longer automatically resubmitted to the categorizer when configuration updates are detected. To automatically resubmit these messages, you need to manually resume the queue.</li><li>The Submission queue. When you suspend this queue, messages aren't picked up by the categorizer until the queue is resumed.</li></ul> <p> Suspending a queue doesn't change the status of the messages in the queue.|
|

### Other queue properties

There are other queue properties that are self-explanatory. You can use most of the queue properties as filter options. By specifying filter criteria, you can quickly locate queues and take action on them. For a complete description of the filterable queue properties, see [Queue properties](queue-properties.md).

An important queue property that's also worth mentioning here is the **MessageCount** property that shows how many messages are in a queue. This property is an important indicator of queue health. For example, a delivery queue that contains a large number of messages that continues to grow and never decreases could indicate a routing or transport pipeline issue that requires your attention.

## Message properties

A message in a queue has many properties. Many of the properties reflect the information that was used to create the message. Some of the messages status and information properties are heavily influenced by corresponding properties on the queue. However, an individual message may have a different value than the corresponding property of the queue. Other properties contain status, time, or other indicators that are updated frequently.

### Message status

The current status of a message is stored in the **Status** property of the message. A message can have one of the status values that's described in the following table:

<br><br>

****

|Message status|Description|
|---|---|
|Active|If the message is in a delivery queue, the message is being delivered to its destination. If the message is in the Submission queue, the message is being processed by the categorizer.|
|Locked|This value is reserved for internal Microsoft use, and isn't used in on-premises Exchange organizations.|
|PendingRemove|The message was deleted by the administrator, but the message was already in the act of being transmitted to the next hop. The message will be deleted if the delivery ends in an error that causes the message to reenter the queue. Otherwise, delivery will continue.|
|PendingSuspend|The message was suspended by the administrator, but the message was already in the act of being transmitted to the next hop. The message will be suspended if the delivery ends in an error that causes the message to reenter the queue. Otherwise, delivery will continue.|
|Ready|The message is waiting in the queue and is ready to be processed.|
|Retry|The last automatic or manual connection attempt fail for the queue that holds the message. The message is waiting for the next automatic queue connection retry.|
|Suspended|The message was manually suspended by an administrator. <p> Any messages in the poison message queue are in a permanently suspended state.|
|

### Other message properties

There are other message properties that are self-explanatory. You can use most of the message properties as filter options. By specifying filter criteria, you can quickly locate messages and take action on them. For a complete description of the filterable message properties, see [Properties of messages in queues](message-properties.md).

## Manage queues and messages in queues

Queue Viewer and the historical queue and message management cmdlets in the Exchange Management Shell are restricted to a single Exchange server. You can view or operate on individual queues or messages, or multiple queues or messages, but only on a specific server.

The **Get-QueueDigest** cmdlet was introduced in Exchange 2013 to provide a high-level, aggregate view of the state of queues on all servers within a specific scope. The scope could be a DAG, an Active Directory site, a list of servers, or the entire Active Directory forest. Note that queues on a subscribed Edge Transport server in the perimeter network aren't included in the results. Also, **Get-QueueDigest** is available on Edge Transport servers, but the results are restricted to queues on the Edge Transport server.

> [!NOTE]
> By default, the **Get-QueueDigest** cmdlet displays delivery queues that contain ten or more messages, and the results are between one and two minutes old. For instructions on how to change these default values, see [Configure Get-QueueDigest](../../../ExchangeServer2013/configure-get-queuedigest-exchange-2013-help.md).

The following table describes the management tasks you can perform on queues or messages in queues.

<br><br>

****

|Task|Description|Tool to use|Instructions|
|---|---|---|---|
|View and filter queues on a server|Displays one or more queues on a transport server. You can use the results to take action on the queues.|Queue Viewer or the **Get-Queue** cmdlet.|[Procedures for queues](queue-procedures.md)|
|View and filter queues on specific servers in specific DAGs, specific Active Directory sites, or in the whole Active Directory forest.|Displays a summary list of queues.|**Get-QueueDigest** cmdlet|[Procedures for queues](queue-procedures.md)|
|Suspend queues|Temporarily prevent delivery of messages that are currently in the queue. The queue continues to accept new messages, but no messages leave the queue.|Queue Viewer or the **Suspend-Queue** cmdlet.|[Procedures for queues](queue-procedures.md)|
|Resume queues|Reverses the effect of the suspend queue action, and enables delivery of queued messages to resume.|Queue Viewer or the **Resume-Queue** cmdlet.|[Procedures for queues](queue-procedures.md)|
|Retry queues|Immediately tries to connect to the next hop. Without manual intervention, when the connection to the next hop fails, the connection is attempted a specific number of times after a specific time interval between each attempt. <p> Whether the connection attempt is manual or automatic, any connection attempt resets the next retry time. For more information, see [Message retry, resubmit, and expiration intervals](message-intervals.md).|Queue Viewer or the **Retry-Queue** cmdlet.|[Procedures for queues](queue-procedures.md)|
|Resubmit messages in queues|Causes messages in the queue to be resubmitted to the Submission queue and to go back through the categorization process.|**Retry-Queue** with the _Resubmit_ parameter <p> Note that you can use Queue Viewer to resubmit messages, but only from the poison message queue. To resubmit a poison message, you first need to resume the message in Queue Viewer, or by using the **Resume-Message** cmdlet.|[Procedures for queues](queue-procedures.md)|
|Suspend messages in queues|Temporarily prevents delivery of a message. You can use the suspend message action to prevent delivery of a message to all the recipients in a specific queue or to all recipients in all queues.|Queue Viewer or the **Suspend-Message** cmdlet.|[Procedures for messages in queues](message-procedures.md)|
|Resume messages in queues|Reverses the effect of the suspend message action, and enables the delivery of queued messages to resume. You can resume the delivery of a message to all recipients in a specific queue, or to all recipients in all queues.|Queue Viewer or the **Resume-Message** cmdlet.|[Procedures for messages in queues](message-procedures.md)|
|Remove messages from queues|Permanently prevents the delivery of a message. You can prevent the delivery of a message to any recipients in a specific queue, or to all recipients in all queues. Optionally, you can send a non-delivery report (also known as an NDR, delivery status notification, DSN or bounce message) to the sender when the message is removed.|Queue Viewer or the **Remove-Message** cmdlet.|[Procedures for messages in queues](message-procedures.md)|
|Export messages from queues|Copies a message to the location that you specify. The messages aren't deleted from the queue, but a copy of the message is saved as a file in the specified location. This enables administrators or officials in an organization to later examine the messages. Before you export a message, you need to temporarily suspend the message.|**Export-Message** cmdlet only.|[Export messages from queues](export-messages.md)|
|
