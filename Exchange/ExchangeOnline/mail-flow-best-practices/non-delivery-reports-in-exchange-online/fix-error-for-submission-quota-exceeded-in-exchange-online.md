---
title: "Fix 'sender's submission quota exceeded' errors in Exchange Online"
ms.author: jhendr
author: msdmaguire
manager: serdars
ms.reviewer: 
audience: Admin
ms.topic: troubleshooting
ms.service: o365-administration
localization_priority: Priority
f1.keywords:
- CSH
ms.custom: MiniMaven
search.appverid:
- BCS160
- MOE150
- MET150
ms.assetid:
description: "Learn how to fix email issues for the error, 'the sender's submission quota was exceeded' in Exchange Online."
---

# Fix email delivery issues for error 'the sender's submission quota was exceeded' in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see the error:

> The message can't be submitted because the sender's submission quota was exceeded.

in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN).

## Why did I get this bounce message?

You received this NDR because you have exceeded the recipient rate limit (10,000 recipients per day).

|||||
|---|---|---|---|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do I fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How can I fix this?](#im-an-email-admin-how-do-i-fix-this)|
|

## I got this bounce message. How do I fix it?

If you knowingly sent 10,000 messages in the last 24 hours, you need to wait one day before you can send email from your mailbox.

If you didn't send the messages and you suspect your account has been compromised, reset your password and scan your devices for malware. However, the attacker might have configured other settings on your mailbox (for example, Inbox rules to forward messages or additional mailbox delegates). So, follow the steps in [How to determine whether your Office 365 account has been compromised](/office365/troubleshoot/sign-In/determine-account-is-compromised).

## I'm an email admin. How do I fix this?

More information about sending and receiving limits in Exchange Online is available at [Receiving and sending limits](/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits#receiving-and-sending-limits).

The sending account might be compromised. You'll need to:

- [Determine if the account is compromised](/office365/troubleshoot/sign-In/determine-account-is-compromised). If the account is compromised, follow the steps in [Responding to a Compromised Email Account in Exchange Online](/microsoft-365/security/office-365-security/responding-to-a-compromised-email-account).

- To help prevent future account compromises, follow the recommendations in [Top 10 ways to secure Microsoft 365 for business plans](/microsoft-365/admin/security-and-compliance/secure-your-business-data).

## Still need help?

[![Get help from the community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://answers.microsoft.com/)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support)

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](/microsoft-365/Admin/contact-support-for-business-products)