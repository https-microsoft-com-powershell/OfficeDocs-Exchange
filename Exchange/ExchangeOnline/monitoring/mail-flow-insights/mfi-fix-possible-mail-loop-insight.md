---
title: "Fix possible mail loop insight in the new EAC"
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
description: "Admins can learn how to use the Fix possible mail loop insight in the new Exchange admin center to identify and fix mail loops in their organization."
---

# Fix possible mail loop insight in the new EAC

A mail loop is bad because it wastes system resources, consumes your organization's mail volume quota, and sends confusing non-delivery reports (also known as NDRs or bounce messages) to the original senders.

The **Fix possible mail loop** insight in the Insights dashboard in the new Exchange admin center (new EAC) reports when a mail loop is detected in your organization, the email domains that are involved in the loop, and the number of messages from the previous day that were in the loop.

![Fix possible mail loop insight in the Insights dashboard](../../media/mfi-fix-possible-mail-loop-insight.png)

You can click **View details** to see the details in a flyout where we identify the most common mail loop scenarios and provide the recommended actions (if available) to fix the loop.

![Details flyout that appears after clicking View details in the Fix possible mail loop insight](../../media/mfi-fix-possible-mail-loop-insight-details.png)


## Related topics

For more information about other mail flow insights in the mail flow dashboard, see [Mail flow insights in the new Exchange admin center](mail-flow-insights.md).
