---
title: "SMTP AUTH clients report in the new EAC"
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
description: "Admins can learn how to use the SMTP AUTH report in the new Exchange admin center to monitor email senders in your organization that use authenticated SMTP (SMTP AUTH) to send email messages."
---

# SMTP AUTH clients report in the new Exchange admin center

The **SMTP AUTH clients** report in the new Exchange admin center (new EAC) highlights the use of the SMTP AUTH client submission protocol by users or system accounts in your organization. This legacy protocol (which uses the endpoint smtp.office365.com) only offers Basic authentication, and is susceptible to being used by compromised accounts to send email. This report allows you to check for unusual activity. It also shows the TLS usage data for clients or devices using SMTP AUTH.

> [!NOTE]
> By default, the report shows data for the last 7 days. If the report is empty, try changing the date range.
>
> For permissions that are required to use this report, see [Permissions required to view mail flow reports](mail-flow-reports.md#permissions-required-to-view-mail-flow-reports).

The overview section contains the following charts:

- The volume of messages per day for each sending domain.
- **Message volume by sender**<sup>\*</sup>
- **Message sent by domain**<sup>\*</sup>
- **Senders by TLS protocol**<sup>\*</sup>

<sup>\*</sup> If you hover over a specific color in the chart, you'll see the number of messages.

![Overview of the SMTP AUTH clients report](../../media/mfr-smtp-auth-clients-report.png)

The **Messages sent using SMTP Auth** section shows the following information:

- **Sender address**
- **Domain**
- **TLS 1.0** (percentage)
- **TLS 1.1** (percentage)
- **TLS 1.2** (percentage)
- **Messages sent**

To quickly filter the results, click **Search** ![Search icon](../../media/modern-eac-search-icon.png) and start typing a value.

To filter the results by a date range, use the box. You can specify a date range up to 90 days.

For more advanced filters that you can also save and use later, click **Filter** ![Filter icon](../../media/modern-eac-filter-icon.png) and select **New filter**. In the **Custom filter** flyout that appears, enter the following information:

- **Name your filter**: Enter a unique name.
- Click **Add new clause**. A clause contains the following elements that you need to enter:
  - **Field**: Select **Sender address**, **Domain**, **TLS 1.0**, **TLS 1.1**, **TLS 1.2**, or **Messages sent**.
  - **Operator**: Select **starts with** or **is**.
  - **Value**: Enter the value you want to search for.

  You can click **Add new clause** as many times as you need. Multiple clauses use AND logic (\<Clause1\> AND \<Clause2\>...).

  To remove a clause, click **Remove** ![Remove icon](../../media/modern-eac-remove-icon.png)

  When you're finished, click **Save**. The new filter is automatically loaded, and the results are changed based on the filter. This is the same result as clicking **Filter** and selecting the customer filter from the list.

  To unload a existing filter (return to the default list), click **Filter** ![Active filter icon](../../media/modern-eac-filter-active-icon.png) and select **Clear all filters**.

Click **Export** to export the displayed results to a .csv file.

If you select a row, a details pane for the sender appears that contains the same information from the main report.
