---
title: "Create and manage groups in the new Exchange admin center"
ms.author: v-bshilpa
author: Benny
manager: Serdars
audience: ITPro
f1.keywords:
ms.topic: article
ms.service: exchange-online
localization_priority: Normal
search.appverid:
ms.collection:
ms.custom:
description: Create and manage groups in the new Exchange admin center.
---

# Create and manage groups in the new Exchange admin center

Use the new Exchange admin center (EAC) to create, modify, export, or remove groups in your Exchange Online organization.

There are four types of groups that can be used to distribute messages:

- **Microsoft 365 group** (formerly known as Office 365 groups), is used for collaboration between teams, both inside and outside your company; by giving them a group email and a shared workspace for conversations, files, and calendars.

  >[!NOTE]
  > **Microsoft 365 group** is the recommended group as it provides the teams a shared workspace to communicate, share files, appointments, emails, contacts and other mailbox items.

- **Distribution list group** is used for sending emails/notifications to a group of people.

- **Dynamic distribution list group** is used to expedite the mass sending of email messages and other information within a Microsoft Exchange organization.

- **Mail-enabled security group** is used for granting access to resources such as OneDrive, SharePoint, and emailing notifications to those users.

For more information see, [groups](/microsoft-365/admin/create-groups/compare-groups).

## Create a group

1. Login to the [new Exchange admin center](https://admin.exchange.microsoft.com/#/), and navigate to **Recipients** > **Groups**.

     The **Groups** page is displayed.

2. Click **Add a group** and follow the instructions in the details pane.

   For more information see, [Create a group](/microsoft-365/admin/create-groups/create-groups).

   - In **Finish** tab, under **Review and finish adding group**, verify all the details, and then click **Create group**.

3. Click **Close**.

For more information see, [Use groups to collaborate effectively](https://support.microsoft.com/office/learn-about-microsoft-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2?WT.mc_id=365AdminCSH&ui=en-US&rs=en-US&ad=US).

## Edit a group

1. From the list view, select the group that you want to edit, and click the selected group name.

2. In the details pane, do the following:

   - In **General** section, you can edit the **Basic information** and the **Email address** of the group.

   - In **Members** section, you can view, manage, and add **Owners** and **Members** to the group.

   - In **Settings** section, you can do the following:

     a. For **Microsoft 365 group**, you can edit/check the confirmation boxes under **General settings**, change the status in **Privacy** settings, and then click                   **Save** to save the changes.

     b. For **Distribution list group** and **Mail-enabled security group**, you can edit/check the confirmation box to allow external senders to email this group and then click **Advanced Settings** to edit/manage more settings in the **Exchange admin center**.

   - In **Microsoft Teams** section, you can manage your **Teams** settings in **Microsoft Teams admin center**.

   >[!NOTE]
   > **Microsoft Teams** can be added to only a **Microsoft 365 group**. This option is not available for the other groups. To create a team, all group owners must have a license that includes **Teams**.

## Export a group

You can export group details in a .csv file format.

1. Select the group from the list view that you want to export and click **Export groups**.

   The dialog box to confirm the export is displayed.

2. Click **Continue**.

   The .csv format of the group details file is downloaded.

## Naming policy

You can add prefixes and suffixes to your group names.

1. Click **Add naming policy**.

2. In **Edit group naming policy** details pane, do the following:

   - In **Policy** section, provide the details.

   - In **Blocked words**, add specific words that you want to block from being used in group names and aliases.

3. Click **Save**.

## Upgrade the Distribution list group

You can upgrade a **Distribution list group** to **Microsoft 365 group**.

1. Select the group from the list view that you want to upgrade and click **Upgrade distribution group**.

   The dialog box to confirm the upgrade is displayed.

2. Click **Upgrade**.

   >[!NOTE]
   > The upgrade is a permanent change and can not be reversed.

## Other actions

1. Click **Refresh** to update the list of groups page after adding a group or editing the details of a group.

2. Click **...** to perform the following actions:

   - Click **Edit name and description** to edit the group information.

   - Click **Delete group** to delete the selected group.

3. Select a group, click **...** > **Edit email address** to edit **Primary** and **Aliases** email address.

4. Click **Filter** to filter the groups based on the displayed options in the drop-down list.

5. Enter information in the **Search** box to search a group, group email id, or other details.

See one of the following topics for managing groups in the Classic Exchange admin center:

- [Create and manage distribution groups](./manage-distribution-groups/manage-distribution-groups.md)

- [Manage mail-enabled security groups](./manage-mail-enabled-security-groups.md)
