---
title: "Setup can't install Exchange on a read-only domain controller"
TOCTitle: Setup cannot install Exchange to a read-only domain controller_ComputerRODC
ms:assetid: 4934d755-65be-47e2-86b0-6ea1ab148a96
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.computerrodc(v=EXCHG.150)
ms:contentKeyID: 46628890
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Setup cannot install Exchange to a read-only domain controller\_ComputerRODC

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Microsoft Exchange Server 2007 Setup cannot continue the installation because the target computer is a Read-Only Domain Controller (RODC).

Read-Only Domain Controllers are a feature of Windows Server 2008 Active Directory. The RODC is a type of domain controller install option that can be installed in remote sites that may have lower levels of physical security.

Exchange Setup requires write access to the target install computer.

To resolve this issue, select a target install computer that is not designated as an RODC, and rerun Setup.
