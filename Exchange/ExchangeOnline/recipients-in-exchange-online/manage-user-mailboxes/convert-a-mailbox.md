---
localization_priority: Normal
description: Converting a mailbox to a different type of mailbox is very similar to the experience in earlier versions of Exchange. You must still use the Set-Mailbox cmdlet in Exchange Online PowerShell to do the conversion.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: dfed045e-a740-4a90-aff9-c58d53592f79
ms.reviewer: 
f1.keywords:
- NOCSH
title: Convert a mailbox
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Convert a mailbox in Exchange Online

You can use the new Exchange admin center (EAC) for the following types of mailbox conversions:

- User mailbox to shared mailbox.
- Shared mailbox to user mailbox.

Other types of mailbox conversions require Exchange Online PowerShell:

- User mailbox to resource (room or equipment) mailbox
- Shared mailbox to user mailbox (also available in the new EAC)
- Shared mailbox to resource mailbox
- Resource mailbox to user mailbox
- Resource mailbox to shared mailbox

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- If your organization uses a hybrid Exchange environment, you need to manage your mailboxes by using the on-premises Exchange management tools. To convert a mailbox in a hybrid environment, you might need to move the mailbox back to on-premises Exchange, convert the mailbox type, and then move it back to Microsoft 365 or Office 365.

- If you're converting a user mailbox to a shared mailbox, you should either remove any mobile devices from the mailbox before the conversion, or you should block mobile access to the mailbox after the conversion. This is because once the mailbox is converted to a shared mailbox, mobile functionality will not work properly. Additionally, if you are trying to prevent access to the converted mailbox, you might have to reset the password. For more information on blocking access, see [Remove a former employee](/microsoft-365/admin/add-users/remove-former-employee).

- Delegated Access Permission (DAP) partners with Administer On Behalf Of (AOBO) permissions can't use the procedures in this topic to convert customer mailboxes. Only members of the Organization Management role group in Exchange Online (global admins) can convert mailboxes.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the new Exchange admin center to convert a mailbox

1. In the EAC, go to **Recipients >  Mailboxes**.

   The **Mailboxes** page is displayed.

   You can filter for display of only user mailboxes or shared mailboxes by clicking **Filter**.

2. Select the user mailbox or a shared mailbox that you want to convert into its other type, and click on the display name.

3. From the **More actions** pane, click **Convert to regular mailbox** or **Convert to shared mailbox**.

   The mailbox conversion wizard is displayed.
 
4. Click **Confirm**.

   The mailbox is converted from its present type to its other type, and the notification message **Mailbox converted successfully is displayed**.

## Use Exchange Online PowerShell to convert a mailbox

To convert a mailbox, use the following syntax:

```PowerShell
Set-Mailbox -Identity <MailboxIdentity> -Type <Regular | Room | Equipment | Shared>

This example converts the shared mailbox named MarketingDept1 to a user mailbox.

```PowerShell
Set-Mailbox -Identity MarketingDept1 -Type Regular
```

For detailed syntax and parameter information, see [Set-Mailbox](/powershell/module/exchange/set-mailbox).

## How do you know this worked?

To verify that you have successfully converted the mailbox, replace \<MailboxIdentity\> with the alias or email address of the mailbox, and run the following command in Exchange Online PowerShell:

```PowerShell
Get-Mailbox -Identity <MailboxIdentity | Format-List RecipientTypeDetails
```

The value for _RecipientTypeDetails_ should be `UserMailbox`.

For detailed syntax and parameter information, see [Get-Mailbox](/powershell/module/exchange/get-mailbox).
