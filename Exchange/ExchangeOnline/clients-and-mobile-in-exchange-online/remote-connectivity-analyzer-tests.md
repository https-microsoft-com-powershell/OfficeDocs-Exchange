---
localization_priority: Normal
description: The Microsoft Exchange Remote Connectivity Analyzer (ExRCA) helps you make sure that connectivity for your Exchange servers is set up correctly. If you're having problems, it can also help you find and fix these problems. The ExRCA website can run tests to check for Microsoft Exchange ActiveSync, Exchange Web Services, Microsoft Outlook, and internet email connectivity.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 251505e6-0907-44cd-859f-bd9402e6a397
ms.reviewer: 
title: Remote Connectivity Analyzer tests for Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Remote Connectivity Analyzer tests for Exchange Online

The [Microsoft Exchange Remote Connectivity Analyzer (ExRCA)](https://testconnectivity.microsoft.com/) helps you make sure that connectivity for your Exchange service is set up correctly. If you're having problems, it can also help you find and fix these problems. The ExRCA website can run tests to check for Microsoft Exchange ActiveSync, Exchange Web Services, Microsoft Outlook, and internet email connectivity.

## Remote Connectivity Analyzer tests

You can perform several tests with the ExRCA. The following tests work on Exchange 2010 and later versions, including Exchange Online:

- Exchange DNS (only available in the Office 365 tab)

- Exchange ActiveSync

- Exchange Web Services

- Outlook

- Internet email

### Exchange Domain Name Server (DNS) tests

You can run the following tests for Exchange DNS:

- **Help Identify My Issue with Exchange DNS (only available in the Office 365 tab)**: This test will check the external domain name settings for your verified domain in Office 365. The test will look for issues with mail delivery such as not receiving incoming email from the Internet and Outlook client connectivity issues that involve connecting to Outlook and Exchange Online.

### Exchange ActiveSync tests

You can run the following tests for Exchange ActiveSync:

- **Exchange ActiveSync**: This test simulates the steps that a mobile device uses to connect to an Exchange server using Exchange ActiveSync.

### Exchange Web Services connectivity tests

The Exchange Web Services tests check the settings for many of the Exchange Web Services. You can run the following tests for Exchange Web Services:

- **Synchronization, Notification, Availability, and Automatic Replies**: These tests walk through many basic Exchange Web Services tasks to confirm that they're working. This is useful for IT administrators who want to troubleshoot external access using Entourage EWS or other Web Services clients.

- **Service Account Access (Developers)**: This test verifies a service account's ability to access a specified mailbox, create and delete items in it, and access it via Exchange impersonation. This test is primarily used by application developers to test the ability to access mailboxes with alternate credentials.

- **Free/Busy (only available in the Office 365 tab)**: This test verifies that an Office 365 mailbox can access the free/busy information of an on-premises mailbox, and vice versa (one direction per test run).

### Microsoft Office Outlook Connectivity tests

You can run the following tests for Outlook connectivity:

- **Outlook Connectivity**: This test walks through the steps Outlook uses to connect from the internet. It tests connectivity using both the RPC over HTTP and the MAPI over HTTP protocols.

### Internet email tests

You can run the following tests for internet email:

- **Inbound SMTP E-Mail**: This test walks through the steps an internet email server uses to send inbound SMTP email to your domain.

- **Outbound SMTP E-Mail**: This test checks your outbound IP address for certain requirements. This includes Reverse DNS, Sender ID, and RBL checks.

- **POP Email**: This test walks through the steps an email client uses to connect to a mailbox using POP3.

- **IMAP Email**: This test walks through the steps an email client uses to connect to a mailbox using IMAP.
