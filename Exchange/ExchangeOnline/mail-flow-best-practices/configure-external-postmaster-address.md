---
localization_priority: Normal
description: Admins can learn how to configure the external postmaster email address in Exchange Online.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: ece00da0-743b-4e26-83f5-a2eb68c7de4e
ms.reviewer: 
title: Configure the external postmaster address in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
f1.keywords:
- NOCSH
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure the external postmaster address in Exchange Online

The external postmaster address is used as the sender for system-generated messages and notifications sent to message senders that exist outside your Microsoft Exchange Online organization. An external sender is any sender that has an email address in a domain that isn't configured as an accepted domain in your organization.

By default, the value of the external postmaster address setting is blank. This default value sets the external postmaster address to the value postmaster@\<_Default accepted domain_\> for your organization.

There's no mailbox associated with the postmaster@\<_Default accepted domain_\> email address.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mail flow" entry in the [Feature permissions in Exchange Online](../permissions-exo/feature-permissions.md) topic.

- You can only use Exchange Online PowerShell to perform this procedure. To learn how to connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell).


## Use Exchange Online PowerShell to configure the external postmaster address

To configure the external postmaster address, use the following syntax:

```powershell
Set-TransportConfig -ExternalPostmasterAddress <EmailAddress>
```

This example sets the external postmaster address to the value `postmaster@contoso.com`.

```powershell
Set-TransportConfig -ExternalPostmasterAddress postmaster@contoso.com
```

This example returns the external postmaster address to the default value.

```powershell
Set-TransportConfig -ExternalPostmasterAddress $null
```

## How do you know this worked?

To verify that you have successfully configured the external postmaster address, do the following:

1. Run the following command to verify the property value:

   ```powershell
   Get-TransportConfig | Format-List ExternalPostmasterAddress
   ```

   A blank value indicates the default value postmaster@\<_Default accepted domain_\>.

2. From an external email account, send a message to your Exchange organization that will generate a non-delivery report (also known as an NDR or bounce message). For example, you can configure a mail flow rule (also known as a transport rule) to send an NDR for a message from that sender that contains specific keywords. Verify that the sender's email address in the DSN matches the external postmaster address you specified.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).