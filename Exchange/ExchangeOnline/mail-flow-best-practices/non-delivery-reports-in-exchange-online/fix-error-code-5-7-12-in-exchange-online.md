---
title: "Fix email delivery issues for error code 5.7.12 in Exchange Online"
ms.author: jhendr
author: msdmaguire
manager: serdars
ms.reviewer: 
audience: Admin
ms.topic: troubleshooting
ms.service: o365-administration
localization_priority: Normal
f1.keywords:
- CSH
ms.custom: MiniMaven
search.appverid:
- BCS160
- MOE150
- MET150
ms.assetid: 08793285-463d-4198-9626-60794835a3b5
description: "Learn how to fix email issues for error code 5.7.12 in Exchange Online (the recipient rejects messages from external or unauthenticated senders)."
---

# Fix email delivery issues for error code 5.7.12 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see status code 550 5.7.12 or 5.7.12 in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN). You'll see this automated notification when the recipient is configured to reject messages that are sent from outside of its organization.

|Icon|Message|Icon|Message|
|---|---|---|---|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do I fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How do I fix this issue?](#im-an-email-admin-how-do-i-fix-this-issue)|
|

## I got this bounce message. How do I fix it?

 Only an email admin in the recipient's organization can fix this issue. Contact the email admin and refer them to this information so they can try to resolve the issue for you.

## I'm an email admin. How do I fix this issue?

The two methods that will allow an external sender to send messages to the recipient in your organization are described in the following sections.

To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md).

### Method 1: Allow all internal and external senders to send messages to this recipient

Open the EAC and use one of the following procedures based on the recipient type.

#### User mailboxes

##### New EAC

1. Go to **Recipients** \> **Mailboxes**.

2. Select a mailbox from the list and click it. The mailbox properties screen appears.

3. Under **Mail flow settings**, click **Manage mail flow settings**. The **Manage mail flow settings** screen appears.

4. In the **Message delivery restriction** pane, click **Edit**. The **Message delivery restrictions** screen appears.

5. Clear the check box for **Require senders to be authenticated** in the **Accept messages from** section.

6. Click **Save**.

##### Classic EAC**

1. Go to **Recipients** \> **Mailboxes**.

2. Select the mailbox from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif). The mailbox properties screen appears.

3. Go to **Mailbox features** \> **Message Delivery Restrictions** \> and then click **View details**. The **message delivery restrictions** screen appears.

4. Clear the check box for **Require that all senders are authenticated** in the **Accept messages from** section.

5. Click **OK**, and then click **Save**.

#### Groups (distribution groups, mail-enabled security groups, and dynamic distribution groups)

##### New EAC

1. Go to **Recipients** \> **Groups**.

2. Select the group from the list and click it. The group properties screen appears.

3. Click the **Settings** tab. The group settings screen appears.

4. Under the **Delivery management** pane, click **Edit delivery management**. The **Delivery management** screen appears.

5. Choose the radio button for **Allow messages from people inside and outside my organization**.

6. Click **Save changes**.

##### Classic EAC

1. Go to **Recipients** \> **Groups** \> select the group from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif). The group properties screen appears.

2. On the left pane, click **Delivery management**.
    
3. Click the radio button for **Senders inside and outside of my organization**.

4. Click **Save**.

#### Mail users

> [!IMPORTANT]
> Currently, editing mail flow settings for a mail user is available only in the Classic EAC.

1. Go to **Recipients** \> **Contacts**.

2. Select the mail user from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif). The mail user properties screen appears.

3. Go to **Mailbox flow settings** \> **Message Delivery Restrictions** and click **View details**. The **Message delivery restrictions** screen appears.

4. Clear the check box for **Require that all senders are authenticated** in the **Accept messages from** section.

5. Click **OK**, and then click **Save**.

#### Shared mailboxes

##### New EAC

1. Go to **Recipients** \> **Mailboxes**.

2. Select a shared mailbox from the list and click it. The mailbox properties screen appears.

3. Under **Mail flow settings**, click **Manage mail flow settings**. The **Manage mail flow settings** screen appears.

4. In the **Message delivery restriction** pane, click **Edit**. The **message delivery restrictions** screen appears.

5. Clear the check box for **Require senders to be authenticated** in the **Accept messages from** section.

6. Click **Save**.

##### Classic EAC

1. Go to **Recipients** \> **Mailboxes**.

2. Select a shared mailbox from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif). The shared mailbox properties screen appears.

3. Go to **Mailbox features** \> **Message Delivery Restrictions** \> and then click **View details**. The **Message delivery restrictions** screen appears.

4. Clear the check box for **Require that all senders are authenticated** in the **Accept messages from** section.

5. Click **OK**, and then click **Save**.

### Method 2: Use the recipient's allowed senders list

Instead of allowing all external senders to send messages to this recipient, you can use the recipient's allowed senders list to selectively allow messages from all internal senders and the specified external senders.

**Notes**:

- To add an external sender to a recipient's allowed senders list, you must first create a [mail contact](../../recipients-in-exchange-online/manage-mail-contacts.md) or a [mail user](../../recipients-in-exchange-online/manage-mail-users.md) to represent the external sender in your organization.

- To add everyone in your organization to a recipient's allowed sender's list, you can create a [distribution group](../../recipients-in-exchange-online/manage-distribution-groups/manage-distribution-groups.md) or a [dynamic distribution group](../../recipients-in-exchange-online/manage-dynamic-distribution-groups/manage-dynamic-distribution-groups.md) that contains everyone in your organization. After you create this group, you can add it to the recipient's allowed senders list.

- The recipient's allowed senders list is different from the organization's allowed senders list for anti-spam that you manage in the EAC at **Protection** \> **Spam filter**.

To configure the recipient's allowed senders list, open the EAC and use one of the following procedures based on the recipient type.

#### User mailboxes

##### New EAC

1. Go to **Recipients** \> **Mailboxes**.

2. Select a user mailbox from the list and click it. The mailbox properties screen appears.

3. Under **Mail flow settings**, click **Manage mail flow settings**. The **Manage mail flow settings** screen appears.

4. In the **Message delivery restriction** pane, click **Edit**. The **Message delivery restrictions** screen appears.

5. Configure the following settings under the **Accept messages from** section:
   - Clear the check box for **Require senders to be authenticated**.
   - Choose the radio button for **Selected senders**.
   - Click **+ Add sender**.
       
     :::image type="content" source="../../media/specifying-senders.png" alt-text="The screen on which specific external senders can be set":::

   - In the **Accept messages from** screen, select the external senders and the "all internal users" group.
   - Add the external senders and the "all internal users" group to the list of the allowed senders of the recipient.
       
     :::image type="content" source="../../media/confirming-the-specified-users.png" alt-text="The screen on which the specific senders are chosen":::

6. When you're finished, click **Confirm**.

7. Click **Save**. 
   
##### Classic EAC

1. Go to **Recipients** \> **Mailboxes**.

2. Select the mailbox from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif). The mailbox properties screen appears.

3. Go to **Mailbox features** \> **Message Delivery Restrictions** \> and then click **View details**. The **message delivery restrictions** screen appears.

4. Configure the following settings in the **Accept messages from** section:
   - Clear the check box for **Require that all senders are authenticated**.
   - Select **Only senders in the following list**, and then click **Add** ![Add icon](../../media/8ee52980-254b-440b-99a2-18d068de62d3.gif). In the **Select Members** dialog box that opens, select external senders and the "all internal users" group. 
   - Add the external senders and the "all internal users" group to the list of the allowed senders of the recipient.
   - When you're finished, click **OK**.

     ![Add an allowed sender in the Admin center to help solve DSN 5.7.136](../../media/7306dda2-69dc-4d47-9d40-0fffaea881d6.png)

5. Click **OK**, and then click **Save**.

#### Groups (distribution groups, mail-enabled security groups, and dynamic distribution groups

##### New EAC

1. Go to **Recipients** \> **Groups**.

2. Select a group from the list and click it. The group properties screen appears.

3. Click the **Settings** tab. The group settings screen appears.

4. In the **Delivery management** pane, click **Edit delivery management**. The **Delivery management** screen appears.

5. Choose the radio button for **Allow messages from people inside and outside my organization**.

6. Under **Specific senders**, select the external senders and the "all internal users" group from the text box.

7. Add the external senders and the "all internal users" group to the list of allowed senders for the recipient.

8.  When you're finished, click **Save changes**.

##### Classic EAC

1. Go to **Recipients** \> **Groups**.

2. Select the group from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif). The group properties screen appears.

3. Go to **Delivery management** and configure the following settings:
   - Select **Senders inside and outside of my organization**.
   - Click **Add** ![Add icon](../../media/8ee52980-254b-440b-99a2-18d068de62d3.gif). In the **Select Allowed Senders** dialog box that opens, select the external senders and the "all internal users" group. When you're finished, click **OK**.
   - Add the external senders and the "all internal users" group to the list of the allowed senders of the recipient.

4. Click **Save**.

#### Mail users

> [!IMPORTANT]
> Currently, the editing mail flow settings for a mail user is available only in the Classic EAC.

1. Go to **Recipients** \> **Contacts**.

2. Select a mail user from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif). The mail user properties screen appears.

3. Go to **Mailbox flow settings** and then click **View details** in the **Message Delivery Restrictions** section. The **Message delivery restrictions** screen appears.

4. Configure the following settings in the **Accept messages from** section:
   - Clear the check box for **Require that all senders are authenticated**.
   - Select **Only senders in the following list**, and then click **Add** ![Add icon](../../media/8ee52980-254b-440b-99a2-18d068de62d3.gif). In the **Select Members** dialog box that opens, select the external senders and the "all internal users" group. 
   - When you're finished, click **OK**.

5. Click **OK**, and then click **Save**.

#### Shared mailboxes

##### New EAC

1. Go to **Recipients** \> **Mailboxes**.

2. Select a shared mailbox from the list and click it. The mailbox properties screen appears.

3. Under **Mail flow settings**, click **Manage mail flow settings**. The **Manage mail flow settings** screen appears.

4. In the **Message delivery restriction** pane, click **Edit**. The **message delivery restrictions** screen appears.

5. Configure the following settings under the **Accept messages from** section:
   - Clear the check box for **Require all senders to be authenticated**.
   - Choose the radio button for **Selected senders**.
   - Click **+ Add sender**
   - In the **Accept messages from** screen, select the external senders and the "all internal users" group.
   - Add the external senders and the "all internal users" group to the list of allowed senders for the recipient.

6. When you're finished, click **Confirm**.
    
7. Click **Save**.

##### Classic EAC

1. Go to **Recipients** \> **Shared**.

2. Select the shared mailbox from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif). The shared mailbox properties screen appears.

3. Go to **Mailbox features** \> **Message Delivery Restrictions** \> and then click **View details**. The **Message delivery restrictions** screen appears.

4. Configure the following settings in the **Accept messages from** section:
  - Clear the check box for **Require that all senders are authenticated**.
  - Select **Only senders in the following list**, and then click **Add** ![Add icon](../../media/8ee52980-254b-440b-99a2-18d068de62d3.gif). In the **Select Members** dialog box that opens, select the external senders and the "all internal users" group. When you're finished, click **OK**.
  - Add the external senders and the "all internal users" group to the list of allowed senders for the recipient.

5. Click **OK**, and then click **Save**.

## Still need help with error code 550 5.7.12?

[![Get help from the community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://answers.microsoft.com/)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support)

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](/microsoft-365/Admin/contact-support-for-business-products)

## See also    

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)
