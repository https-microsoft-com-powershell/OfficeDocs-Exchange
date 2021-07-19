---
localization_priority: Normal
description: Admins can learn how to assign Full Access, Send As, and Send on Behalf permissions to mailboxes and groups in Exchange Online.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 749cdfe3-496b-453f-96eb-20a0bf28fd52
ms.reviewer:
f1.keywords:
- NOCSH
title: Manage permissions for recipients in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage permissions for recipients in Exchange Online

> [!IMPORTANT]
> Check out the new Exchange Admin Center! The experience is modern, intelligent, accessible, and better. Personalize your dashboard, manage cross tenant migration, experience the improved Groups feature, and more. [Try it now](https://admin.exchange.microsoft.com)!

In Exchange Online, you can use the Exchange admin center (EAC) or Exchange Online PowerShell to assign permissions to a mailbox or group so that other users can access the mailbox (the Full Access permission), or send email messages that appear to come from the mailbox or group (the Send As or Send on Behalf permissions). The users that are assigned these permissions on other mailboxes or groups are called *delegates*.

The permissions that you can assign to delegates for mailboxes and groups in Exchange Online are described in the following table:

**Note**: Although you might be able use Exchange Online PowerShell to assign some or all of these permissions to other delegate types on other kinds of recipient objects, this article focuses on the delegate and recipient object types that produce useful results.

<br>

****

|Permission|Description|Recipient types in the EAC|Additional recipient types in PowerShell|**Available delegate types|
|---|---|---|---|---|
|**Full Access**|Allows the delegate to open the mailbox, and view, add and remove the contents of the mailbox. Doesn't allow the delegate to send messages from the mailbox. <p> If you assign the Full Access permission to a mailbox that's hidden from address lists, the delegate won't be able to open the mailbox. By default, discovery mailboxes are hidden from address lists. <p> By default, the mailbox auto-mapping feature uses Autodiscover to automatically open the mailbox in the delegate's Outlook profile (in addition to their own mailbox). Note that auto-mapping will only work for individual users granted the proper permissions and will not work for any kind of group. If you don't want mailboxes to be auto-mapped, you need to take one of the following actions: <ui><li>Use the **Add-MailboxPermission** cmdlet in Exchange Online PowerShell to assign the Full Access permission with the `-AutoMapping $false` setting. For more information, see the [Use Exchange Online PowerShell to assign the Full Access permission to mailboxes](#use-exchange-online-powershell-to-assign-the-full-access-permission-to-mailboxes) section in this article.</li><li>Assign the Full Access permission to a mail-enabled security group. The mailbox won't open in the Outlook profile of each member.</li></ul>|User mailboxes <p> Resource mailboxes <p> Shared mailboxes|Discovery mailboxes|Mailboxes with user accounts <p> Mail users with accounts <p> Mail-enabled security groups|
|**Send As**|Allows the delegate to send messages as if they came directly from the mailbox or group. There's no indication that the message was sent by the delegate. <p> Doesn't allow the delegate to read the contents of the mailbox. <p> If you assign the Send As permission to a mailbox that's hidden from address lists, the delegate won't be able to send messages from the mailbox.|User mailboxes <p> Resource mailboxes <p> Shared mailboxes <p> Distribution groups <p> Dynamic distribution groups <p> Mail-enabled security groups <p> Microsoft 365 groups|n/a|Mailboxes with user accounts <p> Mail users with accounts <p> Mail-enabled security groups|
|**Send on Behalf**|Allows the delegate to send messages from the mailbox or group. The From address of these messages clearly shows that the message was sent by the delegate (" _\<Delegate\>_ on behalf of _\<MailboxOrGroup\>_"). However, replies to these messages are sent to the mailbox or group, not to the delegate. <p> Doesn't allow the delegate to read the contents of the mailbox. <p> If you assign the Send on Behalf permission to a mailbox that's hidden from address lists, the delegate won't be able to send messages from the mailbox. <p> |User mailboxes <p> Resource mailboxes <p> Distribution groups <p> Dynamic distribution groups <p> Mail-enabled security groups <p> Microsoft 365 groups|Shared mailboxes|Mailboxes with user accounts <p> Mail users with accounts <p> Mail-enabled security groups <p> Distribution groups|
|

> [!NOTE]
> If a user has both Send As and Send on Behalf permissions to a mailbox or group, the Send As permission is always used.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox settings" entry in the [Feature permissions in Exchange Online](../permissions-exo/feature-permissions.md) article.

- To open and use the EAC, see [Exchange admin center in Exchange Online](../exchange-admin-center.md). To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell).

- When a mailbox is added to Outlook using Advanced Settings, only the primary mailbox will be added; the archive mailbox won't be added. If a user needs to also access the archive mailbox, the mailbox should be added to Outlook as a second account in the same Outlook profile.

- For information about keyboard shortcuts that may apply to the procedures in this article, see [Keyboard shortcuts for the Exchange admin center](../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the EAC to assign permissions to individual mailboxes

1. In the EAC, click **Recipients** in the feature pane. Depending on the type of mailbox that you want to assign permissions for, click on one of the following tabs:

   - **Mailboxes**: User or linked mailboxes.
   - **Resources**: Room or equipment mailboxes.
   - **Shared**: Shared mailboxes.

2. In the list of mailboxes, select the mailbox that you want to assign permissions for, and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png).

3. On the mailbox properties page that opens, click **Mailbox delegation** and configure one or more of the following permissions:

   - **Send As**: Messages sent by a delegate appear to come from the mailbox.
   - **Send on Behalf**: Messages sent by a delegate have " _\<Delegate\>_ on behalf of _\<Mailbox\>_" in the From address. Note that this permission isn't available in the EAC for shared mailboxes.
   - **Full Access**: The delegate can open the mailbox and do anything except send messages.

   To assign permissions to delegates, click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) under the appropriate permission. A dialog box appears that lists the users or groups that can have the permission assigned to them. Select the user or group from the list, and then click **Add**. Repeat this process as many times as necessary. You can also search for users or groups in the search box by typing all or part of the name, and then clicking **Search** ![Search icon](../media/ITPro_EAC_.png). When you're finished selecting delegates, click **OK**.

   To remove a permission from a delegate, select the delegate in the list under the appropriate permission, and then click **Remove** ![Remove icon](../media/ITPro_EAC_RemoveIcon.png).

4. When you're finished, click **Save**.

## Use the EAC to assign permissions to multiple mailboxes at the same time

1. In the EAC, go to **Recipients** \> **Mailboxes**.

2. Select the mailboxes that you want to assign permissions for. Use click + Shift key + click to select a range of mailboxes, or Ctrl key + click to select multiple individual mailboxes. The title of the details pane changes to **Bulk Edit** as shown in the following diagram.

   ![Bulk select mailboxes in the EAC](../media/ee6acd85-a6b8-44f4-8eb1-a6e84e4dfff1.png)

3. At the bottom of the details pane, click **More options**. Under the **Mailbox Delegation** option that appears, choose **Add** or **Remove**. Depending on your selection, do one of the following steps:

   - **Add**: In the **Bulk Add Delegation** dialog box that appears, click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) under the appropriate permission (**Send As**, **Send on Behalf**, or **Full Access**). When you're finished selecting users or groups to add as delegates, click **Save**.
   - **Remove**: In the **Bulk Remove Delegation** dialog box that appears, click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) under the appropriate permission (**Send As**, **Send on Behalf**, or **Full Access**). When you're finished selecting users or groups to remove from the existing delegates, click **Save**.

## Use the EAC to assign permissions to groups

1. In the EAC, go to **Recipients** \> **Groups**.

2. In the list of groups, select the group that you want to assign permissions for, and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png).

3. On the group properties page that opens, click **Group delegation** and configure one of the following permissions:
   - **Send As**: Messages sent by a delegate appear to come from the group.
   - **Send on Behalf**: Messages sent by a delegate have " _\<Delegate\>_ on behalf of _\<Group\>_" in the From address.

4. To assign permissions to delegates, click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) under the appropriate permission. A dialog box appears that lists the users or groups that can have the permission assigned to them. Select the user or group from the list, and then click **Add**. Repeat this process as many times as necessary. You can also search for users or groups in the search box by typing all or part of the name, and then clicking **Search** ![Search icon](../media/ITPro_EAC_.png). When you're finished selecting delegates, click **OK**.

   To remove a permission from a delegate, select the delegate in the list under the appropriate permission, and then click **Remove** ![Remove icon](../media/ITPro_EAC_RemoveIcon.png).

5. When you're finished, click **Save**.

## Use Exchange Online PowerShell to assign the Full Access permission to mailboxes

You use the **Add-MailboxPermission** and **Remove-MailboxPermission** cmdlets to manage the Full Access permission for mailboxes. These cmdlets use the same basic syntax:

```PowerShell
Add-MailboxPermission -Identity <MailboxIdentity> -User <DelegateIdentity> -AccessRights FullAccess -InheritanceType All [-AutoMapping $false]
```

```PowerShell
Remove-MailboxPermission -Identity <MailboxIdentity> -User <DelegateIdentity> -AccessRights FullAccess -InheritanceType All
```

This example assigns the delegate Raymond Sam the Full Access permission to the mailbox of Terry Adams.

```PowerShell
Add-MailboxPermission -Identity "Terry Adams" -User raymonds -AccessRights FullAccess -InheritanceType All
```

This example assigns Esther Valle the Full Access permission to the organization's default discovery search mailbox, and prevents the mailbox from automatically opening in Esther Valle's Outlook.

```PowerShell
Add-MailboxPermission -Identity "DiscoverySearchMailbox{D919BA05-46A6-415f-80AD-7E09334BB852}" -User estherv -AccessRights FullAccess -InheritanceType All -AutoMapping $false
```

This example assigns members of the Helpdesk mail-enabled security group the Full Access permission to the shared mailbox named Helpdesk Tickets.

```PowerShell
Add-MailboxPermission -Identity "Helpdesk Tickets" -User Helpdesk -AccessRights FullAccess -InheritanceType All
```

This example removes Full Access permission for Jim Hance from Ayla Kol's mailbox.

```PowerShell
Remove-MailboxPermission -Identity ayla -User "Jim Hance" -AccessRights FullAccess -InheritanceType All
```

For detailed syntax and parameter information, see:

- [Add-MailboxPermission](/powershell/module/exchange/add-mailboxpermission).
- [Remove-MailboxPermission](/powershell/module/exchange/remove-mailboxpermission).

### How do you know this worked?

To verify that you've successfully assigned or removed the Full Access permission for a delegate on a mailbox, use either of the following procedures:

- In the properties of the mailbox in the EAC, verify the delegate is or isn't listed in **Mailbox delegation** \> **Full Access**.

- Replace _\<MailboxIdentity\>_ with the identity of the mailbox and run the following command in Exchange Online PowerShell to verify that the delegate is or isn't listed..

   ```PowerShell
   Get-MailboxPermission <MailboxIdentity> | where {$_.AccessRights -like 'Full*'} | Format-Table User,Deny,IsInherited,AccessRights -Auto
   ```

   For more information, see [Get-MailboxPermission](/powershell/module/exchange/get-mailboxpermission).

## Use Exchange Online PowerShell to assign the Send As permission to mailboxes and groups

You use the **Add-RecipientPermission** and **Remove-RecipientPermission** cmdlets to manage the Send As permission for mailboxes and groups. These cmdlets use the same basic syntax:

```PowerShell
<Add-RecipientPermission | Remove-RecipientPermission> -Identity <MailboxOrGroupIdentity> -Trustee <DelegateIdentity> -AccessRights SendAs
```

This example assigns the Send As permission to the Printer Support group on the shared mailbox named Contoso Printer Support.

```PowerShell
Add-RecipientPermission -Identity "Contoso Printer Support" -Trustee "Printer Support" -AccessRights SendAs
```

This example removes the Send As permission for the user Karen Toh on the mailbox for Yan Li.

```PowerShell
Remove-RecipientPermission -Identity "Yan Li" -Trustee "Karen Toh" -AccessRights SendAs
```

For detailed syntax and parameter information, see:

- [Add-RecipientPermission](/powershell/module/exchange/add-recipientpermission)
- [Remove-RecipientPermission](/powershell/module/exchange/remove-recipientpermission)

### How do you know this worked?

To verify that you've successfully assigned or removed the Send As permission for a delegate on a mailbox or group, use either of the following procedures:

- In the properties of the mailbox or group in the EAC, verify the delegate is or isn't listed in **Mailbox delegation** \> **Send As** or **Group delegation** \> **Send As**.

- Replace _\<MailboxIdentity>_ and _\<DelegateIdentity>_ with the name, alias, or email address of the mailbox or group and run the following command in Exchange Online PowerShell to verify that the delegate is or isn't listed.

   ```PowerShell
   Get-RecipientPermission -Identity <MailboxIdentity> -Trustee <DelegateIdentity>
   ```

## Use Exchange Online PowerShell to assign the Send on Behalf permission to mailboxes and groups

You use the _GrantSendOnBehalfTo_ parameter on the various mailbox and group **Set-** cmdlets to manage the Send on Behalf permission for mailboxes and groups:

- **Set-Mailbox**
- **Set-DistributionGroup**: Distribution groups and mail-enabled security groups.
- **Set-DynamicDistributionGroup**
- **Set-UnifiedGroup**: Microsoft 365 groups.

The basic syntax for these cmdlets is:

```PowerShell
<Cmdlet> -Identity <MailboxOrGroupIdentity> -GrantSendOnBehalfTo <Delegates>
```

The _GrantSendOnBehalfTo_ parameter has the following options for delegate values:

- **Replace existing delegates**: `<DelegateIdentity>` or `"<DelegateIdentity1>","<DelegateIdentity2>",...`
- **Add or remove delegates without affecting other delegates**: `@{Add="\<value1\>","\<value2\>"...; Remove="\<value1\>","\<value2\>"...}`
- **Remove all delegates**: Use the value `$null`.

This example assigns the delegate Holly Holt the Send on Behalf permission to the mailbox of Sean Chai.

```PowerShell
Set-Mailbox -Identity seanc@contoso.com -GrantSendOnBehalfTo hollyh
```

This example adds the group tempassistants@contoso.com to the list of delegates that have Send on Behalf permission to the Contoso Executives shared mailbox.

```PowerShell
Set-Mailbox "Contoso Executives" -GrantSendOnBehalfTo @{Add="tempassistants@contoso.com"}
```

This example assigns the delegate Sara Davis the Send on Behalf permission to the Printer Support distribution group.

```PowerShell
Set-DistributionGroup -Identity printersupport@contoso.com -GrantSendOnBehalfTo sarad
```

This example removes the Send on Behalf permission that was assigned to the administrator on the All Employees dynamic distribution group.

```PowerShell
Set-DynamicDistributionGroup "All Employees" -GrantSendOnBehalfTo @{Remove="Administrator"}
```

### How do you know this worked?

To verify that you've successfully assigned or removed the Send on Behalf permission for a delegate on a mailbox or group, use either of the following procedures:

- In the properties of the mailbox or group in the EAC, verify the delegate is or isn't listed in **Mailbox delegation** \> **Send As** or **Group delegation** \> **Send As**.

- Replace _\<MailboxIdentity\>_ or _\<GroupIdentity\>_ with the identity of the mailbox or group and run the one of the following commands in Exchange Online PowerShell to verify that the delegate is or isn't listed.

  - Mailbox:

    ```PowerShell
    Get-Mailbox -Identity <MailboxIdentity> | Format-List GrantSendOnBehalfTo
    ```

  - Distribution group or mail-enabled security group:

    ```PowerShell
    Get-DistributionGroup -Identity <GroupIdentity> | Format-List GrantSendOnBehalfTo
    ```

  - Dynamic distribution group:

    ```PowerShell
    Get-DynamicDistributionGroup -Identity <GroupIdentity> | Format-List GrantSendOnBehalfTo
    ```

  - Microsoft 365 group:

    ```PowerShell
    Get-UnifiedGroup -Identity <GroupIdentity> | Format-List GrantSendOnBehalfTo
    ```

### Next steps

For more information about how delegates can use the permissions that are assigned to them on mailboxes and groups, see the following articles:

- [Access another person's mailbox](https://support.microsoft.com/office/a909ad30-e413-40b5-a487-0ea70b763081)
- [Open and use a shared mailbox in Outlook for Windows](https://support.microsoft.com/office/d94a8e9e-21f1-4240-808b-de9c9c088afd)
- [Open and use a shared mailbox in Outlook on the web](https://support.microsoft.com/office/98b5a90d-4e38-415d-a030-f09a4cd28207)
- [Send email from or on behalf of an Office 365 group](https://support.microsoft.com/office/0f4964af-aec6-484b-a65c-0434df8cdb6b)
