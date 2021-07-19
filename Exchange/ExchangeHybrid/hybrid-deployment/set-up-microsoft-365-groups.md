---
title: "Configure Microsoft 365 Groups with on-premises Exchange hybrid"
ms.author: dmaguire
author: msdmaguire
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Hybrid
- Ent_O365_Hybrid
- M365-email-calendar
ms.assetid: 184dfcfe-4b8e-450a-adc6-e647213b9501
ms.reviewer: 

description: "Learn how to enable on-premises Exchange users to use Microsoft 365 Groups in a hybrid deployment."
---

# Configure Microsoft 365 Groups with on-premises Exchange hybrid

Learn how to enable on-premises Exchange users to use Microsoft 365 Groups in a hybrid deployment.

Microsoft 365 Groups service that enables teams to communicate, schedule meetings, and collaborate on documents more easily. All information shared with a group, from email messages sent to the group, to files stored in the group's OneDrive for Business or SharePoint libraries, is available to any member of a group. If you've configured a hybrid deployment between your on-premises Exchange organization and Microsoft 365 or Office 365, you can make groups created in Microsoft 365 or Office 365 available to your on-premises users by following the steps in this topic.

The following are experiences that on-premises mailbox users can expect after all the steps in this document have been performed:

- Expand a Microsoft 365 group and view the members, just like in a distribution group, in Outlook on the web and Outlook desktop client while composing a new email.
- Send and receive emails and calendar invites to and from Microsoft 365 groups.
- Collaborate on ODB documents sent by Microsoft 365 users.
- Access a SharePoint site, Planner, and OneNote associated with the Microsoft 365 group.

> [!IMPORTANT]
> After the steps in this article have been successfully performed, the on-premises mailbox can perform the previously mentioned tasks. However, the Microsoft 365 group will not appear in the navigation bar of Outlook on the web or Outlook desktop client. For a fully-fledged Microsoft 365 Groups experience, the mailbox must be present in Microsoft 365. 

Check the [Known issues](#known-issues) section at the end of this topic for fixes to known issues.

## Prerequisites

Before you start, make sure that you've done the following:

- Purchased Azure Active Directory Premium licenses for your tenant. This is required to enable the Groups writeback feature in Azure Active Directory Connect.

- Configured a hybrid deployment between your Exchange on-premises organization and Microsoft 365 or Office 365 and verified it's functioning correctly. For more information about Exchange hybrid deployments, see the following articles:
  - [Exchange Server Hybrid Deployments](../exchange-hybrid.md)
  - [Hybrid deployment prerequisites](../hybrid-deployment-prerequisites.md)

- Installed a supported version of Exchange on-premises Exchange integration with Microsoft 365 Groups is available in CU1 and newer releases of Exchange 2016, and CU11 and newer releases of Exchange 2013. However, Exchange hybrid requires the latest Exchange 2013 or Exchange 2016 Cumulative Update (CU) to be installed on your on-premises Exchange servers. If you can't install the latest CU, the update released immediately prior to the current CU can be used.

- Configured single sign-on using Azure Active Directory Connect (Azure AD Connect). This is needed to allow users to click on the **View group files** or cloud attachment links in group email messages.

  When configuring Azure AD Connect for single sign-on in an Exchange hybrid deployment, we recommend that you use password synchronization. Active Directory Federation Services (AD FS) should only be used if you're in a large organization; if you have a complex on-premises Active Directory deployment (for example, multiple Active Directory forests); if another Microsoft product requires AD FS to work with Microsoft 365 or Office 365; or if, due to compliance policies, you're not able to synchronize passwords outside of your on-premises network. For more information about single sign-on, see [Choose a solution for integrating on-premises Active Directory with Azure](/azure/architecture/reference-architectures/identity/).

## Enable Group writeback in Azure AD Connect

This step synchronizes Microsoft 365 Groups from Exchange Online to Exchange on-premises.

1. Open the Azure AD Connect wizard, select **Configure** and then click **Next**.

2. Select Customize synchronization options and then click Next.

3. On the **Connect to Azure AD** page, enter your Microsoft 365 or Office 365 credentials. Click **Next**.

4. On the **Optional features** page, verify that the options you previously configured are still selected. The most commonly-selected options are **Exchange hybrid** and **Password hash synchronization**.

5. Select **Group writeback** and then click **Next**.

6. On the **Writeback** page, select an Active Directory organizational unit (OU) to store objects that are synchronized from Microsoft 365 or Office 365 to your on-premises organization, and then click **Next**.

7. On the **Ready to configure** page, click **Configure**.

8. When the wizard is complete, click **Exit** on the **Configuration complete** page.

9. Open Active Directory Users and Computers on an Active Directory domain controller and locate the user account that begins with **AAD\_**. Make note of this account's name. You can also use a PowerShell cmdlet to [Determine your AD DS Connector Account](/azure/active-directory/hybrid/how-to-connect-configure-ad-ds-connector-account#determine-your-ad-ds-connector-account)

10. Open the Windows PowerShell on the Azure Active Directory Connect server, and run the following commands.

    ```PowerShell
    $AzureADConnectSWritebackAccountDN = <AAD_ account DN>
    Import-Module "C:\Program Files\Microsoft Azure Active Directory Connect\AdSyncConfig\AdSyncConfig.psm1"
    Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN $AzureADConnectSWritebackAccountDN
    ```

## Configure a group domain

The primary SMTP domain of a Microsoft 365 group is called a group domain. By default, the default accepted domain in your Exchange Online organization is chosen as the group domain. For better interoperation with on-premises servers, we recommended adding a dedicated groups domain. You can add a domain using the following steps. For more information about multi-domain support for Microsoft 365 Groups, check out [Multi-domain support for Microsoft 365 Groups](/microsoft-365/admin/create-groups/choose-domain-to-create-groups).

1. Add your new domain to your Microsoft 365 or Office 365 organization. If you need help adding a domain to Microsoft 365 or Office 365, check out [Add a domain to Microsoft 365](/microsoft-365/admin/setup/add-domain).

2. Create the following public DNS records with your DNS provider.

   |DNS record name|DNS record type|DNS record value|
   |---|---|---|
   |groups.contoso.com|MX|groups-contoso-com.mail.protection.outlook.com<sup>1</sup>|
   |autodiscover.groups.contoso.com|CNAME|autodiscover.outlook.com|

   <sup>1</sup> The format of this DNS record value is _\<domain key\>_.mail.protection.outlook.com. To find out what your domain key is, check out [Gather the information you need to create DNS records](/microsoft-365/admin/get-help-with-domains/information-for-dns-records).

   > [!CAUTION]
   > If the MX DNS record for the group domain is set to the on-premises Exchange server, mail flow won't work correctly between users in the on-premises Exchange organization and the Microsoft 365 group.

3. Add the group domain as an accepted domain in your on-premises Exchange organization using the following command. This is needed so that the hybrid Send connector can be used to deliver outbound mail to the group domain in Microsoft 365 or Office 365.

   ```PowerShell
   New-AcceptedDomain -Name groups.contoso.com -DomainName groups.contoso.com -DomainType InternalRelay
   ```

> [!IMPORTANT]
> Ensure that on-premises servers can resolve the MX entry added for the group's domain in step 2. The group's domain must be added as InternalRelay, otherwise, it will cause mail flow issues.

4. Add the group domain to the hybrid Send connector, created by the Hybrid Configuration wizard in your on-premises Exchange organization, using the following command.

   ```PowerShell
   Set-SendConnector -Identity "Outbound to Office 365" -AddressSpaces "contoso.mail.onmicrosoft.com","groups.contoso.com"
   ```

   > [!NOTE]
   > If the Send connector isn't updated, or if the group domain isn't added as an accepted domain in the on-premises Exchange organization, mail sent from an on-premises mailbox won't be delivered to the group unless the group is configured to receive mail from external senders.

## How do you know this worked?

To make sure that groups are working with your Exchange hybrid deployment, you should test them using an on-premises mailbox and using a mailbox that's been moved from your on-premises organization to Microsoft 365 or Office 365. Use the steps in the following sections to do each test.

### Test using an on-premises mailbox

1. Add an on-premises mailbox to a Microsoft 365 group.
2. Add a Microsoft 365 or Office 365 mailbox to the same Microsoft 365 group.
3. Log in to the Microsoft 365 or Office 365 mailbox using Outlook on the web.
4. Post a message to the group using the Microsoft 365 or Office 365 mailbox.
5. Open the on-premises mailbox using Outlook 2016 or Outlook on the web.
6. Verify that the mailbox received an email message containing the post sent to the Microsoft 365 group.
7. In the same mailbox, compose a reply to the message and send it to the group.
8. Verify that the message can be viewed by all of the members of the group.

### Test using a mailbox moved to Microsoft 365 or Office 365

1. Move a mailbox from your on-premises Exchange organization to Microsoft 365 or Office 365.
2. Add the mailbox to a Microsoft 365 group.
3. In a new browser session, log into the mailbox that was moved to Microsoft 365 or Office 365.
4. In Outlook on the web, verify that the group is listed in the left navigation bar.
5. Post a message to the group.
6. Verify that the message can be viewed by all of the members of the group.

## Known issues
<a name="KnownIssues"> </a>

- **Older versions of Azure AD connect won't install DSACLS.exe**: You need to install RSAT or the latest Version of Azure AD Connect to manage permissions on groups (if required).

- **Groups don't appear for mailboxes moved to Microsoft 365 or Office 365**: When a user is moved from your on-premises Exchange organization to Microsoft 365 or Office 365, groups won't appear in the left navigation pane in Outlook or Outlook on the web. To fix the issue, remove the mailbox from any groups of which it is a member, and re-add it to each group.

- **New groups don't appear in the on-premises Exchange global address list (GAL)**: When a new group is created in Microsoft 365 or Office 365, it won't appear in the on-premises GAL automatically. To fix this issue, open the Exchange Management Shell on an on-premises Exchange server and run the following command.

  ```PowerShell
  Update-Recipient -Identity "[group Distinguished Name]"
  ```

- **If the _Outbound to Office 365_ Send connector is using an Edge transport server as the source server**: Messages to Microsoft 365 Groups will end up in a loop, resulting in a non-delivery report. Check out [Accepted domains in Exchange Server](/Exchange/mail-flow/accepted-domains/accepted-domains?view=exchserver-2019) for more details.

- **On-premises users can't use links included in group message footers**: On-premises users can't use the **View group conversations** or **Unsubscribe** links that are included in the footer of each group message sent to them. To unsubscribe from a group, on-premises users need to contact a group administrator.

- **Mail sent to a group's secondary SMTP address fails to be delivered**: When multiple email addresses are added to a group, only the primary SMTP address is written back to your on-premises Active Directory. If an on-premises user tries to send a message to the secondary SMTP address of a group, the message will fail to be delivered. To prevent this issue, configure only one SMTP address on each group.

- **Delivery of external mail to a group fails if you've enabled centralized mail flow**: If centralized mail flow is enabled, mail sent by an external user to a group fails to be delivered, even though the group allows email from external senders.

- **On-premises users can't send mail as a group**: An on-premises user who tries to send a message as a Microsoft 365 group will receive a permission denied error even if they're given Send As permissions on the group. Send As permissions on a group work only for Exchange Online mailbox users.

- **By default, when an email is sent from an on-premises mailbox to an Outlook group that the user is a member of, the user doesn't receive a copy of that email in their Inbox**: The Exchange Online tenant admin can use the following Exchange Online shell command to ensure the on-premises mailbox user can receive a copy of the email in their Inbox:

  ```PowerShell
  
  Get-MailUser <MEU for On-Premises mailbox> | Set-MailboxMessageConfiguration -EchoGroupMessageBackToSubscribedSender $true
  
  ```
- **On-premises users can't send mail as a group**: An on-premises user who tries to send a message as a Microsoft 365 group will receive a permission denied error even if they're given Send As permissions on the group. Send As permissions on a group work only for Exchange Online mailbox users.

- **Allowing on-premises users to manage a Microsoft 365 group**: On-premises users can be assigned as owners of a Microsoft 365 group. However, the on-premises users will have to use the (Azure AD web portal)[https://account.activedirectory.windowsazure.com/r#/groups] to manage the groups they own. The Azure AD admin must enable self-service management in the Azure AD portal using steps provided in [Set up self-service group management in Azure Active Directory](/azure/active-directory/enterprise-users/groups-self-service-management).

- **Selecting a group from Outlook's left navigation pane doesn't open the group's mailbox for an Exchange Online user**: Outlook uses the AutoDiscover URL to open a group mailbox. If a group's primary email address is in a domain that doesn't point to the Microsoft 365 or Office 365 AutoDiscover URL (autodiscover.outlook.com), Outlook won't be able to open the group's mailbox. To fix this issue, groups can be provisioned with a primary address in a domain that points to the Microsoft 365 or Office 365 AutoDiscover URL. You can configure an email address policy to add a primary email address on each group mailbox that points to that AutoDiscover URL. Check out [Choose the domain to use when creating Microsoft 365 groups](/microsoft-365/admin/create-groups/choose-domain-to-create-groups) for more details.
