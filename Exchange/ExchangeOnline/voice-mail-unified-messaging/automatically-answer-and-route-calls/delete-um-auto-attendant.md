---
localization_priority: Normal
description: After you delete a Unified Messaging (UM) auto attendant, the incoming calls that were answered by the UM auto attendant must be answered by a human operator. A UM auto attendant can't be deleted if it's associated with a UM dial plan as the default UM auto attendant.
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 92846bbc-e6b9-45fc-8702-ef5c92eeb08f
ms.reviewer: 
f1.keywords:
- NOCSH
title: Delete a UM auto attendant in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Delete a UM auto attendant in Exchange Online

After you delete a Unified Messaging (UM) auto attendant, the incoming calls that were answered by the UM auto attendant must be answered by a human operator. A UM auto attendant can't be deleted if it's associated with a UM dial plan as the default UM auto attendant.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the EAC to delete a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to edit, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to delete. On the toolbar, click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.gif). On the **Warning** page, click **Yes**.

## Use Exchange Online PowerShell to delete a UM auto attendant

This example deletes a UM auto attendant named `MyUMAutoAttendant`.

```PowerShell
Remove-UMAutoAttendant -Identity MyUMAutoAttendant
```
