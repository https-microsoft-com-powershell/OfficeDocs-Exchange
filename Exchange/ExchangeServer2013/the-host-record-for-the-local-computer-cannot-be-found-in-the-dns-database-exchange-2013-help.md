---
title: "Can't find Host record for local computer in the DNS database"
TOCTitle: The Host record for the local computer cannot be found in the DNS database
ms:assetid: 2f18cb65-29fe-4b72-8d68-52fd503d5673
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.hostrecordmissing(v=EXCHG.150)
ms:contentKeyID: 46628853
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# The Host record for the local computer cannot be found in the DNS database

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 Setup can't continue because the Host (A) record for this computer can't be found in the Domain Name System (DNS) database.

Exchange 2013 Setup requires that the local computer have a valid HOST (A) record registered with the authoritative DNS database for the domain.

Exchange depends on DNS Host (A) records for the IP Address of its next internal or external destination server.

To resolve this issue:

- Verify that the local TCP/IP configuration points to the correct DNS server. For more information, see [Configure TCP/IP settings](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731673(v=ws.10)).

- Use Nslookup.exe to verify that the Host (A) record exists on the DNS server. For more information, see [To verify A resource records exist in DNS](/previous-versions/orphan-topics/ws.10/cc772976(v=ws.10)).

For information about DNS name resolution, troubleshooting, and Host (A) records, see the following:

- [Troubleshooting DNS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753041(v=ws.11))

- [Managing resource records](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754308(v=ws.11))

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Did you find what you're looking for? Please take a minute to [send us feedback](mailto:exsetuphelpfeedback@microsoft.com?subject=exchange%202013%20setup%20help%20feedback) about the information you were hoping to find.