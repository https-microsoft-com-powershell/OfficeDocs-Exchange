---
localization_priority: Normal
description: Learn about how to view transport agents in the transport pipeline in Exchange 2016 and Exchange 2019.
ms.topic: article
author: dariomws
ms.author: serdars
ms.assetid: 
ms.reviewer: 
title: View transport agents in the transport pipeline in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# View transport agents in the transport pipeline in Exchange Server

You can use the Exchange Management Shell to view a list of transport agents in the transport pipeline on Microsoft Exchange Server 2016 or 2019. Specifically, the **Get-TransportPipeline** cmdlet shows information about the following types of transport agents in the transport pipeline:

- Agents based on the **SmtpReceiveAgent**, **RoutingAgent**, **DeliveryAgent**, and **StorageAgent** classes in the Transport service.

- Agents based on the **SmtpReceiveAgentClass** in the Front End Transport service.

You can view a list of all the enabled transport agents that have encountered messages in the transport pipeline and the SMTP events they are registered on. For more information about transport agents, see [Transport agents in Exchange Server](transport-agents.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport agents" entry in the [Mail flow permissions](..//../permissions/feature-permissions/mail-flow-permissions.md) topic.

- You can only use the Exchange Management Shell to perform this procedure.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the Exchange Management Shell to view a list of transport agents in the transport pipeline

To use the Shell to view a list of transport agents in the transport pipeline on an Exchange server, run the following command:

```powershell
Get-TransportPipeline | Format-List
```

To export the results to a text file named C:\\My Documents\\Transport Agents.txt, run the following command:

```powershell
Get-TransportPipeline | Format-List > "C:\My Documents\Transport Agents.txt"
```

## How do you know this worked?

Only transport agents that have encountered messages in the transport pipeline between the time when the transport service was started and the time when the **Get-TransportPipeline** cmdlet was run are displayed by the cmdlet. A transport agent that hasn't encountered a message in the transport pipeline won't appear in the results shown by the **Get-TransportPipeline** cmdlet, even if that transport agent is enabled.
