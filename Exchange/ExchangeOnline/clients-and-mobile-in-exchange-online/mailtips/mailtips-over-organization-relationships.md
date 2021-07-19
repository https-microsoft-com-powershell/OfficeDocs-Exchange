---
localization_priority: Normal
description: Microsoft Exchange Server allows you to configure organization relationships with Microsoft Exchange Online or other Exchange organizations. Establishing an organization relationship allows you to enhance the user experience when dealing with the other organization. For example, you can share free or busy data, configure secure message flow, and enable message tracking across both organizations.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 1784256f-abe1-4503-b8c4-26d544b73452
ms.reviewer: 
title: MailTips over organization relationships
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# MailTips over organization relationships

Microsoft Exchange Server allows you to configure organization relationships with Microsoft Exchange Online or other Exchange organizations. Establishing an organization relationship allows you to enhance the user experience when dealing with the other organization. For example, you can share free or busy data, configure secure message flow, and enable message tracking across both organizations.

## Controlling the MailTips access level

You may want to restrict certain types of MailTips. You can either allow all MailTips to be returned or allow only a limited set that would prevent NDRs. You can configure this setting with the _MailTipsAccessLevel_ parameter on the **Set-OrganizationRelationship** cmdlet. The following table shows which MailTips are returned over the organization relationship.

|**MailTip**|**Is the MailTip available when the access level is set to All?**|**Is the MailTip available when the access level is set to Limited?**|
|:-----|:-----|:-----|
|Large Audience|Yes|No|
|Automatic Replies|Yes  <br/> If the remote domain of the recipient is specified as internal, the internal automatic reply is displayed. Otherwise, the external automatic reply is displayed.|Yes  <br/> The external automatic reply is displayed.|
|Moderated Recipient|Yes|No|
|Oversize Message|Yes|Yes|
|Restricted Recipient|Yes|Yes|
|Mailbox Full|Yes|No|
|Custom MailTips|Yes|No|
|External Recipients|Yes  <br/> If the remote domain of the recipient is specified as internal, this MailTip is suppressed. Otherwise, the external MailTip is returned.|Yes  <br/> If the remote domain of the recipient is specified as internal, this MailTip is suppressed. Otherwise, the external MailTip is returned.|

For detailed steps about how to configure MailTips access levels, see [Manage MailTips for organization relationships](manage-mailtips-for-organization-relationships.md).

## Controlling the MailTips access scope

When you enable MailTips over an organization relationship and set the access level to `All`, the recipient-specific MailTips, Mailbox Full, Automatic Replies, and custom MailTips, are returned for all users. However, you may only want to allow these MailTips for a specific set of users. For example, if you set up an organization relationship with a partner, you may want to allow these MailTips only for the users that work with that partner.

To achieve this, you need to first create a group and add all users for whom you want to share recipient-specific MailTips to that group. You can then specify that group on the organization relationship.

After you implement this restriction, your Client Access servers will first verify whether the recipient for whom they received a MailTips query is part of this group. If the recipient is a member of this group, the Client Access servers will proxy back all MailTips including the recipient-specific MailTips. Otherwise they won't include the recipient-specific MailTips in their response.

For detailed steps about how to configure MailTips access levels, see [Manage MailTips for organization relationships](manage-mailtips-for-organization-relationships.md).
