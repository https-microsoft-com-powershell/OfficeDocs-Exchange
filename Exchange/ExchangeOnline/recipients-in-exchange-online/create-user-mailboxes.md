---
localization_priority: Normal
description: Admins can learn how to create mailboxes in Exchange Online.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 6ed2f969-6c03-4b45-8e2d-05de787de48d
ms.reviewer: 
f1.keywords:
- NOCSH
title: Create user mailboxes in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create user mailboxes in Exchange Online

You have to use the Microsoft 365 admin center or Exchange Online PowerShell to create an Exchange Online user mailbox. **You can't create new user mailboxes using the new Exchange admin center (EAC)**. However, after Exchange Online mailboxes are created, you can manage them using the new EAC. For more information on adding users in Microsoft 365 admin center, see [Add users and assign licenses](/microsoft-365/admin/add-users/add-users).

> [!NOTE]
> After you create a new mailbox using Exchange Online PowerShell, you have to assign it an Exchange Online license or it will be disabled when the 30-day grace period ends.

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../permissions-exo/feature-permissions.md) article.

- It's a good idea to use strong passwords that are at least eight characters long, and combine uppercase and lowercase letters, numbers, and symbols.

- To learn how to use Windows PowerShell to connect to Exchange Online, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell).

- For information about keyboard shortcuts that may apply to the procedures in this article, see [Keyboard shortcuts for the Exchange admin center](../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the Microsoft 365 admin center to create a new mailbox

You can use the Microsoft 365 admin center to create a new user account. When you assign the user account a license for Exchange Online, a mailbox is automatically created for the user. To create new user accounts in the Microsoft 365 admin center, see [Add users individually or in bulk](/microsoft-365/admin/add-users/add-users).

## Use Exchange Online PowerShell to create a new mailbox

This example creates an Exchange Online mailbox and user account for Holly Holt. The optional parameter _ResetPasswordOnNextLogon_ will require the user to reset their password the first time they sign in to Microsoft 365 or Office 365.

```PowerShell
New-Mailbox -Alias hollyh -Name hollyh -FirstName Holly -LastName Holt -DisplayName "Holly Holt" -MicrosoftOnlineServicesID hollyh@corp.contoso.com -Password (ConvertTo-SecureString -String 'P@ssw0rd' -AsPlainText -Force) -ResetPasswordOnNextLogon $true
```

After you create a mailbox by running the previous command, a user account is also created. You have to activate this user account by assigning a license. To assign a license in the Microsoft 365 admin center, see [Add users individually or in bulk](/microsoft-365/admin/add-users/add-users).