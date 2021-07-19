---
title: 'Modify the SMTP banner on a Receive connector: Exchange 2013 Help'
TOCTitle: Modify the SMTP banner on a Receive connector
ms:assetid: d667704e-fd69-4aca-9c35-eef7006944b2
ms:mtpsurl: https://technet.microsoft.com/library/Bb124740(v=EXCHG.150)
ms:contentKeyID: 50934226
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Modify the SMTP banner on a Receive connector

_**Applies to:** Exchange Server 2013_

The *SMTP banner* is the SMTP connection response that a remote SMTP messaging server receives after it connects to a Receive connector that's configured on a computer running Microsoft Exchange Server 2013.

This is the default response received by a remote SMTP messaging server after it connects to the Receive connector:

```powershell
220 <Servername> Microsoft ESMTP MAIL service ready at <RegionalDay-Date-24HourTimeFormat> <RegionalTimeZoneOffset>
```

When you specify a custom value for SMTP banner on a Receive connector, a remote SMTP messaging server that connects to that SMTP Receive connector receives the following response.

```powershell
220 <Banner Text>
```

You may want to modify the SMTP banner for Internet-facing SMTP Receive connectors so the server name and messaging server software aren't disclosed by the SMTP banner.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Receive connectors" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- You can only use the Shell to perform this procedure.

- The replacement SMTP banner text string must always start with `220`. As defined in RFC 2821, the default service ready SMTP response code is 220.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the Shell to modify the SMTP banner on a Receive connector

Run the following command:

```powershell
Set-ReceiveConnector <ConnectorIdentity> -Banner "220 <Banner Text>"
```

This example modifies the SMTP banner on the existing Receive connector named From the Internet so the SMTP banner displays `220 Contoso Corporation`.

```powershell
Set-ReceiveConnector "From the Internet" -Banner "220 Contoso Corporation"
```

This example removes the custom SMTP banner on the Receive connector named From the Internet, which returns the SMTP banner to the default value.

```powershell
Set-ReceiveConnector "From the Internet" -Banner $null
```

## How do you know this worked?

To verify that you have successfully modified the SMTP banner on a Receive connector, do the following:

1. Open a telnet client on a computer that can access the Receive connector, and run the following command:

   ```powershell
   open <Connector FQDN or IP address> <Port>
   ```

2. Verify the response from the Receive connector contains the SMTP banner you configured.

Note that this procedure only works on Receive connectors that allow anonymous or Basic authentication. For more information, see [Use Telnet to test SMTP communication](use-telnet-to-test-smtp-communication-exchange-2013-help.md).
