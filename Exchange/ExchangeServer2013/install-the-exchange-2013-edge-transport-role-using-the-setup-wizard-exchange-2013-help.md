---
title: 'Use the Setup wizard to install the Exchange 2013 Edge Transport role'
TOCTitle: Install the Exchange 2013 Edge Transport role using the Setup wizard
ms:assetid: b8e51b0b-201e-4c64-92c8-3ac0db04b6e2
ms:mtpsurl: https://technet.microsoft.com/library/Dn635117(v=EXCHG.150)
ms:contentKeyID: 61200304
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Install the Exchange 2013 Edge Transport role using the Setup wizard

_**Applies to:** Exchange Server 2013_

This topic explains how to use the Microsoft Exchange Server 2013 Setup wizard to install the Exchange 2013 Edge Transport server role on a computer. The Edge Transport role is available with Exchange 2013 Service Pack 1 (SP1) or later. For more information about planning and deploying Exchange 2013, see [Planning and deployment](planning-and-deployment-for-exchange-2013-installation-instructions.md).

We recommend that the Edge Transport role be installed in a perimeter network outside of your organization's internal Active Directory forest. While you can install the Edge Transport server role on a domain-joined computer, doing so will only enable domain management of Windows features and settings. The Edge Transport role itself doesn't use Active Directory. Instead, it uses the Active Directory Lightweight Directory Services (AD LDS) Windows feature to store configuration and recipient information. For more information about the Edge Transport role, see [Edge Transport servers](edge-transport-servers-exchange-2013-help.md).

If you want to install the Exchange 2013 Mailbox or Client Access roles on a computer, see [Install Exchange 2013 using the Setup wizard](install-exchange-2013-using-the-setup-wizard-exchange-2013-help.md). The Edge Transport role can't be installed on the same computer as the Mailbox or Client Access server roles.

> [!TIP]
> Have you heard about the Exchange Server Deployment Assistant? It's a free online tool that helps you quickly deploy Exchange 2013 in your organization by asking you a few questions and creating a customized deployment checklist just for you. If you want to learn more about it, go to <A href="exchange-server-deployment-assistant-exchange-2013-help.md">Exchange Server Deployment Assistant</A>.

For information about tasks to complete after installation, see [Exchange 2013 post-Installation tasks](exchange-2013-post-installation-tasks-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 40 minutes

- Make sure you've read the release notes prior to installing Exchange 2013. For more information, see [Release notes for Exchange 2013](release-notes-for-exchange-2013-exchange-2013-help.md).

- The computer you install Exchange 2013 on must have a supported operating system (such as Windows Server 2008 R2 with SP1, Windows Server 2012 R2, or Windows Server 2012), have enough disk space, and satisfy other requirements. For information about system requirements, see [Exchange 2013 system requirements](exchange-2013-system-requirements-exchange-2013-help.md).

- To run Exchange 2013 setup, you must install Microsoft .NET Framework 4.5, Windows Management Framework, and other required software. To understand the prerequisites for all server roles, see [Exchange 2013 prerequisites](exchange-2013-prerequisites-exchange-2013-help.md).

- You need to configure the primary DNS suffix on the computer. For example, if the fully qualified domain name of your computer is edge.contoso.com, the DNS suffix for the computer is contoso.com. For more information, see [Primary DNS Suffix is missing](primary-dns-suffix-is-missing-exchange-2013-help.md).

- Exchange 2007 and Exchange 2010 Hub Transport servers need an update before you can create an EdgeSync Subscription between them and an Exchange 2013 Edge Transport server. If you don't install this update, the EdgeSync Subscription won't work correctly. For more information, see the "Supported coexistence scenarios" section in [Exchange 2013 system requirements](exchange-2013-system-requirements-exchange-2013-help.md).

- Make sure the account you use is a member of the local Administrators group on the computer you're installing the Edge Transport role.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!WARNING]
> After you install Exchange on a server, you must not change the server name. Renaming a server after you have installed an Exchange server role is not supported.

## Install Exchange Server 2013

> [!NOTE]
> To download the latest version of Exchange 2013, see <A href="updates-for-exchange-2013-exchange-2013-help.md">Updates for Exchange 2013</A>.

1. Log on to the computer on which you want to install Exchange 2013.

2. Navigate to the network location of the Exchange 2013 installation files.

3. Start Exchange 2013 Setup by double-clicking `Setup.exe`

   > [!IMPORTANT]
   > If you have User Access Control (UAC) enabled, you must right-click <CODE>Setup.exe</CODE> and select <STRONG>Run as administrator</STRONG>.

4. On the **Check for Updates?** page, choose whether you want Setup to connect to the Internet and download product and security updates for Exchange 2013. If you select **Connect to the Internet and check for updates**, Setup will download updates and apply them prior to continuing. If you select **Don't check for updates right now**, you can download and install updates manually later. We recommend that you download and install updates now. Click **Next** to continue.

5. The **Introduction** page begins the process of installing Exchange into your organization. It will guide you through the installation. Several links to helpful deployment content are listed. We recommend that you visit these links prior to continuing setup. Click **Next** to continue.

6. On the **License Agreement** page, review the software license terms. If you agree to the terms, select **I accept the terms in the license agreement**, and then click **Next**.

7. On the **Recommended settings** page, select whether you want to use the recommended settings. If you select **Use recommended settings**, Exchange will automatically send error reports and information about your computer hardware and how you use Exchange to Microsoft. If you select **Don't use recommended settings**, these settings remain disabled but you can enable them at any time after Setup completes. For more information about these settings and how information sent to Microsoft is used, click **?**.

8. On the **Server Role Selection** page, select **Edge Transport**. Remember that you can't add the Mailbox or Client Access server roles to a computer that has the Edge Transport role installed. The management tools are installed automatically if you install any server role.

   Select **Automatically install Windows Server roles and features that are required to install Exchange Server** to have the Setup wizard install required Windows prerequisites. You may need to reboot the computer to complete the installation of some Windows features. If you don't select this option, you must install the Windows features manually.

   > [!NOTE]
   > This option installs only the Windows features required by Exchange. You must install other prerequisites manually. For more information, see <A href="exchange-2013-prerequisites-exchange-2013-help.md">Exchange 2013 prerequisites</A>.

   Click **Next** to continue.

9. On the **Installation Space and Location** page, either accept the default installation location or click **Browse** to choose a new location. Make sure that you have enough disk space available in the location where you want to install Exchange. Click **Next** to continue.

10. On the **Readiness Checks** page, view the status to determine if the organization and server role prerequisite checks completed successfully. If they haven't completed successfully, you must resolve any reported errors before you can install Exchange 2013. You don't need to exit Setup when resolving some of the prerequisite errors. After resolving a reported error, click **back** and then click **Next** to run the prerequisite check again. Be sure to also review any warnings that are reported. If all readiness checks have completed successfully, click **Next** to install Exchange 2013.

11. On the **Completion** page, click **Finish**.

12. Restart the computer after Exchange 2013 has completed.

13. Complete your deployment by performing the tasks provided in [Exchange 2013 post-Installation tasks](exchange-2013-post-installation-tasks-exchange-2013-help.md).

## How do you know this worked?

To verify that you've successfully installed Exchange 2013, see [Verify an Exchange 2013 installation](verify-an-exchange-2013-installation-exchange-2013-help.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Did you find what you're looking for? Please take a minute to [send us feedback](mailto:exsetuphelpfeedback@microsoft.com?subject=exchange%202013%20setup%20help%20feedback) about the information you were hoping to find.
