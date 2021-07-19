---
localization_priority: Normal
description: Admins can learn how to run the external admin audit log report in the Exchange admin center (EAC) and in PowerShell.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 31892014-c921-45fd-9775-7a1ef40e3517
ms.reviewer: 
f1.keywords:
- NOCSH
title: View and export the external admin audit log
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# View and export the external admin audit log

In Exchange Online, actions performed by Microsoft and delegated administrators are logged in the admin audit log. You can use the Exchange admin center (EAC) or Exchange Online PowerShell to search for and view audit log entries to determine if external administrators performed any actions on or changed the configuration of your Exchange Online organization. You can also use Exchange Online PowerShell to export these audit log entries.

## What do you need to know before you begin?

- Estimated time to complete: This will vary based on whether you view or export entries from the admin audit log. See each procedure for its estimated time to complete.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "View-only administrator audit logging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- If you're going to use Outlook on the web (formerly known as Outlook Web App) to view the exported entries, you need to enable .xml attachments in Outlook on the web. For details, see [Configure Outlook on the web to allow XML attachments](exchange-auditing-reports.md#configure-outlook-on-the-web-to-allow-xml-attachments).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) forum.

## Use the EAC to view the external admin audit log report

Estimated time to complete: 3 minutes

1. Go to **Compliance management** \> **Auditing** and click **View the external admin audit log report**. All configuration changes made by Microsoft datacenter administrators and delegated administrators during the specified time period are displayed, and can be sorted, using the following information:

   - **Date**: The date and time that the configuration change was made. The date and time are stored in Coordinated Universal Time (UTC) format.

   - **Cmdlet**: The name of the cmdlet that was used to make the configuration change.

     If you select an individual search result, the following information is displayed in the details pane:

   - The date and time that the cmdlet was run.

   - The user who ran the cmdlet. For all entries in the external admin audit log report, the user is identified as **Administrator**, which indicates a Microsoft datacenter administrator or an external administrator.

   - The cmdlet parameters that were used, and any value specified with the parameter, in the format **Parameter:Value**.

2. If you want to print a specific audit log entry, select it in the search results pane and then click **Print** in the details pane.

3. To narrow the search, choose dates in the **Start date** and **End date** drop-down menus, and then click **Search**.

## Use Exchange Online PowerShell to view entries in the external admin audit log report

Estimated time to complete: 3 minutes

You can use the **Search-AdminAuditLog** cmdlet with the _ExternalAccess_ parameter to view entries from the admin audit log for actions performed by Microsoft datacenter administrators and delegated administrators.

This command returns all entries in the admin audit log for cmdlets run by external administrators.

```PowerShell
Search-AdminAuditLog -ExternalAccess $true
```

This command returns entries in the admin audit log for cmdlets run by external administrators between September 17, 2013 and October 2, 2013.

```PowerShell
Search-AdminAuditLog -ExternalAccess $true -StartDate 09/17/2013 -EndDate 10/02/2013
```

For more information, see [Search-AdminAuditLog](/powershell/module/exchange/search-adminauditlog).

## Use Exchange Online PowerShell to export the admin audit log

Estimated time to complete: Approximately 24 hours

You can use the **New-AdminAuditLogSearch** cmdlet with the _ExternalAccess_ parameter to export entries from the admin audit log for actions performed by Microsoft datacenter administrators or delegated administrators. Microsoft Exchange retrieves entries in the admin audit log that were performed by external administrators and saves them to a file named SearchResult.xml. This XML file is attached to an email message that is sent to the specified recipients within 24 hours.

The following command returns entries in the admin audit log for cmdlets run by external administrators between September 25, 2013 and October 24, 2013. The search results are sent to the admin@contoso.com and pilarp@contoso.com SMTP addresses and the text "External admin audit log" is added to the subject line of the message.

```PowerShell
New-AdminAuditLogSearch -ExternalAccess $true -EndDate 10/24/2013 -StartDate 07/25/2013 -StatusMailRecipients admin@contoso.com,pilarp@contoso.com -Name "External admin audit log"
```

> [!NOTE]
> When you include the _ExternalAccess_ parameter, only entries for actions performed by Microsoft datacenter administrator or delegated administrators are included in the audit log that is exported. If you don't include the _ExternalAccess_ parameter, the audit log will contain entries for actions performed by the administrators in your organization and by external administrators.

To verify that the command to export the admin audit log entries performed by external administrators was successful, and to display information about current admin audit log searches, run the following command:

```PowerShell
Get-AuditLogSearch | Format-List
```

## More information

- In Microsoft 365 and Office 365, you can delegate the ability to perform certain administrative tasks to an authorized partner of Microsoft. These admin tasks include creating or editing users, resetting user passwords, managing user licenses, managing domains, and assigning admin permissions to other users in your organization. When you authorize a partner to take on this role, the partner is referred to as a delegated admin. The tasks performed by a delegated admin are logged in the admin audit log. As previously described, actions performed by delegated admins can be viewed by running the external admin audit log report or exported by using the **New-AdminAuditLogSearch** cmdlet with the _ExternalAccess_ parameter.

- The admin audit log records specific actions, based on Exchange Online PowerShell cmdlets, performed by administrators and users who have been assigned administrative privileges. Actions performed by external administrators are also logged. Entries in the admin audit log provide you with information about the cmdlet that was run, which parameters were used, and what objects were affected.

- The admin audit log doesn't record any action that is based on an Exchange Online PowerShell cmdlet that begins with the verbs **Get**, **Search**, or **Test**.

- Audit log entries are kept for 90 days. When an entry is older than 90 days, it's deleted.
