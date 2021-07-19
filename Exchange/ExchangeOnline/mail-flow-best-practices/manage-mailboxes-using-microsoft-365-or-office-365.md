---
localization_priority: Normal
description: 'Summary: How to use hosted mail flow with Microsoft 365 or Office 365.'
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 24cac303-c8de-474b-a894-e42683db30f1
ms.reviewer: 
title: Manage all mailboxes and mail flow using Microsoft 365 or Office 365
ms.collection: 
- exchange-online
- M365-email-calendar
f1.keywords:
- NOCSH
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage all mailboxes and mail flow using Microsoft 365 or Office 365

 **Summary**: How to use hosted mail flow with Microsoft 365 or Office 365.

For most organizations, we recommend using hosted mail flow because it's the simplest configuration, in which Microsoft 365 or Office 365 manages all mailboxes and filtering. This simple configuration makes it easy to set up and manage mail flow.

## Manage all mailboxes and mail flow using Microsoft 365 or Office 365 (recommended).
<a name="BKMK_HostedMailFlow"> </a>

### Hosted mail flow scenarios

- I'm a new Microsoft 365 or Office 365 customer, and all my users' mailboxes are in Microsoft 365 or Office 365. I want to use all filtering solutions that Office 365 offers.

- I'm a new Microsoft 365 or Office 365 customer. I have an existing email service, but I plan to immediately move all existing mailboxes to the cloud. I want to use all filtering solutions that Microsoft 365 and Office 365 offer.

For this scenario, your organization's mail flow setup looks like the following diagram:

![Mail flow diagram showing mail going from the internet to Microsoft 365 or Office 365, and from Microsoft 365 or Office 365 to the internet.](../media/96ec9aca-fa95-4307-a992-479a1ed65e5c.png)

#### Best practices for hosted mail flow scenarios

To set up hosted mail flow, we recommend using the Microsoft 365 setup wizard. To get to the Microsoft 365 setup wizard, go to **Setup** in the Microsoft 365 admin center.

![Screenshot of the Setup option in the admin center navigation menu](../media/41bc173f-5a06-4325-b613-b307d3eb0873.png)

The Microsoft 365 setup wizard walks you through the following steps.

1. Add your custom domains in Microsoft 365 or Office 365. To prove that you own the domains, follow the instructions in [Add a domain to Microsoft 365](/microsoft-365/admin/setup/add-domain).

2. [Create user mailboxes in Exchange Online](../recipients-in-exchange-online/create-user-mailboxes.md) or [move all users' mailboxes to Microsoft 365 or Office 365](../mailbox-migration/mailbox-migration.md).

3. Update the DNS records for the domains that you added in step 1. (Not sure how to do this? Follow the instructions on [this page](/microsoft-365/admin/get-help-with-domains/create-dns-records-at-any-dns-hosting-provider).)

   The following DNS records control mail flow:

   - **MX record** - Point your MX record to Microsoft 365 or Office 365 in the following format: \<domainKey\>.mail.protection.outlook.com.

   For example, the domain contoso.com should have the MX record contoso-com.mail.protection.outlook.com.

   - **SPF record** - This is a special TXT record in DNS that identifies a service as a valid sender for a particular domain. Because Microsoft 365 and Office 365 are sending all your messages, list only Microsoft 365 or Office 365 as a valid sender for your domain. To do that, add an SPF record for your domain in the following format:

   ```text
   v=spf1 include:spf.protection.outlook.com -all
   ```

For a full list of setup instructions, check out [Set up Microsoft 365 for business](/microsoft-365/admin/setup/setup) or [Deploy Office 365 Enterprise for your organization](/office365/enterprise/setup-overview-for-enterprises).

## See also
<a name="BKMK_HostedMailFlow"> </a>

[Mail flow best practices for Exchange Online, Microsoft 365, and Office 365 (overview)](mail-flow-best-practices.md)

[Manage mail flow using a third-party cloud service](manage-mail-flow-using-third-party-cloud.md)

[Manage mail flow with mailboxes in multiple locations](manage-mail-flow-for-multiple-locations.md)

[Manage mail flow using a third-party cloud service with mailboxes on Microsoft 365 or Office 365 and on-prem](manage-mail-flow-on-office-365-and-on-prem.md)

[Troubleshoot mail flow](troubleshoot-mail-flow.md)

[Test mail flow by validating your connectors](test-mail-flow.md)