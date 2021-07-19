---
title: 'Delete and re-create the default discovery mailbox in Exchange: Exchange 2013 Help'
TOCTitle: Delete and re-create the default discovery mailbox in Exchange
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer: 
ms.assetid: 4bde0b00-bdf7-44b4-ba64-aa062bc10ca2
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Delete and re-create the default discovery mailbox in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use the Exchange Management Shell to delete the default discovery mailbox, re-create it, and then assign permissions to it.

## Why would I want to do this?

In Exchange Server 2013, the maximum size of the default discovery mailbox is 50 GB. It's used to store In-Place eDiscovery search results. Before the size limit was changed, organizations could increase the storage quota to more than 50 GB. As a result, discovery mailboxes could grow to more than 50 GB. There are three issues with a default discovery mailbox that is larger than 50 GB:

- It's not supported.

- It can't be migrated to Microsoft 365 or Office 365.

- If it's the default discovery mailbox in Exchange Server 2010, it can't be upgraded to Exchange Server 2013.

How you resolve this depends on whether you want to save the search results from a default discovery mailbox that's exceeded 50 GB.

|**Do you want to save the search results?**|**Do this**|
|:-----|:-----|
|No|Follow the steps in this topic to delete, and then re-create the default discovery mailbox.|
|Yes|Follow the steps in [Reduce the size of a discovery mailbox in Exchange](reduce-discovery-mailbox-size-exchange-2013-help.md).|

## Use the Exchange Management Shell to delete and re-create the default discovery mailbox

> [!NOTE]
> You can't use the Exchange admin center (EAC) because discovery mailboxes aren't displayed in the EAC.

1. Run the following command to delete the default discovery mailbox.

   ```powershell
   Remove-Mailbox "DiscoverySearchMailbox{D919BA05-46A6-415f-80AD-7E09334BB852}"
   ```

2. In the message asking you to confirm that you want to delete the mailbox and the corresponding Active Directory user object, type Y, and then press Enter.

   A new user object is created in Active Directory when you create the discovery mailbox in the next step.

3. Run the following command to re-create the default discovery mailbox.

   ```powershell
   New-Mailbox -Name "DiscoverySearchMailbox{D919BA05-46A6-415f-80AD-7E09334BB852}" -Alias "DiscoverySearchMailbox{D919BA05-46A6-415f-80AD-7E09334BB852}" -DisplayName "Discovery Search  Mailbox" -Discovery
   ```

4. Run the following command to assign the Discovery Management role group permissions to open the default discovery mailbox and view search results.

   ```powershell
   Add-MailboxPermission "DiscoverySearchMailbox{D919BA05-46A6-415f-80AD-7E09334BB852}" -User "Discovery Management" -AccessRights FullAccess -InheritanceType all
   ```
