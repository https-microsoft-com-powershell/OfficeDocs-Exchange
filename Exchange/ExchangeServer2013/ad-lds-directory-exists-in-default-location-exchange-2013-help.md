---
title: 'AD LDS directory exists in default location: Exchange 2013 Help'
TOCTitle: AD LDS directory exists in default location
ms:assetid: cf830dec-dd74-47b2-bee2-b8956f8023ce
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.adamdatapathexists(v=EXCHG.150)
ms:contentKeyID: 46629121
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# AD LDS directory exists in default location

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 Setup can't continue because the attempt to install Active Directory Lightweight Directory Services (AD LDS) failed.

An older installation of AD LDS exists in the default location. Setup can't perform a new AD LDS install in an existing AD LDS directory structure.

To resolve this issue, remove the existing AD LDS directory and then run Setup again.

For more information about AD LDS directory management, see [Administering AD LDS Directory Partitions](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816929(v=ws.10)).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Did you find what you're looking for? Please take a minute to [send us feedback](mailto:exsetuphelpfeedback@microsoft.com?subject=exchange%202013%20setup%20help%20feedback) about the information you were hoping to find.