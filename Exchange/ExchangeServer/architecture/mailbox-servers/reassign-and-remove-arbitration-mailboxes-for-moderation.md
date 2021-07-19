---
localization_priority: Normal
description: Administrators can learn how to identify the arbitration mailbox that's used for message approval on moderated recipients. After all moderated recipients are configured to no longer use the arbitration mailbox, you can remove the arbitration mailbox.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 860df43f-a05b-4da3-83f1-68d3123a923d
ms.reviewer: 
f1.keywords:
- NOCSH
title: Reassign and remove arbitration mailboxes that are used for moderated recipients
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars
ROBOTS: NOINDEX, NOFOLLOW

---

# Reassign and remove arbitration mailboxes that are used for moderated recipients

Among other things, [arbitration mailboxes](recreate-arbitration-mailboxes.md) are used in the approval flow for moderated recipients. Specifically, messages that are waiting for approval by a moderator are temporarily stored in the arbitration mailbox that's specified for the moderated recipient. The original message is kept in the arbitration mailbox until a moderator takes action on the message.

If you try to remove an arbitration mailbox that's configured for the moderated recipient, you'll receive the following error:

> Can't remove the arbitration mailbox \<mailbox\> because it's being used for the approval workflow for existing recipients that have either membership restrictions or moderation enabled. You should either disable the approval features on those recipients or specify a different arbitration mailbox for those recipients before removing this arbitration mailbox.

To remove the arbitration mailbox, you need to do the following steps:

1. Identify all moderated recipients who are configured to use the arbitration mailbox.
2. Configure a different arbitration mailbox for some, all, or none of the moderated recipients.
3. Turn off moderation for some, all, or none of the moderated recipients who are configured to use the arbitration mailbox.

After you complete these steps so that no moderated recipients are configured to use the arbitration mailbox, you can remove the original arbitration mailbox. The rest of this article describes how to do these steps.

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes

- To use Exchange PowerShell, see [Open the Exchange Management Shell](/powershell/exchange/open-the-exchange-management-shell) or [Connect to Exchange servers using remote PowerShell](/powershell/exchange/connect-to-exchange-servers-using-remote-powershell).

- The _ArbitrationMailbox_ parameter is available to modify the following types of recipients using the corresponding cmdlets:
  - [Set-DistributionGroup](/powershell/module/exchange/set-distributiongroup)
  - [Set-DynamicDistributionGroup](/powershell/module/exchange/set-dynamicdistributiongroup)
  - [Set-Mailbox](/powershell/module/exchange/set-mailbox)
  - [Set-MailContact](/powershell/module/exchange/set-mailcontact)
  - [Set-MailUser](/powershell/module/exchange/set-distributiongroup)

- You need to be assigned permissions before you can perform these procedures. To see what permissions you need, see the "Arbitration" entry in the [Feature permissions](../../permissions/feature-permissions/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

## Step 1: Use Exchange PowerShell to find all recipients who use the arbitration mailbox

In Exchange PowerShell, use the following syntax:

```powerphell
$AM = Get-Mailbox -Identity "<arbitration mailbox identity>" -Arbitration
$AMDN = $AM.DistinguishedName
Get-Recipient -RecipientPreviewFilter "ArbitrationMailbox -eq '$AMDN'"
```

This example find all moderated recipients who are configured to use the arbitration mailbox named Arbitration Mailbox01.

```powershell
$AM = Get-Mailbox "Arbitration Mailbox01" -Arbitration
$AMDN = $AM.DistinguishedName
Get-Recipient -RecipientPreviewFilter "ArbitrationMailbox -eq '$AMDN'"
```

> [!NOTE]
> The arbitration mailbox is specified using the distinguished name (DN). If you know the DN of the arbitration mailbox, you can replace `<DN>` with the actual DN value and run this single command: `Get-Recipient -RecipientPreviewFilter "ArbitrationMailbox -eq '<DN>'"`.

## Step 2: Use Exchange PowerShell to assign a different arbitration mailbox for the moderated recipients

If you choose to specify a different arbitration mailbox for the recipients, use the following syntax in Exchange PowerShell:

```powershell
Set-<RecipientType> -Identity <Identity> -ArbitrationMailbox <identity of different arbitration mailbox>
```

This example reconfigures the distribution group named All Employees to use the arbitration mailbox named Arbitration Mailbox02.

```powershell
Set-DistributionGroup -Identity "All Employees" -ArbitrationMailbox "Arbitration Mailbox02"
```

## Step 3: Use Exchange PowerShell to disable moderation for the moderated recipients who are using the arbitration mailbox

If you choose to disable moderation for the recipients, use the following syntax:

```powershell
Set-<RecipientType> -Identity <Identity> -ModerationEnabled $false
```

This example disables moderation for the mailbox named Human Resources.

```powershell
Set-Mailbox -Identity "Human Resources" -ModerationEnabled $false
```

## How do you know this worked?

The procedure was successful if you can delete the arbitration mailbox without receiving the error that it's being used.
