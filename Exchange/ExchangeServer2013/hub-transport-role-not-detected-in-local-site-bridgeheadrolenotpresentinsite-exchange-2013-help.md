---
title: 'Hub Transport role not detected in local site'
TOCTitle: Hub Transport role not detected in local site_BridgeheadRoleNotPresentInSite
ms:assetid: f318c947-81a8-4c18-975a-0f1e7868042a
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.bridgeheadrolenotpresentinsite(v=EXCHG.150)
ms:contentKeyID: 46629198
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Hub Transport role not detected in local site\_BridgeheadRoleNotPresentInSite

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Microsoft® Exchange Server 2007 setup displays this warning because an existing Hub Transport server role could not be detected in the local Active Directory® directory service site.

You have chosen to install the Mailbox Server role before an instance of the Hub Transport role is installed in the Active Directory site.

Exchange 2007 Hub Transport Services are deployed inside your organization's Active Directory. Hub Transport Services handle all mail flow inside the organization, applies organizational mail flow routing rules, and are responsible for delivering messages to a recipient's mailbox.

Users will be able to log on to their mailboxes, but mail cannot be sent or received from this mailbox server until a Hub Transport role is installed.
