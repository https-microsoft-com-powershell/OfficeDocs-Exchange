---
title: 'Enable anti-spam functionality on Mailbox servers: Exchange 2013 Help'
TOCTitle: Enable anti-spam functionality on Mailbox servers
ms:assetid: 59d22c5e-64bc-4879-8ad1-364862b6ba11
ms:mtpsurl: https://technet.microsoft.com/library/Bb201691(v=EXCHG.150)
ms:contentKeyID: 49248681
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable anti-spam functionality on Mailbox servers

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, the following anti-spam agents are available in the Transport service on Mailbox servers, but they are not installed by default:

- Content Filter agent

- Sender ID agent

- Sender Filter agent

- Protocol Analysis agent for sender reputation

However, you can install these anti-spam agents on a Mailbox server using a script in the Exchange Management Shell. Typically, you would install the anti-spam agents on a Mailbox server only when your organization accepts all incoming mail without any prior anti-spam filtering.

> [!NOTE]
> Although the Recipient Filter agent is available on Mailbox servers, you shouldn't configure it. When recipient filtering on a Mailbox server detects one invalid or blocked recipient in a message that contains other valid recipients, the message is rejected. Although the Recipient Filter agent is enabled by default, it isn't configured to block any recipients. For more information, see <A href="manage-recipient-filtering-on-edge-transport-servers-exchange-2013-help.md">Manage recipient filtering on Edge Transport servers</A>.

What happens if you install the available anti-spam agents in the Transport service on a Mailbox server, but you also have other Exchange anti-spam agents operating on the messages before they reach the Mailbox server? For example, what if you have an Edge Transport server in the perimeter network? The anti-spam agents on the Mailbox server recognize the anti-spam X-header values that are added to messages by other Exchange anti-spam agents, and messages that contain these X-headers pass through without being scanned again. However, recipient look-ups performed by the Recipient Filter agent will occur again on the Mailbox server.

## What do you need to know before you begin?

- Estimated time to complete this task: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport configuration" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- The Connection Filter agent and the Attachment Filter agent aren't available on Mailbox servers. They're only available on an Edge Transport server. However, the Malware agent is installed and enabled by default on a Mailbox server. For more information, see [Anti-malware protection](anti-malware-protection-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Step 1: Use the Shell to run the Install-AntispamAgents.ps1 script

Run the following command:

```powershell
& $env:ExchangeInstallPath\Scripts\Install-AntiSpamAgents.ps1
```

### How do you know this step worked?

You know this step worked if the script runs without errors, and asks you to restart the Microsoft Exchange Transport service.

## Step 2: Use the Shell to restart the Microsoft Exchange Transport service

Run the following command:

```powershell
Restart-Service MSExchangeTransport
```

### How do you know this step worked?

You know this step worked if the Microsoft Exchange Transport service restarts without errors.

## Step 3: Use the Shell to specify the internal SMTP servers in your organization

You need to specify the IP addresses of any internal SMTP servers that should be ignored by the Sender ID agent. In fact, you need to specify the IP address of at least one internal SMTP server. If the Mailbox server where you're running the anti-spam agents is the only SMTP server in your organization, specify the IP address of that computer.

To add the IP addresses of internal SMTP servers without affecting any existing values, run the following command:

```powershell
Set-TransportConfig -InternalSMTPServers @{Add="<ip address1>","<ip address2>"...}
```

This example adds the internal SMTP server addresses 10.0.1.10 and 10.0.1.11 to the transport configuration of your organization.

```powershell
Set-TransportConfig -InternalSMTPServers @{Add="10.0.1.10","10.0.1.11"}
```

### How do you know this step worked?

To verify that you have successfully specified the IP address of at least one internal SMTP server, do the following:

1. Run the following command:

    ```powershell
    Get-TransportConfig | Format-List InternalSMTPServers
    ```

2. Verify the IP address of at least one valid internal SMTP server is displayed.
