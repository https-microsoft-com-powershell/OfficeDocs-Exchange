---
title: "Queued messages report in the new EAC"
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
description: "Admins can learn how to use the Non-delivery details report in the new Exchange admin center to monitor outbound messages that were sent over connectors from your organization that have been delayed for over an hour."
---

# Queued messages report in the new Exchange admin center

When messages can't be sent from your organization to your on-premises or partner email servers using connectors, the messages are queued in Microsoft 365. Common examples that cause this condition are:

- The connector is incorrectly configured.
- There have been networking or firewall changes in your on-premises environment.

Microsoft 365 will continue to retry to delivery for 24 hours. After 24 hours, the messages will expire and will be returned to the senders in non-delivery reports (also known as a NDRs or bounce messages).

If the queued email volume exceeds the pre-defined threshold (the default value is 200 messages), the information is available in the following locations:

- The **Queued messages report** report in the new Exchange admin center (new EAC). For more information, see the [Queues](#queues) section in this topic.

  For permissions that are required to use this report, see [Permissions required to view mail flow reports](mail-flow-reports.md#permissions-required-to-view-mail-flow-reports).

- An alert is displayed on the **Alerts** page in the Microsoft 365 Defender portal (<https://security.microsoft.com> \> **Incidents & alerts** \> **Alerts** or <https://security.microsoft.com/alerts>).
- Admins will receive an email notification based on the configuration of the default alert policy named **Messages have been delayed**. To configure the notification settings for this alert, see the next section.

  For more information about alert policies, see [Alert policies in the Microsoft 365 compliance center](/microsoft-365/compliance/alert-policies).

## Customize queue alerts

1. In the Microsoft 365 Defender portal (<https://security.microsoft.com>), go to **Incidents & alerts** \> **Alerts** \> **Alert policy** or go directly to <https://security.microsoft.com/alertpolicies>.

2. On the **Alert policies** page, find and select the policy named **Messages have been delayed** by clicking on the name. You can sort the policies by name or use the **Search** box.

3. In the **Message have been delayed** flyout that appears, you can turn the alert on or off and configure the notification settings.

   ![Messages have been delayed alert policy details the Microsoft 365 Defender portal](../../media/mfr-queued-messages-alert-policy.png)

   - **Status**: You can toggle the alert on or off.
   - **Email recipients** and **Daily notification limit**: Click the **Edit** link or the **Edit policy** button to configure the settings as described in the next step.

4. In the **Edit policy** flyout that appears, configure the following settings:
   - **Send email notifications**: The default value is **On** (selected).
   - **Email recipients**: The default value is **TenantAdmins**.
   - **Daily notification limit**: The default value is **No limit**.
   - **Threshold**: The default value is 2000.

   ![Notification settings in the Messages have been delayed alert policy details](../../media/mfr-queued-messages-alert-policy-notification-settings.png)

5. When you're finished, click **Save** and **Close**.

## Queues

Even if the queued message volume hasn't exceeded the threshold and generated an alert, you can still use the **Queued messages report** in the new EAC to see messages that have been queued for more than one hour, and take action before the number of queued messages becomes too large.

![Queued messages report in the new EAC](../../media/mfr-queued-messages-report.png)

The same information and fix option is displayed after you click **View queue** in the details of a **Messages have been delayed** alert.

## See also

For more information about other mail flow reports, see [Mail flow reports in the new EAC](mail-flow-reports.md).
