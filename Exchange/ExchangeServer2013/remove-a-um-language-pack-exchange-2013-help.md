---
title: 'Remove a UM language pack: Exchange 2013 Help'
TOCTitle: Remove a UM language pack
ms:assetid: a2bc2753-2c25-4ea0-a9d5-e3d42a699c6c
ms:mtpsurl: https://technet.microsoft.com/library/Bb124004(v=EXCHG.150)
ms:contentKeyID: 49315473
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove a UM language pack in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can use the EAC or the Shell to manage Unified Messaging (UM) languages on Mailbox servers running the Microsoft Exchange Unified Messaging service. However, to remove a language from the list on a UM dial plan, you must remove the appropriate UM language pack from the Mailbox server by using the **Setup.exe /RemoveUmLanguagePack** command. After you remove the UM language pack from the Mailbox server, the language won't be available when you configure a UM dial plan or a UM auto attendant. You can view the UM language packs that are installed by viewing the properties of the Mailbox server or by using the **Get-UMService** cmdlet.

For additional tasks related to UM languages, see [UM languages, prompts, and greetings procedures](um-languages-prompts-and-greetings-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox server (UM service)" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Verify that a UM language pack other than en-US is installed.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use Setup.exe to remove a UM language pack

At a command prompt, run the following command.

```powershell
Setup.exe /RemoveUmLanguagePack:<UmLanguagePackName>
```

In the previous command, *\<UmLanguagePackName\>* is the name of the UM language pack, for example, fr-FR.

> [!WARNING]
> You can't use the Setup.exe file that's located in the \Bin folder to remove a UM language pack after you've installed any updates. You must use the Setup.exe file from the Exchange 2013 DVD or the downloaded source files. If you don't, you'll see the following error: There is a version mismatch between the running application and the installed application.
