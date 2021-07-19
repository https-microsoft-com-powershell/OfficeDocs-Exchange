---
localization_priority: Normal
description: Admins can learn how to use the Exchange admin center (EAC) to run a non-owner mailbox access report in Exchange Online.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: dbbef170-e726-4735-abf1-2857db9bb52d
ms.reviewer: 
f1.keywords:
- NOCSH
title: Run a non-owner mailbox access report in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Run a non-owner mailbox access report in Exchange Online

In Exchange Online, the Non-owner mailbox access report in the Exchange admin center (EAC) lists the mailboxes that have been accessed by someone other than the person who owns the mailbox. When a mailbox is accessed by a non-owner, Exchange Online logs information about this action in a mailbox audit log that's stored as an email message in a hidden folder in the mailbox being audited. Entries from this log are displayed as search results and include a list of mailboxes accessed by a non-owner, who accessed the mailbox and when, the actions performed by the non-owner, and whether the action was successful. By default, entries in the mailbox audit log are retained for 90 days.

When you enable mailbox audit logging for a mailbox, Exchange Online logs specific actions by non-owners, including both administrators and users, called delegated users, who have been assigned permissions to a mailbox. You can also narrow the search to users inside or outside your organization.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- As of January 2019, mailbox audit logging on by default is enabled for all Exchange Online organizations. For more information, see [Manage mailbox auditing](/microsoft-365/compliance/enable-mailbox-auditing).

- To find and open the Exchange admin center (EAC), see, [Exchange admin center in Exchange Online](../../exchange-admin-center.md).

- You need to be assigned permissions before you can perform this procedure. To see what permissions you need, see the "View reports" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) forum.

## Run a non-owner mailbox access report

1. In the EAC, go to **Compliance Management** \> **Auditing**.

2. Click **Run a non-owner mailbox access report**.

   By default, Exchange Online runs the report for non-owner access to any mailboxes in the organization over the past two weeks. The mailboxes listed in the search results have been enabled for mailbox audit logging.

3. To view non-owner access for a specific mailbox, select the mailbox from the list of mailboxes. View the search results in the details pane.

> [!TIP]
> Want to narrow the search results? Select the start date, end date, or both, and select specific mailboxes to search. Click **Search** to re-run the report.

### Search for specific types of non-owner access

You can also specify the type of non-owner access, also called the logon type, to search for. Here are your options:

- **All non-owners**: Search for access by admins and delegated users inside your organization, and by Microsoft datacenter administrators in Exchange Online.
- **External users**: Search for access by Microsoft datacenter administrators.
- **Administrators and delegated users**: Search for access by admins and delegated users inside your organization.
- **Administrators**: Search for access by admins in your organization.

### How do you know this worked?

To verify that you've successfully run a non-owner mailbox access report, check the search results pane. Mailboxes that you ran the report for are displayed in this pane. If there are no results for a specific mailbox, it's possible there hasn't been access by a non-owner or that non-owner access hasn't taken place within the specified date range. As previously described, be sure to verify that audit logging has been enabled for the mailboxes you want to search for access by non-owners.

## What gets logged in the mailbox audit log?

When you run a non-owner mailbox access report, entries from the mailbox audit log are displayed in the search results in the EAC. Each report entry contains this information:

- Who accessed the mailbox and when
- The actions performed by the non-owner
- The affected message and its folder location
- Whether the action was successful

For detailed information, see [Mailbox actions for user mailboxes and shared mailboxes](/microsoft-365/compliance/enable-mailbox-auditing#mailbox-actions-for-user-mailboxes-and-shared-mailboxes).
