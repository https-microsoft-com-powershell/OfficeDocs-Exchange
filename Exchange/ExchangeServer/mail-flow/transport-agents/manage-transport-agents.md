---
localization_priority: Normal
description: Learn about how to manage transport agents in Exchange 2016 and Exchange 2019.
ms.topic: article
author: dariomws
ms.author: serdars
ms.assetid: 
ms.reviewer: 
title: Manage transport agents in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Manage transport agents in Exchange Server

Transport agents use SMTP events to operate on messages as the messages move through the transport pipeline. Most of the built-in transport agents that are included with Microsoft Exchange Server 2016 or 2019 are invisible and unmanageable. However, you can install and configure third-party transport agents on Exchange servers in your organization. For more information about transport agents, see [Transport agents in Exchange Server](transport-agents.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport agents" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- You can only use the Exchange Management Shell to perform this procedure.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the Exchange Management Shell to install a transport agent

When you install a transport agent, Exchange only registers the DLLs associated with the transport agent. You need to make sure all files, registry keys, and other objects that the transport agent depends on are installed correctly and configured. After Exchange loads the DLLs, it continues to reference the DLLs after the command has completed.

Transport agents have full access to all email messages that they encounter. Exchange puts no restrictions on a transport agent's behavior. Transport agents that are unstable or contain security flaws may affect the stability and security of Exchange. Therefore, you should only install transport agents that you fully trust and that have been fully tested in a test environment.

Transport agents are installed in a disabled state to make sure mail flow isn't affected by transport agents that haven't been configured. Therefore, after a transport agent has been configured correctly, you need to enable the transport agent.

Use the following syntax to install a transport agent.

```powershell
Install-TransportAgent -Name <TransportAgentIdentity> -TransportAgentFactory <"TransportAgentFactory"> -AssemblyPath <"FilePath">
```

This example installs a fictitious transport agent named Contoso Transport Agent in the Transport service.

```powershell
Install-TransportAgent -Name "Contoso Transport Agent" -TransportAgentFactory "vendor.exchange.ContosoTransportAgentfactory" -AssemblyPath "C:\Program Files\Vendor\TransportAgent\ContosoTransportAgentFactory.dll"
```

## How do you know this worked?

To verify that you have successfully installed the transport agent, run the command `Get-TransportAgent` and confirm the transport agent is listed.

## Use the Exchange Management Shell to enable a transport agent

Use the following syntax to enable a transport agent.

```powershell
Enable-TransportAgent <TransportAgentIdentity>
```

This example enables the transport agent named Contoso Transport Agent in the Transport service.

```powershell
Enable-TransportAgent "Contoso Transport Agent"
```

## How do you know this worked?

To verify that you have successfully enabled a transport agent, run the command `Get-TransportAgent | Format-List Name,Enabled` and confirm the transport agent is enabled.

## Use the Exchange Management Shell to disable a transport agent

Use the following syntax to disable a transport agent:

```powershell
Disable-TransportAgent <TransportAgentIdentity>
```

This example disables the transport agent named Fabrikam Transport Agent in the Transport service.

```powershell
Disable-TransportAgent "Fabrikam Transport Agent"
```

## How do you know this worked?

To verify that you have successfully disabled a transport agent, run the command `Get-TransportAgent | Format-List Name,Enabled` and confirm the transport agent is disabled.

## Use the Exchange Management Shell to view transport agents

To view a summary list of transport agents, run the following command:

```powershell
Get-TransportAgent
```

To view the detailed configuration of a specific transport agent, run the following command:

```powershell
Get-TransportAgent <TransportAgentIdentity> | Format-List
```

This example provides a detailed configuration of the transport agent named Transport Rule Agent.

```powershell
Get-TransportAgent "Transport Rule Agent" | Format-List
```

## Use the Exchange Management Shell to configure the priority of a transport agent

Transport agents with a priority closest to 0 process email messages first. However, the SMTP event in the transport pipeline where the transport agent is registered may cause a lower priority agent to act on the message before a higher priority agent.

To modify the priority of an existing transport agent, run the following command:

```powershell
Set-TransportAgent <TransportAgentIdentity> -Priority <Integer>
```

This example sets the priority agent value of 3 for the existing transport agent named Contoso Transport Agent in the Transport service.

```powershell
Set-TransportAgent "Contoso Transport Agent" -Priority 3
```

## How do you know this worked?

To verify that you have successfully configured the priority of a transport agent, run the command `Get-TransportAgent | Format-List Name,Priority` and confirm the priority value of the transport agent.

## Use the Exchange Management Shell to uninstall a transport agent

When the transport agent is uninstalled, Exchange unregisters the DLL files used with the agent. Exchange doesn't remove any files, registry keys, or other objects added by the installation of the transport agent.

To uninstall a transport agent, run the following command:

```powershell
Uninstall-TransportAgent <TransportAgentIdentity>
```

This example uninstalls the transport agent named Fabrikam Transport Agent from the Transport service.

```powershell
Uninstall-TransportAgent "Fabrikam Transport Agent"
```

## How do you know this worked?

To verify that you have successfully uninstalled the transport agent, run the command `Get-TransportAgent` and verify the transport agent isn't listed.
