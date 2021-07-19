---
localization_priority: Normal
ms.author: jhendr
manager: serdars
ms.topic: article
author: msdmaguire
ms.service: exchange-online
ms.assetid: ca7b7416-92ed-40ad-abdb-695be46ea2e4
ms.reviewer:
ms.collection:
- exchange-online
- M365-email-calendar
description: Admins can learn how to configure Directory-Based Edge Blocking (DBDB) to reject messages sent to invalid recipients in Exchange Online and Exchange Online Protection during a migration.
f1.keywords:
- NOCSH
audience: ITPro
title: Use Directory Based Edge Blocking to reject messages sent to invalid recipients

---

# Use Directory Based Edge Blocking to reject messages sent to invalid recipients

Directory Based Edge Blocking (DBEB) lets you reject messages for invalid recipients at the service network perimeter in Microsoft 365 organizations with Exchange Online mailboxes and in standalone Exchange Online Protection (EOP) organizations without Exchange Online mailboxes. DBEB lets admins add mail-enabled recipients to Microsoft 365 or Office 365 and block all messages sent to email addresses that aren't present in Microsoft 365 or Office 365.

If a message is sent to a valid email address in Microsoft 365 or Office 365, the message continues through the rest of the service filtering layers: anti-malware, anti-spam, and mail flow rules (also known as transport rules). If the address doesn't exist, the service blocks the message before filtering even occurs, and a non-delivery report (also known as an NDR or _bounce message_) is returned to the sender. The NDR looks like this: `550 5.4.1 Recipient address rejected: Access denied`.

**If all recipients for your domain are in Exchange Online, DBEB is already in effect, and you don't need to do anything**. If you're migrating from another email system to Exchange Online, you can use the procedure in this topic to enable DBEB for the domain before the migration.

> [!NOTE]
>
> - In hybrid environments, in order for DBEB to work, the MX record for the domain must point to Microsoft 365 or Office 365 so that email for the domain is routed to Microsoft 365 or Office 365 first.
>
> - There are additional considerations when using DBEB with mail-enabled public folders. DBEB is not supported for mail-enabled public folders that are hosted in Exchange Online. DBEB is only supported for mail-enabled public folders hosted on-premises. For more information about DBEB and mail-enabled public folders, see [Office 365 Directory Based Edge Blocking support for on-premises Mail Enabled Public Folders](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Office-365-Directory-Based-Edge-Blocking-support-for-on-premises/ba-p/606740).

## What do you need to know before you begin?

- Estimated time to complete: 5 to 10 minutes

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../exchange-admin-center.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange), or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Configure DBEB

1. Verify that your accepted domain in Exchange Online is set to **Internal relay**:
   1. In the EAC, go to **Mail flow** \> **Accepted domains**.
   2. Select the domain and click **Edit**.
   3. Ensure that the domain type is set to **Internal relay**. If it's set to **Authoritative**, change it to **Internal relay** and click **Save**.

2. Add users to Microsoft 365 or Office 365. For example:
   - **Directory synchronization**: Add valid users to Office 365 by synchronizing from your on-premises Active Directory environment to [Azure Active Directory](/azure/active-directory/) in the cloud. For more information about how to set up directory synchronization, see "Use directory synchronization to manage recipients" in [Manage Mail Users in EOP](/exchange/standalone-eop/manage-mail-users-in-eop).
   - **Add users via PowerShell or the EAC**: For more information about how to do this, see [Manage Mail Users in EOP](/exchange/standalone-eop/manage-mail-users-in-eop) or [Manage mail users in Exchange Online](../recipients-in-exchange-online/manage-mail-users.md).

3. Set your accepted domain in Exchange Online to **Authoritative**:
   1. In the EAC, go to **Mail flow** \> **Accepted domains**.
   2. Select the domain and click **Edit**.
   3. Set the domain type to **Authoritative**.

4. Choose **Save** to save your changes, and confirm that you want to enable DBEB.

> [!NOTE]
>
> - Dynamic distribution groups do not sync to Azure AD and are therefore blocked by DBEB. As a workaround in hybrid environments, you can create a mail contact with the same external email address of the blocked dynamic distribution group. In cloud-only environments, this workaround will not work. To use dynamic distribution groups that receive email from external senders in cloud-only environments, you need to disable DBEB (change the domain from **Authoritative** to **Internal relay**).
>
> - Until all of your valid recipients have been added to Exchange Online and replicated through the system, you should leave the accepted domain configured as **Internal relay**. Once the domain type has been changed to **Authoritative**, DBEB is designed to allow any SMTP address that has been added to the service (except for mail-enabled public folders). There might be infrequent instances where recipient addresses that do not exist in your Microsoft 365 or Office 365 organization are allowed to relay through the service.