---
localization_priority: Normal
description: 'Summary: Admins can learn how to remove items from the Recoverable Items folder in Exchange Online.'
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 82c310f8-de2f-46f2-8e1a-edb6055d6e69
ms.reviewer:
f1.keywords:
- NOCSH
title: Clean up or delete items from the Recoverable Items folder in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Clean up or delete items from the Recoverable Items folder in Exchange Online

> [!IMPORTANT]
> Please refer to the [Microsoft 365 security center](https://security.microsoft.com/homepage) and the [Microsoft 365 compliance center](https://compliance.microsoft.com/homepage) for Exchange security and compliance features. They are no longer available in the new [Exchange Admin Center](https://admin.exchange.microsoft.com).

The Recoverable Items folder (known in earlier versions of Exchange as *the dumpster*) exists to protect from accidental or malicious deletions and to facilitate discovery efforts commonly undertaken before or during litigation or investigations.

How you clean up or delete items from a user's Recoverable Items folder depends on whether the mailbox is placed on In-Place Hold or Litigation Hold, or had single item recovery enabled:

- If a mailbox isn't placed on In-Place Hold, Litigation Hold, or another type of hold in Microsoft 365 or Office 365, or if a mailbox doesn't have single item recovery enabled, you can delete items from the Recoverable Items folder. After items are deleted, you can't use single item recovery to recover them.

- If the mailbox is placed on In-Place Hold, Litigation Hold, or another type of hold in Microsoft 365 or Office 365, or if single item recovery is enabled, you'll want to preserve the mailbox data until the hold is removed or single item recovery is disabled. In this case, you need to perform more detailed steps to clean up the Recoverable Items folder.

To learn more about In-Place Hold and Litigation Hold, see [In-Place Hold and Litigation Hold in Exchange Online](../in-place-and-litigation-holds.md). To learn more about single item recovery, see [Single item recovery](recoverable-items-folder.md#single-item-recovery).

## What do you need to know before you begin?

- To create and run a Content Search, you have to be a member of the eDiscovery Manager role group or be assigned the Compliance Search management role. To delete messages, you have to be a member of the Organization Management role group or be assigned the Search And Purge management role. For information about adding users to a role group, see [Assign eDiscovery permissions in the Microsoft 365 compliance center](/microsoft-365/compliance/assign-ediscovery-permissions).

- Because incorrectly cleaning up the Recoverable Items folder can result in data loss, it's important that you're familiar with the Recoverable Items folder and the impact of removing its contents. Before performing this procedure, we recommend that you review the information in [Recoverable Items folder in Exchange Online](recoverable-items-folder.md).

- You can only use Security & Compliance Center PowerShell to perform the procedures in this article. To connect to Security & Compliance Center PowerShell, see [Connect to Security & Compliance Center PowerShell](/powershell/exchange/connect-to-scc-powershell).

> [!TIP]
> Having problems? Ask for help in the Microsoft Tech Community. Visit it at [Microsoft Tech Community - Exchange](https://techcommunity.microsoft.com/t5/exchange/ct-p/Exchange).

## Use Security & Compliance Center PowerShell to delete items from the Recoverable Items folder for mailboxes that aren't placed on hold or don't have single item recovery enabled

You can delete items in the Recoverable Items folder by using the [New-ComplianceSearch](/powershell/module/exchange/new-compliancesearch) and [New-ComplianceSearchAction](/powershell/module/exchange/new-compliancesearchaction) cmdlets in Security & Compliance Center PowerShell.

To search for items that are located in the Recoverable Items folder, we recommend that you perform a *targeted collection*. This means you narrow the scope of your search only to items located in the Recoverable Items folder. You can do this by running the script in the [Use Content Search for targeted collections](/microsoft-365/compliance/use-content-search-for-targeted-collections) article. This script returns the value of the folder ID property for all the subfolders in the target Recoverable Items folder. Then you use the folder ID in a search query to return items located in that folder.

Here's an overview of the process to search for and delete items in a user's Recoverable Items folder:

1. Run the targeted collection script that returns the folder IDs for all folders in the target user's mailbox. The script connects to Exchange Online PowerShell and Security & Compliance PowerShell in the same PowerShell session. For more information, see [Run the script to get a list of folders for a mailbox or site](/microsoft-365/compliance/use-content-search-for-targeted-collections#step-1-run-the-script-to-get-a-list-of-folders-for-a-mailbox-or-site).

2. Copy the folder IDs for all subfolders in the Recoverable Items folder. Alternatively, you can redirect the output of the script to a text file.

   Here is a list and description of the subfolders in the Recoverable Items folder that you can search and delete items from:

   - **Deletions**: Contains soft-deleted items whose deleted item retention period has not expired. Users can recover soft-deleted items from this subfolder using the Recover Deleted Items tool in Outlook.

   - **Purges**: Contains hard-deleted items whose deleted item retention period has expired. Users can also hard-delete items by purging items from their Recoverable Items folder. If the mailbox is on hold, hard-deleted items are preserved. This subfolder isn't visible to end-users.

   - **DiscoveryHolds**: Contains hard-deleted items that have been preserved by an eDiscovery hold or a retention policy. This subfolder isn't visible to end-users.

   - **SubstrateHolds**: Contains hard-deleted items from Teams and other cloud-based apps that have been preserved by a retention policy or other type of hold. This subfolder isn't visible to end-users.

3. Use the **New-ComplianceSearch** cmdlet (in Security & Compliance Center PowerShell) or use the Content Search tool in the Microsoft 365 compliance center to create a content search that returns items from the target user's Recoverable Items folder. You can do this by including the FolderId in the search query for all subfolders that you want to search. For example, the following query returns all messages in the Purges and eDiscoveryHolds subfolders:

   ```text
   folderid:<folder ID of Purges subfolder> OR folderid:<folder ID of DiscoveryHolds subfolder>
   ```

   For more information and examples about running content searches that use the folder ID property, see [Use a folder ID or documentlink to perform a targeted collection](/microsoft-365/compliance/use-content-search-for-targeted-collections#step-2-use-a-folder-id-or-documentlink-to-perform-a-targeted-collection).

   > [!NOTE]
   > If you use the **New-ComplianceSearch** cmdlet to search the Recoverable Items folder, be sure to use the **Start-ComplianceSearch** cmdlet to run the search.

4. After you've created a content search and validated that it returns the items that you want to delete, use the `New-ComplianceSearchAction -Purge -PurgeType HardDelete` command (in Security & Compliance Center PowerShell) to permanently delete the items returned by the content search that you created in the previous step. For example, you can run a command similar to the following command:

   ```powershell
   New-ComplianceSearchAction -SearchName "RecoverableItems" -Purge -PurgeType HardDelete
   ```

5. A maximum of 10 items per mailbox are deleted when you run the previous command. That means you may have to run the `New-ComplianceSearchAction -Purge` command multiple times to delete all the items that you want to delete in the Recoverable Items folder. To delete additional items, you first have to remove the previous compliance search purge action. You do this by running the `Remove-ComplianceSearchAction` cmdlet. For example, to delete the purge action that was run in the previous step, run the following command:

   ```powershell
   Remove-ComplianceSearchAction "RecoverableItems_Purge"
   ```

   After you do this, you can create a new compliance search purge action to delete more items. You'll have to delete each purge action before creating a new one.

   To get a list of the compliance search actions, you can run the `Get-ComplianceSearchAction` cmdlet. Purge actions are identified by `_Purge` appended to the search name.

## Use Exchange Online and Security & Compliance Center PowerShell to clean up the Recoverable Items folder for mailboxes that are placed on hold or have single item recovery enabled

This scenario is fully covered in the article [Delete items in the Recoverable Items folder of cloud mailbox's on hold](/microsoft-365/compliance/delete-items-in-the-recoverable-items-folder-of-mailboxes-on-hold).

## How do you know this worked?

To verify that you've successfully deleted items from the Recoverable Items folder of a mailbox, use the [Get-MailboxFolderStatistics](/powershell/module/exchange/get-mailboxfolderstatistics) cmdlet in [Exchange Online PowerShell](/powershell/exchange/exchange-online-powershell) to check the size and number of items in the Recoverable Items folder. You can compare these statistics with the ones you collected in Step 1.
  
Run the following command to get the current size and the total number of items in folders and subfolders in the Recoverable Items folder in the user's primary mailbox.
  
```powershell
Get-MailboxFolderStatistics <username> -FolderScope RecoverableItems | FL Name,FolderAndSubfolderSize,ItemsInFolderAndSubfolders
```

Run the following command to get the size and total number of items in folders and subfolders in the Recoverable Items folder in the user's archive mailbox.

```powershell
Get-MailboxFolderStatistics <username> -FolderScope RecoverableItems -Archive | FL Name,FolderAndSubfolderSize,ItemsInFolderAndSubfolders
```
