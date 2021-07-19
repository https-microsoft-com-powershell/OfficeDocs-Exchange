---
localization_priority: Normal
description: You can configure the default prompt language setting on a Unified Messaging (UM) auto attendant. The language setting available on a UM auto attendant enables you to configure the default prompt language on the auto attendant. When you're using the default system prompts for the auto attendant, this is the language that the caller hears when the auto attendant answers the incoming call. This language setting affects only the default system prompts. This setting doesn't affect custom prompts that are configured on an auto attendant.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 3a1c1ec0-c726-41fb-a294-59faab205609
ms.reviewer: 
f1.keywords:
- NOCSH
title: Select the language for an auto attendant in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Select the language for an auto attendant in Exchange Online

You can configure the default prompt language setting on a Unified Messaging (UM) auto attendant. The language setting available on a UM auto attendant enables you to configure the default prompt language on the auto attendant. When you're using the default system prompts for the auto attendant, this is the language that the caller hears when the auto attendant answers the incoming call. This setting doesn't affect custom prompts that are configured on an auto attendant.


### Use the EAC to configure the default language setting

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to modify, and then on the toolbar, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

4. On the **General** page, under **Language for automated voice interface**, select the required language from the drop-down list.

5. Click **Save** to accept your changes.

### Use Exchange Online PowerShell to configure the default language setting

This example sets the default language on the UM auto attendant `MyUMAutoAttendant` to English (Great Britain).

```PowerShell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -Language en-GB
```

This example sets the default language on the UM auto attendant `MyUMAutoAttendant` to German.

```PowerShell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -Language de-DE
```
