---
title: 'Local computer is a domain controller of a child domain'
TOCTitle: The local computer is a domain controller of a child domain_LocalComputerIsDCInChildDomain
ms:assetid: 7db1dcc0-d953-41b8-b081-2a47a70950c4
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.localcomputerisdcinchilddomain(v=EXCHG.150)
ms:contentKeyID: 46628987
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# The local computer is a domain controller of a child domain\_LocalComputerIsDCInChildDomain

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Microsoft® Exchange Server 2007 setup cannot continue because the local computer is a domain controller for a child domain.

Exchange 2007 setup will not install on to a domain controller for a child domain unless the domain controller is a global catalog server.

To resolve this issue, promote the domain controller to a global catalog server or install Exchange 2007 to a non-domain controller, a member server, in the child domain, and then rerun the Microsoft Exchange setup.

**To correct this warning by making the Exchange server a global catalog server**

1. On the domain controller, click **Start**, point to **Programs**, click **Administrative Tools**, and then click **Active Directory Sites and Services**.

2. In the console tree, double-click **Sites**, double-click the name of the site, and then double-click **Servers**.

3. Double-click the target domain controller.

4. In the results pane, right-click **NTDS Settings**, and then click **Properties**.

5. On the **General** tab, click to select the **Global catalog** check box.

6. Restart the domain controller.
