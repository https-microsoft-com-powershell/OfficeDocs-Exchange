---
localization_priority: Normal
description: You can use Exchange Online PowerShell to enable the recording of custom prompts and greetings for Unified Messaging (UM) dial plans and auto attendants using the telephone user interface (TUI). This can be useful when you want to change a custom greeting or announcement by using the EAC or Exchange Online PowerShell, or when there's an emergency such as an organization closure because of severe weather. When you're changing a custom greeting or announcement on a UM auto attendant, you must enable TUI prompt recording on the dial plan that the UM auto attendant is linked to.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: f2e5c636-2be9-4d48-b5e7-37913ded62d1
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable custom prompt recording using the telephone user interface in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable custom prompt recording using the telephone user interface in Exchange Online

You can use Exchange Online PowerShell to enable the recording of custom prompts and greetings for Unified Messaging (UM) dial plans and auto attendants using the telephone user interface (TUI). This can be useful when you want to change a custom greeting or announcement by using the EAC or Exchange Online PowerShell, or when there's an emergency such as an organization closure because of severe weather. When you're changing a custom greeting or announcement on a UM auto attendant, you must enable TUI prompt recording on the dial plan that the UM auto attendant is linked to.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](../../voice-mail-unified-messaging/automatically-answer-and-route-calls/um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](../../voice-mail-unified-messaging/automatically-answer-and-route-calls/create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use Exchange Online PowerShell to enable a custom prompt or greeting recording using the TUI

To record custom prompts and greetings by using the telephone user interface (TUI), follow these steps:

1. Create a domain user account that cannot log on interactively.

2. Delegate the Exchange Organization Administrator role to the domain user account.

3. Create a mailbox for the domain user.

4. Enable the domain user's mailbox for Unified Messaging.

   > [!IMPORTANT]
   > Allow only those administrators who are managing prompts and greetings access to the extension number and PIN for the user account. Use this user account only for managing prompts over the telephone.

5. Create and save a .wav or .wma file to use for a custom greeting for the UM dial plan or auto attendant.

   > [!NOTE]
   > MP3 files can't be used for custom prompts.

6. Use the EAC or Exchange Online PowerShell to configure the dial plan to use the custom welcome greeting or configure the auto attendant to use the business or non-business hours greeting. For details about configuring a dial plan, see [Enable a customized greeting for Outlook Voice Access users](../../voice-mail-unified-messaging/set-up-client-voice-mail-features/enable-a-customized-greeting.md). For details about configuring an auto attendant, see [Enable a customized business hours greeting](../../voice-mail-unified-messaging/automatically-answer-and-route-calls/enable-a-customized-business-hours-greeting.md) or [Enable a customized non-business hours greeting](../../voice-mail-unified-messaging/automatically-answer-and-route-calls/enable-a-customized-non-business-hours-greeting.md).

7. Run the following cmdlet:

   ```PowerShell
   Set-UMDialPlan -identity MyUMDialPlan -TUIPromptEditingEnabled $true
   ```

> [!NOTE]
> Before you can enable the recording of a custom prompt or greeting, you must sign in to the mailbox that's set up for recording prompts. After you record the new prompt or greeting, you must sign out and then sign back in before you can hear the new prompt or greeting when you use the TUI.

## Perform TUI prompt recording on a UM auto attendant

1. Verify that the auto attendant is linked to the dial plan that you've enabled for TUI prompt recording.

2. Call a phone number that's been configured on the UM auto attendant.

3. While the non-business or business hours greeting for the auto attendant is being played, press the pound key (#), and then press the star key (\*).

4. You'll be prompted to enter the extension number for the user. Enter the extension number of the UM-enabled user who has permission to perform TUI prompt recording.

5. You'll be prompted for a PIN. Enter the user's PIN.

6. Follow the system prompts to edit or update the greeting or informational announcement for the auto attendant.

## Perform TUI prompt recording on a UM dial plan

1. Call an Outlook Voice Access number you use to sign in to Outlook Voice Access.

2. While the welcome greeting for the dial plan is being played, press the pound key (#), and then press the star key (\*).

3. If you're calling from a phone that's used by a UM-enabled user, you'll be prompted for a PIN. Instead of entering the PIN, press the star key (\*). You'll be prompted for an extension number. Enter the extension number of the UM-enabled user who has permission to perform TUI prompt recording.

4. If you're calling from a phone that's not used by a UM-enabled user, you'll automatically be prompted for an extension number. Enter the extension number of the UM-enabled user who has permission to perform TUI prompt recording.

5. You'll be prompted for a PIN. Enter the user's PIN.

6. Follow the system prompts to edit or update the welcome greeting for the dial plan or the informational announcement.
