---
localization_priority: Normal
description: You can customize the menu prompt to be used by a Unified Messaging (UM) auto attendant during business hours. After you create a UM auto attendant, a default system prompt (Welcome to Unified Messaging) is used as the menu prompt that callers hear after the business hours welcome greeting is played. Although the system prompt mustn't be replaced or changed, you can customize the greetings and menu prompts that are used with UM auto attendants. After you create a customized business hours menu prompt audio file, you must enable menu navigation entries on the UM auto attendant for business hours.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 89053e84-3490-4dc6-ade3-9b6c5dbf4020
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable a customized business hours menu prompt in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable a customized business hours menu prompt in Exchange Online

You can customize the menu prompt to be used by a Unified Messaging (UM) auto attendant during business hours. After you create a UM auto attendant, a default system prompt ("Welcome to Unified Messaging") is used as the menu prompt that callers hear after the business hours welcome greeting is played. Although the system prompt mustn't be replaced or changed, you can customize the greetings and menu prompts that are used with UM auto attendants. After you create a customized business hours menu prompt audio file, you must enable menu navigation entries on the UM auto attendant for business hours.

If you only want to include the name of your organization or business as part of the default system prompt, you can enter the name in the **Business name** box on the UM auto attendant. For details, see [Enter a business name](enter-a-business-name.md).

> [!IMPORTANT]
> You must configure business hours on the auto attendant. For details, see [Configure business hours](configure-business-hours.md).

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- Create a .wav or .wma file to be used for the menu prompt.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the EAC to enable a customized business hours menu prompt

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan that you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to enable a customized business hours menu prompt, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page, \> **Menu navigation**, under **Business hours menu navigation** click **Change**, and then click **Browse** to locate the customized business hours menu prompt file.

    > [!IMPORTANT]
    > The file you use for the menu prompt must be a .wav or .wma file.

4. After you've located the file, click **Open**, and then click **Save**.

## Use Exchange Online PowerShell to enable a customized business hours menu prompt

This example enables a business hours main menu prompt and uses a customized prompt named `businesshoursprompts.wav` on the UM auto attendant `MyUMAutoAttendant`.

```PowerShell
Command Set-UMAutoAttendant -Identity MyUMAutoAttendant -BusinessHoursMainMenuCustomPromptEnabled $true -BusinessHoursMainMenuCustomPromptFilename BusinessHoursPrompts.wav
```

This example configures a UM auto attendant named `MyUMAutoAttendant` that has business hours configured to be 10:45 to 13:15 (Sunday), 09:00 to 17:00 (Monday), and 09:00 to 16:30 (Saturday) and holiday times and their associated greetings configured to be " `New Year`" on January 2, 2013, and " `Building Closed for Construction`" from April 24, 2013 through April 28, 2013.

```PowerShell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -BusinessHoursSchedule 0.10:45-0.13:15,1.09:00-1.17:00,6.09:00-6.16:30 -HolidaySchedule "New Year,newyrgrt.wav,1/2/2013","Building Closed for Construction,construction.wav,4/24/2013,4/28/2013"
```

This example configures a UM auto attendant named `MyAutoAttendant` and enables business hours navigation menus so that when callers press 1, they're forwarded to another UM auto attendant named `SalesAutoAttendant`. When they press 2, they're forwarded to extension number 12345 for `Support`, and when they press 3, they're sent to another auto attendant that plays an audio file.

```PowerShell
Set-UMAutoAttendant -Identity MyAutoAttendant - BusinessHoursKeyMappingEnabled $true -BusinessHoursKeyMapping "1,Sales,,SalesAutoAttendant","2,Support,12345","3,Directions,,,directions.wav"
```
