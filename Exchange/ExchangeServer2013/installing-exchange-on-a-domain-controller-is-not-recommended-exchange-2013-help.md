---
title: 'Installing Exchange on a domain controller is not recommended'
TOCTitle: Installing Exchange on a domain controller is not recommended
ms:assetid: 48922de2-a68c-4092-96a5-d38c8e5f49f5
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.warninginstallexchangerolesondomaincontroller(v=EXCHG.150)
ms:contentKeyID: 46628923
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Installing Exchange on a domain controller is not recommended

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 Setup has detected that the computer you're attempting to install Exchange 2013 on is an Active Directory domain controller. Installing Exchange 2013 on a domain controller isn't recommended.

If you install Exchange 2013 on a domain controller, be aware of the following issues:

  - Configuring Exchange 2013 for Active Directory split permissions isn't supported.

  - The Exchange Trusted Subsystem universal security group (USG) is added to the Domain Admins group when Exchange is installed on a domain controller. When this occurs, all Exchange servers in the domain are granted domain administrator rights in that domain.

  - Exchange Server and Active Directory are both resource-intensive applications. There are performance implications to be considered when both are running on the same computer.

  - You must make sure that the domain controller Exchange 2013 is installed on is a global catalog server.

  - Exchange services may not start correctly when the domain controller is also a global catalog server.

  - System shutdown will take considerably longer if Exchange services aren't stopped before shutting down or restarting the server.

  - Demoting a domain controller to a member server isn't supported.

  - Running Exchange 2013 on a clustered node that is also an Active Directory domain controller isn't supported.

We recommend that you install Exchange 2013 on a member server.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Did you find what you're looking for? Please take a minute to [send us feedback](mailto:exsetuphelpfeedback@microsoft.com?subject=exchange%202013%20setup%20help%20feedback) about the information you were hoping to find.
