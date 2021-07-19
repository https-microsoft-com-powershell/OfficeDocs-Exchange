---
title: "Fix email delivery issues for error code 5.7.133 in Exchange Online"
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
ms.assetid: 991abc19-7756-438f-abcb-39f69b80f284
description: "Learn how to fix email issues for error code 5.7.133 in Exchange Online (the group recipient is configured to reject messages from external or unauthenticated senders)."
---

# Fix email delivery issues for error code 5.7.133 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see error code 550 5.7.133 in a non-delivery report also known as an NDR, bounce message, delivery status notification, or DSN). You'll see this automated notification when the recipient is a group that's configured to reject messages from external senders (senders from outside the organization).

|Icon|Message|Icon|Message|
|---|---|---|---|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do I fix this issue?](#i-got-this-bounce-message-how-do-i-fix-this-issue)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm the group owner or email admin. How do I fix this issue?](#im-the-group-owner-or-email-admin-how-do-i-fix-this-issue)|
|

## I got this bounce message. How do I fix this issue?

Only the group owner or an email admin in the recipient's organization can fix this issue. Contact the group owner or email admin and refer them to this information so they can try to resolve the issue for you.

## I'm the group owner or email admin. How do I fix this issue?

The two methods that will allow an external sender to send messages to the distribution group in your organization are described in the following sections.

To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md).

### Method 1: Allow all internal and external senders to send messages to this group

#### New Exchange admin center (EAC)

1. Go to **Recipients** \> **Groups**.

2. Select a group from the list and click it. The group properties screen appears.

   :::image type="content" source="../../media/group-properties.png" alt-text="The screen displaying the properties of the chosen group":::

3. Click the **Settings** tab.

   :::image type="content" source="../../media/group-screen.png" alt-text="The screen on which group details are displayed":::

4. Under **Delivery management**, click **Edit delivery management**. The **Delivery management** screen appears.
 
5. Under **Sender options**, choose **Allow messages from people inside and outside my organization**.
 
   :::image type="content" source="../../media/adding-all-senders-to-group.png" alt-text="The screen on which all the senders are added to the senders group":::

6. Click **Save changes**.

#### Classic EAC

1. In the EAC, go to **Recipients** \> **Groups** \> select the group from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif).

   ![Finding groups in the Exchange admin center](../../media/8b57ae07-1a2c-4cb7-94a7-70f898be6276.png)

2. In the group properties dialog box that opens, go to **Delivery management** \> select **Senders inside and outside of my organization**.

   ![Use the Exchange admin center to solve DSN 5.7.133 and allow senders](../../media/7223438f-9f43-4601-a457-2fa7dfc977cd.png)

3. Click **Save**.

### Method 2: Use the group's allowed senders list

Instead of allowing all external senders to send messages to this group, you can use the group's allowed senders list to selectively allow messages from all internal senders and the specified external senders.

**Notes**:

- To add an external sender to a group's allowed senders list, you must first create a [mail contact](../../recipients-in-exchange-online/manage-mail-contacts.md) or a [mail user](../../recipients-in-exchange-online/manage-mail-users.md) to represent the external sender in your organization.

- To add everyone in your organization to a group's allowed sender's list, you can create a [distribution group](../../recipients-in-exchange-online/manage-distribution-groups/manage-distribution-groups.md) or a [dynamic distribution group](../../recipients-in-exchange-online/manage-dynamic-distribution-groups/manage-dynamic-distribution-groups.md) that contains everyone in your organization. After you create this group, you can add it to the group's allowed senders list.

- The group's allowed senders list is different from the organization's allowed senders list for anti-spam that you manage in the EAC at **Protection** \> **Spam filter**.

To configure the group's allowed senders list, perform the following steps:

#### New EAC

1. Go to **Recipients** \> **Groups**.

2. Select a group from the list and click it. The group properties screen appears.

   :::image type="content" source="../../media/group-properties.png" alt-text="The screen displaying the properties of the chosen group":::

3. Click the **Settings** tab.

4. Under **Delivery management**, click **Edit delivery management**. The **Delivery management** screen appears.

5. Under **Sender options**, choose **Allow messages from people inside and outside my organization**.

6. Under **Specified senders**, click inside the text box. The list of senders (internal and external) is displayed.

   :::image type="content" source="../../media/list-of-senders.png" alt-text="The screen that lists the internal and external senders who can be added to the senders list":::

7. Choose the senders you want to add to the senders list, and click **Save changes**.

   :::image type="content" source="../../media/choosing-profile-of-a-sender.png" alt-text="The screen on which a chosen sender is added to the senders group":::

#### Classic EAC

1. Go to **Recipients** \> **Groups** \> select the group from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif).

2. In the group properties dialog box that opens, go to **Delivery management** and configure the following settings:
   - Select **Senders inside and outside of my organization**.
   - Click **Add** ![Add icon](../../media/8ee52980-254b-440b-99a2-18d068de62d3.gif). In the **Select Allowed Senders** dialog box that opens, select the external senders and the "all internal users" group. 
   - Add the external senders and the "all internal users" group to the allowed sender's list.
   - When you're finished, click **OK**.

     ![Add allowed external sender to a distribution group to help solve NDR 5.7.133](../../media/c736b5ad-39f0-4c7e-ba74-12518c61814f.png)

3. Click **Save**.

## Still need help with error code 550 5.7.133?

[![Get help from the community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://answers.microsoft.com/)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support)

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](/microsoft-365/Admin/contact-support-for-business-products)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)

[Create and manage distribution groups in Exchange Online](../../recipients-in-exchange-online/manage-distribution-groups/manage-distribution-groups.md).
