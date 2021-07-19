---
title: "Mail flow insights in the new EAC"
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
description: Admins can learn how to use the mail flow insights dashboard in the new Exchange admin center to find and fix mail flow related issues.
ms.custom:
---

# Mail flow insights in the new Exchange admin center

Admins can use the insights dashboard in the new Exchange admin center (new EAC) to discover issues with mail flow and take corrective action. If an item appears in the mail flow insights dashboard, you need to investigate and likely fix the issue.

The following mail flow insights are available:

- [Fix possible mail loop insight](mfi-fix-possible-mail-loop-insight.md)

- [Fix slow mail flow rules insight](mfi-fix-slow-mail-flow-rules-insight.md)

- [New domains being forwarded email insight](mfi-new-domains-being-forwarded-email-insight.md)

- [New users forwarding email insight](mfi-new-users-forwarding-email-insight.md)

- [Domain expiring soon insight](domain-expiry-notification-feature.md)

## Permissions required to view and use mail flow insights

To use the mail flow insights to take corrective action, you need to be a member of one of the following role groups:

- Organization Management
- Security Administrator<sup\>*</sup>

For read only access to the mail flow insights, you need to be a member of one of the following role groups:

- Security Reader<sup>\*</sup>
- View-only Organization Management
- View-Only Recipients

For more information, see [Permissions in Exchange Online](../../permissions-exo/permissions-exo.md) and [Manage role groups in Exchange Online](../../permissions-exo/role-groups.md).

<sup\>*</sup> You manage these role groups in the [Azure Active Directory admin center](https://aad.portal.azure.com).

## Where to find mail flow insights

To see the mail flow insights, open the new EAC at <https://admin.exchange.microsoft.com> and select **Insights**.

To go directly to the mail flow insights dashboard, open <https://admin.exchange.microsoft.com/#/insights>.
