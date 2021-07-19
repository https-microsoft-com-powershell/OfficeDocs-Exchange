---
localization_priority: Normal
description: Admins can learn about deleting and restoring mailboxes in Exchange Online.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: be7f59a5-bbc9-4b7a-a28b-f47b26dd33a7
ms.reviewer: 
f1.keywords:
- NOCSH
title: Delete or restore user mailboxes in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Delete or restore user mailboxes in Exchange Online

> [!IMPORTANT]
> Check out the new Exchange Admin Center! The experience is modern, intelligent, accessible, and better. Personalize your dashboard, manage cross tenant migration, experience the improved Groups feature, and more. [Try it now](https://admin.exchange.microsoft.com)!

There are several things you should consider before you decide to delete a user mailbox. There are different kinds of deletions that you can do on a user mailbox and some of them won't allow you to restore or recover the mailbox. This article walks you through the deleted mailbox scenarios, and how to delete, recover or permanently remove a mailbox from Exchange Online.

> [!NOTE]
> You can't use EAC to delete or restore user mailboxes.

## Soft-deleted user mailboxes

A soft-deleted user mailbox is a mailbox that has been deleted using the Microsoft 365 admin center or the **Remove-Mailbox** cmdlet in Exchange Online PowerShell, and has still been in the Azure Active Directory (Azure AD) recycle bin for less than 30 days.

A soft-deleted user mailbox is a mailbox that has been deleted in the following cases:

- The user mailbox's associated Azure AD user account is soft-deleted (the Azure AD user object is out of scope or in the recycle bin container).

- The user mailbox's associated Azure AD user account has been hard-deleted but a Litigation Hold or an eDiscovery hold was placed on the Exchange Online mailbox before it was deleted.

- The user mailbox's associated Azure AD user account has been purged within the last 30 days, which is the retention length Exchange Online keeps the mailbox in a soft-deleted state before it's permanently purged and unrecoverable.

> [!NOTE]
> If you run the Azure cmdlet `Remove-MsolUser` with the `-RemoveFromRecycleBin` parameter in order to remove a user from the Azure AD recycle bin, it will always put an existing Exchange Online mailbox associated with the Azure AD user in a soft-deleted state, as long as the user's license was not removed. However, if you remove the user's license prior to removing the user from the recycle bin, the user will not go into a soft-deleted user mailbox state.

If in the 30-day time period a new Azure AD user is synchronized from the original on-premises recipient account with the same ExchangeGuid or ArchiveGuid, this will result in an ExchangeGuid validation conflict error.

Check out [Overview of inactive mailboxes](/microsoft-365/compliance/inactive-mailboxes-in-office-365) for more info about creating an inactive mailbox by placing a Litigation Hold on a mailbox before deleting it.

## Hard-deleted user mailboxes

A hard-deleted user mailbox is a mailbox that has been deleted in the following cases:

- The user mailbox has been soft-deleted for more than 30 days, and the associated Azure AD user has been hard-deleted. Check out the **Remove-MsolUser** cmdlet. All mailbox content such as emails, contacts, and files will be permanently deleted.

- The user mailbox's associated user account has been hard-deleted in Azure AD. The user mailbox is now soft-deleted in Exchange Online and stays in the soft deleted state for 30 days. If in the 30 days time period a new Azure AD user is synchronized from the original on-premises recipient account with the same ExchangeGuid or ArchiveGuid, and that new account is licensed for Exchange Online, this results in a hard deletion of the original user mailbox. All mailbox content such as emails, contacts, and files will be permanently deleted.

- The soft deleted mailbox has been deleted using the **Remove-Mailbox** cmdlet with the _PermanentlyDelete_ parameter in Exchange Online PowerShell.

The above scenarios assume that the user mailbox isn't in any of the hold states, like Litigation hold or eDiscovery hold. If there is any type of hold on the user mailbox the mailbox can't be removed from Exchange Online. For all mail user recipient types, Litigation hold or eDiscovery hold are ignored and have no impact on the mail users hard-deleted or soft-delete behavior. The mail user object can't be deleted if there is a journal mailbox associated with it. You can disable journaling on the mail user by using the **Disable-JournalArchiving** cmdlet.

## Delete a user mailbox

### Use the Microsoft 365 admin center to delete a user account

When you delete a user account, the corresponding Exchange Online mailbox is deleted and removed from the list of mailboxes in the EAC. After the user account is deleted, it's listed on the **Deleted Users** page in the Microsoft 365 admin center. It can be recovered within 30 days after being deleted. After 30 days, the user account and mailbox are permanently deleted and not recoverable.

To delete a Microsoft 365 or Office 365 work or school account, see [Delete or restore users](/microsoft-365/admin/add-users/delete-a-user).

### Use Windows PowerShell to permanently delete a user mailbox

This example deletes the user account for Walter Harp from Azure AD.

```PowerShell
Remove-MsolUser -UserPrincipalName <Walter Harp> -RemoveFromRecycleBin
```

For more details, check out, [Remove-MsolUser](/powershell/module/msonline/remove-msoluser).

### Use Exchange Online PowerShell to delete a mailbox

- You need permissions before you can perform this procedure or procedures. To see what permissions you need, see the"Recipients" entry in the [Feature permissions in Exchange Online](../permissions-exo/feature-permissions.md) article.

- To learn how to use Windows PowerShell to connect to Exchange Online, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell).

When you delete an Exchange Online mailbox using Exchange Online PowerShell, the corresponding Microsoft 365 or Office 365 user is deleted and removed from the list of users in the Microsoft 365 admin center. The user will still be recoverable for 30 days. After the 30 days time limit, the user is permanently deleted.

This example deletes an Exchange Online mailbox and the corresponding user account for Walter Harp.

```PowerShell
Remove-Mailbox -Identity "Walter Harp"
```

## Restore a user mailbox

When you delete a mailbox, Exchange Online retains the mailbox and all its contents until the deleted mailbox retention period expires, which is 30 days. After 30 days, the mailbox is permanently deleted and can't be recovered. The method for restoring a mailbox depends on whether the mailbox was deleted by deleting the user account or removing the Exchange Online license.

### Use the Microsoft 365 admin center to restore a user account

If the mailbox was deleted by deleting the corresponding user account, you can restore the mailbox by restoring the user account in the Microsoft 365 admin center.

To restore a user account, see [Delete or restore users](/microsoft-365/admin/add-users/delete-a-user).

### Use Exchange Online PowerShell to restore a user account

You can recover soft-deleted mailboxes using the PowerShell cmdlet below. The cmdlet example below restores the mailbox for Allie Bellew.

1. [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell)

2. Run the **Undo-SoftDeletedMailbox** cmdlet.

   ```PowerShell
   Undo-SoftDeletedMailbox allieb@contoso.com -WindowsLiveID allieb@contoso.com -Password (ConvertTo-SecureString -String 'Pa$$word1' -AsPlainText -Force)
   ```
### License removal

When an Exchange Online license is removed from a user, Exchange Online data associated with that account is held for 30 days. After the 30-day grace period, the data is deleted and can't be recovered. If you add the license back to the user during the grace period, this will restore access, and the mailbox will become fully active.

> [!NOTE]
> If the Microsoft 365 or Office 365 or Exchange Online license is removed from a user, the user's mailbox is no longer searchable by using an eDiscovery tool such as Content Search or Advanced eDiscovery. For more information, see the "Searching disconnected or de-licensed mailboxes" section in [Feature reference for Content search](/microsoft-365/compliance/content-search-reference#searching-disconnected-or-de-licensed-mailboxes).

## Restoring a user in a hybrid deployment

For user mailboxes in a hybrid scenario, if the mailbox has been soft-deleted and the Azure AD user that was associated with the mailbox has been hard-deleted from Azure AD, you can use **New-MailboxRestoreRequest** to recover the mailbox. Read [Configure Microsoft 365 Groups with on-premises Exchange hybrid](../../ExchangeHybrid/hybrid-deployment/set-up-microsoft-365-groups.md) for more info. The procedures in this section explain how to restore the mailbox for a soft-deleted user.

1. [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell)

2. Run the following cmdlet to identify the soft-deleted mailbox that you want to restore.

   ```PowerShell
   Get-Mailbox -SoftDeletedMailbox | Select-Object Name,ExchangeGuid
   ```

   For the soft-deleted mailbox that you want to restore, note its GUID value (you use the value in Step 4).

3. Create a target mailbox for the restored mailbox. For more information, see [Create user mailboxes in Exchange Online](create-user-mailboxes.md). After you create the target mailbox, run the following command to get the GUID value of the target mailbox that you'll need in the next step.

   ```PowerShell
   Get-Mailbox -Identity <NameOrAliasOfNewTargetMailbox> | Format-List ExchangeGuid
   ```

4. Replace \<SoftDeletedMailboxGUID\> with the GUID value from Step 2, and \<NewTargetMailboxGUID\> with the GUID value from Step 3, and run the following cmdlet to restore the mailbox:

   ```PowerShell
   New-MailboxRestoreRequest -SourceMailbox <SoftDeletedMailboxGUID> -TargetMailbox <NewTargetMailboxGUID>
   ```

For other mailbox restoring scenarios related to hybrid infrastructures, refer to [Common mailbox recovery scenarios for hybrid environments](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Common-mailbox-recovery-scenarios-for-hybrid-environments/ba-p/604681).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).
