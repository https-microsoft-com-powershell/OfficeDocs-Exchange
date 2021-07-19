---
localization_priority: Normal
description: Learn about offline address books (OABs) in Exchange Online.
ms.topic: overview
author: msdmaguire
ms.author: jhendr
ms.assetid: 3f4b2c64-6cbc-445f-bf65-05b8fdfe9a0b
ms.reviewer: 
title: Offline address books in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Offline address books in Exchange Online

An offline address book (OAB) is a downloadable address list collection that Outlook users can access while disconnected from Exchange Online. Admins can decide which address lists are made available to users who work offline.

Offline address books are generated every 8 hours.

For more information about address lists in Exchange Online, see [Address lists](../../address-books/address-lists/address-lists.md).

For OAB procedures, see [Offline address book procedures](offline-address-book-procedures.md).

## How users download offline address books

1. In Outlook, click **File** \> **Account Settings** \> **Download Address Book**.

2. On the **Offline address book** dialog box that's displayed, make the following selections:

    - **Download changes since last Send/Receive**: By default, this check box is selected. Unchecking this box causes a full download of the OAB.

    - **Choose address book**: This drop-down list will display the offline address books that are available to you. Depending on what an admin has configured, you might see only one value here (for example, the global address list).

3. Click **OK**. The OAB is downloaded and saved on your computer.

### Conditions that cause a full download of the OAB

There are situations where Outlook will always perform a full OAB download. For example:

- There's no OAB on the client computer (for example, this is the first time you've connected to your Exchange Online mailbox in Outlook on this computer).

- The version of the OAB on the server and the client don't match (a more recent version of the OAB is present on the server).

- One or more OAB files are missing from the client computer.

- A previous full download failed, and Outlook has to start over.

- When a user has multiple MAPI profiles on the same Outlook client computer and they switch between the two profiles that both use Cached Exchange Mode, multiple full OAB downloads of the same OAB files will occur.
