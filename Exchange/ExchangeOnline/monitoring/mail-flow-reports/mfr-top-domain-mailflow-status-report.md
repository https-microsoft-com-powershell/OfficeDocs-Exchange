---
title: "Top domain mailflow status report in the new EAC"
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
description: "Admins can learn how to use the Top domain mailflow status report in the new Exchange admin center to identify and troubleshoot mail flow in your email domains."
---

# Top domain mailflow status report in the new Exchange admin center

The **Top domain mailflow status** report in the new Exchange admin center (new EAC) returns information about whether your email domains are receiving or aren't receiving messages. Typically, these types of issues are related to MX record problems for the domain.

> [!NOTE]
> By default, the report shows data for the last 7 days. If the report is empty, try changing the date range.
>
> For permissions that are required to use this report, see [Permissions required to view mail flow reports](mail-flow-reports.md#permissions-required-to-view-mail-flow-reports).

This report shows the following information for each domain:

- **Domain**
- **Domain status**: The value will be **Healthy** or **Error**
- **Previous MX record**
- **Current MX record**
- **Email received (past 6 hours)**

To quickly filter the results, click **Search** ![Search icon](../../media/modern-eac-search-icon.png) and start typing a value.

For more advanced filters that you can also save and use later, click **Filter** ![Filter icon](../../media/modern-eac-filter-icon.png) and select **New filter**. In the **Custom filter** flyout that appears, enter the following information:

- **Name your filter**: Enter a unique name.
- Click **Add new clause**. A clause contains the following elements that you need to enter:
  - **Field**: Select **Domain**, **Domain status**, **Previous MX record**, **Current MX record**, or **Email received (past 6 hours)**.
  - **Operator**: Select **starts with** or **is**.
  - **Value**: Enter the value you want to search for.

  You can click **Add new clause** as many times as you need. Multiple clauses use AND logic (\<Clause1\> AND \<Clause2\>...).

  To remove a clause, click **Remove** ![Remove icon](../../media/modern-eac-remove-icon.png)

  When you're finished, click **Save**. The new filter is automatically loaded, and the results are changed based on the filter. This is the same result as clicking **Filter** and selecting the customer filter from the list.

  To unload a existing filter (return to the default list), click **Filter** ![Active filter icon](../../media/modern-eac-filter-active-icon.png) and select **Clear all filters**.

Click **Export** to export the displayed results to a .csv file.

If you select a row, a details pane for the domain appears based on the value of **Domain status**:

- **Healthy**: An explanation about MX records and the same information from the main report is displayed.
- **Error**: Additional information about the the cause of the error and how to fix it are available in the **Reason** and **How to fix** sections.
