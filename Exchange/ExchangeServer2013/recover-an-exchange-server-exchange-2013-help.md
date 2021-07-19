---
title: 'Recover an Exchange Server: Exchange 2013 Help'
TOCTitle: Recover an Exchange Server
ms:assetid: 46e9a1cf-b64c-43c3-a898-6171176da761
ms:mtpsurl: https://technet.microsoft.com/library/Dd876880(v=EXCHG.150)
ms:contentKeyID: 48385039
ms.reviewer: 
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Recover an Exchange Server

_**Applies to:** Exchange Server 2013_

You can recover a lost server by using the **Setup /m:RecoverServer** switch in Microsoft Exchange Server 2013. Most of the settings for a computer running Exchange 2013 are stored in Active Directory. The */m:RecoverServer* switch rebuilds an Exchange server with the same name by using the settings and other information stored in Active Directory.

Recovering a lost Exchange server is often accomplished by using new hardware. However, you can also use an existing server.

This topic shows you how to recover a lost Exchange 2013 server that isn't a member of a database availability group (DAG). For detailed steps about how to recover a server that was a member of a DAG, see [Recover a database availability group member server](recover-a-database-availability-group-member-server-exchange-2013-help.md).

> [!NOTE]
> If Exchange is installed in a location other than the default location, you must use the <EM>/TargetDir</EM> switch to specify the location of the Exchange binary files. If you don't use the <EM>/TargetDir</EM> switch, the Exchange files are installed in the default location (%programfiles%\Microsoft\Exchange Server\V15).<BR>To determine the install location, follow these steps:
> <OL>
> <LI>
> <P>Open ADSIEDIT.MSC or LDP.EXE.</P>
> <LI>
> <P>Navigate to the following location: <STRONG>CN=ExServerName,CN=Servers,CN=First Administrative Group,CN=Administrative Groups,CN=ExOrg Name,CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=DomainName,CN=Com</STRONG></P>
> <LI>
> <P>Right-click the Exchange server object, and then click <STRONG>Properties</STRONG>.</P>
> <LI>
> <P>Locate the <STRONG>msExchInstallPath</STRONG> attribute. This attribute stores the current installation path.</P></LI></OL>

Looking for other management tasks related to backing up and restoring data? Check out [Backup, restore, and disaster recovery](backup-restore-and-disaster-recovery-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 20 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Exchange infrastructure permissions" section in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- The server on which recovery is being performed must be running the same operating system as the lost server. For example, you can't recover a server that was running Exchange 2013 and Windows Server 2008 R2 on a server running Windows Server 2012, or vice versa. Likewise, you can't recover a server that was running Exchange 2013 and Windows Server 2012 on a server running Windows Server 2012 R2, or vice versa.

- The same disk drive letters on the failed server for mounted databases must exist on the server on which you're running recovery.

- The server on which recovery is being performed should have the same performance characteristics and hardware configuration as the lost server.

- The _/Mode:RecoverServer_ switch assigns a self-signed certificate to all Exchange Services that require SSL/TLS. If the server previously used an SSL/TLS certificate that was issued by a different certification authority, you'll need to re-import the certificate and configure the services to use the certificate. Otherwise, users will get a certificate prompt when they try to connect (for example, in Outlook).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Recover a Lost Exchange Server

1. Reset the computer account for the lost server. For detailed steps, see [Reset a Computer Account](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753596(v=ws.11)).

2. Install the proper operating system and name the new server with the same name as the lost server. Recovery won't succeed if the server on which recovery is being performed doesn't have the same name as the lost server.

3. Join the server to the same domain as the lost server.

4. Install the necessary prerequisites and operating system components. For details, see [Exchange 2013 system requirements](exchange-2013-system-requirements-exchange-2013-help.md) and [Exchange 2013 prerequisites](exchange-2013-prerequisites-exchange-2013-help.md).

5. Log on to the server being recovered and open a command prompt.

6. Navigate to the Exchange 2013 installation files, and run the following command.

    ```powershell
    Setup /m:RecoverServer /IAcceptExchangeServerLicenseTerms
    ```

7. After Setup has completed, but before the recovered server is put into production, reconfigure any custom settings that were previously present on the server, and then restart the server.

## How do you know this worked?

The successful completion of Setup will be the primary indicator that the recovery was successful. To further verify that you've successfully recovered a lost server, open the Windows Services tool (services.msc) and verify that the Microsoft Exchange services have been installed and are running.

### Possible issues with the Scripting Agent

If you previously enabled the Scripting Agent in your Exchange organization, the recovery process might fail. The error will look like this:

```console
"Initialization failed: '"Scripting Agent initialization failed: "File is not found: 'C:\Program Files\Microsoft\Exchange Server\V15\Bin\CmdletExtensionAgents\ScriptingAgentConfig.xml'.""' ---> Microsoft.Exchange.Provisioning.ProvisioningException: "Scripting Agent initialization failed: "File is not found: 'C:\Program Files\Microsoft\Exchange Server\V15\Bin\CmdletExtensionAgents\ScriptingAgentConfig.xml'."" ---> System.IO.FileNotFoundException: "File is not found: 'C:\Program Files\Microsoft\Exchange Server\V15\Bin\CmdletExtensionAgents\ScriptingAgentConfig.xml'."
```

If you have other Exchange servers in your organization, you'11 need to:

1. Disable the Scripting Agent in the Exchange Management Shell on an existing server:

   ```powershell
   Disable-CmdletExtensionAgent -Identity "Scripting Agent"
   ```

2. Run Exchange Setup in recovery mode as described earlier in this topic.

3. Enable the Scripting Agent in the Exchange Management Shell after the Exchange server recovery is complete:

   ```powershell
   Enable-CmdletExtensionAgent -Identity "Scripting Agent"
   ```

If the recovered Exchange server is the only Exchange server in your organization, you'll need to:

1. Rename the file %ExchangeInstallPath%Bin\CmdletExtensionAgents\ScriptingAgentConfig.**xml.sample** to %ExchangeInstallPath%Bin\CmdletExtensionAgents\ScriptingAgentConfig.**xml**.

    The default value of %ExchangeInstallationPath% is %ProgramFiles%\Microsoft\Exchange Server\V15\, but the actual value is wherever you installed Exchange on the server.

2. Re-run Exchange Setup in recovery mode as described earlier in this topic.