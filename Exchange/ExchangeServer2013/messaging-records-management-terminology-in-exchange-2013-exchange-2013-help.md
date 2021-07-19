---
title: 'Messaging records management terminology in Exchange 2013: Exchange 2013 Help'
TOCTitle: Messaging records management terminology in Exchange 2013
ms:assetid: de3e3503-6de3-4666-aeb9-cd877efb93bb
ms:mtpsurl: https://technet.microsoft.com/library/Bb408414(v=EXCHG.150)
ms:contentKeyID: 49289434
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Messaging records management terminology in Exchange 2013

_**Applies to:** Exchange Server 2013_

This topic defines the core components associated with messaging records management (MRM) in Microsoft Exchange Server 2013. MRM is a records management technology in Exchange 2013 that helps organizations reduce the risks associated with e-mail and other communications. MRM makes it easier to keep messages needed to comply with company policy, government regulations, or legal needs, and to remove content that has no legal or business value.

- **default policy tag (DPT)**: A DPT is a retention tag that applies to all items in a mailbox that don't already have a retention tag applied. You can have only one DPT in a retention policy.

- **filer user or filer**: A user who regularly files mailbox items into folders. Compare to *piler user** or **piler*.

- **journaling**: The ability to record communications, including e-mail communications, in an organization for use in the organization's e-mail retention or archival strategy. In MRM, journaling commonly refers to the process of sending a journal report about messages in a managed folder to the specified SMTP address. This process is performed by the Managed Folder Assistant.

- **managed content settings**: The retention information created for managed folders, the MRM feature available in Exchange Server 2007 and Exchange Server 2010. A managed folder can have multiple content settings for different message types such as e-mail messages, voice mail, and calendar items. Message retention settings defined in content settings for a managed folder apply to messages in that managed folder.

- **managed folder**: Used for the MRM functionality in Exchange Server 2007 and Exchange Server 2010, a managed folder is an Active Directory object that represents a mailbox folder in order to apply managed content settings to it. In a mailbox, a managed folder is a folder to which managed content settings have been applied.

- **Managed Folder Assistant**: One of the Microsoft Exchange Mailbox Assistants in Exchange 2013. The Managed Folder Assistant is responsible for archiving, message expiration, and compliance. It processes mailboxes and applies retention policies.

- **managed folder mailbox policy**: A logical grouping of managed folders. In Exchange 2010 and Exchange 2007, when a managed folder mailbox policy is applied to a user's mailbox, all managed folders linked to the policy are deployed in a single operation.

- **personal tag**: A personal tag is a retention tag available to Outlook Web App and Outlook 2010 and later users for applying retention settings to custom folders and individual items, such as e-mail messages.

- **piler user or piler**: A user who doesn't file mailbox items regularly. Piler users tend to have a large Inbox and rely on search to find messages. Compare to *filer user* or *filer*.

- **policy**: See *retention policy* or *managed folder mailbox policy*.

- **retention policy tag (RPT)**: A RPT is a retention tag that's applied to default folders such as Inbox and Deleted Items.

- **retention policy**: A retention policy is logical grouping of retention tags. When a retention policy is applied to a user's mailbox, all retention tags linked to the policy are deployed in a single operation.

- **retention tag**: Retention tags are used to apply retention settings to messages and folders in user mailboxes. There are three types of retention tags:

  - Default policy tags (DPTs)

  - Retention policy tags (RPTs)

  - Personal tags
