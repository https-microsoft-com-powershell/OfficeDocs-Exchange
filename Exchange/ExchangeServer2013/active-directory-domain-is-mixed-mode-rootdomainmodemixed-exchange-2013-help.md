---
title: 'Active Directory domain is mixed mode_RootDomainModeMixed: Exchange 2013 Help'
TOCTitle: Active Directory domain is mixed mode_RootDomainModeMixed
ms:assetid: 9f60096e-3eaa-40d8-bde5-13ada5855702
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.rootdomainmodemixed(v=EXCHG.150)
ms:contentKeyID: 46629053
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Active Directory domain is mixed mode\_RootDomainModeMixed

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Microsoft® Exchange Server 2007 setup cannot continue because an existing Active Directory domain is not set to Microsoft Windows®°2000 Server native mode or better.

Exchange 2007 setup will create Universal Security Groups that can only exist in Windows 2000 Server native mode, or better, domains.

To resolve this issue, follow these steps to raise the domain functional level to at least the Windows 2000 Server native level, and then rerun Exchange 2007 setup.

**To raise the domain functional level**

1. Open Active Directory Domains and Trusts.

2. In the console tree, right-click the domain for which you want to raise functionality, and then click **Raise Domain Functional Level**.

3. In **Select an available domain functional level**, use one of the following procedures:

   - To raise the domain functional level to Windows 2000 Server native, click **Windows 2000 native**, and then click **Raise**.

   - To raise domain functional level to Windows Server® 2003, click **Windows Server 2003**, and then click **Raise.**

> [!WARNING]
> <BR>If you have or will have any domain controllers running Windows NT®&nbsp;4.0 and earlier, do not raise the domain functional level to Windows&nbsp;2000 Server native. After the domain functional level is set to Windows&nbsp;2000 Server native, it cannot be changed back to Windows&nbsp;2000 Server mixed.<BR>If you have or will have any domain controllers running Windows NT&nbsp;4.0 and earlier or Windows&nbsp;2000 Server, do not raise the domain functional level to Windows Server&nbsp;2003. After the domain functional level is set to Windows Server&nbsp;2003, it cannot be changed back to Windows&nbsp;2000 Server mixed or Windows&nbsp;2000 Server native.
