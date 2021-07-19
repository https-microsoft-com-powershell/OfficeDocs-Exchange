---
title: "Fix email delivery issues for error code 5.7.136 in Exchange Online"
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
ms.assetid: f171e842-9fdb-4e68-8f3d-53fcd9e43c62
description: "Learn how to fix email issues for error code 5.7.136 in Exchange Online (the mail user recipient is configured to reject messages from external or unauthenticated senders)."
---

# Fix email delivery issues for error code 5.7.136 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see error code 550 5.7.136 in a non-delivery report, also known as an NDR, bounce message, delivery status notification, or DSN. You'll see this automated notification when the recipient is a mail user that's configured to reject messages from external senders, that is, senders from outside the organization.

|Icon|Message|Icon|Message|
|---|---|---|---|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do I fix this issue?](#i-got-this-bounce-message-how-do-i-fix-this-issue)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How do I fix this issue?](#im-an-email-admin-how-do-i-fix-this-issue)|
|

## I got this bounce message. How do I fix this issue?

Only an email admin in the recipient's organization can fix this issue. Contact the email admin and refer them to this information so they can try to resolve the issue for you.

## I'm an email admin. How do I fix this issue?

The two methods that will allow an external sender to send messages to the mail user in your organization are described in the following sections. These two methods can be implemented using the Classic EAC.

> [!NOTE]
> Currently, there is no support for the two methods in the New EAC.

To open the Classic EAC, click **Classic Exchange admin center** on the left pane of the home screen of the **New EAC**.

:::image type="content" source="../../media/navigation-to-classic-eac.png" alt-text="The interface from which the user can go to Classic EAC":::.

### Method 1: Allow all internal and external senders to send messages to this mail user

1. Go to **Recipients** \> **Contacts** > select the mail user from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif).

   ![View contacts to help fix DSN 5.7.136](../../media/de84fb82-f697-443b-87f6-b0621dcf8a44.png)

The mail user properties dialog box opens.

2. Go to **Mailbox flow settings** and then click **View details** in the **Message Delivery Restrictions** section.

The **Message delivery restrictions** dialog box opens.

3. Configure the following settings in the **Accept messages from** section:
   - Clear the check box for **Require that all senders are authenticated**.
   - Select **All senders**.

4. Click **OK**, and then click **Save**.

### Method 2: Use the mail user's allowed senders list

Instead of allowing all external senders to send messages to this mail user, you can use the mail user's allowed senders list to selectively allow messages from all internal senders and the specified external senders.

**Notes**:

- To add an external sender to a mail user's allowed senders list, you must first create a [mail contact](../../recipients-in-exchange-online/manage-mail-contacts.md) or a [mail user](../../recipients-in-exchange-online/manage-mail-users.md) to represent the external sender in your organization.

- To add everyone in your organization to a mail user's allowed sender's list, you can create a [distribution group](../../recipients-in-exchange-online/manage-distribution-groups/manage-distribution-groups.md) or a [dynamic distribution group](../../recipients-in-exchange-online/manage-dynamic-distribution-groups/manage-dynamic-distribution-groups.md) that contains everyone in your organization. After you create this group, you can add it to the mail user's allowed senders list.

- The mail user's allowed senders list is different from the organization's allowed senders list for anti-spam that you manage in the EAC at **Protection** \> **Spam filter**.

To configure the mail user's allowed senders list, open the Classic EAC do the following steps:

1. Go to **Recipients** \> **Contacts** > select the mail user from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif).

   ![View contacts to help fix DSN 5.7.136](../../media/de84fb82-f697-443b-87f6-b0621dcf8a44.png)

The mail user properties dialog box opens.

2. Go to **Mailbox flow settings** and then click **View details** in the **Message Delivery Restrictions** section.

The **Message delivery restrictions** dialog box opens.

3. Configure the following settings in the **Accept messages from** section:
   - Clear the check box for **Require that all senders are authenticated**.
   - Select **Only senders in the following list**, and then click **Add** ![Add icon](../../media/8ee52980-254b-440b-99a2-18d068de62d3.gif). In the **Select Members** dialog box that opens, select the external senders and the "all internal users" group. 
   - Add the external senders and the "all internal users" group to the allowed senders list.
   - When you're finished, click **OK**.

     ![Add an allowed sender in the Admin center to help solve DSN 5.7.136](../../media/7306dda2-69dc-4d47-9d40-0fffaea881d6.png)

4. Click **OK**, and then click **Save**.

## Still need help with error code 550 5.7.136?

[![Get help from the Office 365 community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://answers.microsoft.com/)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support)

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](/microsoft-365/Admin/contact-support-for-business-products)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)
