---
localization_priority: Normal
description: 'Summary: Learn how to configure redirection for Outlook on the web in Exchange Server 2016 or Exchange Server 2019 so http requests are automatically redirected to https.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 5fb6a873-f3cf-4f82-87d1-2ff6e47a0080
ms.reviewer: 
title: Configure http to https redirection for Outlook on the web in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure http to https redirection for Outlook on the web in Exchange Server

By default in Exchange Server, the URL `https://*<ServerName>*` redirects users to `https://*<ServerName>*/owa`. But, if anyone tries to access Outlook on the web (formerly known as Outlook Web App) by using `http://*<ServerName>*` or `http://*<ServerName>*/owa`, they'll get an error.

You can configure http redirection for Outlook on the web so that requests for `http://*<ServerName>*` or `http://*<ServerName>*/owa` are automatically redirected to `https://*<ServerName>*/owa`. This requires the following configuration steps in Internet Information Services (IIS):

1. Remove the **Require SSL** setting from the default website.

2. Restore the **Require SSL** setting on other virtual directories in the default website that had it enabled by default (except for /owa).

3. Configure the default website to redirect http requests to the /owa virtual directory.

4. Remove http redirection from all virtual directories in the default website (including /owa).

5. Reset IIS for the changes to take effect.

For the default SSL and http redirect settings on all virtual directories in the default website, see the [Default Require SSL and HTTP Redirect settings in the default website on an Exchange server](#default-require-ssl-and-http-redirect-settings-in-the-default-website-on-an-exchange-server) section at the end of this topic.

## What do you need to know before you begin?

- Estimated time to complete this procedure: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "IIS Manager" entry in the Outlook on the web permissions section of the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- The procedures in this topic might cause a web.config file to be created in the folder `%ExchangeInstallPath%ClientAccess\OAB`. If you later remove http redirection for Outlook on the web, Outlook might freeze when users click **Send and Receive**. To prevent Outlook from freezing after you remove http redirection, delete the web.config file in `%ExchangeInstallPath%ClientAccess\OAB`.

- Secure Sockets Layer (SSL) is being replaced by Transport Layer Security (TLS) as the protocol that's used to encrypt data sent between computer systems. They're so closely related that the terms "SSL" and "TLS" (without versions) are often used interchangeably. Because of this similarity, references to "SSL" in Exchange topics, the Exchange admin center, and the Exchange Management Shell have often been used to encompass both the SSL and TLS protocols. Typically, "SSL" refers to the actual SSL protocol only when a version is also provided (for example, SSL 3.0). To find out why you should disable the SSL protocol and switch to TLS, check out [Protecting you against the SSL 3.0 vulnerability](https://blogs.office.com/2014/10/29/protecting-ssl-3-0-vulnerability/).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver), [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange), or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Step 1: Use IIS Manager to remove the Require SSL setting from the default website

1. Open IIS Manager on the Exchange server. An easy way to do this in Windows Server 2012 or later is to press Windows key + Q, type inetmgr, and select **Internet Information Services (IIS) Manager** in the results.

2. Expand the server, and expand **Sites**.

3. Select **Default Web Site**. and verify **Features View** is selected at the bottom of the page.

4. In the **IIS** section, double-click **SSL Settings**.

    ![In IIS Manager, select the default website, and then select SSL Settings](../../media/e7a2016d-b4a0-4b5c-a807-71bb362dd2ee.png)

5. On the **SSL Settings** page, clear the **Require SSL** check box, and in the **Actions** pane, click **Apply**.

    ![In IIS Manager, in the SSL settings of the default website, uncheck Require under Require SSL.](../../media/d02befd7-bc3d-4f96-991a-42899b6edb45.png)

**Note**: To perform this procedure on the command line, open an elevated command prompt on the Exchange server (a Command Prompt window you open by selecting **Run as administrator**) and run the following command:

```console
%windir%\system32\inetsrv\appcmd.exe set config "Default Web Site" -section:access -sslFlags:None -commit:APPHOST
```

## Step 2: Use IIS Manager to restore the Require SSL setting on other virtual directories in the default website

When you change the **Require SSL** setting on a website in IIS, the setting is automatically inherited by all virtual directories in the website. Because we're only interested in configuring Outlook on the web, you need to restore the **Require SSL** setting for other virtual directories that had it enabled by default.

Based on the information in the [Default Require SSL and HTTP Redirect settings in the default website on an Exchange server](#default-require-ssl-and-http-redirect-settings-in-the-default-website-on-an-exchange-server) section, use the following procedure to restore the setting on the other virtual directories where **Require SSL** was enabled by default:

1. In IIS Manager, expand the server, expand **Sites**, and expand **Default Web Site**.

2. Select the virtual directory, and verify **Features View** is selected at the bottom of the page.

3. In the **IIS** section, double-click **SSL Settings**.

    ![In IIS Manager, expand the Default Web Site, select the virtual directory, and then select SSL Settings](../../media/32a0f32f-8db2-4e23-b145-7253caf565a7.png)

4. On the **SSL Settings** page, select the **Require SSL** check box, and in the **Actions** pane, click **Apply**.

    ![In IIS Manager, in the SSL settings of the virtual directory, select Require under Require SSL.](../../media/b6d83a19-4f5d-4f9a-b681-8945940a0adc.png)

5. Repeat the previous steps on each virtual directory in the default website that had **Require SSL** enabled by default (except for /owa). The only virtual directories that don't have **Require SSL** enabled by default are /PowerShell and /Rpc.

**Note**: To perform these procedures on the command line, replace _\<VirtualDirectory\>_ with the name of the virtual directory, and run the following command in an elevated command prompt:

```console
%windir%\system32\inetsrv\appcmd.exe set config "Default Web Site/<VirtualDirectory>" -section:Access -sslFlags:Ssl,Ssl128 -commit:APPHOST
```

## Step 3: Use IIS Manager to configure the default website to redirect to the /owa virtual directory.

1. In IIS Manager, expand the server, and expand **Sites**.

2. Select **Default Web Site**. and verify **Features View** is selected at the bottom of the page.

3. In the **IIS** section, double-click **HTTP Redirect**.

    ![In IIS Manager, select the default website, and then select HTTP Redirect](../../media/2d799c1b-1b95-4b2f-8947-54cf534e9864.png)

4. On the **HTTP Redirect** page, configure the following settings:

5. Select the **Redirect requests to this destination** check box, and enter the value `https://*<OWAUrl>*/owa` (For example, <https://webmail.contoso.com/owa>).

6. In the **Redirect Behavior** section, select the **Only redirect requests to content in this directory (not subdirectories)** check box.

7. In the **Status code** list, verify **Found (302)** is selected.

   When you're finished, click **Apply** in the **Actions** pane.

    ![In IIS Manager, select the default web site, and then double-click HTTP Redirect](../../media/0d849f12-0310-4f09-9a12-18d5953c4856.png)

**Note**: To perform this procedure on the command line, replace _\<OWAUrl\>_ with the URL of the OWA virtual directory, open an elevated command prompt and run the following command:

```console
%windir%\system32\inetsrv\appcmd.exe set config "Default Web Site" -section:httpredirect -enabled:true -destination:"https://<OWAUrl>/owa" -childOnly:true
```

## Step 4: Use IIS Manager to remove http redirection from all virtual directories in the default website

When you enable redirection on a website in IIS, the setting is automatically inherited by all virtual directories in the website. Because we're only interested in configuring redirection for the default website, you need to remove the redirect setting from all virtual directories. By default, no directories or virtual directories in the default website are enabled for redirection. For more information, see the [Default Require SSL and HTTP Redirect settings in the default website on an Exchange server](#default-require-ssl-and-http-redirect-settings-in-the-default-website-on-an-exchange-server) section.

Use the following procedure to remove the redirect setting from all virtual directories in the default website (including /owa):

1. In IIS Manager, expand the server, expand **Sites**, and expand **Default Web Site**.

2. Select the virtual directory, and verify **Features View** is selected at the bottom of the page.

3. In the **IIS** section, double-click **HTTP Redirect**.

    ![In IIS Manager, expand the Default Web Site, select the virtual directory, and then select HTTP Redirect](../../media/45dcd4ba-f0bd-463e-891c-edf2571f0611.png)

4. On the **HTTP Redirect** page, change the following settings:

5. Clear the **Only redirect requests to content in this directory (not subdirectories)** check box.

6. Clear the **Redirect requests to this destination** check box.

7. In the **Actions** pane, click **Apply**.

    ![In IIS Manager, expand the default web site, select a virtual directory, and then double-click HTTP Redirect](../../media/6cba00fe-2dde-4d55-8075-82184a5f765e.png)

8. Repeat the previous steps on each virtual directory in the default website.

 **Note**: To perform these procedures on the command line, replace _\<VirtualDirectory\>_ with the name of the virtual directory, and run the following command in an elevated command prompt:

```console
%windir%\system32\inetsrv\appcmd.exe set config "Default Web Site/<VirtualDirectory>" -section:httpredirect -enabled:false -destination:"" -childOnly:false
```

## Step 5: Use IIS Manager to restart IIS

1. In IIS Manager, select the server.

2. In the **Actions** pane, click **Restart**.

    ![In IIS Manager, select the server, and in the Actions pane, click Restart](../../media/7d37436a-b89d-4010-bef4-f4276686d5ad.png)

**Note**: To perform this procedure on the command line, open an elevated command prompt on the Exchange server and run the following commands:

```console
net stop w3svc /y
```

```console
net start w3svc
```

## How do you know this worked?

To verify that you have successfully configured http to https redirection for Outlook on the web, perform the following steps:

1. On a client computer, open a web browser and enter the URL http://*\<ServerName\>*. On the local server, you can use the value http://127.0.0.1 or http://localhost.

2. Verify that you're redirected to Outlook on the web in https, and verify that you can log in successfully.

3. Open the URL http://*\<ServerName\>*/owa (or http://127.0.0.1/owa or http://localhost/owa).

4. Verify that you're redirected to Outlook on the web in https, and verify that you can log in successfully.

## Default Require SSL and HTTP Redirect settings in the default website on an Exchange server

The default **Require SSL** and **HTTP Redirect** settings for the default website and all virtual directories in the default website on an Exchange server are described in the following table.

|**Website**|**Virtual directory**|**Require SSL**|**HTTP Redirect**|
|:-----|:-----|:-----|:-----|
|Default Web Site|n/a|yes|none|
|Default Web Site|API|yes|none|
|Default Web Site|aspnet_client (directory)|yes|none|
|Default Web Site|Autodiscover|yes|none|
|Default Web Site|ecp|yes|none|
|Default Web Site|EWS|yes|none|
|Default Web Site|mapi|yes|none|
|Default Web Site|Microsoft-Server-ActiveSync|yes|none|
|Default Web Site|OAB|yes|none|
|Default Web Site|owa|yes <br/><br/> Subdirectories: <br/>• auth: yes  <br/>• Calendar: no  <br/>• Integrated: yes  <br/>• oma: yes|none|
|Default Web Site|PowerShell|no|none|
|Default Web Site|Rpc|no|none|
