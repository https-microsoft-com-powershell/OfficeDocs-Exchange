---
localization_priority: Normal
description: Admins can learn about the different types of address lists that are available in Exchange Online.
ms.topic: overview
author: msdmaguire
ms.author: jhendr
ms.assetid: 8ee2672a-3a45-4897-8cc0-fa23c374dbf9
ms.reviewer: 
title: Address lists in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Address lists in Exchange Online

An address list is a collection of mail-enabled recipient objects in Exchange Online. Address lists are based on recipient filters. You can filter by recipient type (for example, mailboxes and mail contacts), recipient properties (for example, Company or State or Province), or both. Address lists aren't static; they're updated dynamically. When you create or modify recipients in your organization, they're automatically added to the appropriate address lists. These are the different types of address lists that are available:

- **Global address lists (GALs)**: The built-in GAL that's automatically created by Exchange Online includes every mail-enabled object in the organization. You can create additional GALs to separate users by organization or location, but a user can only see and use one GAL.

- **Address lists**: Address lists are subsets of recipients that are grouped together in one list, which makes them easier to find by users. Exchange Online comes with several built-in address lists, and you can create more based on you organization's needs.

- **Offline address books (OABs)**: OABs contain address lists and GALs. OABs are used by Outlook clients in cached Exchange mode to provide local access to address lists and GALs for recipient look-ups. For more information, see [Offline address books in Exchange Online].

Users in your organization use address lists and the GAL to find recipients for email messages. Here's an example of what address lists look like in Outlook 2016:

![Global Address List (GAL)](../../media/54c5aa4b-fbd3-4b37-8642-9a52b9558641.png)

For procedures related to address lists, see [Address list procedures in Exchange Online](address-list-procedures.md).

**Notes:**

- By default, the Address List role isn't assigned to any role groups in Exchange Online. To use any cmdlets or features that require the Address List role, you need to add the role to a role group. For more information, see [Modify role groups](../../permissions-exo/role-groups.md#modify-role-groups).

- Precanned recipient filters or custom recipient filters identify the recipients that are included in address lists and GALs. For more information, see [Recipient filters for address lists in Exchange Online PowerShell](use-recipient-filters-to-create-an-address-list.md).

- You can hide recipients from all address lists and GALs. For more information, see [Hide recipients from address lists](manage-address-lists.md#hide-recipients-from-address-lists).

## Global address lists

By default, a new Exchange Online organization has a GAL named Default Global Address List that's the primary repository of all recipients in the organization. Typically, most organizations have only one GAL, because users can only see and use one GAL in Outlook and Outlook on the web (formerly known as Outlook Web App). You might need to create multiple GALs if you want to prevent groups of recipients from seeing each other (for example, your organization contains two separate companies). If you plan on creating additional GALs, consider the following issues:

- You can only use the Exchange Online PowerShell to create, modify, remove, and update GALs.

- Users can only see a GAL that they belong to (the recipient filter of the GAL includes them). If a user belongs to multiple GALs, they'll still see only one GAL based on the following conditions:
  - The user needs permissions to view the GAL. You assign user permissions to GALs by using address book policies (ABPs). For more information, see [Address book policies in Exchange Online](../address-book-policies/address-book-policies.md).
  - If a user is still eligible to see multiple GALs, only the largest GAL is used (the GAL that contains the most recipients).
  - Each GAL needs a corresponding offline address book (OAB) that includes the GAL. To create OABs, see [Create an offline address book in Exchange Online](../offline-address-books/create-offline-address-book.md).

- The built-in GAL is named Default Global Address List, and any additional GALs that you create require unique names. Depending on the email client, users might not see the actual name of the GAL that they're using:
  - In Outlook on the web, users see the actual name of the GAL that they're using (for example, Default Global Address List).
  - In Outlook, the GAL always appears as Global Address List, which likely doesn't match the actual name.

## Default address lists

By default, Exchange Online comes with five built-in address lists and one GAL. These address lists are described in the following table. Note that by default, system-related mailboxes like arbitration mailboxes and public folder mailboxes are hidden from address lists.

<br><br>

****

|Name|Type|Description|Recipient filter used|
|---|---|---|---|
|All Contacts|Address list|Includes all mail contacts in the organization. To learn more about mail contacts, see [Recipients in Exchange Online](../../recipients-in-exchange-online/recipients-in-exchange-online.md).|`"Alias -ne $null -and (ObjectCategory -like 'person' -and ObjectClass -eq 'contact')"`|
|All Distribution Lists|Address list|Includes all distribution groups and mail-enabled security groups in the organization. To learn more about mail-enabled groups, see [Recipients in Exchange Online](../../recipients-in-exchange-online/recipients-in-exchange-online.md).|`"Alias -ne $null -and ObjectCategory -like 'group'"`|
|All Rooms|Address list|Includes all room mailboxes. Equipment mailboxes aren't included. To learn more about room and equipment (resource) mailboxes, see [Recipients in Exchange Online](../../recipients-in-exchange-online/recipients-in-exchange-online.md).|`"Alias -ne $null -and (RecipientDisplayType -eq 'ConferenceRoomMailbox' -or RecipientDisplayType -eq 'SyncedConferenceRoomMailbox')"`|
|All Users|Address list|Includes all user mailboxes, linked mailboxes, remote mailboxes (Microsoft 365 or Office 365 mailboxes), shared mailboxes, room mailboxes, equipment mailboxes, and mail users in the organization. To learn more about these recipient types, see [Recipients in Exchange Online](../../recipients-in-exchange-online/recipients-in-exchange-online.md).|`"((Alias -ne $null) -and (((((((ObjectCategory -like 'person') -and (ObjectClass -eq 'user') -and (-not(Database -ne $null)) -and (-not(ServerLegacyDN -ne $null)))) -or (((ObjectCategory -like 'person') -and (ObjectClass -eq 'user') -and (((Database -ne $null) -or (ServerLegacyDN -ne $null))))))) -and (-not(RecipientTypeDetailsValue -eq 'GroupMailbox')))))"`|
|Default Global Address List|GAL|Includes all mail-enabled recipient objects in the organization (users, contacts, groups, dynamic distribution groups, and public folders.|`"((Alias -ne $null) -and (((ObjectClass -eq 'user') -or (ObjectClass -eq 'contact') -or (ObjectClass -eq 'msExchSystemMailbox') -or (ObjectClass -eq 'msExchDynamicDistributionList') -or (ObjectClass -eq 'group') -or (ObjectClass -eq 'publicFolder'))))"`|
|Public Folders|Address list|Includes all mail-enabled public folders in your organization. Access permissions determine who can view and use public folders. For more information about public folders, see [Public folders in Microsoft 365 or Office 365 and Exchange Online](../../collaboration-exo/public-folders/public-folders.md).|`"Alias -ne $null -and ObjectCategory -like 'publicFolder'"`|
|

## Custom Address Lists

An Exchange Online organization might contain thousands of recipients, so the built-in address lists could become quite large. To prevent this, you can create custom address lists to help users find what they're looking for.

For example, consider a company that has two large divisions in one Exchange Online organization:

- Fourth Coffee, which imports and sells coffee beans.
- Contoso, Ltd, which underwrites insurance policies.

For most day-to-day activities, employees at Fourth Coffee don't communicate with employees at Contoso, Ltd. Therefore, to make it easier for employees to find recipients who exist only in their division, you can create two new custom address lists: one for Fourth Coffee and one for Contoso, Ltd. However, if an employee is unsure about where recipient exists, they can search in the GAL, which contains all recipients from both divisions.

In Exchange Online, you can only use PowerShell to create custom address lists.

## Best Practices for Creating Address Lists

Although address lists are useful tools for users, poorly planned address lists can cause frustration. To make sure that your address lists are practical for users, consider the following best practices:

- Address lists should make it easier for users to find recipients.
- Avoid creating so many address lists that users can't tell which list to use.
- Use a naming convention and location hierarchy for your address lists so users can immediately tell what the list is for (which recipients are included in the list). If you have difficulty naming your address lists, create fewer lists and remind users that they can find anyone in your organization by using the GAL.

For detailed instructions about creating address lists in Exchange Server, see [Address list procedures in Exchange Online](address-list-procedures.md).
