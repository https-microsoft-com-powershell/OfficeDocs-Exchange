---
localization_priority: Normal
description: 'Summary: How users in your Exchange 2016 or Exchange 2019 organization can quickly set up their Outlook for iOS and Android accounts using Basic authentication.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 013dbe8c-30de-4c9c-baa9-75081b9229e8
title: Account setup in Outlook for iOS and Android using Basic authentication
ms.collection: exchange-server
ms.reviewer: smithre4
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Account setup in Outlook for iOS and Android using Basic authentication

Outlook for iOS and Android offers Exchange administrators the ability to "push" account configurations to their on-premises users who use Basic authentication with the ActiveSync protocol. This capability works with any Unified Endpoint Management (UEM) provider who uses the [Managed App Configuration](https://www.apple.com/business/resources/docs/Managing_Devices_and_Corporate_Data_on_iOS.pdf) channel for iOS or the [Android in the Enterprise](https://developer.android.com/work/managed-configurations) channel for Android.

For on-premises users enrolled in Microsoft Intune, you can deploy the account configuration settings using Intune in the Azure Portal.

Once an account configuration has been created and the user enrolls their device, Outlook for iOS and Android will detect that an account is "Found" and will then prompt the user to add the account. The only information the user needs to enter to complete the setup process is their password. Then, the user's mailbox content will load and the user can begin using the app.

The following images show an example of the end-user setup process after Outlook for iOS and Android has been configured in Intune in the Azure Portal.

![Account setup for Outlook for iOS and Android on-premises](../../media/77f0906c-0d62-48a4-9a93-534e29dae7e0.png)

## Create an app configuration policy for Outlook for iOS and Android using Microsoft Intune

If you're using Microsoft Intune as your mobile device management provider, the following steps will allow you to deploy account configuration settings for your on-premises mailboxes that leverage basic authentication with the ActiveSync protocol. Once the configuration is created, you can assign the settings to groups of users, as detailed in the next section, [Assign configuration settings](account-setup.md#assignconfig).

> [!NOTE]
> If users in your organization use both iOS and Android for Work devices, you'll need to create a separate app configuration policy for each platform.

1. Sign into [Microsoft Endpoint Manager](https://devicemanagement.microsoft.com).

2. Select **Apps** and then select **App configuration policies**.

3. On the **App Configuration policies** blade, choose **Add** and select **Managed devices**.

4. On the **Add app configuration** blade, enter a **Name**, and optional **Description** for the app configuration settings.

5. For **Platform**, choose either **iOS/iPadOS** or **Android**.

6. For **Associated app**, choose **Select the required app**, and then, on the **Targeted apps** blade, choose **Microsoft Outlook**.

    > [!NOTE]
    > If Outlook is not listed as an available app, then you must add it by following the instructions in [Add Android store apps to Microsoft Intune](/intune/store-apps-android) and [How to add iOS store apps to Microsoft Intune](/intune/store-apps-ios).

7. Click **OK** to return to the **Add app configuration** blade.

8. Choose **Configuration Settings**. On the **Configuration** blade, select **Use configuration designer** for the **Configuration settings format**. The key value pairs used in this section are defined in the section [Key value pairs](account-setup.md#kvp).

9. If you want to deploy account setup configuration, select **Yes** for **Configure email account settings** and configure appropriately:

    - For **Authentication type**, select **Basic authentication**. This is required for on-premises accounts that do not leverage hybrid modern authentication.

    - For **Username attribute from AAD**, select **User Principal Name** or **sAMAccountName**. If **sAMAccountName** is selected, enter the NetBIOS domain name in the **Account domain** field.

    - For **Email address attribute from AAD**, select **Primary SMTP Address**.

    - For **Email server**, enter the Exchange ActiveSync externally accessible domain name.
    
    - For **Email account name**, enter a descriptive value for the account.

10. If you want to deploy general app configuration settings, configure the desired settings accordingly:

    - For **Focused Inbox**, choose from the available options: **Not configured** (default), **On** (app default), **Off**.

    - For **Require Biometrics to access the app**, choose from the available options: **Not configured** (default), **On**, **Off** (app default). When selecting **On** or **Off**, administrators can choose to allow the user to change the app setting's value. Select **Yes** (app default) to allow the user to change the setting or choose **No** if you want to prevent the user from changing the setting's value. This setting is only available in Outlook for iOS.

    - For **Save Contacts**, choose from the available options: **Not configured** (default), **On**, **Off** (app default). When selecting **On** or **Off**, administrators can choose to allow the user to change the app setting's value. Select **Yes** (app default) to allow the user to change the setting or choose **No** if you want to prevent the user from changing the setting's value.

    - For **Default app signature**, choose from the available options: **Not configured** (default), **On** (app default), **Off**.

    - For **Block external images**, choose from the available options: **Not configured** (default), **On**, **Off** (app default). When selecting **On** or **Off**, administrators can choose to allow the user to change the app setting's value. Select **Yes** (app default) to allow the user to change the setting or choose **No** if you want to prevent the user from changing the setting's value.

    - For **Organize mail by thread**, choose from the available options: **Not configured** (default), **On** (app default), **Off**.

11. When you are done, choose **OK**.

12. On the **Add app configuration** blade, choose **Add**.

The newly created configuration policy is displayed on the **App configuration** blade.

## Assign configuration settings
<a name="assignconfig"> </a>

You assign the settings to groups of users in Azure Active Directory. When a user has the Microsoft Outlook app installed, the app is managed by the settings you have specified. To do this:

1. From the **Apps - App configuration policies** blade, select the app configuration policy you want to assign.

2. On the next blade, choose **Assignments**.

3. On the **Assignments** blade, select **Select groups to include** and choose the Azure AD group to which you want to assign the app configuration, and then choose **Select**.

4. Select **Save** to save and assign the app configuration policy.

## Key value pairs
<a name="kvp"> </a>

When you create an app configuration policy in the Azure Portal or through your UEM provider, you will need the following key value pairs:

|**Key**|**Values**|
|:-----|:-----|
|com.microsoft.outlook.EmailProfile.EmailAccountName|This value specifies the display name email account as it will appear to users on their devices.  <br/> **Value type**: String  <br/> **Accepted values**: Display Name  <br/> **Default if not specified**: \<blank\>  <br/> **Required**: Yes  <br/> **Example**: user  <br/> **Intune Token**<sup>*</sup>: {{username}}|
|com.microsoft.outlook.EmailProfile.EmailAddress|This value specifies the email address to be used for sending and receiving mail.  <br/> **Value type**: String  <br/> **Accepted values**: Email address  <br/> **Default if not specified**: \<blank\>  <br/> **Required**: Yes  <br/> **Example**: user@companyname.com  <br/> **Intune Token**<sup>*</sup>: {{mail}}|
|com.microsoft.outlook.EmailProfile.EmailUPN|This value specifies the User Principal Name or username for the email profile that will be used to authenticate the account.  <br/> **Value type**: String  <br/> **Accepted values**: UPN Address or username  <br/> **Default if not specified**: \<blank\>  <br/>**Required**: Yes  <br/> **Example**: userupn@companyname.com  <br/> **Intune Token**<sup>*</sup>: {{userprincipalname}}|
|com.microsoft.outlook.EmailProfile.ServerAuthentication|This value specifies the authentication method for the user.  <br/> **Value type**: String  <br/> **Accepted values**: 'Username and Password'  <br/> **Default if not specified**: 'Username and Password'  <br/> **Required**: No  <br/> **Example**: 'Username and Password'|
|com.microsoft.outlook.EmailProfile.ServerHostName|This value specifies the host name of your Exchange server.  <br/> **Value type**: String  <br/> **Accepted values**: ActiveSync FQDN  <br/> **Default if not specified**: \<blank\>  <br/> **Required**: Yes  <br/> **Example**: mail.companyname.com|
|com.microsoft.outlook.EmailProfile.AccountDomain|This value specifies the user's account domain.  <br/> **Value type**: String  <br/> **Accepted values**: Domain  <br/> **Default if not specified**: \<blank\>  <br/> **Required**: No  <br/>**Example**: companyname|
|com.microsoft.outlook.EmailProfile.AccountType|This value specifies the account type being configured based on the authentication model.  <br/> **Value type**: String  <br/> **Accepted values**: BasicAuth  <br/> **Default if not specified**: BasicAuth  <br/> **Required**: No  <br/> **Example**: BasicAuth|

 <sup>*</sup> Microsoft Intune users can use tokens that will expand to the correct value according to the enrolled user. See [Add app configuration policies for managed iOS devices](/intune/app-configuration-policies-use-ios) for more information.