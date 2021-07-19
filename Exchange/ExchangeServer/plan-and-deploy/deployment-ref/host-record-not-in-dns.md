---
localization_priority: Normal
description: Exchange Server 2016 or Exchange Server 2019 can't continue because the target server's A record is missing from DNS.
ms.topic: reference
author: msdmaguire
ms.custom:
- ms.exch.setupreadiness.HostRecordMissing
ms.author: dmaguire
ms.assetid: 2f18cb65-29fe-4b72-8d68-52fd503d5673
ms.reviewer: 
title: The Host record for the local computer cannot be found in the DNS database [HostRecordMissing]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: Developer
ms.prod: exchange-server-it-pro
manager: serdars

---

# The Host record for the local computer cannot be found in the DNS database [HostRecordMissing]

Exchange Setup can't continue because the Host (A) record for this computer can't be found in the DNS zone for the domain. Setup requires a valid A record for the server, and Exchange uses email server A records to find the IP address of the next hop to send messages.

To resolve this issue:

- Verify that the local TCP/IP configuration points to the correct DNS server. For more information, see [Configure TCP/IP settings](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731673(v=ws.10)).

- Use Nslookup.exe to verify that the Host (A) record exists on the DNS server. For more information, see [To verify A resource records exist in DNS](/previous-versions/orphan-topics/ws.10/cc772976(v=ws.10)).

For information about DNS name resolution, troubleshooting, and A records, see the following:

- [Troubleshooting DNS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753041(v=ws.11))

- [Managing resource records](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754308(v=ws.11))

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).