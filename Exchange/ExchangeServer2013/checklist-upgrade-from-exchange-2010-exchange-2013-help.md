---
title: 'Checklist: Upgrade from Exchange 2010: Exchange 2013 Help'
TOCTitle: 'Checklist: Upgrade from Exchange 2010'
ms:assetid: 06c1045a-5fcf-4e24-a901-1a979302fb8d
ms:mtpsurl: https://technet.microsoft.com/library/Ee332309(v=EXCHG.150)
ms:contentKeyID: 50873999
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Checklist: Upgrade from Exchange 2010

_**Applies to:** Exchange Server 2013_

Use this checklist to upgrade from Microsoft Exchange 2010 to Exchange 2013. Before you start working with this checklist, make sure you're familiar with the concepts discussed in:

  - [Planning and deployment](planning-and-deployment-for-exchange-2013-installation-instructions.md)

  - [What's new in Exchange 2013](what-s-new-in-exchange-2013-exchange-2013-help.md)

This checklist is generic in that it provides guidance for a typical upgrade scenario.

> [!NOTE]
> The Exchange Server Deployment Assistant provides you with customized step-by-step guidance about how to deploy Exchange Server. The Deployment Assistant can help you deploy a new installation of Exchange Server 2013, upgrade a previous version to Exchange 2013, or configure a hybrid deployment of Exchange 2013 and Exchange Online. To learn more, see <A href="exchange-server-deployment-assistant-exchange-2013-help.md">Exchange Server Deployment Assistant</A>.

## Checklist for upgrading from Exchange 2010 to Exchange 2013

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Done?</p></td>
<td><p>Task</p></td>
<td><p>Topic(s)</p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>1. Read the release notes.</p></td>
<td><p><a href="release-notes-for-exchange-2013-exchange-2013-help.md">Release notes for Exchange 2013</a></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>2. Verify system requirements</p></td>
<td><p><a href="exchange-2013-system-requirements-exchange-2013-help.md">Exchange 2013 system requirements</a></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>3. Confirm prerequisite steps are done</p></td>
<td><p><a href="exchange-2013-prerequisites-exchange-2013-help.md">Exchange 2013 prerequisites</a></p>
<p><a href="deployment-security-checklist-exchange-2013-help.md">Deployment security checklist</a></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>4. Configure disjoint namespace</p>

> [!NOTE]
> This step is optional. It's only necessary if your organization is running a disjoint namespace.

</td>
<td><p><a href="configure-the-dns-suffix-search-list-for-a-disjoint-namespace-exchange-2013-help.md">Configure the DNS suffix search list for a disjoint namespace</a></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>5. Select an offline address book for all Exchange 2010 mailbox databases</p></td>
<td><p><a href="manage-mailbox-databases-in-exchange-2013-exchange-2013-help.md">Set mailbox database properties</a> in <a href="manage-mailbox-databases-in-exchange-2013-exchange-2013-help.md">Manage mailbox databases in Exchange 2013</a></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>6. Install Exchange 2013</p></td>
<td><p><a href="install-exchange-2013-using-the-setup-wizard-exchange-2013-help.md">Install Exchange 2013 using the Setup wizard</a></p>
<p><a href="install-the-exchange-2013-edge-transport-role-using-the-setup-wizard-exchange-2013-help.md">Install the Exchange 2013 Edge Transport role using the Setup wizard</a></p>
<p><a href="verify-an-exchange-2013-installation-exchange-2013-help.md">Verify an Exchange 2013 installation</a></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>7. Create an Exchange 2013 mailbox</p></td>
<td><p><a href="create-user-mailboxes-exchange-2013-help.md">Create user mailboxes</a></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>8. Configure Exchange-related virtual directories</p>

> [!NOTE]
> This step is necessary if you want to use Exchange Web Services, Outlook Anywhere, or the offline address book. It also may be required if you need to change any of the default settings for Exchange Control Panel, Microsoft Office&nbsp;Outlook Web App, or Exchange ActiveSync.<BR>

</td>
<td><p><a href="exchange-2013-client-access-server-configuration-exchange-2013-help.md">Exchange 2013 Client Access server configuration</a></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>9. Add digital certificates on the Client Access server</p></td>
<td><p><a href="digital-certificates-and-ssl-exchange-2013-help.md">Digital certificates and SSL</a> </p>
<p></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>10. Move arbitration mailbox</p></td>
<td><p><a href="move-the-exchange-2010-system-mailbox-to-exchange-2013-exchange-2013-help.md">Move the Exchange 2010 system mailbox to Exchange 2013</a></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>11. Configure Unified Messaging</p>

> [!NOTE]
> This step is optional. It's only necessary if you want to use Unified Messaging in your organization.

</td>
<td><p><a href="upgrade-exchange-2010-um-to-exchange-2013-um-exchange-2013-help.md">Upgrade Exchange 2010 UM to Exchange 2013 UM</a></p>
<p></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>12. Configure Edge Transport server</p>

> [!NOTE]
> This step is optional. It's only necessary if your organization is uses an Edge Transport server.

</td>
<td><p><a href="configure-internet-mail-flow-through-a-subscribed-edge-transport-server-exchange-2013-help.md">Configure Internet mail flow through a subscribed Edge Transport server</a></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>13. Enable and configure Outlook Anywhere</p></td>
<td><p><a href="outlook-anywhere-exchange-2013-help.md">Outlook Anywhere</a></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>14. Configure service connection point</p></td>
<td><p><a href="exchange-2013-client-access-server-configuration-exchange-2013-help.md">Exchange 2013 Client Access server configuration</a></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>15. Configure DNS records</p></td>
<td><p><a href="/previous-versions/exchange-server/exchange-150/dn307232(v=exchg.150)">Configure DNS records for Exchange 2010 multiple-server install</a></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>16. Move mailboxes from Exchange 2010 to Exchange 2013</p></td>
<td><p><a href="mailbox-moves-in-exchange-2013-exchange-2013-help.md">Mailbox moves in Exchange 2013</a></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>17. Move public folder data from Exchange 2013 to Exchange 2013</p></td>
<td><p><a href="public-folders-exchange-2013-help.md">Public folders</a></p>
<p><a href="/previous-versions/exchange-server/exchange-150/jj150486(v=exchg.150)">Use serial migration to migrate public folders to Exchange 2013 from previous versions</a></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>18. Post-installation tasks</p></td>
<td><p><a href="exchange-2013-post-installation-tasks-exchange-2013-help.md">Exchange 2013 post-Installation tasks</a></p></td>
</tr>
</tbody>
</table>