---
title: "Auto forwarded messages report in the new EAC"
f1.keywords:
- NOCSH
ms.author: chrisda
author: chrisda
manager: dansimp
audience: ITPro
ms.topic: article
ms.service: exchange-online
localization_priority: Normal
ms.assetid:
description: "Admins can learn how to use the Auto forwarded messages report in the new Exchange admin center to see the internal senders and external recipients of forwarded messages from your organization."
---

# Auto forwarded messages report in the new EAC

The **Auto-forwarded messages** report in the new Exchange admin center (new EAC) displays information on messages that are automatically forwarded from your organization to recipients in external domains. You can use this report to look for potential data leaks.

> [!NOTE]
> By default, the report shows data for the last 7 days. If the report is empty, try changing the date range.
>
> For permissions that are required to use this report, see [Permissions required to view mail flow reports](mail-flow-reports.md#permissions-required-to-view-mail-flow-reports).

The overview section contains the following charts:

- **Forwarding type**: Typical values are:
  - **Mail flow rules**
  - **Inbox rules**
  - **SMTP forwarding**: This is automatic forwarding that admins can configure on a mailbox as described in [Configure email forwarding for a mailbox](../../recipients-in-exchange-online/manage-user-mailboxes/configure-email-forwarding.md).
- **Recipient domain**
- **Forwarding users**

If you hover over a specific color in the chart, you'll see the associated numbers for that specific forwarding type, recipient domain, or forwarding user.

![Overview of the Auto forwarded messages report](../../media/mfr-auto-forwarded-messages-report.png)

The **Auto forwarded message details** section shows the following information about each specific forwarder (the user account that's doing the forwarding):

- **Forwarders**
- **Forwarding type**
- **Recipient name**
- **Recipient domain**
- **Details**
- **Forward count**
- **First forward date**

Click **Export** to export the displayed results to a .csv file.

## Insights

Two insights are generated based on the report data: [New domains being forwarded email](../mail-flow-insights/mfi-new-domains-being-forwarded-email-insight.md) and [New users forwarding email](../mail-flow-insights/mfi-new-users-forwarding-email-insight.md). Each insight provides a summary of the number of new forwarders or domains with a link back to this report.

## See also

For more information about other mail flow reports, see [Mail flow reports in the modern EAC](mail-flow-reports.md).