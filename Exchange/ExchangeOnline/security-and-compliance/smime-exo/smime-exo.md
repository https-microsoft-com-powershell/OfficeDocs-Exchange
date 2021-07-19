---
localization_priority: Normal
description: 'Admins can learn about how S/MIME works in Exchange Online for message encryption and digitally signed messages.'
ms.topic: article
author: msdmaguire
ms.author: jhendr
title: S/MIME in Exchange Online
ms.reviewer: smithre4
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# S/MIME for message signing and encryption in Exchange Online

S/MIME (Secure/Multipurpose internet Mail Extensions) is a widely accepted protocol for sending digitally signed and encrypted messages. S/MIME in Exchange Online provides the following services for email messages:

- **Encryption**: Protects the content of email messages.
- **Digital signatures**: Verifies the identity of the sender of an email message.

The rest of this article generally describes S/MIME and how these services work.

To configure S/MIME in Exchange Online, see the following topics:

[Configure S/MIME in Exchange Online](configure-smime-exo.md)

[S/MIME for Outlook for iOS and Android](../../clients-and-mobile-in-exchange-online/outlook-for-ios-and-android/smime-outlook-for-ios-and-android.md)

## S/MIME digital signatures

Digital signatures are the more commonly used service of S/MIME. As the name suggests, digital signatures are the digital counterpart to the traditional, legal signature on a paper document. As with a legal signature, digital signatures provide the following security capabilities:

- **Authentication**: A signature serves to validate an identity. It verifies the answer to "who are you" by providing a means of differentiating that entity from all others and proving its uniqueness. Because there is no authentication in SMTP email, there is no way to know who sent a message. Authentication in a digital signature solves this problem by allowing a recipient to know that a message was sent by the person or organization who claims to have sent the message.

- **Nonrepudiation**: The uniqueness of a signature prevents the owner of the signature from disowning the signature. This capability is called nonrepudiation. Thus, the authentication that a signature provides gives the means to enforce nonrepudiation. The concept of nonrepudiation is most familiar in the context of paper contracts: a signed contract is a legally binding document, and it is impossible to disown an authenticated signature. Digital signatures provide the same function and, increasingly in some areas, are recognized as legally binding, similar to a signature on paper. Because SMTP email does not provide a means of authentication, it cannot provide nonrepudiation. It is easy for a sender to disavow ownership of an SMTP email message.

- **Data integrity**: An additional security service that digital signatures provide is data integrity. Data integrity is a result of the specific operations that make digital signatures possible. With data integrity services, when the recipient of a digitally signed email message validates the digital signature, the recipient is assured that the email message that is received is, in fact, the same message that was signed and sent, and has not been altered while in transit. Any alteration of the message while in transit after it has been signed invalidates the signature. In this way, digital signatures provide an assurance that signatures on paper cannot, because it is possible for a paper document to be altered after it has been signed.

> [!IMPORTANT]
> Although digital signatures provide data integrity, they don't provide confidentiality. Messages with only a digital signature are sent in clear text, like SMTP messages and can be read by others. In the case where the message is opaque-signed, a level of obfuscation is achieved because the message is base64-encoded, but it is still clear text. To protect the contents of email messages, encryption must be used.

## S/MIME encryption

Message encryption provides a solution to information disclosure. SMTP-based internet email does not secure messages. An SMTP internet email message can be read by anyone who sees it as it travels or views it where it is stored. These problems are addressed by S/MIME using encryption. Encryption is a way to change information so that it cannot be read or understood until it is changed back into a readable and understandable form. Message encryption provides two specific security services:

- **Confidentiality**: Message encryption serves to protect the contents of an email message. Only the intended recipient can view the contents, and the contents remain confidential and cannot be known by anyone else who might receive or view the message. Encryption provides confidentiality while the message is in transit and in storage.

- **Data** **integrity:** As with digital signatures, message encryption provides data integrity services as a result of the specific operations that make encryption possible.

> [!IMPORTANT]
> Although message encryption provides confidentiality, it doesn't authenticate the message sender in any way. An unsigned, encrypted message is as susceptible to sender impersonation as a message that isn't encrypted. Because nonrepudiation is a direct result of authentication, message encryption also doesn't provide nonrepudiation. Although encryption does provide data integrity, an encrypted message can show only that the message hasn't been altered since it was sent. No information about who sent the message is provided. To prove the identity of the sender, the message must use a digital signature.

## Related message encryption technologies

Other encryption technologies work together to provide protection for messages at rest and in-transit. S/MIME can work simultaneously with the technologies in the following list, but is not dependent on them:

- **Transport Layer Security (TLS) which replaces Secure Sockets Layer (SSL)**:
  - Encrypts the tunnel or the route between email servers in order to help prevent snooping and eavesdropping.
  - Encrypts the connection between email clients and email servers.
- **BitLocker**: Encrypts data on hard drives in client computers and servers. If an unauthorized party somehow gains access, they can't read the data on the drives.

[Office 365 Message Encryption](/microsoft-365/compliance/email-encryption) is a direct competitor to S/MIME, and has the following advantages over S/MIME:

- It's a policy-based encryption service that's configured by an admin to encrypt messages that are sent to anyone inside or outside of the organization. In contrast, users are required to decide whether to apply or not apply S/MIME to messages that they send.
- It's an online service that's built on Azure Rights Management (Azure RMS) and does not rely on a public key infrastructure. In contrast, S/MIME requires a certificate and certificate publishing infrastructure.
- Office 365 Message Encryption provides additional capabilities. For example, you can customize messages with your organization's brand.
