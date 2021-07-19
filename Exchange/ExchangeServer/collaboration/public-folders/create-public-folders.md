---
localization_priority: Normal
description: 'Summary: Learn how to create a public folder with the Exchange admin center (EAC) or with the Exchange Management Shell.'
ms.topic: article
author: msdmaguire
ms.custom:
- Microsoft.Exchange.Management.PublicFolders.NewPublicFolderWizardForm.NewPublicFolderWizardPage
ms.author: dmaguire
ms.assetid: 6d252e60-c8d0-4efd-b9d7-ba5284a6f8ab
ms.reviewer:
title: Create a public folder
ms.collection: exchange-server
f1.keywords:
- CSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create a public folder

Public folders are designed for shared access and provide an easy and effective way to collect, organize, and share information with other people in your workgroup or organization.

By default, a public folder inherits the settings of its parent folder, including the permissions settings.

For additional management tasks related to public folders, see [Public folder procedures](procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Public folders" entry in the [Sharing and collaboration permissions](../../permissions/feature-permissions/sharing-and-collaboration-permissions.md)topic.

- You can't create a public folder unless you've first created a public folder mailbox. For more information about how to create a public folder mailbox, see [Create a public folder mailbox](create-public-folder-mailboxes.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

## Use the EAC to create a public folder
<a name="BKMK_EAC"> </a>

When using the EAC to create a public folder, you'll only be able to set the name and the path of the public folder. To configure additional settings, you'll need to edit the public folder after it's created.

1. Navigate to **Public folders** \> **Public folders**.

2. If you want to create this public folder as a child of an existing public folder, click the existing public folder in the list view. If you want to create a top-level public folder, skip this step.

3. Click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

4. In **Public Folder**, type the name of the public folder.

    > [!IMPORTANT]
    > Don't use a backslash (\) in the name when creating a public folder.

5. In the **Path** box, verify the path to the public folder. If this isn't the desired path, click **Cancel** and follow Step 2 of this procedure.

6. Click **Save**.

## Use the Exchange Management Shell to create a public folder
<a name="BKMK_Shell"> </a>

This example creates a public folder named Reports in the path Marketing\2016.

```PowerShell
New-PublicFolder -Name Reports -Path \Marketing\2016
```

> [!IMPORTANT]
> Don't use a backslash (\) in the name when creating a public folder.

For detailed syntax and parameter information, see [New-PublicFolder](/powershell/module/exchange/new-publicfolder).

## How do you know this worked?
<a name="BKMK_Shell"> </a>

To verify that you've successfully created a public folder, do the following:

- In the EAC, click **Refresh** to refresh the list of public folders. Your new public folder should be displayed in the list.

- In the Exchange Management Shell, run any of the following commands:

  ```PowerShell
  Get-PublicFolder -Identity \Marketing\2016\Reports | Format-List
  ```

  ```PowerShell
  Get-PublicFolder -Identity \Marketing\2016 -GetChildren
  ```

  ```PowerShell
  Get-PublicFolder -Recurse
  ```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver), [Exchange Online](/answers/topics/office-exchange-server-itpro.html), or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).