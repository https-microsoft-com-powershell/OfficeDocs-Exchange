---
localization_priority: Normal
description: If your organization is involved in a legal action, you may have to take steps to preserve relevant data, such as email messages, that may be used as evidence. In situations like this, you can use litigation hold to retain all email sent and received by specific people or retain all email sent and received in your organization for a specific time period. For more information about what happens when a mailbox is on litigation hold and how to enable and disable it, see theMailbox Featuressection in Manage user mailboxes.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 98c46226-2f48-42c6-a741-34bb5944f519
ms.reviewer: 
f1.keywords:
- NOCSH
title: Run a per-mailbox litigation hold report
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Run a per-mailbox litigation hold report in Exchange Online

If your Exchange Online organization is involved in a legal action, you may have to take steps to preserve relevant data, such as email messages, that may be used as evidence. In situations like this, you can use litigation hold to retain all email sent and received by specific people or retain all email sent and received in your organization for a specific time period. For more information about what happens when a mailbox is on litigation hold and how to enable and disable it, see the "Mailbox Features" section in [Manage user mailboxes](../../recipients-in-exchange-online/manage-user-mailboxes/manage-user-mailboxes.md).

Use the litigation hold report to keep track of the following types of changes made to a mailbox in a given time period:

- Litigation hold was enabled.
- Litigation hold was disabled.

For each of these change types, the report includes the user who made the change and the time and date the change was made.

## What do you need to know before you begin?

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "View reports" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) forum.

## Use the EAC to run a litigation hold report

1. In the EAC, navigate to **Compliance Management** \> **Auditing**.

2. Click **Run a per-mailbox litigation hold report**.

    Microsoft Exchange runs the report for litigation hold changes made to any mailbox in the past two weeks.

3. To view the changes for a specific mailbox, in the search results pane, select the mailbox. View the search results in the details pane.

> [!TIP]
> Want to narrow the search results? Select the start date, end date, or both, and select specific mailboxes to search. Click **Search** to re-run the report.

## How do you know this worked?

To verify that you've successfully run a litigation hold report, mailboxes that had litigation hold changes within the date range are displayed in the search results pane. If there are no results, then no changes to litigation hold have taken place within the date range or recent changes haven't taken effect yet.

> [!NOTE]
> When a mailbox is put on litigation hold, it can take up to 60 minutes for the hold to take effect.
