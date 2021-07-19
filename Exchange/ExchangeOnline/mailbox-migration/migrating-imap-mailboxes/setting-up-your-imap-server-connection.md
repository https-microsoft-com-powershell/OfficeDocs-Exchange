---
localization_priority: Normal
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 49120f09-84fb-4f10-b49f-5d9ec7e69d1f
ms.reviewer: 
description: Learn how to find your IMAP server name is Outlook on the web (Outlook Web App).
title: Learn more about setting up your IMAP server connection
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- MED150
- BCS160
audience: Admin
f1.keywords:
- CSH
ms.custom: 
- Adm_O365
- SetupIMAPOtherServerSettings619135
ms.service: exchange-online
manager: serdars

---

# Learn more about setting up your IMAP server connection

To migrate your email by using Internet Message Access Protocol (IMAP) migration, Microsoft 365 or Office 365 needs to know the name and connection settings of your IMAP server.

## Find your IMAP server name

Microsoft 365 or Office 365 needs the name of the source email server to migrate mailboxes from. In this task, we describe how to get the name of the email server by using Outlook on the web (formerly known as Outlook Web App). If you don't have access to Outlook on the web, or if your IMAP server name isn't listed there, either contact support or consult the help documentation for your source email system.

### To get the name of your source email server by using Outlook on the web

1. Open your mailbox in Outlook on the web.

2. On the toolbar, choose **Settings** ![Microsoft 365 or Office 365 Settings button](../media/a9a59c0f-2e67-4cbf-9438-af273b0d552b.png).

3. In the **Search all settings** box, start typing "pop", and in the results, select **POP and IMAP**.

4. In **POP and IMAP settings**, your IMAP server name is listed in the **IMAP setting** section.

   ![Shows the link for POP or IMAP access settings](../media/fa54c636-4fd3-4fcd-add3-4e7c69072493.png)

   **Note**: The IMAP server for Gmail is: **imap.gmail.com**.

For more information about IMAP connections in Microsoft 365 or Office 365, see [POP and IMAP email settings for Outlook](https://support.microsoft.com/office/8361e398-8af4-4e97-b147-6c6c4ac95353).

## Values for security and port

Microsoft 365 or Office 365 also needs the values for the encryption method and the Transmission Control Protocol (TCP) port number that's used by the source email IMAP server.

- **Security**: This is the encryption method used by the IMAP server. The default value for secure sockets layer (SSL) is appropriate for most IMAP servers.

- **Port**: This is the TCP port number that's used to connect to the IMAP server. In Microsoft 365 or Office 365, the only available value is 993 for SSL connections. Port 993 is appropriate for most IMAP servers.
