---
localization_priority: Normal
description: Use the Exchange admin center (EAC) or Exchange Online PowerShell to create a new distribution group in your Exchange Online organization or to mail-enable an existing group.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: c4c43493-55e1-46d2-bd4b-d6f6cecd747f
ms.reviewer: 
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Recipients.CreateDistributionGroupWizardForm.CreateDistributionGroupIntroductionWizardPage
title: Create and manage distribution groups
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create and manage distribution list groups in Exchange Online

> [!IMPORTANT]
> Check out the new Exchange Admin Center! The experience is modern, intelligent, accessible, and better. Personalize your dashboard, manage cross tenant migration, experience the improved Groups feature, and more. [Try it now](https://admin.exchange.microsoft.com)!

Use the Exchange admin center (EAC) or Exchange Online PowerShell to create, modify, or remove distribution list groups in your Exchange Online organization.

There are two types of groups that can be used to distribute messages:

- Mail-enabled universal distribution groups (also called distribution list groups) can be used only to distribute messages.

- Mail-enabled universal security groups (also called security groups) can be used to distribute messages and to grant access permissions to resources. For more information, see [Manage mail-enabled security groups](../../recipients-in-exchange-online/manage-mail-enabled-security-groups.md).

It's important to note the terminology differences between Active Directory and Exchange Online. In Active Directory, a distribution list group refers to any group that doesn't have a security context, whether it's mail-enabled or not. In contrast, in Exchange Online, all mail-enabled groups are referred to as distribution list groups, whether they have a security context or not.

## What do you need to know before you begin?

- Estimated time to complete: 2 to 5 minutes.

- To open the Exchange admin center, see [Exchange admin center in Exchange Online](../../exchange-admin-center.md).

- To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell).

- You need permissions before you can do this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) article.

- If your organization has configured a group naming policy, it's applied only to groups created by users. When you or other administrators use the EAC to create distribution list groups, the group naming policy is ignored and isn't applied to the group name. However, if you use Exchange Online PowerShell to create or rename a distribution list group, the policy is applied unless you use the _IgnoreNamingPolicy_ parameter to override the group naming policy. For more information, see:

  - [Create a distribution group naming policy](create-group-naming-policy.md)

  - [Override the distribution group naming policy](override-group-naming-policy.md)

## Use the Exchange admin center to manage distribution list groups

### Use the new EAC to create distribution list groups

1. In the [new EAC](https://admin.exchange.microsoft.com), navigate to **Recipients** > **Groups** > **Distribution list**.

2. Click **Add a group** and follow the instructions in the details pane.

   - Under **Choose a group type** section, select **Distribution** and click **Next**.
   
   - Under **Set up the basics** section, enter the details and click **Next**.
   
3. In **Assign owners** section, click **+ Assign owners**, select the group owner from the list, and click **Next**.

4. Under **Add members**, click **+ Add members**, select the group members from the list, and click **Next**.

5. In **Edit settings** section, enter the group email address, select the following boxes and then click **Next**:

   - **Communication**: Select the checkbox to allow people outside of the organization to send email to this distribution list group.
   
   - **Joining the group**: Select who are allowed to join the group.
   
     1. **Open**: Anyone can join this group without owner approval.
   
     2. **Closed**: Only group owners can add members. All requests to join are automatically denied.
   
     3. **Owner approval**: Anyone can request to join this group and owners must approve the request.
   
   - **Leaving the group**: Select who are allowed to leave the group.
   
     1. **Open**: Anyone can leave this group without group owner approval.
   
     2. **Closed**: Only group owners can remove members. All requests to leave are automatically denied.

6. In **Review and finish adding group** section, verify all the details, click **Create group**, and then click **Close**.

### Use the new EAC to modify distribution list groups

1. In the new EAC, navigate to **Recipients** > **Groups** > **Distribution list**.

2. In the list of groups, click the distribution list group that you want to view or change.

3. On the group's properties page, click one of the following sections to view or change properties.

   When you're finished, click **Save**.
   
   #### General

   Use this section to view or change basic information about the group.

   - **Name**: This name appears in the address book, on the **To** line when email is sent to this group, and in the Groups list. The display name is required and should be user-friendly so people recognize what it is. It also has to be unique in your domain.

   - **Description**: Use this box to describe the group so people know what the purpose of the group is. This description appears in the address book and in the Details pane in the new EAC.

   #### Email options

   Use this section to view or change the email addresses associated with the group. This includes the group's primary SMTP addresses and any associated proxy addresses. Under **Edit email addresses** page, change/edit the **Primary email address**, add/delete **Aliases**, and then click **Save changes**. 

   You can also select the group and then click **Edit email address** from the toolbar to change/edit the **Primary email address**, add/delete **Aliases**, and then click **Save changes**.

   #### Members

   Use this section to change/edit the following:

    - Under **Owners** section, click **View all and manage owners** to add/remove group owners from the drop-down list and then click **Save changes**. The distribution list group must have at least one owner. 

    - Under **Members** section, click **View all and manage members** to add/remove group members from the drop-down list and then click **Save changes**. The distribution list group must have at least one member. 

   #### Settings

   Under **General settings** section, select the checkbox **Allow external senders to email this group** if you want to allow the external users to send email to this group.

   #### Delivery management

   Use this section to manage who can send email to this group.

    - **Sender options**

      By default, only people inside your organization can send message to this group. You can also allow people outside the organization to send message to this group.
 
       - **Only allow messages from people inside my organization**: Select this option to allow only senders in your organization to send messages to the group. This means that if someone outside your organization sends an email message to this group, it is rejected. This is the default setting.

       - **Allow messages from people inside and outside my organization**: Select this option to allow anyone to send messages to the group.

    - **Specified senders**

      You can further limit who can send messages to the group by allowing only specific senders to send messages to this group. Select/remove one or more recipients/group from the drop-down list. If you add senders to this list, they are the only ones who can send mail to the group. Mail sent by anyone not in the list will be rejected.

      > [!IMPORTANT]
      > If you've configured the group to allow only senders inside your organization to send messages to the group, email sent from a mail contact is rejected, even if they're added to this list.

   #### Manage delegates

   Use this section to assign permissions to a user (called a delegate) to allow them to send messages as the group or send messages on behalf of the group. You can assign the following permissions:

    - **Send As**: This permission allows the delegate to send messages as the group. After this permission is assigned, the delegate has the option to add the group to the **From** line to indicate that the message was sent by the group.

    - **Send on Behalf**: This permission also allows a delegate to send messages on behalf of the group. After this permission is assigned, the delegate has the option to add the group to the **From** line. The message will appear to be sent by the group and will say that it was sent by the delegate on behalf of the group.

   To assign permissions to delegates in new EAC, add the delegates under the **Edit delegates** page, select the **Permission type** from the drop-down list and click **Save changes**.

   #### Message approval 

   Use this section to set options for moderating the group. Moderators approve or reject messages sent to the group before they reach the group members.

    - **Require moderator approval for messages sent to this group**: This check box isn't selected by default. If you select this check box, incoming messages are reviewed by the group moderators before delivery. Group moderators can approve or reject incoming messages.

    - **Group moderators**: To add/remove group moderators, search/add users from the drop-down list. If you've selected **Require moderator approval for messages sent to this group** and you don't select a moderator, messages to the group are sent to the group owners for approval.

    - **Add senders who don't require message approval**: To add/remove users that can bypass moderation for this group, search/add users from the drop-down list.

    - **Notify a sender if their message isn't approved**: Use this section to set how users are notified about message approval.

      - **Only sender**: This is the default setting. Notify all senders, inside and outside your organization, when their message isn't approved.

      - **Only senders in your organization**: When you select this option, only users or groups in your organization are notified when a message that they sent to the group isn't approved by a moderator.

      - **No notifications**: When you select this option, notifications aren't sent to senders whose messages aren't approved by the group moderators.

   #### Membership approvals

   Use this section to edit membership approvals and to specify if group owner approval is needed for users to join or leave this group.

    - **Joining the group**: View/Edit who are allowed to join the group.
   
      1. **Open**: Anyone can join this group without owner approval.
   
      2. **Closed**: Only group owners can add members. All requests to join are automatically denied.
   
      3. **Owner approval**: Anyone can request to join this group and owners must approve the request.
   
    - **Leaving the group**: View/Edit who are allowed to leave the group.
   
      1. **Open**: Anyone can leave this group without group owner approval.
   
      2. **Closed**: Only group owners can remove members. All requests to leave are automatically denied.

### Use the new EAC to remove distribution list groups

1. In the new EAC, go to **Recipients** \> **Groups**.

2. In the list of groups, select the distribution list group that you want to remove, and then click **Delete group** from the toolbar.

### Use the Classic EAC to create distribution list groups

![New Try Microsoft 365 Groups](../../media/3ea82c95-9dda-450f-823b-cd0772249d81.png)

You can now create a Microsoft 365 group instead of a distribution group, if you have a Microsoft 365 or Office 365 for business plan or an Exchange Online plan. Microsoft 365 groups have the features of a distribution group and much more. With Microsoft 365 groups, you can send email to a group, share a common calendar, and have a library for storing and working on group files and folders. Click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) \> **Microsoft 365 group** to get started and see [Learn about Microsoft 365 Groups](https://support.microsoft.com/office/b565caa1-5c40-40ef-9915-60fdb2d97fa2).

If you have existing distribution groups that you want to migrate to Microsoft 365 groups, see [Upgrade distribution lists to Microsoft 365 Groups in Outlook](/microsoft-365/admin/manage/upgrade-distribution-lists).

If you still want to create distribution list groups, use the following steps:

1. In the Classic EAC, go to **Recipients** \> **Groups**.

2. Click **New** ![New distribution group icon](../../media/ITPro_EAC_NewDGIcon.png), and then select **Distribution group**.

3. In the **New distribution group** page that opens, configure the following settings. Settings marked with an <sup>\*</sup> are required.

   - <sup>\*</sup>**Display name**: This name appears in your organization's address book, on the To: line when email is sent to this group, and in the **Groups** list in the EAC. The display name is required, must be unique, and should be user-friendly so people recognize what it is.

   - <sup>\*</sup>**Alias**: Use this box to type the name of the alias for the group. The alias can't exceed 64 characters and must be unique. When a user types the alias in the To line of an email message, it resolves to the group's display name.

   - <sup>\*</sup>**Email address**: The email address consists of the alias on the left side of the at (@) symbol, and a domain on the right side. By default, the value of **Alias** is used for the alias value, but you can change it. For the domain value, click the drop-down and select and accepted domain in your organization.

   - **Notes**: This description appears in the address book and in the Details pane in the Classic EAC.

   - <sup>\*</sup>**Owners**: A group owner can add members to the group, approve or reject requests to join or leave the group, and approve or reject messages sent to the group. By default, the person who creates a group is the owner. All groups must have at least one owner.

     To add owners, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find, and select a recipient or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

     To remove an owner, select the owner, and then click **Remove** ![Remove](../../media/ITPro_EAC_RemoveIcon.png).

   - **Members**: Add and remove group members and specify whether approval is required for people to join or leave the group.

     Use **Add group owners as members** to add or remove the owners as members (this setting is selected by default).

     To add members, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find, and select a recipient or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

     To remove a member, select the member, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png).

   - **Choose whether owner approval is required to join the group**: Specify whether approval is required for people to join the group. Select one of the following settings:

     - **Open**: Anyone can join this group without being approved by the group owners. This is the default value.

     - **Closed**: Members can be added only by the group owners. All requests to join will be rejected automatically

     - **Owner Approval**: All requests are manually approved or rejected by the group owners. If you select this option, the group owners will receive an email message requesting approval to join the group.

   - **Choose whether the group is open to leave**: Specify whether approval is required for people to leave the group. Select one of the following settings:

     - **Open**: Anyone can leave this group without being approved by the group owners. This is the default value.

     - **Closed**: Members can be removed only by the group owners. All requests to leave will be rejected automatically.
    
   For more information about using Exchange Online PowerShell to create distribution groups, see [New-DistributionGroup](/powershell/module/exchange/new-distributiongroup).

4. When you're finished, click **Save** to create the distribution list group.

   > [!NOTE]
   > - By default, new distribution groups only accept messages from authenticated (internal) senders, and messages from external senders are rejected. To configure a distribution group to accept messages from all senders, you need to modify the [message delivery restriction settings](#delivery-management) for the group.
   > - You can create or mail-enable only universal distribution groups. To convert a domain-local or a global group to a universal group, you can use the [Set-Group](/powershell/module/exchange/set-group) cmdlet using Exchange Online PowerShell. You may have mail-enabled groups that were migrated from previous versions of Exchange that are not universal groups. You can use the Classic EAC or Exchange Online PowerShell to manage these groups

### Use the Classic EAC to modify distribution list groups

1. In the Classic EAC, go to **Recipients** \> **Groups**.

2. In the list of groups, select the distribution list group that you want to modify, and then click **Edit** ![Edit tooltip](../../media/ITPro_EAC_AddIcon.png).

3. On the distribution group properties page that opens, click one of the following tabs to view or change properties.

   When you're finished, click **Save**.

   #### General

   Use this tab to view or change basic information about the group.

    - **Display name**: This name appears in the address book, on the To line when email is sent to this group, and in the **Groups list**. The display name is required and should be user-friendly so people recognize what it is. It also has to be unique in your domain.

       If you've implemented a group naming policy, the display name has to conform to the naming format defined by the policy.

    - **Alias**: This is the portion of the email address that appears to the left of the at (@) symbol. If you change the alias, the primary SMTP address for the group will also be changed, and contain the new alias. Also, the email address with the previous alias will be kept as a proxy address for the group.

    - **Email address**: The email address consists of the alias on the left side of the at (@) symbol, and a domain on the right side. By default, the value of **Alias** is used for the alias value, but you can change it. For the domain value, click the drop-down and select and accepted domain in your organization.

    - **Notes**: This description appears in the address book and in the Details pane in the Classic EAC.

    - **Hide this group from address lists**: Select this check box if you don't want users to see this group in the address book. To send email to this group, a sender has to type the group's alias or email address on the To: or Cc: lines.

   #### Ownership

   Use this tab to assign group owners. The group owner can add members to the group, approve or reject requests to join or leave the group, and approve or reject messages sent to the group. By default, the person who creates a group is the owner. All groups must have at least one owner.

   To add owners, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find and select a recipient, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

   To remove an owner, select the owner, and then click **Remove** ![Remove tooltip](../../media/ITPro_EAC_RemoveIcon.png).

   #### Membership

   Use this tab to add or remove group members. Group owners don't need to be members of the group.

   To add members, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find, and select a recipient or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

   To remove a member, select the member, and then click **Remove** ![Remove image](../../media/ITPro_EAC_RemoveIcon.png).

   #### Membership approval

   Use this tab to specify whether approval is required for users to join or leave the group.

    - **Choose whether owner approval is required to join the group**: Select one of the following settings:

      - **Open**: Anyone can join this group without being approved by the group owners. This is the default value.

      - **Closed**: Members can be added only by the group owners. All requests to join will be rejected automatically

      - **Owner Approval**: All requests are manually approved or rejected by the group owners. If you select this option, the group owners will receive an email message requesting  approval to join the group.

    - **Choose whether the group is open to leave**: Specify whether approval is required for people to leave the group. Select one of the following settings:

      - **Open**: Anyone can leave this group without being approved by the group owners. This is the default value.

      - **Closed**: Members can be removed only by the group owners. All requests to leave will be rejected automatically.

   #### Delivery management

   Use this tab to manage who can send email to this group.

   - **Only senders inside my organization**: Only internal (authenticated) senders are allowed to send messages to this group. Messages from external senders are rejected. This is the default setting.

   - **Senders inside and outside of my organization**: Allow anyone to send messages to the group.

      You can configure the group to accept messages only from specific senders.
  
      To add senders, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find, and select a sender or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

      To remove a sender from the list, select the sender, and then click **Remove** ![Remove image1](../../media/ITPro_EAC_RemoveIcon.png).
  
      > [!IMPORTANT]
      > Mail contacts are always considered external users. So, if you configure the group to only accept messages from internal senders and you add mail contacts to the list of allowed senders, messages from those mail contacts are still rejected.

   #### Message approval

   Use this tab to set options for moderating the group. Moderators approve or reject messages sent to the group before they reach the group members.

   - **Messages sent to this group need to be approved by a moderator**: When you enable moderation for the group, messages sent to the group require approval by a specified moderator or a group owner before the message is delivered to the group members. This setting is disabled by default.

   - **Group moderators**: To add moderators, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find and select a recipient, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

     To remove a moderator, select the moderator, and then click **Remove** ![Remove image2](../../media/ITPro_EAC_RemoveIcon.png).

     If you enable moderation for the group but don't specify any moderators, group owners are responsible for approving messages that are sent to the group.

   - **Senders who don't require message approval**: Messages sent to the group by the specified senders don't require approval from a moderator.

     To add senders, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find, and select a sender or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

     To remove senders, select the senders, and then click **Remove** ![Remove image3](../../media/ITPro_EAC_RemoveIcon.png).

   - **Select moderation notifications**: Configure the sender notification options for message approval:

     - **Notify all senders when their messages aren't approved**: Internal and external senders are notified when their messages aren't approved. This is the default value.

     - **Notify senders in your organization when their messages aren't approved**: Internal senders are notified when their messages aren't approved. External senders aren't notified.

     - **Don't notify anyone when a message isn't approved**: No notification messages are sent.

   #### Email options

   Use the **Email addresses** tab to view or change the email addresses associated with the group. This includes the group's primary SMTP address and any associated proxy addresses. The primary SMTP address (also known as the reply address) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column.

   - **Add**: Click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the **New email address** page that appears, configure the following settings:

     - **Email address type**: Verify **SMTP** is selected.
     
     - **Email address**: Enter the email address to add.
     
     - **Make this the reply address**

     When you're finished, click **OK**.

   - **Edit**: Select the email address that you want to modify, and then click **Edit** ![Edit icon2](../../media/ITPro_EAC_EditIcon.png). In the **Email address** page that appears, configure the following settings:

     - **Email address**: Modify the existing email address.
     
     - **Make this the reply address**: This setting only appears if the email address you selected isn't already the reply address.

     When you're finished, click **OK**.

   - **Remove**: Select the email address that you want to remove, and then click **Remove** ![Remove icon3](../../media/ITPro_EAC_RemoveIcon.gif). You can't remove the reply address.

   #### MailTip

   Use the **MailTip** tab to add an alert for potential issues before a user sends messages to this recipient. The text is displayed in the InfoBar when this recipient is added to the To, Cc, or Bcc lines of a new email message.

   MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

   #### Group delegation

   Use this section to assign permissions to a user (called a delegate) to allow them to send messages as the group or send messages on behalf of the group. You can assign the following permissions:

    - **Send As**: Allows the delegate to send messages as if they came directly from the group. There's no indication that the message was sent by the delegate. After this permission is assigned, the delegate has the option to add the group to the From: line to indicate that the message was sent by the group.

    - **Send on Behalf**: Allows the delegate to send messages from the group. The From address of these messages clearly shows that the message was sent by the delegate ("_\<Delegate\>_ on behalf of _\<Group\>_"). However, replies to these messages are sent to the group, not to the delegate. After this permission is assigned, the delegate has the option to add the group in the From: line.

   To add senders, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find, and select a sender or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

   To remove a sender from the list, select the sender, and then click **Remove** ![Remove icon4](../../media/ITPro_EAC_RemoveIcon.png).

### Use the Classic EAC to remove distribution list groups

1. In the Classic EAC, go to **Recipients** \> **Groups**.

2. In the list of groups, select the distribution list groups that you want to remove, and then click **Remove** ![Remove icon5](../../media/ITPro_EAC_RemoveIcon.gif).

## Use PowerShell to manage distribution list groups

### Use Exchange Online PowerShell to create distribution list groups

This example creates a distribution list group with an alias **itadmin** and the name **IT Administrators**. The distribution group is created in the default OU, and anyone can join this group without approval by the group owners.

```PowerShell
New-DistributionGroup -Name "IT Administrators" -Alias itadmin -MemberJoinRestriction open
```

For detailed syntax and parameter information, see [New-DistributionGroup](/powershell/module/exchange/new-distributiongroup).

### Use Exchange Online PowerShell to modify distribution list groups

Use the **Get-DistributionGroup** and **Set-DistributionGroup** cmdlets to view and change properties for distribution list groups. Advantages of using Exchange Online PowerShell are the ability to change the properties that aren't available in the EAC and to change properties for multiple groups. For information about which parameters correspond to distribution list group properties, see the following articles:

- [Get-DistributionGroup](/powershell/module/exchange/get-distributiongroup)

- [Set-DistributionGroup](/powershell/module/exchange/set-distributiongroup)

Here are some examples of using Exchange Online PowerShell to change distribution group properties.

This example changes the primary SMTP address (also called the reply address) for the Seattle Employees distribution group from employees@contoso.com to sea.employees@contoso.com. Also, the previous reply address will be kept as a proxy address.

```PowerShell
Set-DistributionGroup "Seattle Employees" -EmailAddresses SMTP:sea.employees@contoso.com,smtp:employees@contoso.com
```

This example enables moderation for the distribution group Customer Support and sets the moderator to Amy. In addition, this moderated distribution group will notify senders who send mail from within the organization if their messages aren't approved.

```PowerShell
Set-DistributionGroup -Identity "Customer Support" -ModeratedBy "Amy" -ModerationEnabled $true -SendModerationNotifications 'Internal'
```

This example changes the user-created distribution group Dog Lovers to require the group manager to approve users' requests to join the group. In addition, by using the _BypassSecurityGroupManagerCheck_ parameter, the group manager will not be notified that a change was made to the distribution list group's settings.

```PowerShell
Set-DistributionGroup -Identity "Dog Lovers" -MemberJoinRestriction 'ApprovalRequired' -BypassSecurityGroupManagerCheck
```

## How do you know these procedures worked?

To verify that you've successfully created, modified, or removed a distribution list group, do any of the following steps:

- In the new EAC, select the group to view the property or feature that you changed. Depending on the property that you changed, it might be displayed in the details pane for the selected group.

- In the Classic EAC, go to **Recipients** \> **Groups**. Verify that the group is listed (or not listed). The **Group Type** is **Distribution group**. Select the group and click **Edit** ![Edit icon6](../../media/ITPro_EAC_EditIcon.png) to verify the property settings.

- In Exchange Online PowerShell, replace \<GroupIdentity\> with the name, alias, or email address of the distribution list group, and run the following command to verify the settings:

  ```powershell
  Get-DistributionGroup -Identity "<GroupIdentity>" | Format-List
  ```

  To view specific properties, run the following command:

  ```PowerShell
  Get-DistributionGroup -Identity "<GroupIdentity>" | Format-List Name,PrimarySmtpAddress
  ```

- To get a list of members in the group, replace \<GroupIdentity\> with the name, alias, or email address of the distribution list group, and run the following command:

  ```PowerShell
  Get-DistributionGroupMember -Identity "<GroupIdentity>"
  ```

  For detailed syntax and parameter information, see [Get-DistributionGroupMember](/powershell/module/exchange/get-distributiongroupmember).
