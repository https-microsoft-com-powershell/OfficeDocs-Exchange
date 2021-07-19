---
localization_priority: Normal
description: Admins can learn how to create, modify, and delete mail contacts in Exchange Online.
ms.topic: article
author: msdmaguire
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Recipients.NewMailContactWizardForm.NewMailContactIntroductionWizardPage
ms.author: jhendr
ms.assetid: 74c72aed-e9ff-4927-8eb7-c08a86e79ae0
ms.reviewer: 
title: Manage mail contacts
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage mail contacts in Exchange Online

> [!IMPORTANT]
> Check out the new Exchange Admin Center! The experience is modern, intelligent, accessible, and better. Personalize your dashboard, manage cross tenant migration, experience the improved Groups feature, and more. [Try it now](https://admin.exchange.microsoft.com)!

In Exchange Online organizations, mail contacts are mail-enabled objects that contain information about people who exist outside your organization. Each mail contact has an external email address. For more information about mail contacts, see [Recipients in Exchange Online](recipients-in-exchange-online.md).

You manage mail contacts in the Exchange admin center (EAC) or in PowerShell (Exchange Online PowerShell in organizations with Exchange Online mailboxes; standalone Exchange Online Protection (EOP) in organizations without Exchange Online mailboxes).

## What do you need to know before you begin?

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../exchange-admin-center.md).

- To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell). To connect to standalone EOP PowerShell, see [Connect to Exchange Online Protection PowerShell](/powershell/exchange/connect-to-exchange-online-protection-powershell).

- You need permissions before you can do this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../permissions-exo/feature-permissions.md) article.

- For information about keyboard shortcuts that may apply to the procedures in this article, see [Keyboard shortcuts for the Exchange admin center](../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the Exchange admin center to manage mail contacts

### Use the new EAC to create mail contacts

1. In the new [EAC](https://admin.exchange.microsoft.com), go to **Recipients** \> **Contacts**.

2. Click **+ Add a contact** and configure the following settings in the details pane. Settings marked with an <sup>\*</sup> are required.

   - **Contact type**: Select **Mail contact** from the drop-down list.

   - **First name**
   
   - **Last name**

   - <sup>\*</sup>**Display name**: By default, this box shows the values from the **First name**, and **Last name** boxes. You can accept this value or change it.

   - <sup>\*</sup>**Email**: Enter the user's email address. The domain should be external to your cloud-based organization.
   
   - **Company**
   
   - **Work phone**
   
   - **Mobile phone**
   
4. When you're finished, click **Add** and then click **Close**.

### Use the new EAC to modify mail contacts

1. In the new EAC, go to **Recipients** \> **Contacts**.

2. In the list of contacts, select the mail contact that you want to modify.

3. In the details pane, click ![edit-icon](../media/edit-tooltip.png) to view or edit the user's contact details.

   When you're finished, click **Save**.

   #### Contact Information

   Use the **Contact information** section, to view, or edit the user's contact information. The information on this page is displayed in the address book.

    - **Web site**
    
    - **Fax phone**
    
    - **Street**
    
    - **City**
    
    - **State/Province**
    
    - **ZIP/Postal code**
    
    - **Country/Region**
   
   #### Organization

   Use the **Organization** section, to record detailed information about the user's role in the organization. This information is displayed in the address book. Also, you can create a virtual organization chart that's accessible from email clients such as Outlook.

   - **Title**: Use this box to view or change the recipient's title.

   - **Department**: Use this box to view or change the department in which the user works. You can use this box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

   - **Manager**: To add a manager, enter the name and select from the drop-down list.

   - **Direct reports**: You can't modify this box. A direct report is a user who reports to a specific manager. If you've specified a manager for the user, that user appears as a direct report in the details of the manager's mailbox. For example, Kari manages Chris and Kate, so Kari is specified in the **Manager** box for Chris and Kate, and Chris and Kate appear in the **Direct reports** box in the properties of Kari's account.

### Use the new EAC to remove mail contacts

1. In the new EAC, go to **Recipients** \> **Contacts**.

2. Select the mail contact that you want to remove, and then click **Delete**.

   > [!NOTE]
   > New EAC doesn't allow bulk edit of mail contacts yet.

### Use the Classic EAC to create mail contacts

1. In the Classic EAC, go to **Recipients** \> **Contacts**

2. Click **New** ![New icon](../media/ITPro_EAC_AddIcon.png) and then select **Mail contact**.

3. In the **New mail contact** page that opens, configure the following settings. Settings marked with an <sup>\*</sup> are required.

   - **First name**

   - **Initials**: The person's middle initial.

   - **Last name**

   - <sup>\*</sup>**Display name**: By default, this box shows the values from the **First name**, **Initials**, and **Last name** boxes. You can accept this value or change it. The value should be unique, and has a maximum length of 64 characters.

   - <sup>\*</sup>**Alias**: Enter a unique alias, using up to 64 characters, for the user

   - <sup>\*</sup>**External email address**: Enter the user's email address. The domain should be external to your cloud-based organization.

4. When you're finished, click **Save**.

### Use the Classic EAC to modify mail contacts

1. In the Classic EAC, go to **Recipients** \> **Contacts**.

2. In the list of contacts, select the mail contact that you want to modify, and then click **Edit** ![Edit image](../media/ITPro_EAC_AddIcon.png).

3. On the mail contact properties page that opens, click one of the following tabs to view or change properties.

   When you're finished, click **Save**.

   #### General

   Use the **General** section to view or change basic information about the mail contact.

    - **First name**

    - **Initials**

    - **Last name**

    - **Display name**: This name appears in your organization's address book, on the To: and From: lines in email, and in the list of contacts in the EAC. This name can't contain empty spaces before or after the display name.

    - **Alias**: This is the mail contact's alias. If you change it, it must be unique in the organization and must be 64 characters or less.

    - **External email address**: This is mail contact's primary SMTP address in their external email organization. Email sent to this contact is forwarded to this email address.

    - **Hide from address lists**: Select this check box to prevent the mail contact from appearing in the address book and other address lists that are defined in your organization. After you select this check box, users can still send messages to the recipient by using the email address.

   #### Contact Information

   Use the **Contact information** tab to view or change the user's contact information. The information on this page is displayed in the address book.

    - **Street**
    
    - **City**
    
    - **State/Province**
    
    - **ZIP/Postal code**
    
    - **Country/Region**
    
    - **Office**
    
    - **Work phone**
    
    - **Fax**
    
    - **Home phone**
    
    - **Mobile phone**
    
    - **Notes**

   > [!TIP]
   > You can use the **State/Province** value to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

   #### Organization

   Use the **Organization** tab to record detailed information about the user's role in the organization. This information is displayed in the address book. Also, you can create a virtual organization chart that's accessible from email clients such as Outlook.

    - **Title**: Use this box to view or change the recipient's title.

    - **Department**: Use this box to view or change the department in which the user works. You can use this box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

    - **Company**: Use this box to view or change the company for which the user works. You can use this box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

    - **Manager**: To add a manager, click **Browse**. In **Select Manager**, select a person, and then click **OK**.

    - **Direct reports**: You can't modify this box. A direct report is a user who reports to a specific manager. If you've specified a manager for the user, that user appears as a direct report in the details of the manager's mailbox. For example, Kari manages Chris and Kate, so Kari is specified in the **Manager** box for Chris and Kate, and Chris and Kate appear in the **Direct reports** box in the properties of Kari's account.

   #### Email Options

   Use the **Email Options** section to add or remove proxy addresses for the mail contact or edit existing proxy addresses. The mail contact's primary SMTP address is also displayed in this section, but you can't change it. To change it, you have to change the contact's external email address in the **General** section.

   > [!NOTE]
   > The **Email Options** section is only available in Exchange Server. It's not available in Exchange Online.

   #### MailTip

   Use the **MailTip** tab to add an alert for potential issues before a user sends messages to this recipient. The text is displayed in the InfoBar when this recipient is added to the To, Cc, or Bcc lines of a new email message.

   MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

### Use the Classic EAC to bulk edit mail contacts

When you bulk edit mail contacts in the EAC, you can change the following types of properties:

 - [Contact information](#contact-information)

 - [Organization](#organization)

1. In the Classic EAC, navigate to **Recipients** \> **Contacts**.

2. In the list of contacts, select two or more mail contacts. You can't bulk edit a combination of mail contacts and mail users.

    > [!TIP]
    > You can select multiple adjacent mail contacts by holding down the Shift key and clicking the first mail contact, and then clicking the last mail contact you want to edit. You can also select multiple mail contacts by holding down the Ctrl key and clicking each one that you want to edit.

3. In the Details pane, under **Bulk Edit**, click **Update** under **Contact Information** or **Organization**.

4. Make the changes on the properties page and then save your changes.

### Use the Classic EAC to remove mail contacts

1. In the Classic EAC, go to **Recipients** \> **Contacts**.

2. Select the mail contact that you want to remove, and then click **Remove** ![Remove icon](../media/ITPro_EAC_RemoveIcon.gif).

## Use PowerShell to manage mail contacts

### Use Exchange Online PowerShell to create mail contacts

This example creates a mail contact for Debra Garcia

- The name and display name is Debra Garcia (if you don't use the _DisplayName_ parameter, the value of the _Name_ parameter is used for the display name).

- The alias is dgarcia.

```PowerShell
New-MailContact -Name "Debra Garcia" -ExternalEmailAddress dgarcia@tailspintoys.com -Alias dgarcia
```

For detailed syntax and parameter information, see [New-MailContact](/powershell/module/exchange/new-mailcontact).

### Use Exchange Online PowerShell to modify mail contacts

In general, use the **Get-Contact** and **Set-Contact** cmdlets to view and change organization and contact information properties. Use the **Get-MailContact** and **Set-MailContact** cmdlets to view or change mail-related properties, such as email addresses, the MailTip, custom attributes, and whether the contact is hidden from address lists.

For more information, see the following articles:

- [Get-Contact](/powershell/module/exchange/get-contact)

- [Set-Contact](/powershell/module/exchange/set-contact)

- [Get-MailContact](/powershell/module/exchange/get-mailcontact)

- [Set-MailContact](/powershell/module/exchange/set-mailcontact)

Here are some examples of using Exchange Online PowerShell to change mail contact properties:

This example configures the Title, Department, Company, and Manager properties for the mail contact Kai Axford.

```PowerShell
Set-Contact "Kai Axford" -Title Consultant -Department "Public Relations" -Company Fabrikam -Manager "Karen Toh"
```

This example sets the CustomAttribute1 property to a value of PartTime for all mail contacts and hides them from the organization's address book.

```PowerShell
$Contacts = Get-MailContact -Resultsize unlimited
$Contacts | foreach {Set-MailContact -Identity $_ -CustomAttribute1 PartTime -HiddenFromAddressListsEnabled $true}
```

This example sets the CustomAttribute15 property to a value of TemporaryEmployee for all mail contacts in the Public Relations department.

```PowerShell
$PR = Get-Contact -ResultSize unlimited -Filter "Department -eq 'Public Relations'"
$PR | foreach {Set-MailContact -Identity $_ -CustomAttribute15 TemporaryEmployee}
```

### Use Exchange Online PowerShell to remove mail contacts

To remove a mail contact, use the following syntax:

```powershell
Remove-MailContact -Identity <MailUserIdentity>
```

This example removes the mail contact for Pilar Pinilla:

```powershell
Remove-MailContact -Identity "Pilar Pinilla"
```

For detailed syntax and parameter information, see [Remove-MailContact](/powershell/module/exchange/remove-mailcontact).

## How do you know these procedures worked?

To verify that you've successfully created, modified, or removed mail contacts, do any of the following steps:

- In the new EAC, go to **Recipients** \> **Contacts**. Verify the mail contact is listed (or not listed). The **Contact Type** value is **MailContact**. Select the mail contact from the list, and click ![edit-icon](../media/edit-tooltip.png) to view or edit the user's details.

- In the Classic EAC, go to **Recipients** \> **Contacts**. Verify the mail contact is listed (or not listed). The **Contact Type** value is **Mail contact**. Select the mail contact from the list, and click **Edit** ![Edit tooltip](../media/ITPro_EAC_EditIcon.png) to view the properties.

- In Exchange Online PowerShell, replace \<MailContactIdentity\> with the name, email address, or alias of the mail contact, and run the following command to verify that the mail contact is listed (or not listed).

  ```PowerShell
  Get-MailContact -Identity <MailContactIdentity> | Format-List Name,Alias,DisplayName,ExternalEmailAddress
  ```

- In Exchange Online PowerShell, use the **Get-Contact** and **Get-Contact** cmdlets to verify the property changes you made.

   ```PowerShell
   Get-MailContact | Format-List Name,CustomAttribute1,HiddenFromAddressListsEnabled
   ```

   ```PowerShell
   Get-Contact -Filter "Department -eq 'Public Relations'" | Get-MailContact | Format-List Name,CustomAttribute15
   ```
