---
localization_priority: Normal
description: 'Summary: Learn about email address policies in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: overview
author: msdmaguire
ms.author: dmaguire
ms.assetid: b63b63bb-6faf-4337-8441-50bc64b49bb8
ms.reviewer: 
title: Email address policies in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Email address policies in Exchange Server

Email address policies define the rules that create email addresses for recipients in your Exchange organization. Email address policies in Exchange Server 2016 and Exchange Server 2019 are basically unchanged from Exchange Server 2010.

The SMTP domains that are available to use in email address policies are defined by the *accepted domains* that are configured in the Exchange organization (specifically, authoritative domains and internal relay domains). For more information about accepted domains, see [Accepted domains in Exchange Server](../../mail-flow/accepted-domains/accepted-domains.md).

The basic components of an email address policy are:

- **Email address templates**: Define the email address format for the recipients (for example `<firstname>@contoso.com` or `<lastname>.<firstname>@contoso.com`).

- **Recipient filter**: Specifies the recipients whose email addresses are configured by the policy.

- **Priority**: Specifies the order to apply the email address policies (important if a recipient is identified by more than one policy).

To configure email address policies, see [Procedures for email address policies in Exchange Server](eap-procedures.md).

## Email address templates

An email address template contains the **address type** and the **address format**. An email address policy can contain multiple email address templates. One template must define the primary (reply) SMTP email address, and there can be only one primary SMTP email address defined in the policy (it's the **Reply-To:** email address for recipients). Other email address templates in the policy define the additional or *proxy* addresses for recipients.

### Address types

Although you'll primarily use SMTP email addresses in email address policies, other email address types are available. The valid address type values are:

- **SMTP**

- **GWISE**: Novell GroupWise. By default, looks for the missing `%ExchangeInstallPath%Mailbox\address\gwise\amd64\gwxpxgen.dll` file to validate the email address format.

- **NOTES**: Lotus Notes. By default, uses the included `%ExchangeInstallPath%Mailbox\address\notes\amd64\ntspxgen.dll` file to validate the email address format.

- **X400**: By default, uses the included `%ExchangeInstallPath%Mailbox\address\notes\amd64\x400prox.dll` file to validate the email address format.

 **Notes**:

- In the Exchange Management Shell, the value `SMTP` specifies the primary email address, and the value `smtp` specifies additional (proxy) addresses.

  In the EAC, only the **Make this format the reply email address** check box controls whether the email address is the primary address or a proxy address. It doesn't matter whether you type SMTP or smtp in the **Enter a custom address type** field. However, in the list of email address templates in the policy, the EAC shows the value **SMTP** (bold and uppercase) for the primary address, and smtp (not bold and lowercase) for proxy addresses.

- The types of email addresses that you can configure in a email address policy are limited compared to those you can configure on individual recipients.

- All non-SMTP email addresses are considered custom address types. Exchange doesn't provide unique dialog boxes or property pages for X.400, Novell GroupWise, or Lotus Notes email address types. Non-SMTP email addresses require the appropriate .dll files.

### Address formats

An SMTP email address uses the syntax `chris@contoso.com`, where the value `chris` is the *local part* of the email address, and the value `contoso.com` is the SMTP domain (also known as the *address space* or *name space*). The available SMTP domain values are determined by the accepted domains that are configured for your organization.

You can use email address policies to assign multiple SMTP email addresses to recipients by using different combinations of the local part and domain values. However, only one SMTP email address in a policy can be configured as the primary address.

All SMTP email address formats in the Exchange Management Shell, or custom SMTP email address formats in the EAC require you to use variables to define the local part of the email address. These variables are described in the following table:

|**Variable**|**Value**|
|:-----|:-----|
|%d|Display name|
|%g|Given name (first name)|
|%i|Middle initial|
|%m|Exchange alias|
|%r*xy*|Replace all occurrences of *x* with *y*|
|%r*xx*|Remove all occurrences of *x*|
|%s|Surname (last name)|
|%*n*g|The first *n* letters of the first name. For example, `%2g` uses the first two letters of the first name.|
|%*n*s|The first *n* letters of the last name. For example, `%2s` uses the first two letters of the last name.|

In addition to variables, you can also use US ASCII text characters that are allowed in Exchange email addresses (for example, periods (`.`) or underscores (`_`). Note that each period needs to be surrounded by other valid characters (for example `%g.%s`).

In the EAC, you can selected from a short list of precanned SMTP email address formats. These address formats are described in the following table, where the example user is named Elizabeth Brunner, and the domain is contoso.com:

|**Example**|**Exchange Management Shell equivalent**|
|:-----|:-----|
|`<alias>@contoso.com`|`%m@contoso.com`|
|`elizabeth.brunner@contoso.com`|`%g.%s@contoso.com`|
|`ebrunner@contoso.com`|`%1g%s@contoso.com`|
|`elizabethb@contoso.com`|`%g%1s@contoso.com`|
|`brunner.elizabeth@contoso.com`|`%s.%g@contoso.com`|
|`belizabeth@contoso.com`|`%1s%g@contoso.com`|
|`brunnere@contoso.com`|`%s%1g@contoso.com`|

## Recipient filters for email address policies

Recipient filters identify the recipients that the email address policy applies to. There are two basic options: **precanned recipient filters** and **custom recipient filters**. These are basically the same recipient filtering options that are used by dynamic distribution groups and address books. The following table summarizes the differences between the two filtering methods.

****

|**Recipient filtering method**|**User interface**|**Filterable recipient properties**|**Filter operators**|
|:-----|:-----|:-----|:-----|
|Precanned recipient filters|Exchange admin center (EAC) and the Exchange Management Shell|Limited to: <br/>• Recipient type (All recipient types or any combination of user mailboxes, resource mailboxes, mail contacts, mail users, and groups) <br/>• Company <br/> • Custom Attribute 1 to 15 <br/>• State or Province <br/>• Department|Property values require an exact match. Wildcards and partial matches aren't supported. For example, "Sales" doesn't match the value "Sales and Marketing". <br/> Multiple values of the same property always use the **or** operator. For example, "Department equals Sales or Department equals Marketing". <br/><br/> Multiple properties always use the **and** operator. For example, "Department equals Sales and Company equals Contoso".|
|Custom recipient filters|Exchange Management Shell only|You can use virtually any available recipient attributes.|You use OPATH filter syntax to specify any available Windows PowerShell filter operators. Wildcards and partial matches are supported.|

 **Notes**:

- You can't used precanned filters and customized filters at the same time.

- The recipient's location in Active Directory (the organizational unit or container) is available in both precanned and custom recipient filters.

- If you create an email address policy in the Exchange Management Shell that uses custom recipient filters, you can't edit the recipient filters in the EAC.

  ![Appy to tab in email address policies in the EAC when custom recipient filters are used.](../../media/b6ddfddb-48f8-4599-9407-bc939fb78da5.png)

- You can prevent individual recipients from being affected by email address policies. For example:

  - In the EAC, in the properties of the recipient, on the **Email address** tab, clear the check box: **Automatically update email addresses based on the email address policy applied to this recipient**.

  - In the Exchange Management Shell, set the _EmailAddressPolicyEnabled_ parameter to the value `$false` on the recipient management cmdlet (for example, **Set-Mailbox** or **Set-DistributionGroup**).

## Priority of email address policies

If a recipient is identified by multiple email address policies, the recipient's email addresses are only configured by the first email address policy that's evaluated. You configure the order that the policies are evaluated by using the priority of the policy. A lower priority number indicates a higher priority, higher priority policies are evaluated first, and the default email address policy is always evaluated last. You assign a higher priority (lower number) to policies that use the most specific or restrictive recipient filters.

Here are some other issues to consider:

- A recipient can only be affected by one email address policy. After the recipient is matched by the filtering properties of the policy, all other policies are ignored.

- **All** email address policies, including policies that have never been applied, are evaluated based on priority. For example, if you have a priority 1 policy and a priority 2 policy that both identity a recipient, the match in the first policy prevents the second policy from updating the recipient's email addresses, *even if the first policy has never been applied to the recipient*.

## Default email address policy

Exchange setup creates a default email address policy that applies email addresses to all recipients in your organization. The properties of the default email address policy are described in the following list:

- **Name**: Default Policy

- **Priority**: Lowest (all other email address policies are evaluated before the default policy).

- **Email address format**

  - **Type**: `SMTP` (primary email address)

  - **Domain**: `<alias>@<ADForestRootFQDN>`. This domain value is used because it's the first accepted domain in the Exchange organization.

- **Apply to**: All recipient types.

You can't delete the default email address policy, and you can't designate another policy as the default. You can modify some properties of the default policy, but the modification options are limited:

- You can't filter recipients by type or properties (applies to all recipient types).

- You can't change the name or priority of the policy.

- You can fully customize the email address templates in the policy (modify, add, or remove templates). For more information, see [Modify email address policies](eap-procedures.md#modify-email-address-policies).

## Apply email address policies

After you create or modify an email address policy in the EAC or the Exchange Management Shell, the policy needs to be applied to the affected recipients.

If the updates affect a large number of recipients (our recommendation is more than 3000), you should use the Exchange Management Shell to apply the updates to the affected recipients. For more information, see [Apply email address policies to recipients](eap-procedures.md#apply-email-address-policies-to-recipients).
