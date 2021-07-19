---
localization_priority: Normal
description: Exchange Server 2016 or Exchange Server 2019 Setup can't continue because an AD LDS directory exists in the default location.
ms.topic: reference
author: msdmaguire
ms.custom:
- ms.exch.setupreadiness.ADAMDataPathExists
ms.author: dmaguire
ms.assetid: cf830dec-dd74-47b2-bee2-b8956f8023ce
ms.reviewer: 
title: AD LDS directory exists in default location [ADAMDataPathExists]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# AD LDS directory exists in default location [ADAMDataPathExists]

Exchange Setup can't continue because the attempt to install Active Directory Lightweight Directory Services (AD LDS) failed.

An older installation of AD LDS exists in the default location. Setup can't perform a new AD LDS install in an existing AD LDS directory structure.

To resolve this issue, remove the existing AD LDS directory and then run Setup again.

For more information about AD LDS directory management, see [Administering AD LDS Directory Partitions](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816929(v=ws.10)).

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).