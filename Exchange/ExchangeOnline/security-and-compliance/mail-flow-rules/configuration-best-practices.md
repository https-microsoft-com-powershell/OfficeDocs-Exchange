---
localization_priority: Normal
description: Learn about the recommendations for mail flow rules in Exchange Online.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: abd863c3-c0ce-42f3-9470-a573adc3cbba
ms.reviewer: 
f1.keywords:
- NOCSH
title: Best practices for configuring mail flow rules in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Best practices for configuring mail flow rules in Exchange Online

In Exchange Online organizations or standalone Exchange Online Protection (EOP) organizations without Exchange Online mailboxes, follow these best practice recommendations for mail flow rules (also known as transport rules) in order to avoid common configuration errors. Each recommendation links to a article with an example and step-by-step instructions.

## Test your rules

To make sure unexpected things don't happen to email messages, and to make sure you're really meeting the business, legal, or compliance intentions of your rule, be sure to test it thoroughly. There are many options, and rules can interact with each other, so it's important to test messages that you expect both will match the rule and won't match the rule in case you inadvertently made a rule too general. To learn all the options for testing rules, see [Test mail flow rules in Exchange Online](test-mail-flow-rules.md).

## Scope your rule

Make sure your rule applies only to the messages you intend it to. For example:

- **Restrict a rule to messages either coming into or going out of the organization**: By default, a new rule applies to messages that are sent by **and** received by people in your organization. So if you want the rule to apply only one way, be sure to specify that in the conditions for the rule. For examples, see [Use mail flow rules for attachment blocking scenarios in Exchange Online](common-attachment-blocking-scenarios.md)
- **Restrict a rule based on the sender's or receiver's domain**: By default, a new rule applies to messages sent from or received by any domain. Sometimes you want a rule to apply to all domains except for one, or to just one domain. See [Create blocked sender lists in EOP](/microsoft-365/security/office-365-security/create-block-sender-lists-in-office-365).

For a complete list of all the conditions and exceptions that are available for mail flow rules, see [Mail flow rule conditions and exceptions (predicates) in Exchange Online](conditions-and-exceptions.md).

## Know when you need two rules

Sometimes it takes two rules to do what you want. Mail flow rules are processed in order, so multiple rules can apply to the same message. For example, if one of the actions is to block the message, and you also have another action you'd like to apply, such as copying the message to the sender's manager or changing the subject for the notification message, you would need two rules. The first rule could copy the message to the sender's manager and change the subject, and the second rule could block the message.

If you use two rules like this, be sure that the conditions are identical. For example:

- [Set up a message approval chain](common-message-approval-scenarios.md#set-up-a-message-approval-chain)
- [Modify the subject line for notifications](common-attachment-blocking-scenarios.md#example-3-modify-the-subject-line-for-notifications)

## Don't repeat an action on every email in a conversation

The chain of email in a conversation can include many individual messages, and repeating the action on each message in the thread might get annoying. For example, if you have an action such as adding a disclaimer, you might want it to apply only to the first message in the thread. If so, add an exception for messages that already include the disclaimer text. For an example, see [Organization-wide message disclaimers, signatures, footers, or headers in Exchange Online](disclaimers-signatures-footers-or-headers.md).

## Know when to stop rule processing

Sometimes it makes sense to stop rule processing once a rule is matched. For example, if you have one rule to block messages with attachments and one to insert a disclaimer in messages that match a pattern, you probably should stop rule processing once the message is blocked. There's no need for further action.

To stop rule processing after a rule is triggered, in the rule, select the **Stop processing more rules** check box.

## If you have lots of keywords or patterns to match, load them from a file

For example, you might want to prevent emails from being sent if they contain a list of unacceptable or bad words. You can create a text file containing these words and phrases, and then use PowerShell to set up a mail flow rule that blocks messages that use them.

The text file can contain regular expressions for patterns. These expressions are not case-sensitive. Common regular expressions include:

<br>

****

|Expression|Matches|
|---|---|
|**.**|Any single character|
|**\***|Any additional characters|
|**\d**|Any decimal digit|
|[*character_group*]|Any single character in *character_group*.|
|

For an example that shows a text file with regular expressions and the Exchange module Windows PowerShell commands to use, see [Use mail flow rules to route email based on a list of words, phrases, or patterns in Exchange Online](use-rules-to-route-email.md).

To learn how to specify patterns using regular expressions, see [Regular Expression Reference](/dotnet/standard/base-types/regular-expression-language-quick-reference).
