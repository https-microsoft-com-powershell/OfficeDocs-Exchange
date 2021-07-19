---
localization_priority: Normal
description: Exchange Server 2016 or Exchange Server 2019 Setup can't continue because Active directory doesn't exist or can't be contacted.
ms.topic: reference
author: msdmaguire
ms.custom:
- ms.exch.setupreadiness.CannotAccessAD
ms.author: dmaguire
ms.assetid: 56adb6fe-ecb8-4a7f-b440-89aa401c28b7
ms.reviewer: 
title: Active Directory does not exist or cannot be contacted [CannotAccessAD]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: Developer
ms.prod: exchange-server-it-pro
manager: serdars

---

# Active Directory does not exist or cannot be contacted [CannotAccessAD]

Exchange Setup can't continue because it can't contact a valid Active Directory site. Setup requires that the target server is able to locate the configuration naming context in Active Directory.

To resolve this issue, verify that the account that you're using an Active Directory account to run Setup and then try running Setup again. If this doesn't resolve the issue, follow the guidance about using the support tools in the following topics to further diagnose the problem.

For more information about Active Directory troubleshooting and configuration for Exchange, see the following topics:

- [Prepare Active Directory and domains](../../plan-and-deploy/prepare-ad-and-domains.md)

- [Troubleshooting Active Directory Domain Services](/windows-server/identity/ad-ds/manage/ad-ds-troubleshooting)

- [Configuring a Computer for Troubleshooting](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc990293(v=ws.10))

- [Troubleshooting Active Directory Replication Problems](/windows-server/identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems)

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).