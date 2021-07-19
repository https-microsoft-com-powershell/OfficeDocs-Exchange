---
title: "Block legacy authentication in Exchange 2019 hybrid"
ms.author: chrisda
author: chrisda
manager: serdars
ms.date: 
ms.audience: Developer
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Strat_EX_EXOBlocker
- Ent_O365_Hybrid
- Hybrid
ms.assetid: 
description: "Administrators can learn how to block legacy authentication methods for clients connecting to Exchange Server 2019."
---

# Block legacy authentication in Exchange 2019 hybrid

The following legacy authentication methods can be used to access Exchange servers:

- Basic authentication

- Digest authentication

- Windows authentication (NTLM and Kerberos)

In Exchange Server 2019 Cumulative Update 1 (CU1) or later, we provide a way to block these legacy authentication methods in hybrid environments that use [Hybrid Modern Auth](https://techcommunity.microsoft.com/t5/exchange-team-blog/announcing-hybrid-modern-authentication-for-exchange-on-premises/ba-p/607476/).

When you disable legacy authentication for users in Exchange, their email clients and apps must support modern authentication. Those clients are:

- Outlook 2013 or later (Outlook 2013 [requires a registry key change](/microsoft-365/admin/security-and-compliance/enable-modern-authentication))

- Outlook 2016 for Mac or later

- Outlook for iOS and Android

- Mail for iOS 11.3.1 or later

## How legacy authentication is blocked in Exchange 2019

You block legacy authentication in Exchange hybrid environments by creating _authentication policies_. Authentication policies define the client protocols where legacy authentication is blocked (all protocols or specific protocols, although we typically recommend blocking legacy authentication for all protocols).

After you create authentication policies, you assign them to users. Assigning a policy to users blocks their legacy authentication requests for the specified protocols. You can assign different policies to different users, or you can specify a default authentication policy that's assigned to all users. An authentication policy that's directly assigned to a user takes precedence over the default policy.

You manage all aspects of authentication policies in the Exchange Management Shell. The protocols and services in Exchange that you can block legacy authentication for are described in the following table.

|**Protocol or service**|**Description**|**Parameter name**|
|:-----|:-----|:-----|
|Exchange Active Sync (EAS)|Used by some email clients on mobile devices.|*BlockLegacyAuthActiveSync*|
|Autodiscover|Used by Outlook and EAS clients to find and connect to mailboxes in Exchange|*BlockLegacyAuthAutodiscover*|
|IMAP|Used by IMAP email clients.|*BlockLegacyAuthImap*|
|MAPI over HTTP (MAPI/HTTP)|Used by Outlook 2013 and later.|*BlockLegacyAuthMapi*|
|Offline Address Book (OAB)|A copy of address list collections that are downloaded and used by Outlook.|*BlockLegacyAuthOfflineAddressBook*|
|POP3|Used by POP email clients.|*BlockLegacyAuthPop*|
|Outlook Anywhere (RPC over HTTP)|Used by Outlook 2016 and earlier.|*BlockLegacyAuthRpc*|
|Exchange Web Services (EWS)|A programming interface that's used by Outlook, Outlook for Mac, and third-party apps.|*BlockLegacyAuthWebServices*|

Typically, when you block legacy authentication for a user, we recommend that you block legacy authentication for all protocols. However, you can use the *BlockLegacyAuth\** parameters (switches) on the **New-AuthenticationPolicy** and **Set-AuthenticationPolicy** cmdlets to selectively allow or block legacy authentication for specific protocols.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 3 minutes.

- To open the Exchange Management Shell, see [Open the Exchange Management Shell](/powershell/exchange/open-the-exchange-management-shell).

- Verify that modern authentication is enabled in your Exchange environment.

- Verify your email clients and apps support modern authentication (see the list at the beginning of the topic). Also, verify that your Outlook desktop clients are running the minimum required cumulative updates. For more information, see [Outlook Updates](/officeupdates/outlook-updates-msi).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver), [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange), or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Create and apply authentication policies

The steps to create and apply authentication policies to block legacy authentication in Exchange 2019 in hybrid environments are:

1. Create the authentication policy.

2. Assign the authentication policy to users.

These steps are described in the following sections.

### Step 1: Create the authentication policy

To create a policy that blocks legacy authentication for the specified client protocols, use the following syntax:

```powershell
New-AuthenticationPolicy -Name "<Descriptive Name>" [-BlockLegacyAuthActiveSync] [-BlockLegacyAuthAutodiscover] [-BlockLegacyAuthImap] [-BlockLegacyAuthMapi] [-BlockLegacyAuthOfflineAddressBook] [-BlockLegacyAuthPop] [-BlockLegacyAuthRpc] [-BlockLegacyAuthWebServices]
```

This example creates an authentication policy named "Block Legacy Auth" to block legacy authentication for all client protocols in Exchange 2019 (the recommended configuration).

```powershell
New-AuthenticationPolicy -Name "Block Legacy Auth" -BlockLegacyAuthActiveSync -BlockLegacyAuthAutodiscover -BlockLegacyAuthImap -BlockLegacyAuthMapi -BlockLegacyAuthOfflineAddressBook -BlockLegacyAuthPop -BlockLegacyAuthRpc -BlockLegacyAuthWebServices
```

### Step 2: Assign the authentication policy to users

The methods that you can use to assign authentication policies to users are described in this section:

- **Individual user accounts**: Use the following syntax:

  ```powershell
  Set-User -Identity <UserIdentity> -AuthenticationPolicy <PolicyIdentity>
  ```

  This example assigns the policy named Block Legacy Auth to the user account laura@contoso.com.

  ```powershell
  Set-User -Identity laura@contoso.com -AuthenticationPolicy "Block Legacy Auth"
  ```

- **Filter user accounts by attributes**: This method requires that the user accounts all share a unique filterable attribute (for example, Title or Department) that you can use to identify the users. The syntax uses the following commands (two to identify the user accounts, and the other to apply the policy to those users):

   ```powershell
   $<VariableName1> = Get-User -ResultSize unlimited -Filter <Filter>
   $<VariableName2> = $<VariableName1>.SamAccountName
   $<VariableName2> | foreach {Set-User -Identity $_ -AuthenticationPolicy "Block Legacy Auth"}
   ```

   This example assigns the policy named Block Legacy Auth to all user accounts whose **Title** attribute contains the value "Sales Associate".

   ```powershell
   $SalesUsers = Get-User -ResultSize unlimited -Filter {(RecipientType -eq 'UserMailbox') -and (Title -like '*Sales Associate*')}
   $Sales = $SalesUsers.SamAccountName
   $Sales | foreach {Set-User -Identity $_ -AuthenticationPolicy "Block Legacy Auth"}
   ```

- **Use a list of specific user accounts**: This method requires a text file to identify the user accounts. Values that don't contain spaces (for example, the user principal name or UPN) work best. The text file must contain one user account on each line like this:

  `akol@contoso.com`

  `tjohnston@contoso.com`

  `kakers@contoso.com`

  The syntax uses the following two commands (one to identify the user accounts, and the other to apply the policy to those users):

  ```powershell
  $<VariableName> = Get-Content "<text file>"
  $<VariableName> | foreach {Set-User -Identity $_ -AuthenticationPolicy <PolicyIdentity>}
  ```

  This example assigns the policy named Block Legacy Auth to the user accounts specified in the file C:\My Documents\BlockLegacyAuth.txt.

  ```powershell
  $BLA = Get-Content "C:\My Documents\BlockLegacyAuth.txt"
  $BLA | foreach {Set-User -Identity $_ -AuthenticationPolicy "Block Legacy Auth"}
  ```

### View authentication policies

To view a summary list of the names of all existing authentication policies, run the following command:

```powershell
Get-AuthenticationPolicy | Format-Table -Auto Name
```

To view detailed information about a specific authentication policy, use this syntax:

```powershell
Get-AuthenticationPolicy -Identity <PolicyIdentity>
```

This example returns detailed information about the policy named Block Legacy Auth.

```powershell
Get-AuthenticationPolicy -Identity "Block Legacy Auth"
```

## Configure the default authentication policy

The default authentication policy is assigned to all users who don't already have a specific policy assigned to them (a directly assigned policy takes precedence). To configure the default authentication policy for the organization, use this syntax:

```powershell
Set-OrganizationConfig -DefaultAuthenticationPolicy \<PolicyIdentity\>
```

This example configures the authentication policy named "Block Legacy Auth" as the default policy.

```powershell
Set-OrganizationConfig -DefaultAuthenticationPolicy "Block Legacy Auth"
```

### Remove authentication policies

To remove an existing authentication policy, use this syntax:

```powershell
Remove-AuthenticationPolicy -Identity <PolicyIdentity>
```

This example removes the policy named Test Auth Policy.

```powershell
Remove-AuthenticationPolicy -Identity "Test Auth Policy"
```

For detailed syntax and parameter information, see [Remove-AuthenticationPolicy](/powershell/module/exchange/remove-authenticationpolicy).

### How do you know that you've successfully disabled legacy authentication in Exchange?

To confirm that the authentication policy was applied to users:

1. Run the following command to find the distinguished name (DN) value of the authentication policy:

   ```powershell
   Get-AuthenticationPolicy | Format-List Name,DistinguishedName
   ```

2. Use the DN value of the authentication policy in the following command:

   ```powershell
   Get-User -Filter {AuthenticationPolicy -eq '<AuthPolicyDN>'}
   ```