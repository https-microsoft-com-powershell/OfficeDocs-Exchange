---
title: 'Public folders: Exchange 2013 Help'
TOCTitle: Public folders
ms:assetid: 94c4fb69-9234-4b34-8c1c-da2a0a11da65
ms:mtpsurl: https://technet.microsoft.com/library/JJ150538(v=EXCHG.150)
ms:contentKeyID: 47560056
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Public folders

_**Applies to:** Exchange Server 2013_

Public folders are designed for shared access and provide an easy and effective way to collect, organize, and share information with other people in your workgroup or organization. Public folders help organize content in a deep hierarchy that's easy to browse. Users will see the full hierarchy in Outlook, which makes it easy for them to browse for the content they're interested in.

> [!NOTE]
> Public folders are available in the following Outlook clients: Outlook Web App for Exchange 2013, Outlook 2007, Outlook 2010, Outlook 2013, and Outlook for Mac.

Public folders can also be used as an archiving method for distribution groups. When you mail-enable a public folder and add it as a member of the distribution group, email sent to the group is automatically added to the public folder for later reference.

> [!NOTE]
> You must use Outlook 2007 or later to access public folders on Exchange 2013 servers.

Public folders aren't designed to do the following:

- **Data archiving** Users who have mailbox limits sometimes use public folders instead of mailboxes to archive data. This practice isn't recommended because it affects storage in public folders and undermines the goal of mailbox limits. Instead, we recommend that you use [In-Place Archiving in Exchange 2013](in-place-archiving-in-exchange-2013-exchange-2013-help.md) as your archiving solution.

- **Document sharing and collaboration** Public folders don't provide versioning or other document management features, such as controlled check-in and check-out functionality and automatic notifications of content changes. Instead, we recommend that you use [SharePoint](/sharepoint/) as your documentation sharing solution.

To learn more about public folders and other collaboration methods in Exchange 2013, see [Collaboration](collaboration-exchange-2013-help.md).

To browse some frequently asked questions about public folders in Exchange 2013, see [FAQ: Public folders](faq-public-folders-exchange-2013-help.md).

For more information about the limits and quotas for public folders, see [Limits for public folders](limits-for-public-folders-exchange-2013-help.md).

For a list of public folder management tasks, see [Public folder procedures](public-folder-procedures-exchange-2013-help.md).

Looking for the Exchange Online version of this topic? See [Public folders in Exchange Online](../ExchangeOnline/collaboration-exo/public-folders/public-folders.md).

## Public folder architecture

In Exchange 2013, public folders were re-engineered using mailbox infrastructure to take advantage of the existing high availability and storage technologies of the mailbox database. Public folder architecture uses specially designed mailboxes to store both the public folder hierarchy and the content. This also means that there's no longer a public folder database. High availability for the public folder mailboxes is provided by a database availability group (DAG). To learn more about DAGs, see [Database availability groups (DAGs)](database-availability-groups-dags-exchange-2013-help.md).

The main architectural components of public folders are the public folder mailboxes, which can reside in one or more mailbox databases.

## Public folder mailboxes

There are two types of public folder mailboxes: the *primary hierarchy mailbox* and *secondary hierarchy mailboxes*. Both types of mailboxes can contain content:

- **Primary hierarchy mailbox**: The primary hierarchy mailbox is the one writable copy of the public folder hierarchy. The public folder hierarchy is copied to all other public folder mailboxes, but these will be read-only copies.

- **Secondary hierarchy mailboxes**: Secondary hierarchy mailboxes contain public folder content as well and a read-only copy of the public folder hierarchy.

> [!NOTE]
> Retention policies aren't supported for public folder mailboxes.

There are two ways you can manage public folder mailboxes:

- In the Exchange admin center (EAC), navigate to **Public folders** \> **Public folder mailboxes**.

- In the Exchange Management Shell, use the **\*-Mailbox** set of cmdlets. The following parameters have been added to the [New-Mailbox](/powershell/module/exchange/New-Mailbox) cmdlet to support public folder mailboxes:

  - *PublicFolder*: This parameter is used with the **New-Mailbox** cmdlet to create a public folder mailbox. When you create a public folder mailbox, a new mailbox is created with the mailbox type of `PublicFolder`. For more information, see [Create a public folder mailbox](../ExchangeOnline/collaboration-exo/public-folders/create-public-folder-mailbox.md).

  - *HoldForMigration*: This parameter is used only if you are migrating public folders from a previous version to Exchange 2013. For more information, see Migrate Public folders from previous versions later in this topic.

  - *IsHierarchyReady*: This parameter indicates whether the public folder mailbox is ready to serve the public folder hierarchy to users. It's set to `$True` only after the entire hierarchy has been synced to the public folder mailbox. If the parameter is set to $False, users won't use it to access the hierarchy. However, if you set the *DefaultPublicFolderMailbox* property on a user mailbox to a specific public folder mailbox, the user will still access the specified public folder mailbox even if the *IsHierarchyReady* parameter is set to `$False`.

  - *IsExcludedFromServingHierarchy*: This parameter prevents users from accessing the public folder hierarchy on the specified public folder mailbox. For load-balancing purposes, users are equally distributed across public folder mailboxes by default. When this parameter is set on a public folder mailbox, that mailbox isn't included in this automatic load balancing and won't be accessed by users to retrieve the public folder hierarchy. However, if you set the *DefaultPublicFolderMailbox* property on a user mailbox to a specific public folder mailbox, the user will still access the specified public folder mailbox even if the *IsExcludedFromServingHierarchy* parameter is set for that public folder mailbox.

A secondary hierarchy mailbox will serve only public folder hierarchy information to users if it's specified explicitly on the users' mailboxes using the *DefaultPublicFolderMailbox* property, or if the following conditions are met:

- The *IsHierarchyReady* property on the public folder mailbox is set to `$True`.

- The *IsExcludedFromServingHierarchy* property on the public folder mailbox is set to `$False`.

## Public folder hierarchy

The public folder hierarchy contains the folders' properties and organizational information, including tree structure. Each public folder mailbox contains a copy of the public folder hierarchy. There's only one writeable copy of the hierarchy, which is in the primary public folder mailbox. For a specific folder, the hierarchy information is used to identify the following:

- Permissions on the folder

- The folder's position in the public folder tree, including its parent and child folders

> [!NOTE]
> The hierarchy doesn't store information about email addresses for mail-enabled public folders. The email addresses are stored on the directory object in Active Directory.

## Hierarchy synchronization

The public folder hierarchy synchronization process uses Incremental Change Synchronization (ICS), which provides a mechanism to monitor and synchronize changes to an Exchange store hierarchy or content. The changes include creating, modifying, and deleting folders and messages. When users are connected to and using content mailboxes, synchronization occurs every 15 minutes. If no users are connected to content mailbox, synchronization will be triggered less often (every 24 hours).If a write operation such as a creating a folder is performed on the primary hierarchy, synchronization is triggered immediately (synchronously) to the content mailbox.

> [!IMPORTANT]
> Because there's only one writeable copy of the hierarchy, folder creation is proxied to the hierarchy mailbox by the content mailbox users are connected to.

In a large organization, when you create a new public folder mailbox, the hierarchy must synchronize to that public folder before users can connect to it. Otherwise, users may see an incomplete public folder structure when connecting with Outlook. To allow time for this synchronization to occur without users attempting to connect to the new public folder mailbox, set the *IsExcludedFromServingHierarchy* parameter on the **New-Mailbox** cmdlet when creating the public folder mailbox. This parameter prevents users from connecting to the newly created public folder mailbox. When synchronization is complete, run the [Set-Mailbox](/powershell/module/exchange/Set-Mailbox) cmdlet with the *IsExcludedFromServingHierarchy* parameter set to `false`, indicating that the public folder mailbox is ready to be connected to. You can use also the [Get-PublicFolderMailboxDiagnostics](/powershell/module/exchange/Get-PublicFolderMailboxDiagnostics) cmdlet to view the sync status by the *SyncInfo* and the *AssistantInfo* properties.

For more information, see [Create a public folder](../ExchangeOnline/collaboration-exo/public-folders/create-public-folder.md).

## Public folder content

Public folder content can include email messages, posts, documents, and eForms. The content is stored in the public folder mailbox but isn't replicated across multiple public folders mailboxes. All users access the same public folder mailbox for the same set of content. Although a full text search of public folder content is available, public folder content isn't searchable across public folders and the content isn't indexed by Exchange Search.

> [!NOTE]
> Outlook Web App is supported, but with limitations. You can add and remove favorite public folders and perform item-level operations such as creating, editing, deleting posts, and replying to posts. However, you can't create or delete public folders from Outlook Web App. Also, only Mail, Post, Calendar, and Contact public folders can be added to the Favorites list in Outlook Web App.

## Migrate public folders

You can migrate your public folders from pervious version of Exchange Server to Exchange 2013, or from previous versions of Exchange Server to Exchange Online. You can also migrate your Exchange 2013 public folders to Exchange Online.

If you already have Exchange 2010 SP3 or Exchange 2007 SP3 RU10 public folders in your organization prior to installing Exchange 2013, you must migrate those public folders to Exchange 2013. To do this, use the **PublicFolderMigrationRequst** cmdlets. For more information, see [Use batch migration to migrate public folders to Exchange 2013 from previous versions](use-batch-migration-to-migrate-public-folders-to-exchange-2013-from-previous-versions-exchange-2013-help.md). If your organization is moving to Exchange Online, you can migrate your public folders to the cloud and upgrade them at the same time. For details, see [Use batch migration to migrate legacy public folders to Microsoft 365 or Office 365 and Exchange Online](../ExchangeOnline/collaboration-exo/public-folders/batch-migration-of-legacy-public-folders.md) and [Use batch migration to migrate Exchange Server public folders to Exchange Online](../ExchangeServer/collaboration/public-folders/migrate-to-exchange-online.md)

Due to the changes in how public folders are stored, legacy Exchange mailboxes are unable to access the public folder hierarchy on Exchange 2013 servers or on Exchange Online. However, user mailboxes on Exchange 2013 servers or Exchange Online can connect to legacy public folders. Exchange 2013 public folders and legacy public folders can't exist in your Exchange organization simultaneously. This effectively means that there's no coexistence between versions. Migrating public folders to Exchange Server 2013 or Exchange Online is currently a one-time cutover process.

For this reason, it's recommended that prior to migrating your public folders, you should first migrate your legacy mailboxes to Exchange 2013 or Exchange Online. For more information about migrating mailboxes, see [Mailbox moves in Exchange 2013](mailbox-moves-in-exchange-2013-exchange-2013-help.md), [Migrate email using the Exchange cutover method](../ExchangeOnline/mailbox-migration/cutover-migration-to-office-365.md), and [Perform a staged migration of email to Microsoft 365 or Office 365](../ExchangeOnline/mailbox-migration/perform-a-staged-migration/perform-a-staged-migration.md).

## Public folder moves

You can move public folders to a different public folder mailbox, and you can move public folder mailboxes to different mailbox databases. To move public folders to different public folder mailboxes, use the **PublicFolderMoveRequest** set of cmdlets. Subfolders under the public folder that's being moved won't be moved by default. If you want to move a branch of public folders, you can use the `Move-PublicFolderBranch.ps1` script that's installed by default with Exchange 2013. For more information, see [Move a public folder to a different public folder mailbox](move-a-public-folder-to-a-different-public-folder-mailbox-exchange-2013-help.md).

In addition to moving public folders, you can move public folder mailboxes to different mailbox databases by using the **MoveRequest** set of cmdlets. This is the same set of cmdlets that are used for moving regular mailboxes. For more information, see [Move a public folder mailbox to a different mailbox database](move-a-public-folder-mailbox-to-a-different-mailbox-database-exchange-2013-help.md).

**PublicFolderMoveRequest** cmdlets and the **MoveRequest** cmdlets use the Mailbox Replication Service to move public folders asynchronously. That means that the cmdlet doesn't do the actual work and, during most of the move, the public folder and public folder mailboxes will still be available to users. Because the Mailbox Replication Service performs mailbox moves, import and export requests, and public folder move requests, it's important to consider throttling and workload management.

## Public folder quotas

When created, public folder mailboxes automatically inherit the size limits of the mailbox database defaults. As a result, to accurately evaluate the current storage quota status when using the [Get-Mailbox](/powershell/module/exchange/Get-Mailbox) cmdlet, you must review at the *UseDatabaseQuotaDefaults* property in addition to the *ProhibitSendQuota*, *ProhibitSendReceiveQuota*, and *IssueWarningQuota* properties. If the *UseDatabaseQuotaDefaults* property is set to `true`, the per-mailbox settings are ignored and the mailbox database limits are used. If this property is set to `true` and the *ProhibitSendQuota*, *ProhibitSendReceiveQuota*, and *IssueWarningQuota* properties are set to `unlimited`, the mailbox size isn't really unlimited. Instead, you must use the **Get-MailboxDatabase** cmdlet and review the mailbox database storage limits to find out what the limits for the mailbox are. If the *UseDatabaseQuotaDefaults* property is set to `false`, the per-mailbox settings are used. In Exchange 2013, the default mailbox database quota limits are as follows:

- *Issue warning quota*: 1.9 GB

- *Prohibit send quota*: 2 GB

- *Prohibit receive quota*: 2.3 GB

To find the mailbox database quotas, run the [Get-MailboxDatabase](/powershell/module/exchange/Get-MailboxDatabase) cmdlet.

To set the quotas on a public folder mailbox, use the [Set-OrganizationConfig](/powershell/module/exchange/Set-OrganizationConfig) cmdlet.

## Disaster recovery

Exchange 2013 public folders are built on mailbox infrastructure and use the same mechanisms for availability and redundancy. Every public folder mailbox can have multiple redundant copies with automatic failover, just like regular mailboxes. To learn more, see [High availability and site resilience](high-availability-and-site-resilience-exchange-2013-help.md).

In addition to the overall disaster recovery scenario, you can also restore public folders in the following situations:

- **Soft-deleted public folder restore**: The public folder was deleted but is still within the retention period.

- **Soft-deleted public folder mailbox restore**: The public folder mailbox was deleted and is still within the mailbox retention period.

- **Public folder mailbox restore from a recovery database**: You can recover an individual public folder mailbox from backup when the deleted mailbox retention period has elapsed. You then extract data from the restored mailbox and copy it to a target folder or merge it with another mailbox.

In all of these situations, the public folder or public folder mailbox is recoverable by using the **MailboxRestoreRequest** cmdlets.

For more information, see [Restore public folders and public folder mailboxes from failed moves](restore-public-folders-and-public-folder-mailboxes-from-failed-moves-exchange-2013-help.md).