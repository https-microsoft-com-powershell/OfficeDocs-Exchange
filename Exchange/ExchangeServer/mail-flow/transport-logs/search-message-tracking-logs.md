---
localization_priority: Normal
description: Administrators can learn how to search the message tracking log in Exchange 2016 and Exchange 2019 by using the Get-MessageTrackingLog cmdlet in Exchange PowerShell.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: e1678327-bcd5-42d4-a363-67f33067fe9a
ms.reviewer: 
title: Search message tracking logs
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Search message tracking logs

Message tracking records the message activity as mail flows through the transport pipeline on Mailbox servers and Edge Transport servers. You can use the **Get-MessageTrackingLog** cmdlet in the Exchange Management Shell to search for entries in the message tracking log by using specific search criteria. For example:

- Find out what happened to a message that was sent by a user to a specific recipient.

- Find out if a mail flow rule (also known as a transport rule) acted on a message.

- Find out if a message sent from an Internet sender made it into your Exchange organization.

- Find all messages sent by a specified user during a specified time period.

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Message tracking" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- Searching the message tracking logs requires that the Microsoft Exchange Transport Log Search service is running. If you disable or stop this service, you can't search the message tracking logs or run delivery reports. However, stopping this service does not affect other features in Exchange.

- The field names displayed in the results from the **Get-MessageTrackingLog** cmdlet are similar to the actual field names found in the message tracking log files. The biggest differences are:

  - Dashes are removed from the field names. For example, **internal-message-id** is displayed as `InternalMessageId`.

  - The **date-time** field is displayed as `Timestamp`.

  - The **recipient-address** field is displayed as `Recipients`.

  - The **sender-address** field is displayed as `Sender`.

- The **date-time** field in the message tracking log stores information in Coordinated Universal Time (UTC). However, you need to enter your date-time search criteria for the _Start_ or _End_ parameters in the regional date-time format of the computer that you're using to perform the search.

- You can't copy the message tracking log files from another Exchange server and then search them by using the **Get-MessageTrackingLog** cmdlet. Also, if you manually save an existing message tracking log file, the change in the file's date-time stamp breaks the query logic that Exchange uses to search the message tracking logs.

- In Exchange 2016, the **Get-MessageTrackingLog** cmdlet is able to search the message tracking logs on Exchange 2013 Mailbox servers and Exchange 2010 Hub Transport servers in the same Active Directory site. In Exchange 2019, the **Get-MessageTrackingLog** cmdlet is able to search the message tracking logs on Exchange 2016 and Exchange 2013 Mailbox servers in the same Active Directory site.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver), [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange), or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the Exchange Management Shell to search the message tracking logs

To search the message tracking log entries for specific events, use the following syntax.

```powershell
Get-MessageTrackingLog [-Server <ServerIdentity>] [-ResultSize <Integer> | Unlimited] [-Start <DateTime>] [-End <DateTime>] [-EventId <EventId>] [-InternalMessageId <InternalMessageId>] [-MessageId <MessageId>] [-MessageSubject <Subject>] [-Recipients <RecipientAddress1,RecipientAddress2...>] [-Reference <Reference>] [-Sender <SenderAddress>]
```

To view the 1000 most recent message tracking log entries on the server, run the following command:

```powershell
Get-MessageTrackingLog
```

This example searches the message tracking logs on the local server for all entries from 3/28/2015 8:00 AM to 3/28/2015 5:00 PM for all **FAIL** events where the message sender was pat@contoso.com.

```powershell
Get-MessageTrackingLog -ResultSize Unlimited -Start "3/28/2015 8:00AM" -End "3/28/2015 5:00PM" -EventId "Fail" -Sender "pat@contoso.com"
```

## Use the Exchange Management Shell to control the output of a message tracking log search

Use the following syntax.

```powershell
Get-MessageTrackingLog <SearchFilters> | <Format-Table | Format-List> [<FieldNames>] [<OutputFileOptions>]
```

This example searches the message tracking logs using the following search criteria:

- Return results for the first 1,000 **Send** events.

- Display the results in the list format.

- Display only those field names that begin with `Send` or `Recipient`.

- Write the output to a new file named `D:\Send Search.txt`

```powershell
Get-MessageTrackingLog -EventId Send | Format-List Send*,Recipient* | Set-Content -Path "D:\Send Search.txt"
```

## Use the Exchange Management Shell to search the message tracking logs for message entries on multiple servers

Typically, the value in the **MessageID:** header field remains constant as the message travels throughout the Exchange organization. This property is named **InternetMessageId** in queue viewing utilities, and **MessageId** in the message tracking log viewing utilities. After you have determined the **MessageID:** value of a specific message, you can search for information about that message in the message tracking logs on every Mailbox server in your Exchange organization.

To search all message tracking log entries for a specific message across all Mailbox servers and Exchange 2010 Hub Transport servers, use the following syntax.

```powershell
$Servers = Get-ExchangeServer;  $Servers | where {$_.isHubTransportServer -eq $true -or $_.isMailboxServer -eq $true} | Get-MessageTrackingLog -MessageId <MessageID>  | Select-Object <CommaSeparatedFieldNames>  | Sort-Object -Property <FieldName>
```

This example searches the message tracking logs on all Mailbox servers and Exchange 2010 Hub Transport server by using the following search criteria:

- Find any entries related to a message that has a **MessageID:** value of `<ba18339e-8151-4ff3-aeea-87ccf5fc9796@mailbox01.contoso.com>`. Note that you can omit the angle bracket characters (`<` `>`). If you don't, you need to enclose the entire **MessageID:** value in quotation marks.

- For each entry, display the fields **date-time**, **server-hostname**, **client-hostname**, **source**, **event-id**, and **recipient-address**.

- Sort the results by the **date-time** field.

```powershell
$Servers = Get-ExchangeServer; $Servers | where {$_.isHubTransportServer -eq $true -or $_.isMailboxServer -eq $true} | Get-MessageTrackingLog -MessageId ba18339e-8151-4ff3-aeea-87ccf5fc9796@mailbox01.contoso.com | Select-Object Timestamp,ServerHostname,ClientHostname,Source,EventId,Recipients | Sort-Object -Property Timestamp
```

## Use the EAC to search the message tracking logs

You can use the Delivery Reports for administrators feature in the Exchange admin center (EAC) to search the message tracking logs for information about messages sent by or received by a specific mailbox in your organization. For more information, see [Track messages with delivery reports](track-messages-with-delivery-reports.md).
