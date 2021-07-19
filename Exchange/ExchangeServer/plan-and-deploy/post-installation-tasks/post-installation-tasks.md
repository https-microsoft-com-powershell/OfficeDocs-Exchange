---
localization_priority: Normal
description: "Summary: Learn about tasks you'll need to do after you install Exchange 2016 or Exchange 2019."
ms.topic: get-started-article
author: msdmaguire
ms.author: dmaguire
ms.assetid: bd99aaa4-b82c-427c-ab65-b9230ff63fb2
ms.reviewer: 
title: Exchange Server post-installation tasks
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Exchange Server post-installation tasks

Read the following topics to help you configure your new Exchange 2016 or Exchange 2016 organization.

|**Topic**|**Description**|
|:-----|:-----|
|[Enter your Exchange product key](enter-product-key.md)|Learn how to license your Exchange server.|
|[Configure mail flow and client access on Exchange servers](configure-mail-flow-and-client-access.md)|Learn how to configure mail flow to and from the Internet and configure Exchange to accept client connections from the Internet.|
|[Verify Exchange Server installations](verify-installation.md)|Learn how to verify that Exchange 2016 was installed successfully in your organization.|
|[Install the Exchange management tools](install-management-tools.md)|Learn how to install the Exchange Management Shell and Exchange Toolbox on client workstations or other non-Exchange servers in your organization.|
|[Configure instant messaging integration with Outlook on the web in Exchange](configure-im-integration-with-owa.md)|Learn how to configure instant messaging (IM) integration between Skype for Business Server and Outlook on the web (formerly known as Outlook Web App)|
|[Change the offline address book generation schedule in Exchange](change-oab-generation-schedule.md)|Learn how to change the offline address book (OAB) generation schedule on specific Exchange servers or for the whole organization|
|[Configure certificate based authentication in Exchange 2016](configure-certificate-based-auth.md)|Learn how to configure CBA in Exchange 2016 CU1 or later|
|[Edge Subscriptions](../../architecture/edge-transport-servers/edge-subscriptions.md)|Learn how to configure an EdgeSync Subscription between a new Edge Transport server in the perimeter network and the Exchange Mailbox servers in an internal Active Directory site.|

**Note**:

If you've enabled the Scripting Agent in your Exchange organization, and you keep a customized %ExchangeInstallPath%Bin\CmdletExtensionAgents\ScriptingAgentConfig.xml file on all of your Mailbox servers, you need to copy that file to every new Mailbox server that you deploy in your organization (the file isn't used on Edge Transport servers).

- The default value of %ExchangeInstallationPath% is %ProgramFiles%\Microsoft\Exchange Server\V15\, but the actual value is wherever you installed Exchange on the server.

- The default name of the file on a new Exchange server is %ExchangeInstallPath%Bin\CmdletExtensionAgents\ScriptingAgentConfig.xml.sample. As part of enabling the Scripting Agent in your organization, you need to rename this file to ScriptingAgentConfig.xml and customize it or replace it with your existing ScriptingAgentConfig.xml file.

For more information about the Scripting Agent in Exchange 2013 (which still applies to Exchange 2016 and 2019), see [Scripting Agent](../../../ExchangeServer2013/cmdlet-extension-agents-exchange-2013-help.md#scripting-agent).