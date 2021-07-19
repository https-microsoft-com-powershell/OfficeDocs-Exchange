---
title: "Current account isn't logged into an Active Directory domain"
TOCTitle: The current account isn't logged into an Active Directory domain
ms:assetid: 0e229d10-605a-420f-bf8b-58a7fcb5b259
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.loggedontodomain(v=EXCHG.150)
ms:contentKeyID: 46628803
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# The current account isn't logged into an Active Directory domain

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 Setup can't continue because it detected that the current account isn't logged on to an Active Directory domain. You must log in using an Active Directory account that has the permissions required to install Exchange Server 2013.

Setup requires that the user who is logged on when Exchange 2013 is installed has permission to create and modify objects in Active Directory. If you're running Exchange 2013 Setup in your organization for the first time, the account you use must be a member of the Schema Admins and Enterprise Admins groups. These permissions are required because Active Directory is prepared for Exchange 2013 the first time Setup is run. After Active Directory is prepared, the account you use to install additional Exchange 2013 servers must be a member of the Organization Management management role group.

To resolve this issue, grant the logged-on user the appropriate permissions, or log on with an account that has those permissions and run Exchange 2013 Setup again.

> [!IMPORTANT]
> Cross-forest installation of Exchange 2013 isn't supported. Use an account that is a member of the Active Directory forest where you're installing Exchange 2013.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Did you find what you're looking for? Please take a minute to [send us feedback](mailto:exsetuphelpfeedback@microsoft.com?subject=exchange%202013%20setup%20help%20feedback) about the information you were hoping to find.
