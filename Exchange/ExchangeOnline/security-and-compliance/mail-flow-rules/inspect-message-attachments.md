---
localization_priority: Normal
description: Admins can learn how to configure mail flow rules to inspect message attachments in Exchange Online.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 874d1c78-a8ec-4938-b388-d3208c2fa971
ms.reviewer: 
f1.keywords:
- NOCSH
title: Use mail flow rules to inspect message attachments in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Use mail flow rules to inspect message attachments in Exchange Online

In Exchange Online organizations or standalone Exchange Online Protection (EOP) organizations without Exchange Online mailboxes, you can inspect email attachments by setting up mail flow rules (also known as transport rules). Mail flow rules allow you to examine email attachments as a part of your messaging security and compliance needs. When you inspect attachments, you can then take action on the messages based on the content or characteristics of the attachments. Here are some attachment-related tasks you can do by using mail flow rules:

- Search for files with text that matches a pattern you specify, and add a disclaimer to the end of the message.
- Inspect content within attachments and, if there are any keywords you specify, redirect the message to a moderator for approval before it's delivered.
- Check for messages with attachments that can't be inspected and then block the entire message from being sent.
- Check for attachments that exceed a certain size and then notify the sender of the issue, if you choose to prevent the message from being delivered.
- Check whether the properties of an attached Office document match the values that you specify. With this condition, you can integrate the requirements of your mail flow rules and DLP policies with a third-party classification system, such as SharePoint or the Windows Server File Classification Infrastructure (FCI).
- Create notifications that alert users if they send a message that has matched a mail flow rule.
- Block all messages containing attachments. For examples, see [Use mail flow rules for attachment blocking scenarios in Exchange Online](common-attachment-blocking-scenarios.md).

> [!NOTE]
> All of these conditions will scan compressed archive attachments.

Exchange Online admins can create mail flow rules in the Exchange admin center (EAC) at **Mail flow** \> **Rules**. You need permissions to do this procedure. After you start to create a new rule, you can see the full list of attachment-related conditions by clicking **More options** \> **Any attachment** under **Apply this rule if**. The attachment-related options are shown in the following diagram.

![List of conditions for attachments](../../media/c8ab24df-dbb6-4760-bfb0-b62938bfb447.png)

 For more information about mail flow rules, including the full range of conditions and actions that you can choose, see [Mail flow rules (transport rules) in Exchange Online](mail-flow-rules.md). Exchange Online Protection (EOP) and hybrid customers can benefit from the mail flow rules best practices provided in [Best Practices for Configuring EOP](/microsoft-365/security/office-365-security/best-practices-for-configuring-eop). If you're ready to start creating rules, see [Manage mail flow rules in Exchange Online](manage-mail-flow-rules.md).

## Inspect the content within attachments

You can use the mail flow rule conditions in the following table to examine the content of message attachments. For these conditions, only the first 1 megabyte (MB) of text extracted from an attachment is inspected. The 1 MB limit refers to the extracted text, not the file size of the attachment. For example, a 2 MB file may contain less than 1 MB of text, so all of the text would be inspected.

To start using these conditions when inspecting messages, you need to add them to a mail flow rule. Learn about creating or changing rules at [Manage mail flow rules in Exchange Online](manage-mail-flow-rules.md).

<br>

****

|Condition name in the EAC|Condition name in Exchange Online PowerShell|Description|
|---|---|---|
|**Any attachment's content includes** <br/> **Any attachment** \> **content includes any of these words**|_AttachmentContainsWords_|This condition matches messages with supported file type attachments that contain a specified string or group of characters.|
|**Any attachment's content matches** <br/> **Any attachment** \> **content matches these text patterns**|_AttachmentMatchesPatterns_|This condition matches messages with supported file type attachments that contain a text pattern that matches a specified regular expression.|
|**Any attachment's content can't be inspected** <br/> **Any attachment** \> **content can't be inspected**|_AttachmentIsUnsupported_|Mail flow rules only can inspect the content of supported file types. If the mail flow rule finds an attachment that isn't supported, the _AttachmentIsUnsupported_ condition is triggered. The supported file types are described in the next section.|
|

> [!NOTE]
>
> - The condition names in Exchange Online PowerShell are parameter names on the **New-TransportRule** and **Set-TransportRule** cmdlets. For more information, see [New-TransportRule](/powershell/module/exchange/new-transportrule).
>
> - Learn more about property types for these conditions at [Mail flow rule conditions and exceptions (predicates) in Exchange Online](conditions-and-exceptions.md).
>
> - To learn how to use Windows PowerShell to connect to Exchange Online, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell).

### Supported file types for mail flow rule content inspection

The following table lists the file types supported by mail flow rules. The system automatically detects file types by inspecting file properties rather than the actual file name extension, thus helping to prevent malicious hackers from being able to bypass mail flow rule filtering by renaming a file extension. A list of file types with executable code that can be checked within the context of mail flow rules is listed later in this article.

<br>

****

|Category|File extension|Notes|
|---|---|---|
|Office 2007 and later|.docm, .docx, .pptm, .pptx, .pub, .one, .xlsb, .xlsm, .xlsx|Microsoft OneNote and Microsoft Publisher files aren't supported by default. <br/> The contents of any embedded parts contained within these file types are also inspected. However, any objects that aren't embedded (for example, linked documents) aren't inspected.|
|Office 2003|.doc, .ppt, .xls|None|
|Other Office files|.rtf, .vdw, .vsd, .vss, .vst|None|
|Adobe PDF|.pdf|None|
|HTML|.html|None|
|XML|.xml, .odp, .ods, .odt|None|
|Text|.txt, .asm, .bat, .c, .cmd, .cpp, .cxx, .def, .dic, .h, .hpp, .hxx, .ibq, .idl, .inc, inf, .ini, inx, .js, .log, .m3u, .pl, .rc, .reg, .txt, .vbs, .wtx|None|
|OpenDocument|.odp, .ods, .odt|No parts of .odf files are processed. For example, if the .odf file contains an embedded document, the contents of that embedded document aren't inspected.|
|AutoCAD Drawing|.dxf|AutoCAD 2013 files aren't supported.|
|Image|.jpg, .tiff|Only the metadata text associated with these image files is inspected. There's no optical character recognition.|
|Compressed archive files|.bz2, cab, .gz, .rar, .tar, .zip, .7z|The content of these files, that were originally in a supported file type format, are inspected and processed in a manner similar to messages that have multiple attachments. The properties of the compressed archive file itself aren't inspected. For example, if the container file type supports comments, that field isn't inspected.|
|

## Inspect the file properties of attachments

The following conditions can be used in mail flow rules to inspect different properties of files that are attached to messages. To start using these conditions when inspecting messages, you need to add them to a mail flow rule. For more information about creating or changing rules, see [Manage mail flow rules](manage-mail-flow-rules.md).

<br>

****

|Condition name in the EAC|Condition name in Exchange Online PowerShell|Description|
|---|---|---|
|**Any attachment's file name matches** <p> **Any attachment** \> **file name matches these text patterns**|_AttachmentNameMatchesPatterns_|This condition matches messages with attachments whose file name contains the characters you specify.|
|**Any attachment's file extension matches** <p> **Any attachment** \> **file extension includes these words**|_AttachmentExtensionMatchesWords_|This condition matches messages with attachments whose file name extension matches what you specify.|
|**Any attachment is greater than or equal to** <p> **Any attachment** \> **size is greater than or equal to**|_AttachmentSizeOver_|This condition matches messages with attachments when those attachments are greater than or equal to the size you specify. <p> **Note:** This condition refers to the sizes of individual attachments, not the cumulative size. For example, if you set a rule to reject any attachment that is 10 MB or greater, a single attachment with a size of 15 MB will be rejected, but a message with three 5 MB attachments will be allowed.|
|**The message didn't complete scanning** <p> **Any attachment** \> **didn't complete scanning**|_AttachmentProcessingLimitExceeded_|This condition matches messages when an attachment is not inspected by the mail flow rules agent.|
|**Any attachment has executable content** <p> **Any attachment** \> **has executable content**|_AttachmentHasExecutableContent_|This condition matches messages that contain executable files as attachments. The supported file types are listed [here](#supported-file-types-for-mail-flow-rule-content-inspection).|
|**Any attachment is password protected** <p> **Any attachment** \> **is password protected**|_AttachmentIsPasswordProtected_|This condition matches messages with attachments that are protected by a password. Password detection only works for Office documents, .zip files, and .7z files.|
|**Any attachment has these properties, including any of these words** <p> **Any attachment** \> **has these properties, including any of these words**|_AttachmentPropertyContainsWords_|This condition matches messages where the specified property of the attached Office document contains specified words. A property and its possible values are separated with a colon. Multiple values are separated with a comma. Multiple property/value pairs are also separated with a comma.|
|

> [!NOTE]
>
> - The condition names in Exchange Online PowerShell are parameter names on the **New-TransportRule** and **Set-TransportRule** cmdlets. For more information, see [New-TransportRule](/powershell/module/exchange/new-transportrule).
>
> - Learn more about property types for these conditions at [Mail flow rule conditions and exceptions (predicates) in Exchange Online](conditions-and-exceptions.md).
>
> - To learn how to connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell).

### Supported executable file types for mail flow rule inspection

The mail flow rules use true type detection to inspect file properties rather than merely the file extensions. This helps to prevent malicious hackers from being able to bypass your rule by renaming a file extension. The following table lists the executable file types supported by these conditions. If a file is found that isn't listed here, the `AttachmentIsUnsupported` condition is triggered.

<br>

****

|Type of file|Native extension|
|---|---|
|32-bit Windows executable file with a dynamic link library extension.|.dll|
|Self-extracting executable program file.|.exe|
|Uninstallation executable file.|.exe|
|Program shortcut file.|.exe|
|32-bit Windows executable file.|.exe|
|Microsoft Visio XML drawing file.|.vxd|
|OS/2 operating system file.|.os2|
|16-bit Windows executable file.|.w16|
|Disk-operating system file.|.dos|
|European Institute for Computer Antivirus Research standard antivirus test file.|.com|
|Windows program information file.|.pif|
|Windows executable program file.|.exe|
|

> [!IMPORTANT]
> **.rar** (self-extracting archive files created with the WinRAR archiver), **.jar** (Java archive files), and **.obj** (compiled source code, 3D object, or sequence files) files are **not** considered to be executable file types. To block these files, you can use mail flow rules that look for files with these extensions as described earlier in this article, or you can configure an antimalware policy that blocks these file types (the common attachment types filter). For more information, see [Configure anti-malware policies in EOP](/microsoft-365/security/office-365-security/configure-anti-malware-policies).

## Data loss prevention policies and attachment mail flow rules

> [!NOTE]
> This section does not apply to standalone EOP organizations.

To help you manage important business information in email, you can include any of the attachment-related conditions along with the rules of a data loss prevention (DLP) policy.

DLP policies and attachment-related conditions can help you enforce your business needs by defining those needs as mail flow rule conditions, exceptions, and actions. When you include the sensitive information inspection in a DLP policy, any attachments to messages are scanned for that information only. However, attachment-related conditions such as size or file type aren't included until you add the conditions listed in this article. DLP isn't available with all versions of Exchange; learn more at [Data loss prevention](../../security-and-compliance/data-loss-prevention/data-loss-prevention.md).

## For more information

For information on broadly blocking email with attachments, regardless of malware status, see [Common attachment blocking scenarios for mail flow rules in Exchange Online](common-attachment-blocking-scenarios.md).
