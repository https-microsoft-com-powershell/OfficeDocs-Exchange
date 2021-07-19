---
localization_priority: Normal
description: Learn how to view the admin audit log in Exchange Online
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 5c62072a-556d-4fea-9973-d668c6b9fd57
ms.reviewer: 
f1.keywords:
- NOCSH
title: View the admin audit log
search.appverid: MET150
ms.collection: 
- exchange-online
- M365-email-calendar
audience: Admin
ms.service: exchange-online
manager: serdars

---

# View the admin audit log in Exchange Online

In Exchange Online organizations or standalone Exchange Online Protection (EOP) organizations without Exchange Online mailboxes, you can use the Exchange admin center (EAC) or PowerShell to search for and view entries in the admin audit log.

The admin audit log records specific actions, based on Exchange Online PowerShell or standalone Exchange Online Protection PowerShell cmdlets, done by admins and users who have been assigned administrative privileges. Entries in the admin audit log provide you with information about what cmdlet was run, which parameters were used, who ran the cmdlet, and what objects were affected.

**Notes**:

- Admin auditing logging is enabled by default, and you can't disable it.
- The admin audit log doesn't record actions based on cmdlets that begins with the verbs **Get**, **Search**, or **Test**.
- When a change is made in your organization, it may take up to 15 minutes to appear in audit log search results. If a change doesn't appear in the admin audit log, wait a few minutes and run the search again.
- Audit log entries are kept for 90 days. When an entry is older than 90 days, it's deleted.

## What do you need to know before you begin?

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md).

- To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell). To connect to standalone Exchange Online Protection PowerShell see [Connect to Exchange Online Protection PowerShell](/powershell/exchange/connect-to-exchange-online-protection-powershell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "View-only administrator audit logging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this article, see [Keyboard shortcuts for the Exchange admin center in Exchange Online](/Exchange/accessibility/keyboard-shortcuts-in-admin-center).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the EAC to view the admin audit log

1. In the EAC, go to **Compliance management** \> **Auditing**, and then choose **Run the admin audit log report**.

2. In the **Search for changes to administrator role groups** page that opens, choose a **Start date** and **End date** (the default range is the past two weeks), and then choose **Search**. All configuration changes made during the specified time period are displayed, and can be sorted, using the following information:

   - **Date**: The date and time that the configuration change was made. The date and time are stored in Coordinated Universal Time (UTC) format.
   - **Cmdlet**: The name of the cmdlet that was used to make the configuration change.
   - **User**: The name of the user account of the user who made the configuration change.

     Up to 5000 entries will be displayed on multiple pages. Specify a smaller date range if you need to narrow your results. If you select an individual search result, the following additional information is displayed in the details pane:

   - **Object modified**: The object that was modified by the cmdlet.
   - **Parameters (Parameter:Value)**: The cmdlet parameters that were used, and any value specified with the parameter.

3. If you want to print a specific audit log entry, choose the **Print** button in the details pane.

## Use PowerShell to view the admin audit log

You can use Exchange Online PowerShell or standalone Exchange Online Protection PowerShell to search for audit log entries that meet the criteria you specify. Use the following syntax:

```PowerShell
Search-AdminAuditLog [-Cmdlets <Cmdlet1,Cmdlet2,...CmdletN>] [-Parameters <Parameter1,Parameter2,...ParameterN>] [-StartDate <UTCDateTime>] [-EndDate <UTCDateTime>] [-UserIds <"User1","User2",..."UserN">] [-ObjectIds <"Object1","Object2",..."ObjectN">] [-IsSuccess <$true | $false>]
```

**Notes**:

- You can only use the _Parameters_ parameter together with the _Cmdlets_ parameter.
- The _ObjectIds_ parameter filters the results by the object that was modified by the cmdlet. A valid value depends on how the object is represented in the audit log. For example:
  - Name
  - Canonical distinguished name (for example, contoso.com/Users/Akia Al-Zuhairi)

  You'll likely need to use other filtering parameters on this cmdlet to narrow down the results and identify the types of objects that you're interested in.

- The _UserIds_ parameter filters the results by the user who made the change (who ran the cmdlet).
- For the _StartDate_ and _EndDate_ parameters, if you specify a date/time value without a time zone, the value is in Coordinated Universal Time (UTC). To specify a date/time value for this parameter, use either of the following options:
  - Specify the date/time value in UTC: For example, "2016-05-06 14:30:00z".
  - Specify the date/time value as a formula that converts the date/time in your local time zone to UTC: For example, `(Get-Date "5/6/2016 9:30 AM").ToUniversalTime()`. For more information, see [Get-Date](/powershell/module/microsoft.powershell.utility/get-date).
- The cmdlet returns a maximum of 1,000 log entries by default. Use the _ResultSize_ parameter to specify up to 250,000 log entries. Or, use the value `Unlimited` to return all entries.

This example performs a search for all audit log entries with the following criteria:

- **Start date**: August 4, 2019
- **End date**: October 3, 2019
- **Cmdlets**: Update-RoleGroupMember

```PowerShell
Search-AdminAuditLog -Cmdlets Update-RoleGroupMember -StartDate (Get-Date "08/04/2019").ToUniversalTime() -EndDate (Get-Date "10/03/2019").ToUniversalTime()
```

For detailed syntax and parameter information, see [Search-AdminAuditLog](/powershell/module/exchange/search-adminauditlog).

### View details of audit log entries

The **Search-AdminAuditLog** cmdlet returns the fields described in the [Audit log contents](#audit-log-contents) section later in this article. Of the fields returned by the cmdlet, two fields, **CmdletParameters** and **ModifiedProperties**, contain additional information that isn't returned by default.

To view the contents of the **CmdletParameters** and **ModifiedProperties** fields, use the following steps.

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

## Audit log contents

Each audit log entry contains the information described in the following table. The audit log contains one or more audit log entries.

<br>

****

|Field|Description|
|---|---|
|`RunspaceId`|This field is used internally.|
|`ObjectModified`|This field contains the object that was modified by the cmdlet specified in the `CmdletName` field.|
|`CmdletName`|This field contains the name of the cmdlet that was run by the user in the `Caller` field.|
|`CmdletParameters`|This field contains the parameters that were specified when the cmdlet in the `CmdletName` field was run. Also stored in this field, but not visible in the default output, is the value specified with the parameter, if any.|
|`ModifiedProperties`|This field contains the properties that were modified on the object in the `ObjectModified` field. Also stored in this field, but not visible in the default output, are the old value of the property and the new value that was stored.|
|`Caller`|This field contains the user account of the user who ran the cmdlet in the `CmdletName` field.|
|`ExternalAccess`|This field is used internally.|
|`Succeeded`|This field specifies whether the cmdlet in the `CmdletName` field ran successfully. The value is either `True` or `False`.|
|`Error`|This field contains the error message generated if the cmdlet in the `CmdletName` field failed to complete successfully.|
|`RunDate`|This field contains the date and time when the cmdlet in the `CmdletName` field was run. The date and time are stored in Coordinated Universal Time (UTC) format.|
|`OriginatingServer`|This field indicates the server on which the cmdlet specified in the `CmdletName` field was run.|
|`ClientIP`|This field is used internally.|
|`SessionId`|This field is used internally.|
|`AppId`|This field is used internally.|
|`ClientAppId`|This field is used internally.|
|`Identity`|This field is used internally.|
|`IsValid`|This field is used internally.|
|`ObjectState`|This field is used internally.|
|
