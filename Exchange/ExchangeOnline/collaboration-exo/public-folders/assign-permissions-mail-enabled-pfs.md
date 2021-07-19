---
localization_priority: Normal
ms.author: jhendr
manager: serdars
ms.topic: article
author: msdmaguire
ms.service: exchange-online
description: 'Summary: How to assign "Send As" or "Send on Behalf" permissions to your Exchange Online public folders.'
audience: ITPro
f1.keywords:
- NOCSH
title: Assign "Send As" or "Send on Behalf" permissions for mail-enabled public folders

---

# Assign "Send As" or "Send on Behalf" permissions for mail-enabled public folders

You can assign either "Send As" or "Send on Behalf" permissions for mail-enabled public folders to users in Microsoft Exchange Online.

## What do you need to know before you begin?

- Estimated time to complete this task: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Public folders" entry in [Sharing and collaboration permissions](../../../ExchangeServer/permissions/feature-permissions/sharing-and-collaboration-permissions.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [**Exchange Online**](/answers/topics/office-exchange-server-itpro.html) or [**Exchange Online Protection**](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the Exchange admin center (EAC) to assign permissions

1. Sign in to Exchange admin center as an administrator.

2. Select **public folders** \> **public folders**.

3. In the list view, select the public folder that requires the permissions, and then click **Edit** (the pencil icon).

4. Select **delivery options**, and then add the user to **Send As** or **Send on Behalf** permissions, as required.

5. Select **Save**.

## Use Exchange Online PowerShell to assign permissions

The following example assigns "Send on Behalf" permissions for the mail-enabled public folder *NewPF1* to the user *Jason*.

`Set-MailPublicFolder -Identity '\\NewPF1' -GrantSendOnBehalfTo "Jason"`

The following example assigns "Send As" permissions for the mail-enabled public folder *NewPF1* to the user *Jason*.

`Add-RecipientPermission -Identity 'NewPF1' -Trustee "Jason" -AccessRights 'SendAs'`

For detailed syntax and parameter information, see the following articles:

- [Set-MailPublicFolder](/powershell/module/exchange/set-mailpublicfolder)

- [Add-RecipientPermission](/powershell/module/exchange/add-recipientpermission)

## Send As mail enabled public folder in Hybrid scenario

For Exchange Online mailboxes accessing public folders deployed at On-Premises:

1) Ensure Mail Enabled Public Folders are synced to Exchange Online 
Use following EXO PowerShell command to ensure On-Premises mail public folder are synced:

`Get-MailPublicFolder <MEPFName>`
Example:
Following example lists MEPF named OnPremPF

`Get-MailPublicFolder OnPremPF`

If the MEPF from On-Premises are not showing in EXO, use the Sync-MailPublicFolders.ps1 (for Exchange Server 2010) or Sync-ModernMailPublicFolders.ps1 (For Exchange 2013/2016/2019) to sync the MEPF's first.

2) Use following command in EXO PowerShell to assign SendAs permission:

`Add-RecipientPermission -Identity 'OnPremPF1' -Trustee "Richard" -AccessRights 'SendAs'`