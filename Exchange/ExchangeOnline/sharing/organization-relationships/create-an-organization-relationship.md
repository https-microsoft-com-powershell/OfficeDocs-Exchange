---
localization_priority: Normal
description: Set up an organization relationship to share calendar information with an external business partner. Microsoft 365 and Office 365 admins can set up an organization relationship with another Microsoft 365 or Office 365 organization or with an Exchange on-premises organization.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 8b9a1782-f6be-46bc-bec9-49633be0dc1f
ms.reviewer: 
f1.keywords:
- NOCSH
title: Create an organization relationship in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create an organization relationship in Exchange Online

Set up an organization relationship to share calendar information with an external business partner. Microsoft 365 and Office 365 admins can set up an organization relationship with another Microsoft 365 or Office 365 organization or with an Exchange on-premises organization.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the [Permissions in Exchange Online](../../permissions-exo/permissions-exo.md) topic.

- If you want to share calendars with an on-premises Exchange organization, the on-premises Exchange administrator has to set up an authentication relationship with the cloud (also known as "federation") and must meet minimum software requirements.

## Use the Exchange admin center to create an organization relationship
<a name="BKMK_EAC"> </a>

1. From the Microsoft 365 admin center dashboard, go to **Admin** \> **Exchange**.

2. Go to **organization** \> **sharing**.

3. Under **Organization Sharing**, click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

4. In **new organization relationship**, in the **Relationship name** box, type a friendly name for the organization relationship.

5. In the **Domains to share with** box, type the domain for the external Microsoft 365, Office 365, or Exchange on-premises organization you want to let see your calendars. If you need to add more than one domain, you can do it after you create the organization relationship by editing it.

6. Select the **Enable calendar free/busy information sharing** check box to turn on calendar sharing with the domains you listed. Set the sharing level for calendar free/busy information and set which users can share calendar free/busy information.

   To set the free/busy access level, select one of the following values:

   - **Calendar free/busy information with time only**
   - **Calendar free/busy with time, subject, and location**

    To set which users will share calendar free/busy information, select one of the following values:

   - **Everyone in your organization**
   - **A specified security group**

    Click **Browse** to pick the security group from a list, then click **OK**.

7. Click **Save** to create the organization relationship.

> [!NOTE]
> Cross-tenant configurations do not support personal contacts for free/busy lookup. Contacts must be included in the global address list for free/busy lookup to work.

## Use Exchange Online PowerShell to create an organization relationship
<a name="BKMK_Shell"> </a>

This example creates an organization relationship with Contoso, Ltd with the following conditions:

- An organization relationship is set up with contoso.com, northamerica.contoso.com, and europe.contoso.com.
- Free/busy access is enabled.
- Contoso.com and the subdomains get free/busy time, subject, and location information from your organization.

```PowerShell
New-OrganizationRelationship -Name "Contoso" -DomainNames "contoso.com","northamerica.contoso.com","europe.contoso.com" -FreeBusyAccessEnabled $true -FreeBusyAccessLevel LimitedDetails
```

If you're not sure which domains Contoso has set up for cloud-based authentication, you can run this command to automatically find the configuration information. The **Get-FederationInformation** cmdlet is used to find the right information, which is then passed to the **New-OrganizationRelationship** cmdlet.

```PowerShell
Get-FederationInformation -DomainName Contoso.com | New-OrganizationRelationship -Name "Contoso" -FreeBusyAccessEnabled $true -FreeBusyAccessLevel LimitedDetails
```

For detailed syntax and parameter information, see [Get-FederationInformation](/powershell/module/exchange/get-federationinformation) and [New-OrganizationRelationship](/powershell/module/exchange/new-organizationrelationship).

If you're setting up an organization relationship with an on-premises Exchange organization, you may want to provide the connection settings. This example creates an organization relationship with Fourth Coffee and specifies the connection settings to use. The following conditions apply:

- The organization relationship is established with the domain fourthcoffee.com.
- The Exchange Web Services application URL is mail.fourthcoffee.com.
- The Autodiscover URL is `https://mail.fourthcoffee.com/autodiscover/autodiscover.svc/wssecurity`.
- Free/busy access is enabled.
- Fourth Coffee sees free/busy information with the time.

```PowerShell
New-OrganizationRelationship -Name "Fourth Coffee" -DomainNames "fourthcoffee.com" -FreeBusyAccessEnabled $true -FreeBusyAccessLevel AvailabilityOnly -TargetAutodiscoverEpr "https://mail.fourthcoffee.com/autodiscover/autodiscover.svc/wssecurity" -TargetApplicationUri "mail.fourthcoffee.com"
```

For detailed syntax and parameter information, see [New-OrganizationRelationship](/powershell/module/exchange/new-organizationrelationship).

## How do you know this worked?

The successful completion of the **New organization relationship** wizard indicates that the organization relationship was created.

You can also run the following command to verify the organization relationship information:

```PowerShell
Get-OrganizationRelationship | Format-List
```

## Organization relationships with GCC High

Tenants in the [GCC High](/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc-high-and-dod) cloud can now create organization relationships with tenants in the World Wide and the [GCC](/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc) clouds.

> [!IMPORTANT]
> Organization relationships between the [DoD](/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc-high-and-dod) cloud and other clouds is not supported.

### Create cross-cloud organization relationships

Using PowerShell is the best way to create organization relationships between clouds.

This example creates an organization relationship between Contoso, Ltd in the WorldWide cloud and Fourth Coffee in the GCC-H cloud. with the following conditions:

- Contoso domains are contoso.com, northamerica.contoso.com, and europe.contoso.com.
- Fourth Coffee domains are fourthcoffee.com
- Free/busy access is enabled.
- Each tenant gets free/busy time, subject, and location information from the other tenant

In Fourth Coffee run the following command:

```PowerShell
New-OrganizationRelationship -Name "Contoso" -DomainNames "contoso.com","northamerica.contoso.com","europe.contoso.com" -FreeBusyAccessEnabled $true -FreeBusyAccessLevel LimitedDetails -TargetApplicationUri "outlook.com" -TargetAutodiscoverEpr "https://autodiscover-s.outlook.com/autodiscover/autodiscover.svc/WSSecurity"
```

In Contoso, run the following command:

```PowerShell
New-OrganizationRelationship -Name "Fourth Coffee" -DomainNames "fourthcoffee.com" -FreeBusyAccessEnabled $true -FreeBusyAccessLevel LimitedDetails -TargetApplicationUri "office365.us" -TargetAutodiscoverEpr "https://autodiscover-s.office365.us/autodiscover/autodiscover.svc/WSSecurity"
```

You can't use the **Get-FederationInformation** cmdlet to automatically discover the domains and other configurations needed for cross-cloud organization relationship setup.

The configuration parameters that you need to set are described in the following table:

<br>

****

|Parameter|OrgRel in WW/GCC for GCC-H Tenant|OrgRel in GCC-H for WW/GCC Tenant|
|---|---|---|
|***DomainNames***|All the domains for the remote org. You need to collect and add these manually.|All the domains for the remote org. You need to collect and add these manually.|
|***TargetApplicationUri***|Office365.us|Outlook.com|
|***TargetAutodiscoverEpr***|`https://autodiscover-s.office365.us/autodiscover/autodiscover.svc/WSSecurity`|`https://autodiscover-s.outlook.com/autodiscover/autodiscover.svc/WSSecurity`|
|

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).
