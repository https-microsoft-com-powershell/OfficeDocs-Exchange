---
localization_priority: Normal
description: Exchange Server 2016 or Exchange Server 2019 Setup detected that you're installing Exchange on a domain controller in an Active Directory split permissions organization.
ms.topic: reference
author: msdmaguire
ms.custom:
- ms.exch.setupreadiness.InstallOnDCInADSplitPermissionMode
ms.author: dmaguire
ms.assetid: 977e3758-5e09-40a2-80c1-fe344b1d8a2a
ms.reviewer: 
title: Installation on domain controllers is not supported with Active Directory split permissions [InstallOnDCInADSplitPermissionMode]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Installation on domain controllers is not supported with Active Directory split permissions [InstallOnDCInADSplitPermissionMode]

Exchange Setup detected that you're installing Exchange on an Active Directory domain controller and one of the following conditions is true:

- The Exchange organization is already configured for Active Directory split permissions.

- You selected the Active Directory split permissions option in Exchange Setup.

Installing Exchange on domain controllers isn't supported when the Exchange organization is configured for Active Directory split permissions. To install Exchange on a domain controller, you need to configure the Exchange organization for Role Based Access Control (RBAC) split permissions or shared permissions.

> [!IMPORTANT]
> We don't recommend installing Exchange on Active Directory domain controllers. For more information, see [Installing Exchange on a domain controller is not recommended [WarningInstallExchangeRolesOnDomainController]](ms-exch-setupreadiness-warninginstallexchangerolesondomaincontroller.md).

If you want to use Active Directory split permissions, you need install Exchange on a member server.

For more information about split and shared permissions in Exchange 2013 or later, see the following topics:

- [Understanding Split Permissions](../../../ExchangeServer2013/understanding-split-permissions-exchange-2013-help.md)

- [Configure Exchange 2013 for Split Permissions](../../../ExchangeServer2013/configure-exchange-2013-for-split-permissions-exchange-2013-help.md)

- [Configure Exchange 2013 for Shared Permissions](../../../ExchangeServer2013/configure-exchange-2013-for-shared-permissions-exchange-2013-help.md)

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).