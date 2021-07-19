---
title: 'Exchange Server 2013 Performance Recommendations: Exchange 2013 Help'
TOCTitle: Exchange Server 2013 Performance Recommendations
ms:assetid: 6d0aea68-10d5-4a18-b632-a814ce3daa43
ms:mtpsurl: https://technet.microsoft.com/library/Dn879084(v=EXCHG.150)
ms:contentKeyID: 63917937
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange Server 2013 Performance Recommendations

_**Applies to:** Exchange Server 2013_

Exchange Server 2013 performance tuning and troubleshooting is most effective when your environment has been properly sized and planned. While Exchange 2013 was designed to simplify the underlying resource infrastructure, it can still consume a large amount of system resources, such as memory, storage capacity, and CPU capacity.

The articles in this section were written by the Exchange performance team. They contain expertise from the Exchange product group, as well as best practices learned from customer support cases. The goal of these articles is to help you understand the impact of changes introduced in Exchange 2013, and the importance of appropriately sizing your Exchange 2013 infrastructure. We've also included recommended optimizations and guidance on identifying performance issues.

## Architectural Changes in Exchange 2013 and other Resources

The architectural changes in Exchange 2013 are already documented on TechNet and in the [Exchange Team Blog](https://techcommunity.microsoft.com/t5/exchange-team-blog/bg-p/Exchange). We'll first touch upon a few high level changes you should consider in order to better understand performance cost and sizing. Then, below, we've included a list of recommended references to provide further context and background in these important areas.

> [!NOTE]
> Please see <A href="exchange-2013-virtualization-exchange-2013-help.md">Exchange 2013 virtualization</A> for performance optimization guidance about deploying Exchange Server 2013 in a virtualized environment.

In Exchange 2013, the Client Access server role is a stateless proxy server. Now the Client Access server role's primary responsibility is to authenticate incoming requests and then proxy each request to the appropriate Mailbox server, the one hosting the active copy of the user mailbox. This means it's no longer necessary to configure affinity between the Client Access server and load balancer for specific protocols.

Another noteworthy change in Exchange 2013 is in the Information Store. The Information Store now consists of two kinds of processes: host and worker. Each database instance is associated with its own Microsoft.Exchange.Store.Worker.exe process. This allows for better isolation of problematic database issues, and can reduce the performance impact of a database problem to just the one worker instance for that database.

The Microsoft Exchange Replication service is responsible for all high availability services related to the Mailbox Server role. This replication service hosts the Active Manager component, which is responsible for monitoring failures and taking corrective actions.

A great post on architectural changes, including the impact to re-sizing an Exchange 2013 environment from earlier versions, can be found in [Exchange 2013 Server Role Architecture](https://techcommunity.microsoft.com/t5/exchange-team-blog/exchange-2013-server-role-architecture/ba-p/596821).

More about Exchange 2013 architectural changes, and background information on other relevant areas, can be found in the following:

[Exchange Server 2013 Architecture](https://channel9.msdn.com/events/TechEd/NewZealand/2014/OFC306)

[Plan it the right way: Exchange Server 2013 sizing scenarios](https://channel9.msdn.com/events/MEC/2014/ARC308)

[Monitoring and Tuning Microsoft Exchange Server 2013 Performance](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/OFC-B321)

[Implementing Exchange Server 2013: (01) Upgrade and Deploy Exchange Server 2013](https://channel9.msdn.com/Series/Implementing-Exchange-Server-2013/01)

[Implementing Exchange Server 2013: (02) Plan It the Right Way: Exchange Server 2013 Sizing](https://channel9.msdn.com/Series/Implementing-Exchange-Server-2013/02)

[Implementing Exchange Server 2013: (03) Exchange Server 2013 Virtualization Best Practices](https://channel9.msdn.com/Series/Implementing-Exchange-Server-2013/03)

[Implementing Exchange Server 2013: (04) Exchange Architecture: High Availability and Site Resilience](https://channel9.msdn.com/Series/Implementing-Exchange-Server-2013/04)

[Implementing Exchange Server 2013: (05) Outlook Connectivity](https://channel9.msdn.com/Series/Implementing-Exchange-Server-2013/05)

[The Preferred Architecture](https://techcommunity.microsoft.com/t5/exchange-team-blog/the-preferred-architecture/ba-p/586755)

[Exchange 2013 Client Access Server Role](https://techcommunity.microsoft.com/t5/exchange-team-blog/exchange-2013-client-access-server-role/ba-p/596493)

[Exchange Server 2013 Virtualization Best Practices](https://channel9.msdn.com/Events/MEC/2014/ARC305)

[Load balancing](load-balancing-exchange-2013-help.md)

[Exchange Server Updates: build numbers and release dates](../ExchangeServer/new-features/build-numbers-and-release-dates.md)

[Release notes for Exchange 2013](release-notes-for-exchange-2013-exchange-2013-help.md)

[Updates for Exchange 2013](updates-for-exchange-2013-exchange-2013-help.md)

[ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0](/archive/blogs/tmarq/asp-net-thread-usage-on-iis-7-5-iis-7-0-and-iis-6-0)