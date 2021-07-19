---
title: Standalone Exchange Online Protection
f1.keywords: 
  - NOCSH
ms.author: chrisda
author: chrisda
manager: dansimp
ms.date: 
audience: ITPro
ms.topic: overview

localization_priority: Normal
ms.assetid:
description: Admins can learn about the the standalone Exchange Online Protection (EOP) that used to protect on-premises email environments (including hybrid environments).
ms.technology: mdo
ms.prod: m365-security
---

# Standalone Exchange Online Protection

Exchange Online Protection (EOP) is the cloud-based filtering service that helps protect your organization against spam and malware. EOP is included in all Microsoft 365 organizations with Exchange Online mailboxes. For more information, see [Exchange Online Protection overview](/microsoft-365/security/office-365-security/exchange-online-protection-overview).

However, EOP is also available in the following on-premises scenarios:

- **In a standalone scenario**: EOP provides cloud-based email protection for your on-premises Exchange organization or for any other on-premises SMTP email environment.

- **In a hybrid deployment**: EOP can be configured to protect your on-premises Exchange environment and control mail routing when you have a mix of on-premises and cloud mailboxes. For more information about hybrid, see [Exchange Server hybrid deployments](../exchange-hybrid.md).

> [!NOTE]
> If you have an on-premises Exchange organization and you've purchased additional Exchange Enterprise CAL with Services licenses, EOP is part of the included services. EOP in this environment also includes [Data loss prevention (DLP)](/exchange/security-and-compliance/data-loss-prevention/data-loss-prevention).

In these scenarios, EOP can simplify the management of your email environment and alleviate many of the burdens that come with maintaining on-premises hardware and software.

The EOP articles in this node are specifically targeted for on-premises email and hybrid scenarios.

To learn more about EOP, check out the [Exchange Online Protection service description](/office365/servicedescriptions/exchange-online-protection-service-description/exchange-online-protection-service-description).

**General**:

[Set up your standalone EOP service](set-up-your-eop-service.md)

[Best practices for configuring standalone EOP](best-practices-for-configuring-eop.md)

[Exchange admin center in standalone EOP](exchange-admin-center-eop.md)

[Protect on-premises mailboxes in China with standalone EOP](standalone-eop-china.md)

**Permissions**:

[Permissions in standalone EOP](feature-permissions-in-eop.md)

[Manage role groups in standalone EOP](manage-admin-role-group-permissions-in-eop.md)

**Recipients**:

[Manage recipients in standalone EOP](manage-recipients-in-eop.md)

[Manage groups in EOP](manage-groups-in-eop.md)

[Manage mail users in standalone EOP](manage-mail-users-in-eop.md)

**Auditing**:

[Auditing reports in standalone EOP](auditing-reports-in-eop.md)

[View the admin audit log](/exchange/security-and-compliance/exchange-auditing-reports/view-administrator-audit-log)

[Run an administrator role group report](/exchange/security-and-compliance/exchange-auditing-reports/search-role-group-changes)

**Anti-spam**:

[Configure EOP to deliver spam to Junk Email folders in hybrid environments](configure-eop-spam-protection-hybrid.md)

**Scripts**:

[Move domains and settings from one EOP organization to another](move-domains-and-settings-between-eop-orgs.md)

[Sample script for applying standalone EOP settings to multiple tenants](sample-script-standalone-eop-settings-to-multiple-tenants.md)
