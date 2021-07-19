---
localization_priority: Normal
description: An In-Place Hold preserves all mailbox content, including deleted items and original versions of modified items. All such mailbox items are returned in an In-Place eDiscovery search. When you place an In-Place Hold on a user's mailbox on, the contents in the corresponding archive mailbox (if it's enabled) are also placed on hold, and returned in a eDiscovery search.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 9d5d8d37-a053-4830-9cb1-6e1ede25e963
ms.reviewer: 
f1.keywords:
- NOCSH
title: Remove an In-Place Hold
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars
---

# Remove an In-Place Hold

> [!IMPORTANT]
>  As we continue to invest in different ways to preserve mailbox content, we're announcing the retirement of In-Place Holds in the Exchange admin center (EAC) in Exchange Online. Starting July 1, 2020, you won't be able to create new In-Place Holds. But you'll still be able to manage In-Place Holds in the EAC or by using the **Set-MailboxSearch** cmdlet in Exchange Online PowerShell. However, starting October 1, 2020, you won't be able to manage In-Place Holds. You'll only be remove them in the EAC or by using the **Remove-MailboxSearch** cmdlet. Using In-Place Holds in Exchange Server and Exchange hybrid deployments will still be supported. For more information about the retirement of In-Place Holds in Exchange Online, see [Retirement of legacy eDiscovery tools](/microsoft-365/compliance/legacy-ediscovery-retirement).

An In-Place Hold preserves all mailbox content, including deleted items and original versions of modified items. All such mailbox items are returned in an [In-Place eDiscovery](in-place-ediscovery/in-place-ediscovery.md) search. When you place an In-Place Hold on a user's mailbox on, the contents in the corresponding archive mailbox (if it's enabled) are also placed on hold, and returned in a eDiscovery search.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure. To see what permissions you need, see the "In-Place Hold" entry in the [Feature permissions in Exchange Online](../permissions-exo/feature-permissions.md) topic.

- Depending on your Active Directory topology and replication latency, it may take up to an hour for the removal of an In-Place Hold to take effect.

- As previously explained, when you place an In-Place Hold on a user's mailbox, content in the user's archive mailbox is also placed on hold. When you remove an In-Place Hold on an on-premises primary mailbox in an Exchange hybrid deployment, the hold is also removed from the cloud-based archive mailbox (if enabled).

- If a user was placed on multiple In-Place Holds, the search queries from any query-based hold are combined (with **OR** operators). In this case, the maximum number of keywords in all query-based holds placed on a mailbox is 500. If there are more than 500 keywords, then all content in the mailbox is placed on hold (not just that content that matches the search criteria). All content is held until the total number of keywords is reduced to 500 or less.

## Remove an In-Place Hold

> [!IMPORTANT]
> Mailbox searches can be used for an In-Place Hold and In-Place eDiscovery. You can't remove a mailbox search that's used for In-Place Hold. You must first disable the In-Place Hold by clearing the **Place content matching the search query in selected mailboxes on hold** check box on the **In-Place Hold settings** page or by setting the _InPlaceHoldEnabled_ parameter to `$false` in Exchange Online PowerShell. You can also remove a mailbox by using the _SourceMailboxes_ parameter specified in the search.

### Use the EAC to remove an In-Place Hold

1. Navigate to **Compliance management** \> **In-Place eDiscovery & hold**.

2. In the list view, select the In-Place Hold you want to remove and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif).

3. In **In-Place eDiscovery & Hold** properties, on the **In-Place Hold** page, clear the **Place content matching the search query in selected mailboxes on hold**, and then click **Save**.

4. Select the In-Place Hold again from the list view, and then click **Delete** ![Delete icon](../media/ITPro_EAC_DeleteIcon.gif).

5. In warning, click **Yes** to remove the search.

### Use Exchange Online PowerShell to remove an In-Place Hold

This example first disables In-Place Hold named Hold-CaseId012 and then removes the mailbox search.

```PowerShell
Set-MailboxSearch "Hold-CaseId012" -InPlaceHoldEnabled $false
Remove-MailboxSearch "Hold-CaseId012"
```

For detailed syntax and parameter information, see [Set-MailboxSearch](/powershell/module/exchange/set-mailboxsearch).

### How do you know this worked?

To verify that you have successfully removed an In-Place Hold, do one of the following:

- Use the EAC to verify that the In-Place Hold doesn't appear in the list view of the **In-place eDiscovery & hold** tab.

- Use the **Get-MailboxSearch** cmdlet to retrieve all mailbox searches and check that the search you removed is no longer listed. For an example of how to retrieve a mailbox search, see the examples in [Get-MailboxSearch](/powershell/module/exchange/get-mailboxsearch).