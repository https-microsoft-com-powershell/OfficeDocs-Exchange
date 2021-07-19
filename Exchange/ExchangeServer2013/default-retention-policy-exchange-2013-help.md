---
title: 'Default Retention Policy in Exchange: Exchange 2013 Help'
TOCTitle: Default Retention Policy in Exchange
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer: 
ms.assetid: bcf31b2d-463b-4623-b488-c8ac40f14f62
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Default Retention Policy in Exchange 2013

_**Applies to:** Exchange Server 2013_

Exchange creates the retention policy Default MRM Policy in your Exchange organization. The policy is applied when you create an archive for the mailbox. You can change the retention policy that's applied to a user at any time.

You can modify tags included in the Default MRM Policy, for example by changing the retention age or retention actions, disable a tag, or modify the policy by adding or removing tags from it. The updated policy is applied to mailboxes the next time they're processed by the Managed Folder Assistant

## Retention tags linked to the Default MRM Policy

The following table lists the default retention tags linked to the Default MRM Policy.

|**Name**|**Type**|**Retention age (days)**|**Retention action**|
|:-----|:-----|:-----|:-----|
|Default 2 years move to archive|Default Policy Tag (DPT)|730|Move to Archive|
|Recoverable Items 14 days move to archive|Recoverable Items folder|14|Move to Archive|
|Personal 1 year move to archive|Personal tag|365|Move to Archive|
|Personal 5 year move to archive|Personal tag|1,825|Move to Archive|
|Personal never move to archive|Personal tag|Not applicable|Move to Archive|
|1 Week Delete|Personal tag|7|Delete and Allow Recovery|
|1 Month Delete|Personal tag|30|Delete and Allow Recovery|
|6 Month Delete|Personal tag|180|Delete and Allow Recovery|
|1 Year Delete|Personal tag|365|Delete and Allow Recovery|
|5 Year Delete|Personal tag|1,825|Delete and Allow Recovery|
|Never Delete|Personal tag|Not applicable|Delete and Allow Recovery|

## What you can do with the Default MRM Policy

|**You can...**|**In Exchange Server...**|
|:-----|:-----|
|Apply the Default MRM Policy automatically to new users|Yes, applied by default if you also create an archive for the new user. <br/> If you create an archive for the user later, the policy is applied automatically only if the user doesn't have an existing Retention Policy.|
|Modify the retention age or retention action of a retention tag linked to the policy|Yes|
|Disable a retention tag linked to the policy|Yes|
|Add a retention tag to the policy|Yes|
|Remove a retention tag from the policy|Yes|
|Set another policy as the default retention policy to be applied automatically to new users|No|

## More information

- A Retention Tag can be linked to more than one Retention Policy. For details about managing [Retention tags and retention policies](retention-tags-and-policies-exchange-2013-help.md), see [Messaging Records Management Procedures](messaging-records-management-procedures-exchange-2013-help.md).

- The Default MRM Policy doesn't include a DPT to automatically delete items (but it does contain personal tags with the delete retention action that users can apply to mailbox items). If you want to automatically delete items after a specified period, you can create a DPT with the required delete action and add it to the policy. For details, see [Create a Retention Policy](create-a-retention-policy-exchange-2013-help.md) and [Add retention tags to or remove retention tags from a retention policy](add-or-remove-retention-tags-exchange-2013-help.md).

- Retention policies are applied to mailbox users. The same policy applies to the user's mailbox and archive.
