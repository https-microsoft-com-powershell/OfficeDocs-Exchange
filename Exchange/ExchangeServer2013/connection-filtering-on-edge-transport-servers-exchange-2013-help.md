---
title: 'Connection Filtering on Edge Transport Servers: Exchange 2013 Help'
description: 'Summary: This article describes the connection-filtering feature.'
TOCTitle: Connection Filtering on Edge Transport Servers
ms:assetid: b513755c-5d3e-44fa-a6cb-771d48b544ac
ms:mtpsurl: https://technet.microsoft.com/library/Bb124320(v=EXCHG.150)
ms:contentKeyID: 61200295
ms.reviewer: 
manager: serdars
ms.author: serdars
author: serdars
audience: ITPro
ms.topic: article
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Connection Filtering on Edge Transport Servers

_**Applies to:** Exchange Server 2013_

Connection filtering is an anti-spam feature in Microsoft Exchange Server 2013 that allows or blocks email based on the message source. Connection filtering is performed by the Connection Filtering agent that's available only on Edge Transport servers. The Connection Filtering agent relies on the IP address of the connecting mail server to determine what action, if any, to take on an inbound message.

By default, the Connection Filtering agent is the first anti-spam agent to evaluate an inbound message on an Edge Transport server. The source IP address of the SMTP connection is checked against the allowed and blocked IP addresses. If the source IP address is allowed, the message is sent to the recipients in your organization without further processing by other anti-spam agents. If the source IP address is blocked, the SMTP connection is dropped. If the source IP address isn't allowed or blocked, the message flows through the other anti-spam agents on the Edge Transport server.

Connection filtering compares the IP address of the source mail server to the values in the IP Allowlist, the IP Blocklist, IP Allowlist providers, and IP Blocklist providers. Configure at least one of these four IP address data stores for connection filtering to function. If you don't specify any IP address data, you should disable the Connection Filtering agent. For more information, see [Manage Connection Filtering on Edge Transport Servers](manage-connection-filtering-on-edge-transport-servers-exchange-2013-help.md).

## IP Blocklist

The IP Blocklist contains the IP addresses of email servers that you want to block. You manually maintain the IP addresses in the IP Blocklist. You can add individual IP addresses or IP address ranges. You can specify an expiration time that specifies how long the IP address entry will be blocked. When the expiration time is reached, the IP address entry in the IP Blocklist is disabled.

If the Connection Filtering agent finds the source IP address on the IP Blocklist, the SMTP connection will be dropped after all the **RCPT TO** headers (envelope recipients) in the message are processed.

IP addresses can also be automatically added to the IP Blocklist by the Sender Reputation feature of the Protocol Analysis agent. For more information, see [Sender reputation and the Protocol Analysis agent](sender-reputation-and-the-protocol-analysis-agent-exchange-2013-help.md).

## IP Allowlist

The IP Allowlist contains the IP addresses of email servers that you want to designate as trustworthy sources of email. Email from mail servers that you specify in the IP Allowlist is exempt from processing by other Exchange anti-spam agents.

You manually maintain the IP addresses in the IP Allowlist. You can add individual IP addresses or IP address ranges. You can specify an expiration time that specifies how long the IP address entry will be allowed. When the expiration time is reached, the entry in the IP Allowlist is disabled.

## IP Blocklist providers

IP Blocklist providers are frequently referred to as *real-time blocklists*, or RBLs. IP Blocklist providers compile lists of mail server IP addresses that send spam. Many IP Blocklist providers also compile lists of mail server IP addresses that could be used for spam. Examples include mail servers that are configured for open relay, Internet service providers (ISPs) that assign dynamic IP addresses, and ISPs that allow SMTP mail server traffic from dial-up accounts.

When you configure connection filtering to use an IP Blocklist provider, the Connection Filtering agent compares the IP address of the connecting mail server to the list of IP addresses at the IP Blocklist provider. If there's a match, the message isn't allowed in your organization. You can configure connection filtering to use multiple IP Blocklist providers, and you assign different priority values to each provider.

The Connection Filtering agent checks the source IP address at the IP Allowlist and the IP Blocklist. If the IP address doesn't exist on either list, the Connection Filtering agent queries the IP Blocklist provider according to the priority value that you assigned. If the IP address is defined at an IP Blocklist provider, the Edge Transport server waits for and processes the **RCPT TO** header, responds to the sending mail server with an `SMTP 550` error, and closes the connection. The connection isn't immediately dropped so that the connection attempt can be logged, and because you can specify recipients that are exempt from having messages blocked by any IP Blocklist providers.

If the IP address isn't defined at any of the IP Blocklist providers, the Content Filtering agent hands the message off to the next transport agent on the Edge Transport server.

For each IP Blocklist provider, you can customize the `SMTP 550` error that's returned to the sender when a message is blocked. Identify the IP Blocklist provider that identified the message source as spam. If a legitimate source mail server is erroneously identified as a spam source, the administrator can then contact the IP Blocklist provider and take the steps necessary to remove the mail server from the IP Blocklist provider.

IP Blocklist providers can return different codes to identify why an IP address is defined in their lists. Most IP Blocklist providers return bitmask or absolute value data types. Within these data types, the IP Blocklist provider can use multiple values to classify the IP address by threat type.

There are issues to consider when using IP Blocklist providers:

- Outages or delays at the IP Blocklist provider service can cause delays in the processing of messages on the Edge Transport server. Select reliable IP Blocklist providers.

- Source servers that you know to be legitimate can be erroneously identified as spam sources. For example, the mail server can be unintentionally configured to act as an open relay. Select IP Blocklist providers that provide clear procedures for evaluation and removal from their services.

## Bitmask and absolute value examples

This section shows an example of the status codes returned by most Blocklist providers. For details about the status codes that the provider returns, see the documentation from the specific provider.

For bitmask data types, the IP Blocklist provider service returns a status code of 127.0.0.*x*, where the integer *x* is any one of the values listed in the following table.

### Values and status codes for bitmask data types

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Value</th>
<th>Status code</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>1</p></td>
<td><p>The IP address is on an IP Blocklist.</p></td>
</tr>
<tr class="even">
<td><p>2</p></td>
<td><p>The SMTP server is configured to act as an open relay.</p></td>
</tr>
<tr class="odd">
<td><p>4</p></td>
<td><p>The IP address supports a dial-up IP address.</p></td>
</tr>
</tbody>
</table>

For absolute value types, the IP Blocklist provider returns explicit responses that define why the IP address is defined in their blocklists. The following table shows examples of absolute values and the explicit responses.

### Values and status codes for absolute value data types

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Value</th>
<th>Explicit response</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>127.0.0.2</p></td>
<td><p>The IP address is a direct spam source.</p></td>
</tr>
<tr class="even">
<td><p>127.0.0.4</p></td>
<td><p>The IP address is a bulk mailer.</p></td>
</tr>
<tr class="odd">
<td><p>127.0.0.5</p></td>
<td><p>The remote server sending the message is known to support multistage open relays.</p></td>
</tr>
</tbody>
</table>

## IP Allowlist providers

IP Allowlist providers are also known as *safe lists* or *allowlists*. IP Allowlist providers are configured just like IP Blocklist providers, but the results are the opposite: they define mail server IP addresses that are definitely not associated with spam activity. If the IP address of the connecting mail server is defined at an IP Allowlist provider, the message is exempt from processing by other Exchange anti-spam agents. For this reason, IP Blocklist providers are used much more frequently than IP Allowlist providers. Choose your IP Allowlist providers carefully.

## Test IP Blocklist providers and IP Allowlist providers

After you configure connection filtering to use an IP Blocklist provider or an IP Allowlist provider, you can run tests to verify that the providers are working correctly. Most providers provide test IP addresses that you can use to test their services. When you test a provider, the Connection Filtering agent issues a DNS query that should result in a specific response from the provider. For more information about how to test IP addresses against an IP Blocklist provider service or an IP Allowlist provider service, see [Manage Connection Filtering on Edge Transport Servers](manage-connection-filtering-on-edge-transport-servers-exchange-2013-help.md).

## Configure connection filtering on Edge Transport servers that aren't directly connected to the Internet

You can use connection filtering on Edge Transport servers that don't directly receive email from the Internet. In this scenario, the Edge Transport server is behind another mail server that receives and processes messages directly from the Internet. For example, your organization might send email traffic through an anti-spam server, service, or appliance before the messages reach the Edge Transport server. In this scenario, the Connection Filtering agent needs to extract the correct source IP address from the message. To extract source IP address, the Connection Filtering agent needs to parse the **Received** header field values in the message header and compare those values to the known IP addresses of the mail server that sits between the Edge Transport server and the Internet.

Every mail server that accepts and relays an SMTP message along the delivery path adds its own **Received** header field in the message header. The **Received** header typically contains the domain name and IP address of the mail server that processed the message.

If the Edge Transport server doesn't accept messages directly from the Internet, you need to use the *InternalSMTPServers* parameter on the **Set-TransportConfig** cmdlet on an Exchange 2013 Mailbox server to identify the IP address of the mail server that sits between the Edge Transport server and the Internet. The IP address data is replicated to Edge Transport servers by EdgeSync. When messages are received by the Edge Transport server, the Connection Filtering agent assumes an IP address in a **Received** header field that doesn't match a value specified by the *InternalSMTPServers* parameter is the source IP address that needs to be checked. Therefore, you need specify all internal SMTP servers in order for connection filtering to function correctly.
