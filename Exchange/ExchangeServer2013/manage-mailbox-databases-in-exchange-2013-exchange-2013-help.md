---
title: 'Manage mailbox databases in Exchange 2013: Exchange 2013 Help'
description: 'Summary: This article describes the management of mailbox databases in Exchange 2013.'
TOCTitle: Manage mailbox databases in Exchange 2013
ms:assetid: ead4a96b-1717-435b-bcfc-9901ac4e3b58
ms:mtpsurl: https://technet.microsoft.com/library/JJ150580(v=EXCHG.150)
ms:contentKeyID: 47560127
ms.reviewer: 
manager: serdars
ms.author: serdars
author: serdars
audience: ITPro
ms.topic: article
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage mailbox databases in Exchange 2013

_**Applies to:** Exchange Server 2013_

A mailbox database is a unit of granularity where mailboxes are created and stored. A mailbox database is stored as an Exchange database (.edb) file. In Microsoft Exchange Server 2013, each mailbox database has its own properties that you can configure.

This topic shows you how to perform configuration tasks related to managing your mailbox databases in Microsoft Exchange Server 2013.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes

- You require permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox databases" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Create a mailbox database

## Use the EAC to create a mailbox database

1. From the Exchange admin center, navigate to **Servers**.

2. Select **Databases**, and then click the **+** symbol to create a database.

3. Use the new database wizard to create your database.

## Use the Shell to create a mailbox database

For an example of how to create a mailbox database, see Example 1 in [New-MailboxDatabase](/powershell/module/exchange/New-MailboxDatabase).

## How do you know this process worked?

To verify that you have successfully created a database, implement the following tasks:

- From the EAC, verify that the mailbox database you created is listed in the **Databases** page.

- From the Shell, verify that the database was created on server Mailbox01 by running the following command.

    ```powershell
    Get-MailboxDatabase -Server "Mailbox01"
    ```

## Get mailbox database properties

For detailed syntax and parameter information, see [Get-MailboxDatabase](/powershell/module/exchange/Get-MailboxDatabase).

## Use the Shell to get mailbox database properties

For an example of how to get mailbox database properties, see Example 2 in [New-MailboxDatabase](/powershell/module/exchange/New-MailboxDatabase).

## How do you know that this process worked?

To verify that you have successfully retrieved your mailbox database information, implement the following task:

From the Shell, verify that all your mailbox database information is represented correctly.

## Set mailbox database properties

## Use the EAC to set mailbox database properties

1. From the EAC, navigate to **Servers**.

2. Select **Databases**, and then click to select the mailbox database you want to configure.

3. Click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon")to configure the attributes of a mailbox database.

4. Use the **General** tab to view status about the mailbox database, including the mailbox database path, last backup, and mailbox database status:

   - *Database path*: This read-only field displays the full path to the Exchange 2013 database (.edb) file for the selected mailbox database. To view the entire path, you may have to click the path and use the Right Arrow key. You can't use this field to change the path. To change the location of the database files, use the [Move-DatabasePath](/powershell/module/exchange/Move-DatabasePath) cmdlet.

   - *Last full backup*: This read-only field displays the date and time of the last complete backup of the mailbox database.

   - *Last incremental backup*: This read-only field displays the date and time of the last incremental backup of the mailbox database.

   - *Status*: This read-only field displays whether the mailbox database is mounted or dismounted.

   - *Mounted on server*: This read-only field displays which server the database is mounted on.

> [!NOTE]
> This article contains references to the term **master**, a term that Microsoft no longer uses. When the term is removed from the software, we’ll remove it from this article.

   - *Master*: This read-only field displays the primary server for the mailbox database. The Mailbox server that hosts the active copy of a database is referred to as the mailbox database master.

   - *Master type*: This read-only field displays the type of mailbox database master.

   - *Modified*: This read-only field displays the date and time the database was last modified.

   - *Servers hosting a copy of this database*: This read-only field displays the other servers that have a copy of this database.

5. Use the **Maintenance** tab to configure mailbox database settings, including specifying a journal recipient, setting a maintenance schedule, and mounting the database at startup:

   - *Journal Recipient*: Click **Browse** to specify a recipient to enable journaling on this mailbox database. Remove the recipient listed to disable journaling.

   - *Maintenance schedule*: Use this list to select one of the preset maintenance schedules. You can also configure a custom schedule. To configure a custom schedule, click **Customize**.

   - *Enable background database maintenance (24 x 7 ESE scanning)*: Select this check box to enable online database scanning, which runs continuously in the background. Online database scanning performs a checksum calculation of the database and performs operations that allow Exchange to scan for lost space on the database and recover it. If you select this check box, Exchange scans the database no more than one time per day and will issue a warning event if it can't finish scanning the database in a seven-day period.

   - *Don't mount this database at startup*: Select this check box to prevent Exchange from mounting this mailbox database when it starts.

   - *This database can be overwritten by a restore*: Select this check box to allow the mailbox database to be overwritten during a restore process.

   - *Enable circular logging*: Select this check box to enable circular logging.

6. Use the **Limits** tab to specify the storage limits, the warning message interval, and the deletion settings for a mailbox database:

   - *Issue warning at (GB)*: Select this check box to automatically warn mailbox users that their mailbox is approaching its storage limit. To specify the storage limit, select the check box, and then specify in gigabytes (GB) how much content can be stored in the mailbox before a warning email message is sent to the mailbox users. You can enter a value from 0 through 2,097,151 megabytes (MB) (2.0 terabytes).

   - *Prohibit send at (GB)*: Select this check box to prevent users from sending new email messages after the size of their mailbox reaches the specified limit. To specify this limit, select the check box, and then type the size of the mailbox in GB at which you want to prohibit the sending of new email messages and notify the user. You can enter a value from 0 through 2,097,151 MB (2.0 terabytes).

   - *Prohibit send and receive at (GB)*: Select this check box to prevent users from sending and receiving email messages after their mailbox size reaches the specified limit. To specify this limit, select the check box, and then type the size of the mailbox in GB at which you want to prohibit the sending and receiving of email messages and notify the user. You can enter a value from 0 through 2,097,151 MB (2.0 terabytes).

   - *Keep deleted items for (days)*: Select this check box to set the number of days that deleted items are retained in a mailbox. You can enter a value from 0 through 24,855 days.

   - *Keep deleted mailboxes for (days)*: Select this check box to set the number of days that deleted mailboxes are retained. You can enter a value from 0 through 24,855 days.

   - *Don't permanently delete items until the database has been backed up*: Select this check box to prevent mailboxes and email messages from being deleted until after the mailbox database has been backed up.

7. Use the **Client Settings** tab to select the offline address book (OAB) for the mailbox:

   *Offline address book*: To select an offline address book, click **Browse**, and then select the offline address book.

## Use the Shell to set mailbox database properties

For an example of how to set mailbox database properties, see Example 1 in [Set-MailboxDatabase](/powershell/module/exchange/Set-MailboxDatabase).

## How do you know that this process has worked?

To verify that you have successfully set the attributes, implement the following tasks:

- Verify that your changes are saved in the EAC.

- From the Shell, run the following command to retrieve mailbox database properties.

  ```powershell
  Get-MailboxDatabase -Identity MailboxDatabase01 -Status | Format-List
  ```

## Move a mailbox database path

For detailed syntax and parameter information, see [Move-DatabasePath](/powershell/module/exchange/Move-DatabasePath).

## Use the Shell to move a mailbox database path

For an example of how to set mailbox database properties, see Example 1 in [Move-DatabasePath](/powershell/module/exchange/Move-DatabasePath).

## How do you know if this process worked?

To verify that you have successfully moved the database path, implement the following steps:

1. From the EAC, select **Servers** \> **Databases**, and then click to select the appropriate mailbox.

2. Click the **pen** symbol and verify that the database path is correct.

## Mount a mailbox database

For detailed syntax and parameter information, see [Mount-Database](/powershell/module/exchange/Mount-Database).

## Use the Shell to mount a mailbox database

For an example of how to mount a mailbox database, see Example 1 in [Mount-Database](/powershell/module/exchange/Mount-Database).

## How do you know if this process has worked?

From the Shell, run the following command to retrieve mailbox database properties for all mailbox databases.

```powershell
Get-MailboxDatabase -IncludePreExchange2013
```

## Dismount a mailbox database

For detailed syntax and parameter information, see [Dismount-Database](/powershell/module/exchange/Dismount-Database).

## Use the Shell to dismount a mailbox database

For an example of how to dismount a mailbox database, see Example 1 in [Dismount-Database](/powershell/module/exchange/Dismount-Database).

## How do you know whether this process worked?

To verify that you have successfully dismounted the database, implement the following steps:

1. From EAC, select **Servers** \> **Databases**, and then click to select the appropriate mailbox.

2. Click the **pen** symbol, and verify that the database status is **Dismounted**.

## Remove a mailbox database

## Use the EAC to remove a mailbox database

1. From the EAC, select **Servers** \> **Databases**, and then click to select the appropriate mailbox.

2. Click **Delete** ![Delete icon](images/Dd298078.14f639f6-61e8-4418-bbfb-0db14de9d2f5(EXCHG.150).gif "Delete icon")to remove the mailbox database.

## Use the Shell to remove a mailbox database

For detailed syntax and parameter information, see [Remove-MailboxDatabase](/powershell/module/exchange/Remove-MailboxDatabase).

1. Run the following command to remove the mailbox database MyDatabase.

    ```powershell
    Remove-MailboxDatabase -Identity "MyDatabase"
    ```

2. When you're prompted about whether you're sure that you want to perform the action, type **Y**.

3. When the dialog box appears stating that the database was removed successfully, note the location of the Exchange 2013 database (.edb) file. If you want to remove this file from the hard drive, you must remove it manually.

## How do you know whether this process has worked?

To verify that you have successfully removed the mailbox database, implement the following tasks:

- From the EAC, select **Servers** \> **Databases**.

- Verify that the mailbox database has been removed.