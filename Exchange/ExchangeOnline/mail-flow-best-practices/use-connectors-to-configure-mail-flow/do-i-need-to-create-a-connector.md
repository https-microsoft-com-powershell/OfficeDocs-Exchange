---
localization_priority: Normal
description: Find your mail flow scenario to see if you need to create a connector for your Exchange Online or Exchange Online Protection organization.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 16731ae9-c909-49dd-bffc-a46e6151fc29
ms.reviewer: 
f1.keywords:
- CSH
ms.custom:
- ms.exch.eac.ConnectorIsConnectorNeeded
title: Do I need to create a connector in Exchange Online?
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Do I need to create a connector in Exchange Online?

Find your mail flow scenario to see if you need to create a connector for your Exchange Online organization.

<br>

****

|Scenario|Description|Connector required?|Connector settings|
|---|---|:---:|---|
|You have a standalone EOP subscription.|You have your own on-premises email servers, and you subscribe to EOP only for email protection services for your on-premises mailboxes (you have no mailboxes in Exchange Online). <p> For more information, see the topic [Exchange Online Protection overview](/microsoft-365/security/office-365-security/exchange-online-protection-overview) and [How connectors work with my on-premises email servers](use-connectors-to-configure-mail-flow.md#how-connectors-work-with-my-on-premises-email-servers).|Yes|**Connector for incoming email:** <ul><li>**From**: Your on-premises email server</li><li>**To**: Office 365</li></ul> <p> **Connector for outgoing email**: <ul><li>**From**: Office 365</li><li>**To**: Your on-premises mail server</li></ul>|
|Some of your mailboxes are on your on-premises email servers, and some are in Exchange Online.|Before you manually configure connectors, check whether an Exchange hybrid deployment better meets your business needs. <p> For details, see [I have my own email servers](use-connectors-to-configure-mail-flow.md#i-have-my-own-email-servers) and [Exchange Server Hybrid Deployments](../../../ExchangeHybrid/exchange-hybrid.md).|Yes|**Connector for incoming email:** <ul><li>**From**: Your on-premises email server</li><li>**To**: Office 365</li></ul> <p> **Connector for outgoing email:** <ul><li>**From**: Office 365</li><li>**To**: Your on-premises email server</li></ul>|
|All of your mailboxes are in Exchange Online, but you need to send email from sources in your on-premises organization.|You don't have your own email servers, but you need to send email from non-mailboxes: printers, fax machines, apps, or other devices. <p> For details, see [Option 3: Configure a connector to send mail using Microsoft 365 or Office 365 SMTP relay](../how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365.md#option-3-configure-a-connector-to-send-mail-using-microsoft-365-or-office-365-smtp-relay)|Optional|**Only one connector for incoming email:** <ul><li>**From**: Your organization's email server</li><li>**To**: Office 365</li></ul>|
|You frequently exchange sensitive information with business partners, and you want to apply security restrictions.|You want to use Transport Layer Security (TLS) to encrypt sensitive information or you want to limit the source (IP addresses) for email from the partner domain. <p> For details, see [Set up connectors for secure mail flow with a partner organization](set-up-connectors-for-secure-mail-flow-with-a-partner.md).|Optional|**Connector for incoming email:** <ul><li>**From**: Partner organization</li><li>**To**: Office 365</li></ul> <p> **Connector for outgoing email:**<ul><li>**From**: Office 365</li><li>**To**: Partner organization</li></ul>|
|

> [!NOTE]
> For more information about these scenarios, see [Configure mail flow using connectors in Office 365](use-connectors-to-configure-mail-flow.md).
