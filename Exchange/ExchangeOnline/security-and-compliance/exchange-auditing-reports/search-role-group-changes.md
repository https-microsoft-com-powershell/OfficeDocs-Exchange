---
localization_priority: Normal
description: Learn how to search the admin audit logs to discover who made changes to your organization.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: c7188d53-e672-492b-b57d-cd711379ddb3
ms.reviewer: 
f1.keywords:
- NOCSH
title: Search the role group changes or admin audit logs in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Search for role group changes or admin audit logs in Exchange Online

In Exchange Online organizations or standalone Exchange Online Protection (EOP) organizations without Exchange Online mailboxes, you can use the following options to search the admin audit logs to discover who made changes to the organization and recipient configuration:

- Run an administrator role group report in the Exchange admin center (EAC).
- Use PowerShell to search for admin audit log entries and send the results to a recipient.

These options can be helpful when you're trying to track the cause of unexpected behavior, to identify a malicious administrator, or to verify that compliance requirements are being met. Both of these options are described in this article.

> [!TIP]
> You can also use the EAC to view entries in the admin audit log. For more information, see [View the admin audit log](view-administrator-audit-log.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: less than 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "View-only administrator audit logging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md).

- To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell). To connect to standalone Exchange Online Protection PowerShell see [Connect to Exchange Online Protection PowerShell](/powershell/exchange/connect-to-exchange-online-protection-powershell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the EAC to run an administrator role group report

Use the administrator role group report to see the changes in membership that have been made to management roles.

1. In the EAC, go to **Compliance management** \> **Auditing**, and then choose **Run an administrator role group report**.

2. In the **Search for changes to administrator role groups** page that opens, configure the following settings:

   - **Start date** and **End date**: Enter a date range. By default, the report searches for changes made to administrator role groups in the past two weeks.

   - **Select role groups**: By default, all role groups are searched. To filter the results by specific role groups, click **Select role groups**. In the dialog that appears, select a role group and click **add ->**. Repeat this step as many times as necessary, and then click **OK** when you're finished.

3. When you're finished, click **Search**.

If any changes are found using the specified criteria, they will appear in the results pane. Click a role group in the search results to see the changes in the details pane.

### Monitor changes to role group membership

When members are added to or removed from a role group, the search results displayed in the details pane indicate that the role group membership was updated and lists the current members. The results don't explicitly state which user was added or removed.

To determine if a user was added or removed, you have to compare two separate entries in the report. For example, let's look at the following log entries for the **HelpDesk** role group:

> 1/27/2021 4:43 PM <br> Administrator <br> Updated members: Administrator;annb,florencef;pilarp <br> 2/06/2018 10:09 AM <br> Administrator <br> Updated members: Administrator;annb;florencef;pilarp;tonip <br> 2/19/2021 2:12 PM <br> Administrator <br> Updated members: Administrator;annb;florencef;tonip

In this example, the Administrator user account made the following changes:

- On 2/06/2021, they added the user tonip.
- On 2/19/2021, they removed the user pilarp.

### Use the EAC to export the admin audit log

> [!NOTE]
> In standalone EOP, you can't export the admin audit log from the EAC. But, you can [Use PowerShell to search for audit log entries and send results to a recipient](#use-powershell-to-search-for-audit-log-entries-and-send-results-to-a-recipient)
>
> If you're going to use Outlook on the web (formerly known as Outlook Web App) to view the exported entries, you need to enable .xml attachments in Outlook on the web. For details, see [Configure Outlook on the web to allow XML attachments](exchange-auditing-reports.md#configure-outlook-on-the-web-to-allow-xml-attachments).

Exporting the admin audit log writes the information to an XML file and sends it to you as an attachment in an email message. The maximum size of the XML file is 10 megabytes (MB).

1. In the EAC, select **Compliance management** \> **Auditing**, and then click **Export the admin audit log**.
2. Select a date range using the **Start date** and **End date** fields.
3. In the **Send the auditing report to** field, click **Select users** and then select the recipient you want to send the report to.
4. Click **Export**.

If any log entries are found using the criteria you specified, an XML file will be created and sent as an email attachment to the recipient you specified.

## Use PowerShell to search for audit log entries

You can use Exchange Online PowerShell or standalone Exchange Online Protection PowerShell to search for audit log entries that meet the criteria you specify. For a list of search criteria, see [Search-AdminAuditLog cmdlet](../../../ExchangeServer/policy-and-compliance/admin-audit-logging/admin-audit-logging.md#search-adminauditlog-cmdlet). This procedure uses the **Search-AdminAuditLog** cmdlet and displays search results in PowerShell. You can use this cmdlet when you need to return a set of results that exceeds the limits defined on the **New-AdminAuditLogSearch** cmdlet or in the EAC auditing reports.

To search the audit log for criteria you specify, use the following syntax.

```PowerShell
Search-AdminAuditLog - Cmdlets <cmdlet 1, cmdlet 2, ...> -Parameters <parameter 1, parameter 2, ...> -StartDate <start date> -EndDate <end date> -UserIds <user IDs> -ObjectIds <object IDs> -IsSuccess <$True | $False >
```

> [!NOTE]
> The **Search-AdminAuditLog** cmdlet returns a maximum of 1,000 log entries by default. Use the _ResultSize_ parameter to specify up to 250,000 log entries. Or, use the value `Unlimited` to return all entries.

This example performs a search for all audit log entries with the following criteria:

- **Start date**: 08/04/2020
- **End date**: 10/03/2020
- **User IDs**: `davids`, `chrisd`, `kima`
- **Cmdlets**: **Set-Mailbox**
- **Parameters**: _ProhibitSendQuota_, _ProhibitSendReceiveQuota_, _IssueWarningQuota_, _MaxSendSize_, _MaxReceiveSize_

```PowerShell
Search-AdminAuditLog -Cmdlets Set-Mailbox -Parameters ProhibitSendQuota,ProhibitSendReceiveQuota,IssueWarningQuota,MaxSendSize,MaxReceiveSize -StartDate 08/04/2020 -EndDate 10/03/2020 -UserIds davids,chrisd,kima
```

This example searches for changes made to a specific mailbox. This is useful if you're troubleshooting or you need to provide information for an investigation. The following criteria are used:

- **Start date**: 05/01/2020
- **End date**: 10/03/2020
- **Object ID**: contoso.com/Users/DavidS

```PowerShell
Search-AdminAuditLog -StartDate 05/01/2020 -EndDate 10/03/2020 -ObjectID contoso.com/Users/DavidS
```

If your searches return many log entries, we recommend that you use the procedure provided in [Use PowerShell to search for audit log entries and send results to a recipient](#use-powershell-to-search-for-audit-log-entries-and-send-results-to-a-recipient) later in this article. The procedure in that section sends an XML file as an email attachment to the recipients you specify, enabling you to more easily extract the data you're interested in.

For detailed syntax and parameter information, see [Search-AdminAuditLog](/powershell/module/exchange/search-adminauditlog).

### View details of audit log entries

The **Search-AdminAuditLog** cmdlet returns the fields described in [Audit log contents](/Exchange/policy-and-compliance/admin-audit-logging/admin-audit-logging#audit-log-contents). Of the fields returned by the cmdlet, two fields, **CmdletParameters** and **ModifiedProperties**, contain additional information that isn't viewable by default.

To view the contents of the **CmdletParameters** and **ModifiedProperties** fields, use the following steps. Or, you can use the procedure in [Use PowerShell to search for audit log entries and send results to a recipient](#use-powershell-to-search-for-audit-log-entries-and-send-results-to-a-recipient) later in this article to create an XML file.

This procedure uses the following concepts:

- [PowerShell arrays](/powershell/module/microsoft.powershell.core/about/about_arrays)
- [PowerShell variables](/powershell/module/microsoft.powershell.core/about/about_variables)

1. Decide the criteria you want to search for, run the **Search-AdminAuditLog** cmdlet, and store the results in a variable using the following command.

   ```PowerShell
   $Results = Search-AdminAuditLog <search criteria>
   ```

2. Each audit log entry is stored as an array element in the variable `$Results`. You can select an array element by specifying its array element index. Array element indexes start at zero (0) for the first array element. For example, to retrieve the 5th array element, which has an index of 4, use the following command.

   ```PowerShell
   $Results[4]
   ```

3. The previous command returns the log entry stored in array element 4. To see the contents of the **CmdletParameters** and **ModifiedProperties** fields for this log entry, use the following commands.

   ```PowerShell
   $Results[4].CmdletParameters
   $Results[4].ModifiedProperties
   ```

4. To view the contents of the **CmdletParameters** or **ModifiedParameters** fields in another log entry, change the array element index.

## Use PowerShell to search for audit log entries and send results to a recipient

> [!NOTE]
> The report that the **New-AdminAuditLogSearch** cmdlet generates can be a maximum of 10 MB in size. If your search returns a report larger than 10 MB, change the search criteria you specified. For example, reduce the date range and run multiple reports to cover the original date range.
>
> If you're going to use Outlook on the web (formerly known as Outlook Web App) to view the exported entries, you need to enable .xml attachments in Outlook on the web. For details, see [Configure Outlook on the web to allow XML attachments](exchange-auditing-reports.md#configure-outlook-on-the-web-to-allow-xml-attachments).

You can use Exchange Online PowerShell or standalone Exchange Online Protection PowerShell to search for audit log entries that meet the criteria you specify, and then send those results to a recipient you specify as an XML file attachment. The results are sent to the recipient within 15 minutes. For a list of search criteria, see [Search-AdminAuditLog cmdlet criteria](../../../ExchangeServer/policy-and-compliance/admin-audit-logging/admin-audit-logging.md#search-adminauditlog-cmdlet).

To search the audit log for criteria you specify, use the following syntax.

```PowerShell
New-AdminAuditLogSearch -Cmdlets <cmdlet1, cmdlet2, ...> -Parameters <parameter1, parameter2, ...> -StartDate <start date> -EndDate <end date> -UserIds <user IDs> -ObjectIds <object IDs> -IsSuccess <$true | $false > -StatusMailRecipients <recipient1, recipient2, ...> -Name <string to include in subject>
```

This example performs a search for all audit log entries with the following criteria:

- **Start date**: 08/04/2020
- **End date**: 10/03/2020
- **User IDs** davids, chrisd, kima
- **Cmdlets**: **Set-Mailbox**
- **Parameters**: _ProhibitSendQuota_, _ProhibitSendReceiveQuota_, _IssueWarningQuota_, _MaxSendSize_, _MaxReceiveSize_

The command sends the results to the davids@contoso.com SMTP address with "Mailbox limit changes" included in the subject line of the message.

```PowerShell
New-AdminAuditLogSearch -Cmdlets Set-Mailbox -Parameters ProhibitSendQuota,ProhibitSendReceiveQuota,IssueWarningQuota,MaxSendSize,MaxReceiveSize -StartDate 08/04/2020 -EndDate 10/03/2020 -UserIds davids,chrisd,kima -StatusMailRecipients davids@contoso.com -Name "Mailbox limit changes"
```

For more information about the format of the XML file, see [admin audit log structure](../../../ExchangeServer/policy-and-compliance/admin-audit-logging/log-structure.md).

For detailed syntax and parameter information, see [New-AdminAuditLogSearch](/powershell/module/exchange/new-adminauditlogsearch).
