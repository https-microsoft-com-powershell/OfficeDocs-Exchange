---
title: 'Disconnected mailboxes: Exchange 2013 Help'
TOCTitle: Disconnected mailboxes
ms:assetid: 508ebe2b-387d-4867-bdb0-028ef351ce56
ms:mtpsurl: https://technet.microsoft.com/library/Bb232039(v=EXCHG.150)
ms:contentKeyID: 50387716
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Disconnected mailboxes

_**Applies to:** Exchange Server 2013_

Each Microsoft Exchange mailbox consists of an Active Directory user account and the mailbox data stored in the Exchange mailbox database. All configuration data for a mailbox is stored in the Exchange attributes of the Active Directory user object. The mailbox database contains the mail data that's in the mailbox associated with the user account. The following figure shows the components of a mailbox.

**Mailbox components**

![Parts that make up a mailbox](images/Bb201680.5fcb5e6d-656e-42ae-871f-0eef8aea456b(EXCHG.150).gif "Parts that make up a mailbox")

A *disconnected mailbox* is a mailbox object in the mailbox database that isn't associated with an Active Directory user account. There are two types of disconnected mailboxes:

- **Disabled mailboxes**: When a mailbox is disabled or deleted in the Exchange admin center (EAC) or using the **Disable-Mailbox** or **Remove-Mailbox** cmdlet in the Exchange Management Shell, Exchange retains the deleted mailbox in the mailbox database, and switches the mailbox to a disabled state. This is why mailboxes that are either disabled or deleted are referred to as *disabled mailboxes*. The difference is that when you disable a mailbox, the Exchange attributes are removed from the corresponding Active Directory user account, but the user account is retained. When you delete a mailbox, both the Exchange attributes and the Active Directory user account are deleted.

  Disabled and deleted mailboxes are retained in the mailbox database until the deleted mailbox retention period expires, which is 30 days by default. After the retention period expires, the mailbox is permanently deleted (also called *purged*). If a mailbox is deleted using the **Remove-Mailbox** cmdlet, it's also retained for the duration of the retention period.

  > [!IMPORTANT]
  > If a mailbox is deleted using the <STRONG>Remove-Mailbox</STRONG> cmdlet and either the <EM>Permanent</EM> or <EM>StoreMailboxIdentity</EM> parameter, it will be immediately deleted from the mailbox database.

  To identify the disabled mailboxes in your organization, run the following commands in the Shell.

  ```powershell
  $dbs = Get-MailboxDatabase
  $dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisconnectReason -eq "Disabled"} | Format-Table DisplayName,Database,DisconnectDate
  ```

- **Soft-deleted mailboxes**: When a mailbox is moved to a different mailbox database, Exchange doesn't fully delete the mailbox from the source mailbox database when the move is complete. Instead, the mailbox in the source mailbox database is switched to a *soft-deleted* state. Like disabled mailboxes, soft-deleted mailboxes are retained in the source database either until the deleted mailbox retention period expires or until the **Remove-StoreMailbox** cmdlet is used to purge the mailbox.

  Run the following command to identify soft-deleted mailboxes in your organization.

  ```powershell
  $dbs = Get-MailboxDatabase
  $dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisconnectReason -eq "SoftDeleted"} | Format-Table DisplayName,Database,DisconnectDate
  ```

## Working with disabled mailboxes

You can perform several operations on a disabled mailbox before it's purged from the mailbox database:

- Reconnect it to the same user account.

- Connect it to a different user account that isn't mail-enabled, which means the user account doesn't have a mailbox.

- Restore it to a user account that has an existing mailbox. For example, if a user whose mailbox was deleted has a new mailbox, you can restore the user's disabled mailbox to their new mailbox.

- Permanently delete it from the Exchange mailbox database.

## Connecting or restoring a disabled mailbox

Here are scenarios in which you may want to connect or restore a disabled mailbox before the mailbox retention period expires or before it's permanently deleted:

- You disabled a mailbox and now want to reconnect the mailbox to the same Active Directory user account.

- You deleted a mailbox by using the EAC or the [Remove-Mailbox](/powershell/module/exchange/Remove-Mailbox) cmdlet and now want to reconnect the mailbox to a different Active Directory user account.

- You deleted a mailbox and now want to restore the mailbox to an existing mailbox. For example, if a user whose mailbox was deleted has a new mailbox, you can restore the user's disabled mailbox to their new mailbox.

- You want to convert a user mailbox to a linked mailbox associated with a user account that's external to the forest in which your Exchange organization exists. The resource forest scenario is an example of when you would want to associate a mailbox with an external account. In this scenario, user objects in the Exchange forest have mailboxes, but the user objects are disabled for logon. You must associate a mailbox in the Exchange forest with a user account in the external account forest.

There are two ways you can reconnect or restore a disabled mailbox. The first method is to use the EAC or the **Connect-Mailbox** cmdlet to connect a disabled mailbox to a user account. For procedures to reconnect disabled mailboxes, see [Connect a disabled mailbox](connect-a-disabled-mailbox-exchange-2013-help.md).

The second method uses the **New-MailboxRestoreRequest** cmdlet to merge the contents of the disabled mailbox with an existing mailbox. This cmdlet uses the Mailbox Replication Service (MRS) to restore the mailbox. For procedures to restore disabled mailboxes, see [Connect or restore a deleted mailbox](connect-or-restore-a-deleted-mailbox-exchange-2013-help.md).

## Permanently deleting a disabled mailbox

As stated previously, Exchange retains disabled mailboxes in the mailbox database based on the deleted mailbox retention settings configured for that mailbox database. After the specified retention period, a disabled mailbox is purged from the Exchange mailbox database. You can also permanently delete a disabled mailbox and all its message content from the mailbox database by using the **Remove-StoreMailbox** cmdlet. After a disabled mailbox is automatically purged or permanently deleted by an administrator, the data loss is permanent and the mailbox can't be recovered.

For more information, see [Permanently delete a mailbox](permanently-delete-a-mailbox-exchange-2013-help.md).

## Working with disabled archive mailboxes

Archive mailboxes become disconnected when they're disabled. Similar to a disabled primary mailbox, a disconnected archive mailbox can be connected by using the **Connect-Mailbox** cmdlet with the *Archive* parameter.

The primary mailbox and the archive mailbox share the same legacy distinguished name (DN), so you must connect the archive mailbox to the same user mailbox that it was previously connected to. You can't connect the archive mailbox to a different user mailbox.

You can perform two operations on a disconnected archive mailbox:

- **Connect it to an existing primary mailbox**: Like a disconnected primary mailbox, a disconnected archive mailbox is retained in the mailbox database until the deleted mailbox retention period expires, which is 30 days by default. During this time, you can recover the archive mailbox by reconnecting it to the same user account that it was connected to before it was disabled.

  > [!NOTE]
  > If you disable an archive mailbox for a user mailbox and then enable an archive mailbox for that same user, that user mailbox will get a new archive mailbox. While you can use the <STRONG>Connect-Mailbox</STRONG> cmdlet to connect a primary mailbox to a user, you must use the <STRONG>Enable-Mailbox</STRONG> cmdlet to connect a disabled archive mailbox to an existing mailbox.

    For more information, see [Manage In-Place Archives in Exchange 2013](manage-in-place-archives-in-exchange-2013-exchange-2013-help.md).

- **Permanently delete it from the Exchange mailbox database**    Exchange retains disconnected archive mailboxes based on the deleted mailbox retention settings configured for the mailbox database. The default retention period is 30 days. After the specified mailbox retention period, a disconnected archive mailbox is purged from the Exchange mailbox database.

  Like a disabled primary mailbox, you can permanently delete a disabled archive mailbox at any time by using the **Remove-StoreMailbox** cmdlet. For more information, see [Permanently delete a mailbox](permanently-delete-a-mailbox-exchange-2013-help.md).

## Working with soft-deleted mailboxes

A soft-deleted mailbox is created when a mailbox is moved from one Exchange mailbox database to any other mailbox database. Exchange doesn't fully delete the mailbox from the source database after a move in case an error occurs during the move that causes the mailbox on the destination database to fail. You can always restore the source mailbox and try again. Exchange will retain the soft-deleted mailbox for the duration of the mailbox retention period.

You can perform two operations on a soft-deleted mailbox:

- Restore it to an existing mailbox.

- Permanently delete it from the Exchange mailbox database.

The procedures for restoring and permanently deleting a soft-deleted mailbox are similar to those for a disabled mailbox. For more information, see the following topics:

- [Restore a soft-deleted mailbox](restore-a-soft-deleted-mailbox-exchange-2013-help.md)

- [Permanently delete a mailbox](permanently-delete-a-mailbox-exchange-2013-help.md)

## Summary of working with disconnected mailboxes

The following table summarizes the information about disconnected mailboxes, including how the mailbox was disconnected, what happens to the corresponding Active Directory user account when a mailbox is disconnected, and the options and tools you have to connect or restore disconnected mailboxes.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>How mailbox was disabled</th>
<th>Value of <em>DisconnectReason</em> property</th>
<th>Is Active Directory user account retained?</th>
<th>Connect or restore options</th>
<th>Tools</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><ul>
<li><p>The EAC: <strong>Recipients</strong> &gt; <strong>Mailboxes</strong> &gt; <strong>Disable</strong></p></li>
<li><p>The Shell: <strong>Disable-Mailbox</strong> cmdlet</p></li>
</ul></td>
<td><p>Disabled</p></td>
<td><p>Yes</p></td>
<td><p>Connect to same user account</p></td>
<td><ul>
<li><p>The EAC: <strong>Recipients</strong> &gt; <strong>Mailboxes</strong> &gt; <strong>Connect a Mailbox</strong></p></li>
<li><p>The Shell: <strong>Connect-Mailbox</strong> cmdlet</p></li>
</ul></td>
</tr>
<tr class="even">
<td><ul>
<li><p>The EAC: <strong>Recipients</strong> &gt; <strong>Mailboxes</strong> &gt; <strong>Delete</strong></p></li>
<li><p>The Shell: <strong>Remove-Mailbox</strong> cmdlet</p></li>
</ul></td>
<td><p>Disabled</p></td>
<td><p>No</p></td>
<td><ul>
<li><p>Connect to a different user account</p></li>
<li><p>Restore to a different mailbox</p></li>
</ul></td>
<td><ul>
<li><p>The EAC: <strong>Recipients</strong> &gt; <strong>Mailboxes</strong> &gt; <strong>Connect a Mailbox</strong></p></li>
<li><p>The Shell: <strong>Connect-Mailbox</strong> cmdlet</p></li>
<li><p><strong>Enable-Mailbox</strong></p></li>
<li><p>The Shell: <strong>New-MailboxRestore</strong> cmdlet</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Moved to a different mailbox database</p></td>
<td><p>SoftDeleted</p></td>
<td><p>Yes</p></td>
<td><ul>
<li><p>Connect to a different user account</p></li>
<li><p>Restore to a different mailbox</p></li>
</ul></td>
<td><ul>
<li><p>The EAC: <strong>Recipients</strong> &gt; <strong>Mailboxes</strong> &gt; <strong>Connect a Mailbox</strong></p></li>
<li><p>The Shell: <strong>Connect-Mailbox</strong> cmdlet</p></li>
<li><p><strong>Enable-Mailbox</strong></p></li>
<li><p>The Shell: <strong>New-MailboxRestore</strong> cmdlet</p></li>
</ul></td>
</tr>
</tbody>
</table>

## Disconnected mailbox documentation

The following table contains links to topics that will help you manage disconnected mailboxes. This includes managing disconnected user mailboxes, linked mailboxes, resource mailboxes, and shared mailboxes.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Topic</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><a href="disable-or-delete-a-mailbox-exchange-2013-help.md">Disable or delete a mailbox</a></p></td>
<td><p>Learn how to disable or delete mailboxes.</p></td>
</tr>
<tr class="even">
<td><p><a href="connect-a-disabled-mailbox-exchange-2013-help.md">Connect a disabled mailbox</a></p></td>
<td><p>Learn how to connect a disabled mailbox to an existing user account.</p></td>
</tr>
<tr class="odd">
<td><p><a href="connect-or-restore-a-deleted-mailbox-exchange-2013-help.md">Connect or restore a deleted mailbox</a></p></td>
<td><p>Learn how to connect a deleted mailbox to a user account or restore the contents of a deleted mailbox to an existing mailbox.</p></td>
</tr>
<tr class="even">
<td><p><a href="restore-a-soft-deleted-mailbox-exchange-2013-help.md">Restore a soft-deleted mailbox</a></p></td>
<td><p>Learn how to connect a soft-deleted mailbox to a user account or restore a soft-deleted mailbox to an existing mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><a href="manage-mailbox-restore-requests-exchange-2013-help.md">Manage mailbox restore requests</a></p></td>
<td><p>Learn how to manage mailbox restore requests using the Shell.</p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p></p></td>
</tr>
<tr class="odd">
<td><p><a href="permanently-delete-a-mailbox-exchange-2013-help.md">Permanently delete a mailbox</a></p></td>
<td><p>Learn how to permanently delete a mailbox.</p></td>
</tr>
</tbody>
</table>