---
localization_priority: Normal
description: Email has become a reliable and ubiquitous communication medium for information workers in organizations of all sizes. Messaging stores and mailboxes have become repositories of valuable data. It's important for organizations to formulate messaging policies that dictate the fair use of their messaging systems, provide user guidelines for how to act on the policies, and where required, provide details about the types of communication that may not be allowed.
ms.topic: hub-page
author: msdmaguire
ms.author: dmaguire
ms.assetid: 65f20a20-27a4-4f6e-9b27-f8705d65b8d9
ms.reviewer: 
title: Messaging policy and compliance in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Messaging policy and compliance in Exchange Server

Email has become a reliable and ubiquitous communication medium for information workers in organizations of all sizes. Messaging stores and mailboxes have become repositories of valuable data. It's important for organizations to formulate messaging policies that dictate the fair use of their messaging systems, provide user guidelines for how to act on the policies, and where required, provide details about the types of communication that may not be allowed.

Organizations must also create policies to manage email lifecycle, retain messages for the length of time based on business, legal, and regulatory requirements, preserve email records for litigation and investigation purposes, and be prepared to search and provide the required email records to fulfill eDiscovery requests.

## Messaging policy and compliance in Exchange Server

The following table provides an overview of the messaging policy and compliance features in Exchange 2016 and Exchange 2019 and includes links to topics that will help you learn about and use these features.

|**Feature**|**Description**|**Resources**|
|:-----|:-----|:-----|
|In-Place Archiving| *In-Place Archiving* helps you regain control of your organization's messaging data by eliminating the need for personal store (.pst) files and allowing users to store messages in an *archive mailbox* accessible in Outlook 2010 and later and Outlook on the web.|[In-Place Archiving in Exchange Server](in-place-archiving/in-place-archiving.md)|
|In-Place Hold and Litigation Hold|When a reasonable expectation of litigation exists, organizations are required to preserve electronically stored information, including email that's relevant to the case. In-Place Hold allows you to search and preserve messages matching query parameters. Litigation Hold only allows you to place all items in a mailbox on hold. For both types of holds, messages are protected from permanent deletion, modification, and tampering and can be preserved indefinitely or for a specified period.|[In-Place Hold and Litigation Hold in Exchange Server](holds/holds.md)|
|In-Place eDiscovery|In-Place eDiscovery allows you to search mailbox data across your Exchange organization, preview search results, copy search results to a Discovery mailbox, or export the results to a PST file|[In-Place eDiscovery in Exchange Server](ediscovery/ediscovery.md)|
|Administrator audit logging|Administrator audit logs enable you to keep a log of changes made by administrators to Exchange server and organization configuration and to Exchange recipients. You might use administrator audit logging as part of your change control process or to track changes and access to configuration and recipients for compliance purposes.|[Administrator audit logging in Exchange Server](admin-audit-logging/admin-audit-logging.md)|
|Mailbox audit logging|Because mailboxes can potentially contain sensitive, high business impact information and personally identifiable information, it's important that you track who logs on to the mailboxes in your organization and what actions are taken. It's especially important to track access to mailboxes by users other than the mailbox owner (known as delegate users). Using mailbox audit logging, you can log mailbox access by administrators, delegates (including administrators with full access permissions), and mailbox owners.|[Mailbox audit logging in Exchange Server](mailbox-audit-logging/mailbox-audit-logging.md)|
|Data loss prevention|Data loss prevention (DLP) in Exchange Server includes 80 sensitive information types that are ready for you to use in your DLP policies.|[Sensitive information types in Exchange Server](data-loss-prevention/sensitive-information-types.md)|
|Mail flow rules (also known as transport rules)|Use mail flow rules to look for specific conditions in messages that pass through your organization and take action on them. You can use conditions and exceptions to define when a mail flow rule is applied, and then apply an action on messages when the conditions are met.|[Mail flow rule conditions and exceptions (predicates) in Exchange Server](mail-flow-rules/conditions-and-exceptions.md) <br/> [Mail flow rule actions in Exchange Server](mail-flow-rules/actions.md)|
