---
localization_priority: Normal
description: By default, POP3 and IMAP4 are enabled for all users in Exchange Online. You can disable them for individual users. For additional information related to POP3 and IMAP4, see POP3 and IMAP4.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 4e351027-1a96-42c6-b4b2-8218d2070f25
ms.reviewer: 
title: Enable or Disable POP3 or IMAP4 access for a user
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Enable or Disable POP3 or IMAP4 access for a user

By default, POP3 and IMAP4 are enabled for all users in Exchange Online. You can disable them for individual users. For additional information related to POP3 and IMAP4, see [POP3 and IMAP4](pop3-and-imap4.md).

## What do you need to know before you begin?

- Estimated time to finish: two minutes.

- If you've enabled _security defaults_ in your organization, POP3 and IMAP4 are already disabled in Exchange Online. For more information, see [What are security defaults?](/azure/active-directory/conditional-access/concept-conditional-access-security-defaults).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "POP3 and IMAP4 settings" section in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the EAC to enable or disable POP3 or IMAP4 for a user

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the result pane, select the user for which you want to enable or disable POP3, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. In the **User Mailbox** dialog box, in the console tree, click **Mailbox Features**.

4. In the result pane, under **Email Connectivity**, do one of the following:

   - To enable POP3 for the user, under **POP3: Disabled**, click **Enable**.

   - To enable IMAP4 for the user, under **IMAP4: Disabled**, click **Enable**.

   - To disable POP3 for the user, under **POP3: Enabled**, click **Disable**.

   - To disable IMAP4 for the user, under **IMAP4: Enabled**, click **Disable**.

5. Click **Save**.

## Use Exchange Online PowerShell to enable or disable POP3 or IMAP4 for a user

This example enables POP3 for the user Christa Knapp.

```PowerShell
Set-CASMailbox -Identity "Christa Knapp" -POPEnabled $true
```

This example enables IMAP4 for the user Christa Knapp.

```PowerShell
Set-CASMailbox -Identity "Christa Knapp" -IMAPEnabled $true
```

This example disables POP3 for the user Christa Knapp.

```PowerShell
Set-CASMailbox -Identity "Christa Knapp" -POPEnabled $false
```

This example disables IMAP4 for the user Christa Knapp.

```PowerShell
Set-CASMailbox -Identity "Christa Knapp" -IMAPEnabled $false
```

## How do you know this worked?

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the result pane, select the user for which you want to enable or disable POP3 or IMAP4, and then click **Edit**.

3. In the **User Mailbox** dialog box, in the console tree, click **Mailbox Features**.

4. In the result pane, look under **Email Connectivity**.

   - If POP3 is disabled for the user, you will see **POP3: Disabled**.

   - If IMAP4 is disabled for the user, you will see **IMAP4: Disabled**.

   - If POP3 is enabled for the user, you will see **POP3: Enabled**.

   - If IMAP4 is enabled for the user, you will see **IMAP4: Enabled**.

5. Click **Save**.