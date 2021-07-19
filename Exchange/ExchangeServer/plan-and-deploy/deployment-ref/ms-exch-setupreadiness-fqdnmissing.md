---
localization_priority: Normal
description: Exchange Server 2016 or Exchange Server 2019 Setup can't continue because the primary DNS suffix hasn't been configured on the target server.
ms.topic: reference
author: msdmaguire
ms.custom:
- ms.exch.setupreadiness.FqdnMissing
ms.author: dmaguire
ms.assetid: 310765bf-a650-4a3d-a5e4-6173b559d4f6
ms.reviewer: 
title: Primary DNS Suffix is missing [ms.exch.setupreadiness.FqdnMissing]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Primary DNS Suffix is missing [ms.exch.setupreadiness.FqdnMissing]

Exchange Setup can't continue because the primary DNS suffix (for example, contoso.com) hasn't been configured on the target server. Typically, you'll encounter this error when you're trying to install the Edge Transport server role.

To resolve this issue, add a primary DNS suffix on the computer and then run Setup again.

1. Replace \<Value\> with the DNS suffix you want to use (for example, contoso.com), and run the following command in Winows Powershell on the target server:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name 'NV Domain' -Value <Value>
```

2. Restart the computer and run Setup again.

> [!IMPORTANT]
> Changing the computer name or primary DNS suffix after you install Exchange isn't supported.

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).
