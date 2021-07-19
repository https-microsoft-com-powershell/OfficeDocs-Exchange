---
localization_priority: Normal
description: Learn about the auditing reports that are available in the Exchange admin center (EAC) in Exchange Online.
ms.topic: overview
author: msdmaguire
ms.author: jhendr
ms.assetid: 2b3e1529-1677-4564-be0b-ce22757ddc0d
ms.reviewer: 
f1.keywords:
- NOCSH
title: Exchange Online auditing reports
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Auditing reports in the Exchange admin center in Exchange Online

Use audit logging to troubleshoot configuration issues by tracking specific changes made by admins and to help you meet regulatory, compliance, and litigation requirements. Exchange Online or standalone Exchange Online Protection (EOP) without Exchange Online mailboxes provides two types of audit logging:

- **Admin audit logging**: Records any action, based on an Exchange Online PowerShell or standalone Exchange Online Protection PowerShell cmdlet, performed by an admin. These records can help you troubleshoot configuration issues or identify the cause of security-related or compliance-related problems. Actions performed by Microsoft datacenter administrators and delegated admins, are also recorded in Exchange Online.

- **Mailbox audit logging (Exchange Online only)**: Records when a mailbox is accessed by an admin, a delegated user, or the person who owns the mailbox. This can help you determine who has accessed a mailbox and what they've done.

## Export audit logs

On the **Compliance Management** \> **Auditing** page in the Exchange admin center (EAC), you can search for and export entries from the admin audit log and the mailbox audit log.

> [!NOTE]
> Mailbox audit logging is not available in standalone EOP. Admin audit log export from the EAC is not available in standalone EOP, but is available in PowerShell by using the **New-AdminAuditLogSearch** cmdlet. For instructions, see [Use PowerShell to search for audit log entries and send results to a recipient](search-role-group-changes.md#use-powershell-to-search-for-audit-log-entries-and-send-results-to-a-recipient).

- **Export the admin audit log**: Any action performed by an admin that's based on an Exchange Online PowerShell or standalone Exchange Online Protection PowerShell cmdlet that doesn't begin with the verbs **Get**, **Search**, or **Test** is logged in the admin audit log. Audit log entries include the cmdlet that was run, the parameter and values used with the cmdlet, and when the operation was successful. You can search for and export entries from the admin audit log. When you export your search results, Exchange Online saves them in an XML file and attaches it to an email message. For more information, see:

  - [Search the role group changes or admin audit logs](search-role-group-changes.md)
  - [View and export the external admin audit log](view-external-admin-audit-log.md) (Exchange Online only)

    > [!NOTE]
    > By default, admin audit log entries are kept for 90 days. When an entry is older than 90 days, it's deleted. This setting can't be changed in a cloud-based organization. However, it can be changed in an on-premises Exchange organization by using the **Set-AdminAuditLog** cmdlet.

- **Export mailbox audit logs**: When mailbox audit logging is enabled for a mailbox, Exchange Online stores a record of actions performed on mailbox data by non-owners in the mailbox audit log, which is stored in a hidden folder in the mailbox being audited. Mailbox audit logging can also be configure to log owner actions. Entries in this log indicate who accessed the mailbox and when, the actions performed, and whether the action was successful. When you search for entries in the mailbox audit log and export them, Exchange Online saves the search results in an XML file and attaches it to an email message. For more information, see [Export mailbox audit logs](export-mailbox-audit-logs.md).

### Configure Outlook on the web to allow XML attachments

When you export the mailbox audit log or admin audit log the log is attached as an XML file in an email message. However, Outlook on the web (formerly known as Outlook Web App) blocks XML attachments by default. If you want to use Outlook on the web to access exported audit logs, you need to configure Outlook on the web to allow XML attachments.

In [Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell) or standalone [Exchange Online Protection PowerShell](/powershell/exchange/connect-to-exchange-online-protection-powershell), run the following command to allow XML attachments in Outlook on the web:

```PowerShell
Set-OwaMailboxPolicy -Identity Default -AllowedFileTypes @{Add=".xml"}
```

For detailed syntax and parameter information, see [Set-OwaMailboxPolicy](/powershell/module/exchange/set-owamailboxpolicy)

## Run auditing reports

When you run any of the following reports on the **Auditing** page in the EAC, the results are displayed in the details pane of the report:

- **Run a non-owner mailbox access report**: Use this report to find mailboxes that have been accessed by someone other than the person who owns the mailbox. For more information, see [Run a non-owner mailbox access report](non-owner-mailbox-access-report.md).

- **Run an administrator role group report**<sup>\*</sup>: Use this report to search for changes made to administrator role groups. For more information, see [Search the role group changes or admin audit logs](search-role-group-changes.md).

- **Run an In-Place eDiscovery & Hold report**: Use this report to find mailboxes that have been put on, or removed from, In-Place Hold. For more information, see:
  - [In-Place Hold and Litigation Hold](../../security-and-compliance/in-place-and-litigation-holds.md)
  - [In-Place eDiscovery](../../security-and-compliance/in-place-ediscovery/in-place-ediscovery.md)

- **Run a per-mailbox Litigation Hold report**: Use this report to find mailboxes that were put on, or removed from, litigation hold. For more information, see [Run a per-mailbox litigation hold report](per-mailbox-litigation-hold-report.md).

- **Run the admin audit log report**<sup>\*</sup>: Use this report to view entries from the admin audit log. Instead of exporting the admin audit log, which can take up to 24 hours to receive in an email message, you can run this report in the EAC. This report records configuration changes made by admins in your organization. Up to 5000 entries will be displayed on multiple pages. To narrow the search, you can specify a date range. For more information, see [View the admin audit log](view-administrator-audit-log.md).

- **Run the external admin audit log report**: Actions performed by Microsoft datacenter administrators or delegated admins are logged in the admin audit log. Use the external admin audit log report to search for and view the actions that administrators outside your organization performed on the configuration of your Exchange Online organization. For more information, see [View and export the external admin audit log](view-external-admin-audit-log.md).

<sup>\*</sup> This report is available in standalone EOP organizations.

## Configure mailbox audit logging

> [!NOTE]
> Mailbox audit logging is not available in standalone EOP.

As of January 2019, mailbox audit logging on by default is enabled for all Exchange Online organizations. For more information, see [Manage mailbox auditing](/microsoft-365/compliance/enable-mailbox-auditing).

## Give users access to Auditing reports

By default, admins can access and run any of the reports on the **Auditing** page in the EAC. However, other users, such as a records manager or legal staff, have to be assigned the necessary permissions.

- The **Auditing Logs** role allows users to view the **Auditing** page to run any of the available reports, export the mailbox audit log, and export and view the admin audit log. By default, this role is assigned to the **Organization Management**, **Compliance Management**, and **Records Management** role groups.
- The **View-Only Audit Logs** role allows user to run auditing reports, but not to export audit logs. By default, this role is assigned to the **Organization Management** and **Compliance Management** role groups.

The easiest way to give users access to the reports is to add them to the **Records Management** role group, which has the **Auditing Logs** role assigned.

### Use the EAC to add users to the Records Management role group

1. Go to **Permissions** \> **Admin Roles**.

2. In the list of role groups, click **Records Management**, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. Under **Members**, click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

4. In the **Select Members** dialog box, select the user. You can search for a user by typing all or part of a display name, and then clicking **Search** ![Search icon](../../media/ITPro_EAC_.gif). You can also sort the list by clicking the **Name** or **Display Name** column headings.

5. Click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) and then click **OK** to return to the role group page.

6. Click **Save** to save the change to the role group.

In the details pane, the user is listed under **Members** and can access the Auditing page in the EAC, run auditing reports, and export audit logs.

### Use PowerShell to add users to the Records Management role group

In [Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell) or standalone [Exchange Online Protection PowerShell](/powershell/exchange/connect-to-exchange-online-protection-powershell), replace \<Identity\> with the name, alias, email address, or account name of the user or group, and then run the following command to assign the Audit Logs role to the user:

```PowerShell
Add-RoleGroupMember -Identity "Records Management" -Member <Identity>
```

For detailed syntax and parameter information, see [Add-RoleGroupMember](/powershell/module/exchange/add-rolegroupmember).
