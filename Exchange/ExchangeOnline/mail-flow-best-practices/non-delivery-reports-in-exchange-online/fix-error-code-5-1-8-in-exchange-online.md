---
title: "Fix email delivery issues for error code 5.1.8 in Exchange Online"
ms.author: jhendr
author: msdmaguire
manager: serdars
ms.reviewer: 
audience: Admin
ms.topic: article
ms.prod: office-online-server
localization_priority: Normal
f1.keywords:
- CSH
ms.custom: MiniMaven
search.appverid:
- BCS160
- MOE150
- MET150
ms.assetid: 303238b8-658d-46b6-8f45-a789acd2173b
description: "Learn how to fix email issues for error code 5.1.8 in Exchange Online (the account has been blocked for sending too much spam)."
---

# Fix email delivery issues for error code 5.1.8 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see error code 550 5.1.8 in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN).

|||||
|---|---|---|---|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do I fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How can I fix this?](#im-an-email-admin-how-do-i-fix-this)|
|

## Why did I get this bounce message?

You received this NDR with error code 5.1.8 because your account has been blocked for sending too much spam. Typically, this problem occurs because your account has been compromised (hacked) by phishing or malware.

## I got this bounce message. How do I fix it?

First, you need to reset your password and scan your devices for malware. However, the hacker might have configured other settings on your mailbox (for example, created Inbox rules to auto-forward email messages or added additional mailbox delegates). So, follow the additional steps in [How to determine whether your account has been compromised](/office365/troubleshoot/sign-In/determine-account-is-compromised).

Then, you need to tell your email admin that you think your account has been compromised. Your admin will need to unblock your account before you can send email again.

## I'm an email admin. How do I fix this?

The sending account might be compromised. You'll need to:

- [Determine if the account is compromised](/office365/troubleshoot/sign-In/determine-account-is-compromised). If the account is compromised, follow the steps in [Responding to a Compromised Email Account in Exchange Online](/microsoft-365/security/office-365-security/responding-to-a-compromised-email-account).

- Go to the **Restricted users** page in the Microsoft 365 Defender portal at <https://security.microsoft.com/restrictedusers> to unblock the account. After you unblock the account, the user should be able to resume sending messages *within a few hours*.

- To help prevent future account compromises, follow the recommendations in [Top 10 ways to secure Microsoft 365 for business plans](/microsoft-365/admin/security-and-compliance/secure-your-business-data).

## Still need help with error code 5.1.8?

[![Get help from the community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://answers.microsoft.com/)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support)

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](/microsoft-365/Admin/contact-support-for-business-products)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)