---
title: 'Older database files present_SecondSGFilesExist: Exchange 2013 Help'
TOCTitle: Older database files present_SecondSGFilesExist
ms:assetid: fe2908e7-df8b-4f35-946a-cfbf8521e93a
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.secondsgfilesexist(v=EXCHG.150)
ms:contentKeyID: 46629214
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Older database files present\_SecondSGFilesExist

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

Microsoft® Exchange Server 2007 setup cannot continue because it detected existing Microsoft Exchange database files in the target installation path.

Exchange 2007 setup requires that the target installation path be empty of Microsoft Exchange database files.

To resolve this issue, remove all existing files from target installation paths and then rerun setup.

**To delete existing Exchange Server database files from the target installation path**

1. In My Computer or Windows Explorer, locate the target install path.

    > [!NOTE]
    > By default, the database files are located in:<BR>&lt;systemDrive&gt;:\Program Files\Microsoft\Exchange Server\Mailbox\First Storage Group.

2. Right-click the files to be removed, and then select **Delete**.

3. At the **Confirm File Delete** dialog, click **Yes**.

4. Repeat steps 2 and 3 for all files in the target install paths.
