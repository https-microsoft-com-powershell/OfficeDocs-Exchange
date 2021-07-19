---
title: 'Address rewriting on Edge Transport servers: Exchange 2013 Help'
TOCTitle: Address rewriting on Edge Transport servers
ms:assetid: 23f1eaf6-247a-4671-ad72-aae19d9b511d
ms:mtpsurl: https://technet.microsoft.com/library/Aa996806(v=EXCHG.150)
ms:contentKeyID: 61200279
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Address rewriting on Edge Transport servers

_**Applies to:** Exchange Server 2013_

Address rewriting modifies email addresses of senders and recipients in messages that enter or leave your organization through an Edge Transport server. Two transport agents on the Edge Transport server provide the rewriting functionality: the Address Rewriting Inbound Agent and the Address Rewriting Outbound Agent. The primary reason for address rewriting on outbound messages is to present a single, consistent email domain to external recipients. The primary reason for address rewriting on inbound messages is to deliver messages to the correct recipient.

The *address rewrite entry*, which you create, specifies the internal addresses (the email addresses you want to change) and the external addresses (the final email addresses you want). You can specify whether email addresses are rewritten in inbound and outbound messages, or in outbound messages only. You can create address writing entries for a single user (chris@contoso.com to support@contoso.com), all users in a single domain (contoso.com to fabrikam.com), or for users in multiple subdomains with exceptions (\*.fabrikam.com to contoso.com, except legal.fabrikam.com).

> [!IMPORTANT]
> Regardless of how you plan to use address rewriting, you need to verify that the resulting email addresses are unique in your organization so you don't end up with duplicates. This is because address rewriting doesn't verify the uniqueness of a rewritten email address.

## Scenarios for address rewriting

The following scenarios are examples of how you can use address rewriting:

- **Group consolidation**: Some organizations segment their internal businesses into separate domains that are based on business or technical requirements. This configuration can cause email messages to appear as if they come from separate groups or even separate organizations.

    The following example shows how an organization, Contoso, Ltd., can hide its internal subdomains from external recipients:

  - Outbound messages from the northamerica.contoso.com, europe.contoso.com, and asia.contoso.com domains are rewritten so they appear to originate from a single contoso.com domain. All messages are rewritten as they pass through Edge Transport servers that provide SMTP connectivity between the whole organization and the Internet.

  - Inbound messages to contoso.com recipients are relayed by the Edge Transport server to a Mailbox server. The message is delivered to the correct recipient based on the proxy address that's configured on the recipient's mailbox.

- **Mergers and acquisitions**: An acquired company might continue to run as a separate business, but you can use address rewriting to make the two organizations appear as if they're one integrated organization.

    The following example shows how Contoso, Ltd. can hide the email domain of the newly acquired company, Fourth Coffee:

  - Contoso, Ltd. wants all outbound messages from Fourth Coffee's Exchange organization to appear as if they originate from contoso.com. All messages from both organizations are sent through the Edge Transport servers at Contoso, Ltd., where email messages are rewritten from *user*@fourthcoffee.com to *user*@contoso.com.

  - Inbound messages to *user*@contoso.com are rewritten and routed to *user*@fourthcoffee.com mailboxes. Inbound messages that are sent to *user*@fourthcoffee.com are routed directly to Fourth Coffee's email servers.

- **Partners**: Many organizations use external partners to provide services for their customers, other organizations, or their own organization. To avoid confusion, the organization might replace the email domain of the partner organization with its own email domain.

    The following example shows how Contoso, Ltd. can hide a partner's email domain:

  - Contoso, Ltd. provides support for the larger Wingtip Toys organization. Wingtip Toys wants a unified email experience for its customers, and it requires all messages from support personnel at Contoso, Ltd. to appear as if they were sent from Wingtip Toys. All outbound messages that relate to Wingtip Toys are sent through their Edge Transport servers, and all contoso.com email addresses are rewritten to wingtiptoys.com email addresses.

  - Inbound messages for support@wingtiptoys.com are accepted by Wingtip Toy's Edge Transport servers, rewritten, and then routed to the support@contoso.com email address.

## Message properties modified by address rewriting

A standard SMTP email message consists of a *message envelope* and message content. The message envelope contains information required for transmitting and delivering the message between SMTP mail servers. The message content contains message header fields (collectively called the *message header*) and the message body. The message envelope is described in RFC 2821, and the message header is described in RFC 2822.

When a sender composes an email message and submits it for delivery, the message contains the basic information required to comply with SMTP standards, such as a sender, a recipient, the date and time that the message was composed, an optional subject line, and an optional message body. This information is contained in the message itself and, by definition, in the message header.

The sender's mail server generates a message envelope for the message by using the sender's and recipient's information found in the message header. It then transmits the message to the Internet for delivery to the recipient's mail server. Recipients never see the message envelope because it's generated by the message transmission process, and it isn't actually part of the message.

Address rewriting changes an email address by rewriting specific fields in the message header or message envelope. Address rewriting changes several fields in outbound messages but only one field in inbound email messages. The following table shows which SMTP header fields are rewritten in outbound and inbound messages.

### Message fields rewritten on outbound and inbound messages

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Field name</th>
<th>Location</th>
<th>Outbound messages</th>
<th>Inbound messages</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>MAIL FROM</strong></p></td>
<td><p>Message envelope</p></td>
<td><p>Rewritten</p></td>
<td><p>Not rewritten</p></td>
</tr>
<tr class="even">
<td><p><strong>RCPT TO</strong></p></td>
<td><p>Message envelope</p></td>
<td><p>Not rewritten</p></td>
<td><p>Rewritten</p></td>
</tr>
<tr class="odd">
<td><p><strong>To</strong></p></td>
<td><p>Message header</p></td>
<td><p>Rewritten</p></td>
<td><p>Not rewritten</p></td>
</tr>
<tr class="even">
<td><p><strong>Cc</strong></p></td>
<td><p>Message header</p></td>
<td><p>Rewritten</p></td>
<td><p>Not rewritten</p></td>
</tr>
<tr class="odd">
<td><p><strong>From</strong></p></td>
<td><p>Message header</p></td>
<td><p>Rewritten</p></td>
<td><p>Not rewritten</p></td>
</tr>
<tr class="even">
<td><p><strong>Sender</strong></p></td>
<td><p>Message header</p></td>
<td><p>Rewritten</p></td>
<td><p>Not rewritten</p></td>
</tr>
<tr class="odd">
<td><p><strong>Reply-To</strong></p></td>
<td><p>Message header</p></td>
<td><p>Rewritten</p></td>
<td><p>Not rewritten</p></td>
</tr>
<tr class="even">
<td><p><strong>Return-Receipt-To</strong></p></td>
<td><p>Message header</p></td>
<td><p>Rewritten</p></td>
<td><p>Not rewritten</p></td>
</tr>
<tr class="odd">
<td><p><strong>Disposition-Notification-To</strong></p></td>
<td><p>Message header</p></td>
<td><p>Rewritten</p></td>
<td><p>Not rewritten</p></td>
</tr>
<tr class="even">
<td><p><strong>Resent-From</strong></p></td>
<td><p>Message header</p></td>
<td><p>Rewritten</p></td>
<td><p>Not rewritten</p></td>
</tr>
<tr class="odd">
<td><p><strong>Resent-Sender</strong></p></td>
<td><p>Message header</p></td>
<td><p>Rewritten</p></td>
<td><p>Not rewritten</p></td>
</tr>
</tbody>
</table>

## What address rewriting doesn't change

Address rewriting doesn't modify any message header fields that would break SMTP functionality. For example, modifying certain header fields can affect routing loop detection, invalidate the signature, or make a rights-protected message unreadable. Therefore, the following header fields aren't modified by address rewriting.

- **Return-Path**

- **Received**

- **Message-ID**

- **X-MS-TNEF-Correlator**

- **Content-Type Boundary=string**

- Header fields located inside MIME body parts

Address rewriting ignores domains that aren't controlled by the Exchange organization. In other words, address rewriting doesn't rewrite header fields that contain domains for which the Exchange organization isn't authoritative. Rewriting such domains would cause an uncontrollable form of message relay.

Address rewriting also doesn't modify the header fields of messages that are embedded in another message. Senders and recipients expect embedded messages to remain intact and be delivered without modification, as long as the messages don't trigger transport rules that are implemented between the sender and recipient.

## Considerations for outbound-only address rewriting

Outbound-only address rewriting on an Edge Transport server modifies the sender's email address as the message leaves the Exchange organization. You can configure outbound-only address rewriting for a single user (chris@contoso.com to support@contoso.com) or for all users in a single domain (contoso.com to fabrikam.com). You are required to configure outbound-only address rewriting for users in multiple subdomains (\*.fabrikam.com to .contoso.com).

The rewritten email address must be configured as a proxy address on the affected recipients. For example, if laura@sales.contoso.com is rewritten to laura@contoso.com, the proxy address laura@contoso.com must be configured on Laura's mailbox. This allows replies and inbound messages to be delivered correctly.

## Considerations for inbound and outbound address rewriting

Inbound and outbound, or *bidirectional* address rewriting on an Edge Transport server modifies the sender's email address in messages that leave the Exchange organization, and it modifies the recipient's email address in messages that enter the Exchange organization.

You can configure outbound-only address rewriting for a single user (chris@contoso.com to support@contoso.com) and all users in a single domain (contoso.com to fabrikam.com). You can't configure bidirectional address rewriting for users in multiple subdomains (\*.fabrikam.com to contoso.com).

## Considerations for rewriting email addresses in multiple domains

When you flatten multiple internal domains or subdomains into a single external domain, you need to consider the following factors:

- **Verify unique aliases**: All email aliases (the part to the left of the @ sign) must be unique across all subdomains. For example, if there is a joe@sales.contoso.com, there can't be a joe@marketing.contoso.com because the rewritten email address for both users would be joe@contoso.com.

- **Add proxy addresses**: The rewritten email address must be configured as a proxy address for all affected senders in the affected domains. For example, if joe@sales.contoso.com is rewritten to joe@contoso.com, you need to add the proxy address joe@contoso.com to Joe's mailbox. This allows replies and inbound messages to be delivered correctly.

- **Mail contacts for non-Exchange organizations**: If you're rewriting email addresses from a non-Exchange email system, you need to create email contacts in Exchange to represent the users in the non-Exchange email system. These email contacts must contain the original email addresses and the rewritten email addresses. For example, if joe@unix.contoso.com is rewritten to joe@contoso.com, you need to create a mail contact with joe@unix.contoso.com as the external email address and joe@contoso.com as a proxy address.

## Verify unique aliases

When you rewrite email addresses in multiple subdomains, you need to make sure that all email aliases are unique across all your subdomains. For example, consider the following configuration:

The following users are in the subdomains sales.contoso.com, marketing.contoso.com, and research.contoso.com:

- maria@sales.contoso.com

- chris@sales.contoso.com

- david@marketing.contoso.com

- brian@marketing.contoso.com

- chris@research.contoso.com

- adam@research.contoso.com

Suppose you want to rewrite the subdomains sales.contoso.com, marketing.contoso.com, and research.contoso.com into the single domain contoso.com.

When the email addresses in each subdomain are rewritten, a conflict occurs between chris@sales.contoso.com and chris@research.contoso.com because both email addresses are rewritten to chris@contoso.com. To resolve this situation, you need to change the email address of one of the affected recipients. For example, you can change chris@research.contoso.com to christopher@research.contoso.com so the email address is rewritten to christopher@contoso.com.

## Priority of address rewrite entries

If a user's email address matches multiple address rewrite entries, the email address is only rewritten once based on the closest match. The following list describes the order of precedence of address rewrite entries from highest priority to lowest priority:

1. **Individual email addresses**: An address rewrite entry is configured to rewrite the email address of john@contoso.com to support@contoso.com.

2. **Domain or subdomain mapping**: An address rewrite entry is configured to rewrite all contoso.com email addresses to northwindtraders.com or all sales.contoso.com email addresses to contoso.com.

3. **Domain flattening**: An address rewrite entry is configured to rewrite \*.contoso.com email addresses to contoso.com.

For example, consider an Edge Transport server where the following outbound address rewrite entries are configured:

- \*.contoso.com email addresses are rewritten to contoso.com

- japan.sales.contoso.com email addresses are rewritten to contoso.jp

If masato@japan.sales.contoso.com sends an email message, the address is rewritten to masato@contoso.jp, because that entry most closely matches the sender's email address.

## Digitally signed, encrypted, and rights-protected messages

Address rewriting shouldn't affect most signed, encrypted, or rights-protected messages. If address rewriting were to invalidate or otherwise change the security status of these types of messages in any way, address rewriting isn't applied.

The following values can be rewritten because the information isn't part of message signing, encryption, or rights protection:

- Fields in the message envelope

- Top-level message body headers

The following values aren't rewritten because the information is part of message signing, encryption, or rights protection:

- Header fields located inside MIME body parts that may be signed

- The boundary string parameter of the MIME content type
