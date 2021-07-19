---
title: 'Logged-on user is not a member of the local Administrators group'
TOCTitle: The logged-on user is not a member of the local Administrators group_NotLocalAdmin
ms:assetid: d06f0894-b139-49ba-afe3-f58d3bd28e32
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.notlocaladmin(v=EXCHG.150)
ms:contentKeyID: 46629123
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# The logged-on user is not a member of the local Administrators group\_NotLocalAdmin

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Microsoft® Exchange Server 2007 setup cannot continue because the logged-on user is not a member of the local computer's administrators group.

Exchange 2007 setup requires that the logged-on user who installs Microsoft Exchange has full access to the local computer.

To resolve this issue, log on to the computer by using an account that has local administrator access and then rerun Microsoft Exchange setup.
