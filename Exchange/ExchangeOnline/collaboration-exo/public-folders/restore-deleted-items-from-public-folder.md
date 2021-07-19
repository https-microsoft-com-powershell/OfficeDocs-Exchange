---
localization_priority: Normal
ms.author: v-bshilpa
manager: serdars
ms.topic: article
author: Benny-54
ms.service: exchange-online
ms.reviewer:
ms.collection:
- exchange-online
description: Restore deleted items of public folder from the recycle bin of public folder.
audience: ITPro
f1.keywords:
- NOCSH
title: Restore deleted items from public folder

---

# Restore deleted items from public folder

The items deleted from public folders are stored in the recoverable items (dumpster) of public folder until the retention period is over.

## Permissions required

The user restoring items from the public folder must have atleast the `Author` public folder client permission assigned. For more information on public folder client permissions, see [Add-PublicFolderClientPermission](/powershell/module/exchange/add-publicfolderclientpermission).

## Restore deleted items

1. In outlook, under **Public Folders**, select the folder from which items were deleted, and click **Recover Deleted Items**.

   ![recover deleted items](../../media/restore-pf-imag1.png)

2. Select the item to be restored, ensure **Restore Selected Items** option is selected and click **Ok**.

   ![restore selected items](../../media/restore-pf-imag2.png)

3. The item is restored.

   ![restored](../../media/restore-pf-imag3.png)

> [!NOTE]
> To enable the **Recover Deleted Items** option for non-mail type (for example: Calendar, Contact, Tasks) of public folders, see [Can't recover deleted items from a non-mail public folder](/outlook/troubleshoot/user-interface/cannot-recover-deleted-items-from-non-mail-public-folder).

## Related articles

[Recover a deleted public folder mailbox](./recover-deleted-public-folder-mailbox.md)

[Restore a deleted public folder](./restore-deleted-public-folder.md)