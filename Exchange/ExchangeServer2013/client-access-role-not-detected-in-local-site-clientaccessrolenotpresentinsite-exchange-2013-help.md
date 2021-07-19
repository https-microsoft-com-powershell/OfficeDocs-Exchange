---
title: 'Client Access role not detected in local site'
TOCTitle: Client Access role not detected in local site_ClientAccessRoleNotPresentInSite
ms:assetid: b5bfc6af-9c55-46c0-a293-6078b64e87dd
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.clientaccessrolenotpresentinsite(v=EXCHG.150)
ms:contentKeyID: 46629089
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Client Access role not detected in local site\_ClientAccessRoleNotPresentInSite

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Microsoft® Exchange Server 2007 setup displayed this warning because an existing Client Access role could not be detected in the local Active Directory® directory service site.

You have chosen to install the Mailbox Server role before an instance of the Client Access role is installed in the Active Directory site.

The Client Access server role accepts connections to your Exchange 2007 server from a variety of different clients. Software clients such as Microsoft Outlook Express and Eudora use POP3 or IMAP4 connections to communicate with the Exchange server. Hardware clients, such as mobile devices, use ActiveSync, POP3, or IMAP4 to communicate with the Exchange server.

If users access their Inbox by using any client other than Microsoft Office Outlook®, you must install the Client Access server role in your Exchange organization.

Users will be able to logon to their mailboxes with Outlook but will not be able to use Outlook Web Access or mobile devices against the same mailbox until a Client Access role is present in the local Active Directory site.
