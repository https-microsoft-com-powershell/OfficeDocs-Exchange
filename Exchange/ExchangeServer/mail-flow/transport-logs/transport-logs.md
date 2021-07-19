---
localization_priority: Normal
description: 'Summary: Learn about transport logging in Exchange Server 2016 and Exchange Server 2019 and the kinds of logs and information that is logged.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: f8cf635d-60c2-4aa3-9c06-244c29942cba
ms.reviewer:
title: Transport logs in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Transport logs in Exchange Server

Transport logs provide information about what's happening in the transport pipeline. For more information about the transport pipeline, see [Mail flow and the transport pipeline](../../mail-flow/mail-flow.md).

The transport logs in Exchange Server are described in the following sections.

## Agent logging

Agent logging records the actions that are performed on messages by specific antispam transport agents on the Exchange server. For more information, see these topics:

- [Antispam Agent Logging](../../../ExchangeServer2013/anti-spam-agent-logging-exchange-2013-help.md)

- [Configure Antispam Agent Logging](../../../ExchangeServer2013/configure-anti-spam-agent-logging-exchange-2013-help.md)

- [Enable antispam functionality on Mailbox servers](../../antispam-and-antimalware/antispam-protection/antispam-on-mailbox-servers.md)

 **Enabled by default?**: Yes

 **Default location of log files**: Note that the folder isn't created until an agent attempts to write information to the log.

- **Mailbox servers**:

  - **Front End Transport service**: `%ExchangeInstallPath%TransportRoles\Logs\FrontEnd\AgentLog`

  - **Transport service**: `%ExchangeInstallPath%TransportRoles\Logs\Hub\AgentLog`

- **Transport service on Edge Transport servers**: `%ExchangeInstallPath%TransportRoles\Logs\Edge\AgentLog`

## Connectivity logging

Connectivity logging records outbound message transmission activity by the transport services on the Exchange server. For more information, see these topics:

- [Connectivity logging in Exchange Server](connectivity-logging.md)

- [Configure connectivity logging in Exchange Server](configure-connectivity-logging.md)

 **Enabled by default?**: Yes

 **Default location of log files**:

- **Mailbox servers**:

  - **Front End Transport service**: `%ExchangeInstallPath%TransportRoles\Logs\FrontEnd\Connectivity`

  - **Transport service**: `%ExchangeInstallPath%TransportRoles\Logs\Hub\Connectivity`

  - **Mailbox Transport Delivery service**: `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\Connectivity\Delivery`

  - **Mailbox Transport Submission service**:

- **Transport service on Edge Transport servers**: `%ExchangeInstallPath%TransportRoles\Logs\Edge\Connectivity`

## Message tracking and delivery reports for administrators

Message tracking is a detailed record of all message activity as mail flows through the transport pipeline on an Exchange server. For more information, see these topics:

- [Message tracking](message-tracking.md)

- [Configure message tracking](configure-message-tracking.md)

- [Search message tracking logs](search-message-tracking-logs.md)

Delivery reports for administrators is a targeted search of the message tracking log for messages that were sent to or from a specified mailbox. For more information, see these topics:

- [Delivery reports for administrators](delivery-reports.md)

- [Track messages with delivery reports](track-messages-with-delivery-reports.md)

 **Enabled by default?**: Yes

 **Default location of log files**:

- **Mailbox servers**: `%ExchangeInstallPath%TransportRoles\Logs\MessageTracking`:

  - `MSGTRK` files for the Transport service.

  - `MSGTRMD` files for the Mailbox Transport Delivery service.

  - `MSGTRMS` files for the Mailbox Transport Submission service.

- **Transport service on Edge Transport servers**: `%ExchangeInstallPath%TransportRoles\Logs\MessageTracking`

## Pipeline tracing

Pipeline tracing records snapshots of messages before and after the message is affected by transport agents in the transport pipeline. For more information, see these topics:

- [Pipeline Tracing](/pipeline-tracing/pipeline-tracing.md)

- [Configure Pipeline Tracing](/pipeline-tracing/configure-pipeline-tracing.md)

 **Enabled by default?**: No

 **Default location of log files**: Note that the folder isn't created until pipeline tracing is enabled.

- **Mailbox servers**:

  - **Transport service**: `%ExchangeInstallPath%TransportRoles\Logs\Hub\PipelineTracing`

  - **Mailbox Transport service**: `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\PipelineTracing`

- **Transport service on Edge Transport servers**: `%ExchangeInstallPath%TransportRoles\Logs\Edge\PipelineTracking`

## Protocol logging

Protocol logging records the SMTP conversations that occur on Send connectors and Receive connectors during message delivery. For more information, see these topics:

- [Protocol logging](../../mail-flow/connectors/protocol-logging.md)

- [Configure protocol logging](../../mail-flow/connectors/configure-protocol-logging.md)

 **Enabled by default?**: Only on these connectors:

- The default Receive connector named Default Frontend _\<ServerName\>_ in the Front End Transport service on Mailbox servers.

- The implicit and invisible Send connector in the Front End Transport service on Mailbox servers.

For more information about these connectors, see [Default Receive connectors created during setup](../connectors/receive-connectors.md#default-receive-connectors-created-during-setup) and [Implicit Send connectors](../connectors/send-connectors.md#implicit-send-connectors).

 **Default location of log files**:

- **Mailbox servers**:

  - **Front End Transport service**:

  - **Receive connectors**: `%ExchangeInstallPath%TransportRoles\Logs\FrontEnd\ProtocolLog\SmtpReceive`

  - **Send connectors**: `%ExchangeInstallPath%TransportRoles\Logs\FrontEnd\ProtocolLog\SmtpSend`

  - **Transport service**:

  - **Receive connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Hub\ProtocolLog\SmtpReceive`

  - **Send connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Hub\ProtocolLog\SmtpSend`

  - **Mailbox Transport Delivery service (Receive Connectors)**: `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\ProtocolLog\SmtpReceive\Delivery`

  - **Mailbox Transport Submission service**:

  - **Send connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\ProtocolLog\SmtpSend\Submission`

  - **Side effect messages**: `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\ProtocolLog\SmtpSend\Delivery`

- **Transport service on Edge Transport servers**:

  - **Receive connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Edge\ProtocolLog\SmtpReceive`

  - **Send connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Edge\ProtocolLog\SmtpSend`

## Routing table logging

 **Note**: The Routing Log Viewer is no longer available in the Exchange Toolbox.

Routing table logging periodically records snapshots of the routing table that Exchange servers uses to deliver messages. For more information, see these topics:

- [Understanding Routing Table Logging](/previous-versions/office/exchange-server-2010/bb125148(v=exchg.141))

- [Configure Routing Table Logging](../../../ExchangeServer2013/configure-routing-table-logging-exchange-2013-help.md)

 **Enabled by default?**: Yes

 **Default location of log files**:

- **Mailbox servers**:

  - **Front End Transport service**: `%ExchangeInstallPath%TransportRoles\Logs\FrontEnd\Routing`

  - **Transport service**: `%ExchangeInstallPath%TransportRoles\Logs\Hub\Routing`

  - **Mailbox Transport service:**: `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\Routing`:

  - `MDRoutingConfig` files for the Mailbox Transport Delivery service.

  - `MSRoutingConfig` files for the Mailbox Transport Submission service.

- **Transport service on Edge Transport servers**: `%ExchangeInstallPath%TransportRoles\Logs\Edge\Routing`