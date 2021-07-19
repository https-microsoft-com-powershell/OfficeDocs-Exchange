---
localization_priority: Normal
description: "Summary: Learn how administrators can search for and recover deleted email messages in a user's mailbox in Exchange Server 2016 or Exchange Server 2019"
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 9e0e34ce-efc5-454e-8d15-57b4da867f12
ms.reviewer:
title: Recover deleted messages in a user's mailbox in Exchange Server
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Recover deleted messages in a user's mailbox in Exchange Server

Administrators can search for items that are purged (hard-deleted) by a user by using the Recover Deleted Items feature in Outlook or Outlook on the web. They can also search for items deleted by an automated process, such as the retention policy assigned to user mailboxes. In these situations, the purged items can't be recovered by a user. But administrators can recover purged messages if the deleted item retention period for the item hasn't expired.

> [!NOTE]
> In addition to using this procedure to search for and recover deleted items, you can also use this procedure to search for items residing in other folders in the mailbox and to delete items from the source mailbox (also known as *search and purge*).

## What you need to know before you begin?

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- Single item recovery should be enabled for a mailbox before the item you want to recover is deleted. In Exchange Server, single item recovery is disabled when a mailbox is created. For more information, see [Enable or disable single item recovery for a mailbox](single-item-recovery.md).

- To search for and recover items, you need the following information:

  - **Source mailbox**: The mailbox being searched.

  - **Target mailbox**: The discovery mailbox in which messages will be recovered. Exchange Server Setup creates a default discovery mailbox. In Exchange Online, a discovery mailbox is also created by default. If required, you can create additional discovery mailboxes. For details, see [Create a Discovery Mailbox](../../../ExchangeServer2013/create-a-discovery-mailbox-exchange-2013-help.md).

    > [!NOTE]
    > When using the **Search-Mailbox** cmdlet, you can also specify a target mailbox that isn't a discovery mailbox. However, you can't specify the same mailbox as the source and target mailbox.

  - **Search criteria**: Criteria include sender or recipient, or keywords (words or phrases) in the message.

## Step 1: Search for and recover missing items

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Messaging policy and compliance permissions in Exchange Server](../../permissions/feature-permissions/policy-and-compliance-permissions.md) topic.

> [!NOTE]
> You can use In-Place eDiscovery in the Exchange admin center (EAC) to search for missing items. However, when using the EAC, you can't restrict the search to the Recoverable Items folder. All messages matching your search parameters will be returned even if they're not deleted. After they're recovered to the specified discovery mailbox, you may need to review the search results and remove unnecessary messages before recovering the remaining messages to the user's mailbox or exporting them to a .pst file. For details about how to use the EAC to perform an In-Place eDiscovery search, see [Create an In-Place eDiscovery search in Exchange Server](../../policy-and-compliance/ediscovery/create-searches.md).

The first step in the recovery process is to search for messages in the source mailbox. Use one of the following methods to search a user mailbox and copy messages to a discovery mailbox.

### Use the Exchange Management Shell to search for messages

```PowerShell
Get-RecoverableItems -Identity laura@contoso.com -SubjectContains "FY17 Accounting" -FilterItemType IPM.Note -FilterStartTime "2/1/2018 12:00:00 AM" -FilterEndTime "2/5/2018 11:59:59 PM"
```

This example returns all of the available recoverable deleted messages with the specified subject in the mailbox laura@contoso.com for the specified date/time range.

> [!TIP]
> Use the **Get-RecoverableItems** cmdlet to create a search query to find an Outlook item. Once you have a list of results you can use properties like last modified date, item type, etc. to narrow the amount of items restored or to restore a specific item.

For detailed syntax and parameter information, see [Get-RecoverableItems](/powershell/module/exchange/get-recoverableitems?view=exchange-ps).

### How do you know this worked?

To verify that you have successfully searched the messages you want to recover, log on to the discovery mailbox you selected as the target mailbox and review the search results.

## Step 2: Restore recovered items

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Messaging policy and compliance permissions in Exchange Server](../../permissions/feature-permissions/policy-and-compliance-permissions.md) topic.

> [!NOTE]
> You can't use the EAC to restore recovered items.

After messages have been recovered to a discovery mailbox, you can restore them to the user's mailbox by using the **Search-Mailbox** cmdlet. In Exchange Server, you can also use the **New-MailboxExportRequest** and **New-MailboxImportRequest** cmdlets to export the messages to or import the messages from a .pst file.

### Use the Exchange Management Shell to restore messages

```PowerShell
$mailboxes = Import-CSV "C:\My Documents\RestoreMessage.csv"; $mailboxes | foreach {Restore-RecoverableItems -Identity $_.SMTPAddress -SubjectContains Project X" -SourceFolder DeletedItems -FilterItemType IPM.Note}
```

This example restores the deleted email message "Project X" for the mailboxes that are specified in the comma-separated value (CSV) file C:\\My Documents\\RestoreMessage.csv. The CSV file uses the header value SMTPAddress, and contains the email address of each mailbox on a separate line like this:

SMTPAddress

chris@contoso.com

michelle@contoso.com

laura@contoso.com

julia@contoso.com

The first command reads the CSV file to the variable named $mailboxes. The second command restores the specified message from the Deleted Items folder in those mailboxes.

For detailed syntax and parameter information, see [Restore-RecoverableItems](/powershell/module/exchange/restore-recoverableitems?view=exchange-ps).

 **How do you know this worked?**

To verify that you have successfully recovered messages to the user's mailbox, have the user review messages in the target folder you specified in the above command.

### Use the Exchange Management Shell to export and import messages from a .pst file

In Exchange Server, you can export contents from a mailbox to a .pst file and import the contents of a .pst file to a mailbox. To learn more about mailbox import and export, see [Mailbox imports and exports in Exchange Server](../../recipients/mailbox-import-and-export/mailbox-import-and-export.md). You can't perform this task in Exchange Online.

This example uses the following settings to export messages from the folder April Stewart Recovery in the Discovery Search Mailbox to a .pst file:

- **Mailbox**: Discovery Search Mailbox

- **Source folder**: April Stewart Recovery

- **ContentFilter**: April travel plans

- **PST file path**: \\MYSERVER\HelpDeskPst\AprilStewartRecovery.pst

```PowerShell
New-MailboxExportRequest -Mailbox "Discovery Search Mailbox" -SourceRootFolder "April Stewart Recovery" -ContentFilter "Subject -eq 'April travel plans'" -FilePath \\MYSERVER\HelpDeskPst\AprilStewartRecovery.pst
```

For detailed syntax and parameter information, see [New-MailboxExportRequest](/powershell/module/exchange/new-mailboxexportrequest).

This example uses the following settings to import messages from a .pst file to the folder Recovered By Helpdesk in April Stewart's mailbox:

- **Mailbox**: April Stewart

- **Target folder**: Recovered By Helpdesk

- **PST file path**: \\MYSERVER\HelpDeskPst\AprilStewartRecovery.pst

```PowerShell
New-MailboxImportRequest -Mailbox "April Stewart" -TargetRootFolder "Recovered By Helpdesk" -FilePath \\MYSERVER\HelpDeskPst\AprilStewartRecovery.pst
```

For detailed syntax and parameter information, see [New-MailboxImportRequest](/powershell/module/exchange/new-mailboximportrequest).

 **How do you know this worked?**

To verify that you have successfully exported messages to a .pst file, use Outlook to open the .pst file and inspect its contents. To verify that you have successfully imported messages from the .pst file, have the user inspect the contents of the target folder you specified in the above command.

## More information

- The ability to recover deleted items is enabled by *single item recovery*, which lets an administrator recover a message that's been purged by a user or by retention policy as long as the deleted item retention period hasn't expired for that item. To learn more about single item recovery, see [Recoverable Items folder in Exchange Server](../../policy-and-compliance/recoverable-items-folder/recoverable-items-folder.md).

- In Exchange Server, a mailbox database is configured to retain deleted items for 14 days, by default. You can configure deleted item retention settings for a mailbox or mailbox database. For more information, see:

  - [Configure Deleted Item retention and Recoverable Items quotas](deleted-item-retention-and-recoverable-items-quotas.md)

- Users can recover a deleted item if it hasn't been purged and if the deleted item retention period for that item hasn't expired. If users need to recover deleted items from the Recoverable Items folder, point them to the following topics:

  - [Recover deleted items in Outlook for Windows](https://support.microsoft.com/office/49e81f3c-c8f4-4426-a0b9-c0fd751d48ce)

  - [Recover deleted items or email in Outlook on the web](https://support.microsoft.com/office/98b5a90d-4e38-415d-a030-f09a4cd28207)

- This topic shows you how to use the **Search-Mailbox** cmdlet to search for and recover missing items. If you use this cmdlet, you can search only one mailbox at a time. If you want to search multiple mailboxes at the same time, you can use [In-Place eDiscovery in Exchange Server](../../policy-and-compliance/ediscovery/ediscovery.md) in the Exchange admin center (EAC) or the [New-ComplianceSearch](/powershell/module/exchange/new-compliancesearch) cmdlet in Windows PowerShell.

- In addition to using this procedure to search for and recover deleted items, you can also use a similar procedure to search for items in user mailboxes and then delete those items from the source mailbox. For more information, see [Search for and delete messages in Exchange Server](../../policy-and-compliance/ediscovery/delete-messages.md).

## Related article

Are you using Exchange Online? See [Recover deleted messages in a user's mailbox in Exchange Online](../../../ExchangeOnline/recipients-in-exchange-online/manage-user-mailboxes/recover-deleted-messages.md).