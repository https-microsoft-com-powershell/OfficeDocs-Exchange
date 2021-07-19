---
localization_priority: Normal
description: 'Summary: Admins can learn how deleted items in mailboxes are protected in Exchange Online.'
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: efc48fb4-2ed8-4d05-93af-f3505fbc389d
ms.reviewer:
f1.keywords:
- NOCSH
title: Recoverable Items folder in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars
---

# Recoverable Items folder in Exchange Online

> [!IMPORTANT]
> Please refer to the [Microsoft 365 security center](https://security.microsoft.com/homepage) and the [Microsoft 365 compliance center](https://compliance.microsoft.com/homepage) for Exchange security and compliance features. They are no longer available in the new [Exchange Admin Center](https://admin.exchange.microsoft.com).

To protect from accidental or malicious deletion and to facilitate discovery efforts commonly undertaken before or during litigation or investigations, Exchange Online uses the Recoverable Items folder. The Recoverable Items folder replaces the feature that was known as *the dumpster* in earlier versions of Exchange. The following Exchange features use the Recoverable Items folder:

- Deleted item retention

- Single item recovery

- In-Place Hold

- Litigation Hold

- eDiscovery hold

- Microsoft 365 and Office 365 retention policies

- Mailbox audit logging

- Calendar logging

## Terminology

Knowledge of the following terms will help you understand the content in this article.

 **Delete**: Describes when an item is deleted from any folder and placed in the Deleted Items default folder.

 **Soft delete**: Describes when an item is deleted from the Deleted Items default folder and placed in the Recoverable Items folder. Also describes when an Outlook user deletes an item by pressing Shift+Delete, which bypasses the Deleted Items folder and places the item directly in the Recoverable Items folder.

 **Hard delete**: Describes when an item is marked to be purged from the mailbox database. This is also known as a *store hard delete*.

## Recoverable Items folder

Each user mailbox is divided into two subtrees: the IPM (interpersonal messaging) subtree, which contains the normal, visible folders such as Inbox, Calendar, and Sent Items and the non-IPM subtree, which contains internal data, preferences, and other operational data about the mailbox. The Recoverable Items folder resides in the non-IPM subtree of each mailbox. This subtree isn't visible to users using Outlook, Outlook on the web (formerly known as Outlook Web App), or other email clients.

This architectural change provides the following key benefits:

- When a mailbox is moved to another mailbox database, the Recoverable Items folder moves with it.

- The Recoverable Items folder is indexed by Exchange Search and can be discovered by using In-Place eDiscovery or Content Search in the Microsoft 365 compliance centers.

- The Recoverable Items folder has its own storage quota.

- Exchange can prevent data from being purged from the Recoverable Items folder.

- Exchange can track edits of certain content.

The Recoverable Items folder contains the following subfolders:

- **Deletions**: This subfolder contains all items deleted from the Deleted Items folder. (In Outlook, a user can soft delete an item by pressing Shift+Delete.) This subfolder is available to users through the Recover Deleted Items feature in Outlook and Outlook on the web.

- **Versions**: If In-Place Hold, Litigation Hold, or a Microsoft 365 or Office 365 retention policy is enabled, this subfolder contains the original copy of the item and also if the item is modified multiple times, a copy of the item before modification is saved. To understand what action is considered as modification, refer the Copy-on-Write section later in this article. This folder isn't visible to end users.

- **Purges**: If either Litigation Hold or single item recovery is enabled, this subfolder contains all items that are hard deleted. This folder isn't visible to end users.

- **Audits**: If mailbox audit logging is enabled for a mailbox, this subfolder contains the audit log entries. To learn more about mailbox audit logging, see [Export mailbox audit logs in Exchange Online](../exchange-auditing-reports/export-mailbox-audit-logs.md).

- **DiscoveryHolds**: If In-Place Hold is enabled or if a Microsoft 365 or Office 365 retention policy is assigned to the mailbox, this subfolder contains all items that meet the hold query parameters and are hard deleted.

- **Calendar Logging**: This subfolder contains calendar changes that occur within a mailbox. This folder isn't available to users.

- **SubstrateHolds**: If In-Place Hold, Litigation Hold, or a Microsoft 365 or Office 365 Teams Chat retention policy is enabled, this subfolder contains the original copy of the Teams message if the message has been modified or deleted. A copy of the item before modification is saved. This folder isn't visible to end users.



The following illustration shows the subfolders in the Recoverable Items folders. It also shows the deleted item retention, single item recovery, and hold workflow processes that are described in the following sections.

![Recoverable Items folder](../../media/ITPro_RecoverableItems.gif)

### Deleted item retention

An item is considered to be soft deleted in the following cases:

- A user deletes an item or empties all items from the Deleted Items folder.

- A user presses Shift+Delete to delete an item from any other mailbox folder.

Soft-deleted items are moved to the Deletions subfolder of the Recoverable Items folder. This provides an additional layer of protection so users can recover deleted items without requiring Help desk intervention. Users can use the Recover Deleted Items feature in Outlook or Outlook on the web to recover a deleted item. Users can also use this feature to permanently delete an item. For more information, see:

- [Recover deleted items in Outlook for Windows](https://support.microsoft.com/office/49e81f3c-c8f4-4426-a0b9-c0fd751d48ce)

- [Recover deleted items or email messages in Outlook on the web](https://support.microsoft.com/office/98b5a90d-4e38-415d-a030-f09a4cd28207)

Items remain in the Deletions subfolder until the deleted item retention period is reached. The default deleted item retention period for Exchange Online is 14 days. You can modify this period for mailboxes up to a maximum of 30 days. In addition to a deleted item retention period, the Recoverable Items folder is also subject to quotas. To learn more, see [Recoverable Items mailbox quotas](#recoverable-items-mailbox-quotas) later in this article.

When the deleted item retention period expires, the item is removed from Exchange Online.

### Single item recovery

If an item is removed from the Deletions subfolder, either by a user purging the item by using the Recover Deleted Items feature or by an automated process such as the Managed Folder Assistant (retention tag set to permanently delete for example), the item is moved to the Purges subfolder, and it can't be recovered by the user. When the Managed Folder Assistant processes the Recoverable Items folder for a mailbox that has single item recovery enabled, any item in the Purges subfolder isn't purged if the deleted item retention period hasn't expired for that item. This means that an admin can still recover the item by using an eDiscovery tool such as In-Place eDiscovery or Content Search.

The following table lists the contents of and actions that can be performed in the Recoverable Items folder if single item recovery is enabled.

****

|State of single item recovery|Recoverable Items folder contains soft-deleted items|Recoverable Items folder contains hard-deleted items|Users can purge items from the Recoverable Items folder|Managed Folder Assistant automatically purges items from the Recoverable Items folder|
|---|---|---|---|---|
|Enabled|Yes|Yes|No|Yes. By default, all items are purged after 14 days, except for calendar items, which are purged after 120 days.|
|Disabled|Yes|No|Yes|Yes. By default, all items are purged after 14 days, except for calendar items, which are purged after 120 days. If the Recoverable Items warning quota is reached before the deleted item retention period elapses, messages are deleted in first in, first out (FIFO) order.|
|

### In-Place Hold and Litigation Hold

In Exchange Online, discovery managers can use In-Place eDiscovery with delegated [Discovery Management](../../permissions-exo/permissions-exo.md#role-groups) role group permissions to perform eDiscovery searches of mailbox content. In Exchange Online, you can use In-Place Hold to preserve mailbox items that match query parameters and protect the items from deletion by users or automated processes. You can also use Litigation Hold to preserve all items in user mailboxes and protect the items from deletion by users or automated processes.

Putting a mailbox on In-Place Hold or Litigation Hold stops the Managed Folder Assistant from automatically purging messages from the DiscoveryHolds, Deletions, and Purges subfolders. Additionally, copy-on-write page protection is also enabled for the mailbox. Copy-on-write page protection creates a copy of the original item before any modifications are written to the Exchange store. After the mailbox is removed from hold, the Managed Folder Assistant resumes automated purging.

> [!NOTE]
> If you put a mailbox on both In-Place Hold and Litigation Hold, Litigation Hold takes preference because this puts the entire mailbox on hold.

The following table lists the contents of and actions that can be performed in the Recoverable Items folder if Litigation Hold is enabled.

****

|State of hold|Recoverable Items folder contains soft-deleted items|Recoverable Items folder contains modified and hard-deleted items|Users can purge items from the Recoverable Items folder|Managed Folder Assistant automatically purges items from the Recoverable Items folder|
|---|---|---|---|---|
|Enabled|Yes|Yes|No|No|
|Disabled|Yes|No|Yes|Yes|
|

To learn more about In-Place eDiscovery, In-Place Hold, and Litigation Hold, see the following articles:

- [In-Place eDiscovery in Exchange Online](../in-place-ediscovery/in-place-ediscovery.md)

- [In-Place Hold and Litigation Hold in Exchange Online](../in-place-and-litigation-holds.md)

### Copy-on-write page protection and modified items

If a user who is placed on In-Place Hold or Litigation Hold modifies specific properties of a mailbox item, a copy of the original mailbox item is created before the changed item is written. The original copy is saved in the Versions subfolder. This process is known as *copy-on-write page protection*. Copy-on-write page protection applies to items residing in any mailbox folder. The Versions subfolder isn't visible to users.

The following table lists the message properties that trigger copy-on-write page protection.

****

|Item type|Properties that trigger copy-on-write page protection|
|---|---|
|Messages (IPM.Note\*) <br/><br/> Posts (IPM.Post\*)|Subject <br/><br/> Body <br/><br/> Attachments <br/><br/> Senders and recipients <br/><br/> Sent and received dates|
|Items other than messages and posts|Any change to a visible property, except the following: <ul><li>Item location (when an item is moved between folders)</li><li>Item status change (read or unread)</li><li>Changes to a retention tag applied to an item</li></ul>|
|Items in the Drafts default folder|None. Items in the Drafts folder are exempt from copy-on-write page protection.|

> [!IMPORTANT]
> Copy-on-write page protection doesn't save a version of the meeting when a meeting organizer receives responses from attendees and the meeting's tracking information is updated. Also, changes to RSS feeds aren't captured by copy-on-write page protection.

When a mailbox is no longer on In-Place Hold or Litigation Hold, copies of modified items stored in the Versions folder are removed.

## Recoverable Items mailbox quotas

When an item is moved to the Recoverable Items folder, its size is deducted from the mailbox quota and added to the size of the Recoverable Items folder. In Exchange Online, the default limits for the Recoverable Items quota are: a soft limit of 20 GB and a hard limit of 30 GB. However, the quotas for the Recoverable Items folder are automatically increased to 90 GB and 100 GB, respectively, when you place a mailbox on Litigation Hold or In-Place Hold or if a Microsoft 365 or Office 365 retention policy is applied to the mailbox. For more information, see [Increase the Recoverable Items quota for mailboxes on hold](/microsoft-365/compliance/increase-the-recoverable-quota-for-mailboxes-on-hold).

If the Recoverable Items folder for a mailbox reaches the Recoverable Items quota, no more items can be stored in the folder. This impacts mailbox functionality in the following ways:

- Mailbox users can't delete items.

- The Managed Folder Assistant can't delete items based on retention tag or managed folder settings.

- For mailboxes that have single item recovery, In-Place Hold or Litigation Hold enabled, the copy-on-write page protection process can't maintain versions of items edited by the user.

- For mailboxes that have mailbox audit logging enabled, no mailbox audit log entries can be saved in the Audits subfolder.

For mailboxes that aren't placed on In-Place Hold or Litigation Hold, the Managed Folder Assistant automatically purges items from the Recoverable Items folder when the deleted item retention period expires. If the folder reaches the Recoverable Items warning quota, the assistant automatically purges items in first-in-first-out order.

If the mailbox is placed on In-Place Hold or Litigation Hold or assigned to a Microsoft 365 or Office 365 retention policy, copy-on-write page protection can't maintain versions of modified items. To maintain versions of modified items, you need to reduce the size of the Recoverable Items folder. For more information, see [Delete items in the Recoverable Items folder of cloud-based mailboxes on hold](/microsoft-365/compliance/delete-items-in-the-recoverable-items-folder-of-mailboxes-on-hold).

## More information

- Copy-on-write is only enabled when a mailbox is on In-Place Hold or Litigation Hold.

- If users need to recover deleted items from the Recoverable Items folder, point them to the following articles:

  - [Recover deleted items in Outlook for Windows](https://support.microsoft.com/office/49e81f3c-c8f4-4426-a0b9-c0fd751d48ce)

  - [Recover deleted items or email in Outlook on the web](https://support.microsoft.com/office/98b5a90d-4e38-415d-a030-f09a4cd28207)
  
- If you need to change the default deleted item retention period for Exchange Online, read the following article:

  - [Change how long permanently deleted items are kept for an Exchange Online mailbox](../../recipients-in-exchange-online/manage-user-mailboxes/change-deleted-item-retention.md)
