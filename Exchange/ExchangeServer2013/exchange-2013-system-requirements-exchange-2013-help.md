---
title: 'Exchange 2013 system requirements: Exchange 2013 Help'
TOCTitle: Exchange 2013 system requirements
ms:assetid: 1e80857c-b870-4a6d-a0f4-ff7b3a7be037
ms:mtpsurl: https://technet.microsoft.com/library/Aa996719(v=EXCHG.150)
ms:contentKeyID: 48384881
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange 2013 system requirements

_**Applies to:** Exchange Server 2013_

Learn about the Exchange 2013 requirements that you need to know before you install Exchange 2013. For example, you'll learn about the hardware, network, and operating system requirements.

Before you install Microsoft Exchange Server 2013, we recommend that you review this topic to ensure that your network, hardware, software, clients, and other elements meet the requirements for Exchange 2013. In addition, make sure you understand the coexistence scenarios that are supported for Exchange 2013 and earlier versions of Exchange.

## Supported coexistence scenarios

The following table lists the scenarios in which coexistence between Exchange 2013 and earlier versions of Exchange is supported.

### Coexistence of Exchange 2013 and earlier versions of Exchange Server

<br>

****

|Exchange version|Exchange organization coexistence|
|---|---|
|Exchange Server 2003 and earlier versions|Not supported|
|Exchange 2007|Supported with the following minimum versions of Exchange: <ul><li><p>Update Rollup 10 for Exchange 2007 Service Pack 3 (SP3) on all Exchange 2007 servers in the organization, including Edge Transport servers.<sup>1</sup></li><li>Exchange 2013 Cumulative Update 2 (CU2) or later on all Exchange 2013 servers in the organization.</li></ul>|
|Exchange 2010|Supported with the following minimum versions of Exchange: <ul><li><p>Exchange 2010 SP3 on all Exchange 2010 servers in the organization, including Edge Transport servers).<sup>2</sup></li><li>Exchange 2013 CU2 or later on all Exchange 2013 servers in the organization.</li></ul>|
|Mixed Exchange 2010 and Exchange 2007 organization|Supported with the following minimum versions of Exchange: <ul><li><p>Update Rollup 10 for Exchange 2007 SP3 on all Exchange 2007 servers in the organization, including Edge Transport servers.<sup>1</sup></li><li>Exchange 2010 SP3 on all Exchange 2010 servers in the organization, including Edge Transport servers.<sup>2</sup></li><li>Exchange 2013 CU2 or later on all Exchange 2013 servers in the organization.</li></ul>|
|

<sup>1</sup> If you want to create an EdgeSync Subscription between an Exchange 2007 Hub Transport server and an Exchange 2013 SP1 Edge Transport server, you need to install Exchange 2007 SP3 Update Rollup 13 or later on the Exchange 2007 Hub Transport server.

<sup>2</sup> If you want to create an EdgeSync Subscription between an Exchange 2010 Hub Transport server and an Exchange 2013 SP1 Edge Transport server, you need to install Exchange 2010 SP3 Update Rollup 5 or later on the Exchange 2010 Hub Transport server.

## Supported hybrid deployment scenarios

Exchange 2013 supports hybrid deployments with Microsoft 365 or Office 365 organizations that have been upgraded to the latest version of Microsoft 365 or Office 365. For more information about specific hybrid deployments, see [Hybrid deployment prerequisites](../ExchangeHybrid/hybrid-deployment-prerequisites.md).

## Network and directory servers

The following table lists the requirements for the network and the directory servers in your Exchange 2013 organization.

<br>

****

|Component|Requirement|
|---|---|
|Schema master|By default, the schema master runs on the first Active Directory domain controller installed in a forest. The schema master must be running one of the following: <ul><li>Windows Server 2016 Standard or Datacenter<sup>1</sup></li><li>Windows Server 2012 R2 Standard or Datacenter<sup>1</sup></li><li>Windows Server 2012 R2 Standard or Datacenter<sup>1</sup></li><li>Windows Server 2012 Standard or Datacenter</li><li>Windows Server 2008 R2 Standard or Enterprise</li><li>Windows Server 2008 R2 Datacenter RTM or later</li><li>Windows Server 2008 Standard or Enterprise (32-bit or 64-bit)</li><li>Windows Server 2008 Datacenter RTM or later</li><li>Windows Server 2003 Standard Edition with Service Pack 2 (SP2) or later (32-bit or 64-bit)</li><li>Windows Server 2003 Enterprise Edition with SP2 or later (32-bit or 64-bit)</li></ul>|
|Global catalog server|In each Active Directory site where you plan to install Exchange 2013, you must have at least one global catalog server running one of the following: <ul><li>Windows Server 2016 Standard or Datacenter<sup>1</sup></li><li>Windows Server 2012 R2 Standard or Datacenter<sup>1</sup></li><li>Windows Server 2012 Standard or Datacenter</li><li>Windows Server 2008 R2 Standard or Enterprise</li><li>Windows Server 2008 R2 Datacenter RTM or later</li><li>Windows Server 2008 Standard or Enterprise (32-bit or 64-bit)</li><li>Windows Server 2008 Datacenter RTM or later</li><li>Windows Server 2003 Standard Edition with Service Pack 2 (SP2) or later (32-bit or 64-bit)</li><li>Windows Server 2003 Enterprise Edition with SP2 or later (32-bit or 64-bit)</li></ul> <p> For more information about global catalog servers, see [What is the Global Catalog](/previous-versions/windows/it-pro/windows-server-2003/cc728188(v=ws.10)).|
|Domain controller|In each Active Directory site where you plan to install Exchange 2013, you must have at least one writeable domain controller running one of the following: <ul><li>Windows Server 2016 Standard or Datacenter<sup>1</sup></li><li>Windows Server 2012 R2 Standard or Datacenter<sup>1</sup></li><li>Windows Server 2012 Standard or Datacenter</li><li>Windows Server 2008 R2 Standard or Enterprise SP1 or later</li><li>Windows Server 2008 R2 Datacenter RTM or later</li><li>Windows Server 2008 Standard or Enterprise SP1 or later (32-bit or 64-bit)</li><li>Windows Server 2008 Datacenter RTM or later</li><li>Windows Server 2003 Standard Edition with Service Pack 2 (SP2) or later (32-bit or 64-bit)</li><li>Windows Server 2003 Enterprise Edition with SP2 or later (32-bit or 64-bit)</li></ul>|
|Active Directory forest|Active Directory must be at Windows Server 2003 forest functionality mode or higher.<sup>2</sup>|
|DNS namespace support|Exchange 2013 supports the following domain name system (DNS) namespaces: <ul><li>Contiguous</li><li>Noncontiguous</li><li>Single label domains</li><li>Disjoint</li></ul> <p> For more information about DNS namespaces supported by Exchange, see [KB2269838](https://support.microsoft.com/help/2269838).|
|IPv6 support|In Exchange 2013, IPv6 is supported only when IPv4 is also installed and enabled. If Exchange 2013 is deployed in this configuration, and the network supports IPv4 and IPv6, all Exchange servers can send data to and receive data from devices, servers, and clients that use IPv6 addresses. For more information, see [IPv6 support in Exchange 2013](ipv6-support-in-exchange-2013-exchange-2013-help.md).|
|

<sup>1</sup> Windows Server 2012 R2 and Windows Server 2016 are supported only with Exchange 2013 SP1 or later.

<sup>2</sup> Windows Server 2012 R2 or Windows Server 2016 forest functionality mode is supported only with Exchange 2013 SP1 or later.

## Directory server architecture

The use of 64-bit Active Directory domain controllers increases directory service performance for Exchange 2013.

> [!NOTE]
> In multi-domain environments, on Windows Server 2008 domain controllers that have the Active Directory language locale set to Japanese, your servers might not receive some attributes that are stored on an object during inbound replication. For more information, see Microsoft Knowledge Base article 949189, <A href="https://www.microsoft.com/download/details.aspx?id=1050">A Windows Server 2008 domain controller that is configured with the Japanese language locale may not apply updates to attributes on an object during inbound replication</A>.

## Installing Exchange 2013 on directory servers

For security and performance reasons, we recommend that you install Exchange 2013 only on member servers and not on Active Directory directory servers. However, you can't run DCPromo on a computer running Exchange 2013. After Exchange 2013 is installed, changing its role from a member server to a directory server, or vice versa, isn't supported.

## Hardware

The recommended hardware requirements for Exchange 2013 servers vary depending on a number of factors including the server roles that are installed and the anticipated load that will be placed on the servers.

For detailed information on how to properly size and configure your deployment, see [Exchange 2013 Sizing and Configuration Recommendations](exchange-2013-sizing-and-configuration-recommendations-exchange-2013-help.md).

For information about deploying Exchange in a virtualized environment, see [Exchange 2013 virtualization](exchange-2013-virtualization-exchange-2013-help.md).

<br>

****

|Component|Requirement|Notes|
|---|---|---|
|Processor|<ul><li>x64 architecture-based computer with Intel processor that supports Intel 64 architecture (formerly known as Intel EM64T)</li><li>AMD processor that supports the AMD64 platform</li><li>Intel Itanium IA64 processors not supported</li></ul>|See the [Operating system](#operating-system) section later in this topic for supported operating systems.|
|Memory|Varies depending on Exchange roles that are installed: <ul><li>**Mailbox**: 8GB minimum</li><li>**Client Access**: 4GB minimum</li><li>**Mailbox and Client Access combined**: 8GB minimum</li><li>**Edge Transport**: 4GB minimum</li></ul>|None.|
|Paging file size|The page file size minimum and maximum must be set to physical RAM plus 10  MB, to a maximum size of 32778MB if you're using more than 32GB of RAM.|For detailed pagefile recommendations, see [Pagefile](exchange-2013-sizing-and-configuration-recommendations-exchange-2013-help.md#pagefile).|
|Disk space|<ul><li>At least 30 GB on the drive on which you install Exchange</li><li>An additional 500 MB of available disk space for each Unified Messaging (UM) language pack that you plan to install</li><li>200 MB of available disk space on the system drive</li><li>A hard disk that stores the message queue database on with at least 500 MB of free space.</li></ul>|For detailed information on storage recommendations, see Exchange 2013 storage configuration options.|
|Drive|DVD-ROM drive, local or network accessible|None.|
|Screen resolution|1024 x 768 pixels or higher|None.|
|File format|Disk partitions formatted as NTFS file systems, which applies to the following partitions: <ul><li>System partition</li><li>Partitions that store Exchange binary files or files generated by Exchange diagnostic logging</li></ul> <p> Disk partitions containing the following types of files can be formatted as ReFS: <ul><li>Partitions containing transaction log files</li><li>Partitions containing database files</li><li>Partitions containing content indexing files</li></ul>|None.|
|

## Operating system

The following table lists the supported operating systems for Exchange 2013.

> [!IMPORTANT]
> We don't support the installation of Exchange 2013 on a computer that's running in Windows Server Core mode. The computer must be running the full installation of Windows Server.

If you want to install Exchange 2013 on a computer that's running in Windows Server Core mode, you must convert the server to a full installation of Windows Server by doing one of the following steps:

- **Windows Server 2008 R2**: Reinstall Windows Server and select the **Full Installation** option.
- **Windows Server 2012 R2** or **Windows Server 2012**: Convert your Windows Server Core mode server to a full installation by running the following command:

  ```Powershell
  Install-WindowsFeature Server-Gui-Mgmt-Infra,Server-Gui-Shell -Restart
  ```

### Supported operating systems for Exchange 2013

<br>

****

|Component|Requirement|
|---|---|
|Mailbox, Client Access, and Edge Transport server roles|One of the following: <ul><li>Windows Server 2012 R2 Standard or Datacenter<sup>1</sup></li><li>Windows Server 2012 Standard or Datacenter</li><li>Windows Server 2008 R2 Standard with Service Pack 1 (SP1)</li><li>Windows Server 2008 R2 Enterprise with Service Pack 1 (SP1)</li><li>Windows Server 2008 R2 Datacenter RTM or later</li></ul>|
|Management tools|One of the following: <ul><li>Windows Server 2012 R2 Standard or Datacenter<sup>1</sup></li><li>Windows Server 2012 Standard or Datacenter</li><li>Windows Server 2008 R2 Standard with SP1</li><li>Windows Server 2008 R2 Enterprise with SP1</li><li>Windows Server 2008 R2 Datacenter RTM or later</li><li>64-bit edition of Windows 8.1<sup>2</sup></li><li>64-bit edition of Windows 8</li><li>64-bit edition of Windows 7 with Service Pack 1</li></ul>|
|

<sup>1</sup> Windows Server 2012 R2 is supported only with Exchange 2013 SP1 or later.

<sup>2</sup> Windows 8.1 is supported only with Exchange 2013 SP1 or later.

### Supported Windows Management Framework versions for Exchange 2013

Exchange 2013 only supports the version of Windows Management Framework that's built into the release of Windows that you're installing Exchange on. Don't install versions of Windows Management Framework that are made available as stand-alone downloads on servers running Exchange.

## .NET Framework

We strongly recommend that you use the latest version of .NET Framework that's supported by the release of Exchange you're installing.

<br>

***

|Exchange 2013 version|.NET Framework 4.8|.NET Framework 4.7.2|.NET Framework 4.7.1|.NET Framework 4.6.2|
|---|:---:|:---:|:---:|:---:|
|CU23|X|X|||
|CU21, CU22||X|X||
|CU19, CU20||X|X||
|CU16, CU17, CU18||||X|
|CU15||||X|
|

**Note**: For older versions of the .NET Framework, see the [Exchange Server supportability matrix](../ExchangeServer/plan-and-deploy/supportability-matrix.md#exchange-2013)

## Supported clients

Exchange 2013 supports the following versions of Outlook and Entourage for Mac:

- Microsoft 365 Apps for enterprise
- Outlook 2019
- Outlook 2016
- Outlook 2013
- Outlook 2010
- Outlook 2007
- Entourage 2008 for Mac, Web Services Edition
- Outlook for Mac for Office 365
- Outlook for Mac 2011

For a list of Outlook releases that Exchange supports, see [Outlook Updates](/officeupdates/outlook-updates-msi).

> [!IMPORTANT]
> We strongly recommend that you install the latest available service packs and updates available so that your users receive the best possible experience when connecting to Exchange.

Outlook clients earlier than Outlook 2007 are not supported. Email clients on Mac operating systems that require DAV, such as Entourage 2008 for Mac RTM and Entourage 2004, are not supported.

Outlook Web App supports several browsers on a variety of operating systems and devices. For detailed information, see [What's new for Outlook Web App in Exchange 2013](what-s-new-for-outlook-web-app-in-exchange-2013-exchange-2013-help.md).
