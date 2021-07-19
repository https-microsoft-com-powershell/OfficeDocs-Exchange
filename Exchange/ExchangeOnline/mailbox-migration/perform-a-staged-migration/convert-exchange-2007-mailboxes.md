---
localization_priority: Normal
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: a1f79f3c-4967-4a15-8b3a-f4933aac0c34
ms.reviewer: 
description: Convert Exchange 2007 mailboxes to mail enabled users
title: Convert Exchange 2007 mailboxes to mail-enabled users
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- MED150
- GMA150
- GPA150
- GEA150
- BCS160
audience: Admin
f1.keywords:
- CSH
ms.custom: Adm_O365
ms.service: exchange-online
manager: serdars

---

# Convert Exchange 2007 mailboxes to mail-enabled users

After you have completed a staged migration, convert the mailboxes to mail-enabled users so that the mailboxes can automatically connect to the cloud mailbox.

## Why convert mailboxes to mail-enabled users?

If you've completed a staged Exchange migration to migrate your organization's Exchange 2007 on-premises mailboxes to Microsoft 365 or Office 365 and you want to manage cloud-based users from your on-premises organization (using Active Directory) you should convert the on-premises mailboxes to mail-enabled users (MEUs). Why? Two things happen after a mailbox is migrated to the cloud in a staged Exchange migration:

- A user has an on-premises mailbox and a cloud mailbox.

- Mail sent to the user's on-premises mailbox is forwarded to their cloud mailbox. This happens because during the migration process, the **TargetAddress** property on the on-premises mailbox is populated with the remote routing address of the cloud mailbox. This means that users need to connect to their cloud mailboxes to access their e-mail.

This behavior results in two issues:

- If a person uses Microsoft Outlook to open their mailbox, the Autodiscover service still tries to connect to the on-premises mailbox, and the user won't be able to connect to their cloud mailbox. If there are users that haven't been migrated to the cloud, you can't point your Autodiscover CNAME record to the cloud until all users are migrated.

- If an organization decommissions Exchange after all on-premises mailboxes are migrated to the cloud, messaging-related user information on the cloud mailbox will be lost. The Microsoft Online Services Directory Synchronization tool (DirSync) removes data (such as proxy addresses) from the cloud mailbox object because the on-premises mailbox no longer exists and DirSync can't match it to the corresponding cloud mailbox.

The solution is to convert the on-premises mailbox to a mail-enabled user (MEU) in your on-premises organization after the user's mailbox has been migrated to the cloud. When you convert an on-premises mailbox to an MEU:

- The proxy addresses from a cloud-based mailbox are copied to the new MEU; if you decommission Exchange, these proxy addresses are still retained in Active Directory.

- The properties of the MEU enable DirSync to match the MEU with its corresponding cloud mailbox.

- The Autodiscover service uses the MEU to connect Outlook to the cloud mailbox after the user creates a new Outlook profile.

## PowerShell scripts to create MEUs

You can use the scripts below to collect information about the cloud-based mailboxes, and to convert the Exchange 2007 mailboxes to MEUs.

The following script collects information from your cloud mailboxes and saves it to a CSV file. Run this script first.

Copy the script below and give it a filename ExportO365UserInfo.ps1.

> [!NOTE]
>
> - Before you run the following script, you need to install the Exchange Online PowerShell V2 module. For instructions, see [Install and maintain the EXO V2 module](/powershell/exchange/exchange-online-powershell-v2#install-and-maintain-the-exo-v2-module). The EXO V2 module uses modern authentication.
>
> - Typically, you can use the script as-is if your organization is Microsoft 365 or Microsoft 365 GCC. If your organization is Office 365 Germany, Microsoft 365 GCC High, or Microsoft 365 DoD, you need to edit the `Connect-ExchangeOnline` line in the script. Specifically, you need to use the *ExchangeEnvironmentName* parameter and the appropriate value for your organization type. For more information, see the examples in [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell#connect-to-exchange-online-powershell-without-using-mfa).

```PowerShell
Param($migrationCSVFileName = "migration.csv")
function O365Logon
{
    #Check for current open O365 sessions and allow the admin to either use the existing session or create a new one
    $session = Get-PSSession | ?{$_.ConfigurationName -eq 'Microsoft.Exchange'}
    if($session -ne $null)
    {
        $a = Read-Host "An open session to Exchange Online PowerShell already exists. Do you want to use this session?  Enter y to use the open session, anything else to close and open a fresh session."
        if($a.ToLower() -eq 'y')
        {
            Write-Host "Using existing Exchange Online Powershell session." -ForeGroundColor Green
            return
        }
        Disconnect-ExchangeOnline -Confirm:$false
    }
    Import-Module ExchangeOnlineManagement
    Connect-ExchangeOnline -Prefix "Cloud"
}
function Main
{
    #Verify the migration CSV file exists
    if(!(Test-Path $migrationCSVFileName))
    {
        Write-Host "File $migrationCSVFileName does not exist." -ForegroundColor Red
        Exit
    }
    #Import user list from migration.csv file
    $MigrationCSV = Import-Csv $migrationCSVFileName

    #Get mailbox list based on email addresses from CSV file
    $MailBoxList = $MigrationCSV | %{$_.EmailAddress} | Get-CloudMailbox
    $Users = @()

    #Get LegacyDN, Tenant, and On-Premises Email addresses for the users
    foreach($user in $MailBoxList)
    {
        $UserInfo = New-Object System.Object

        $CloudEmailAddress = $user.EmailAddresses | ?{($_ -match 'onmicrosoft') -and ($_ -match 'smtp:')}
        if ($CloudEmailAddress.Count -gt 1)
        {
            $CloudEmailAddress = $CloudEmailAddress[0].ToString().ToLower().Replace('smtp:', '')
            Write-Host "$user returned more than one cloud email address. Using $CloudEmailAddress" -ForegroundColor Yellow
        }
        else
        {
            $CloudEmailAddress = $CloudEmailAddress.ToString().ToLower().Replace('smtp:', '')
        }

        $UserInfo | Add-Member -Type NoteProperty -Name LegacyExchangeDN -Value $user.LegacyExchangeDN
        $UserInfo | Add-Member -Type NoteProperty -Name CloudEmailAddress -Value $CloudEmailAddress
        $UserInfo | Add-Member -Type NoteProperty -Name OnPremiseEmailAddress -Value $user.PrimarySMTPAddress.ToString()
        $UserInfo | Add-Member -Type NoteProperty -Name MailboxGUID -Value $user.ExchangeGUID
        $Users += $UserInfo
    }
    #Check for existing csv file and overwrite if needed
    if(Test-Path ".\cloud.csv")
    {
        $delete = Read-Host "The file cloud.csv already exists in the current directory. Do you want to delete it?  Enter y to delete, anything else to exit this script."
        if($delete.ToString().ToLower() -eq 'y')
        {
            Write-Host "Deleting existing cloud.csv file" -ForeGroundColor Red
            Remove-Item ".\cloud.csv"
        }
        else
        {
            Write-Host "Will NOT delete current cloud.csv file. Exiting script." -ForeGroundColor Green
            Exit
        }
    }
    $Users | Export-CSV -Path ".\cloud.csv" -notype
    (Get-Content ".\cloud.csv") | %{$_ -replace '"', ''} | Set-Content ".\cloud.csv" -Encoding Unicode
    Write-Host "CSV File Successfully Exported to cloud.csv" -ForeGroundColor Green
}
O365Logon
Main
```

The following script converts on-premises Exchange 2007 mailboxes to MEUs. Run this script after you have ran the script to collect information from the cloud mailboxes.

Copy the script below to a .txt file and then save the file and give it a filename Exchange2007MBtoMEU.ps1.

```PowerShell
param($DomainController = [String]::Empty)
function Main
{
    #Script Logic flow
    #1. Pull User Info from cloud.csv file in the current directory
    #2. Lookup AD Info (DN, mail, proxyAddresses, and legacyExchangeDN) using the SMTP address from the CSV file
    #3. Save existing proxyAddresses
    #4. Add existing legacyExchangeDN's to proxyAddresses
    #5. Delete Mailbox
    #6. Mail-Enable the user using the cloud email address as the targetAddress
    #7. Disable RUS processing
    #8. Add proxyAddresses and mail attribute back to the object
    #9. Add msExchMailboxGUID from cloud.csv to the user object (for offboarding support)

    if($DomainController -eq [String]::Empty)
    {
        Write-Host "You must supply a value for the -DomainController switch" -ForegroundColor Red
        Exit
    }

    $CSVInfo = Import-Csv ".\cloud.csv"
    foreach($User in $CSVInfo)
    {
        Write-Host "Processing user" $User.OnPremiseEmailAddress -ForegroundColor Green
        Write-Host "Calling LookupADInformationFromSMTPAddress" -ForegroundColor Green
        $UserInfo = LookupADInformationFromSMTPAddress($User)

        #Check existing proxies for On-Premises and Cloud Legacy DN's as x500 proxies. If not present add them.
        if($UserInfo.ProxyAddresses -notcontains ("X500:"+$UserInfo.CloudLegacyDN))
        {
            $X500Proxy = "x500:" + $UserInfo.CloudLegacyDN
            Write-Host "Adding $X500Proxy to EmailAddresses" -ForegroundColor Green
            $UserInfo.ProxyAddresses.Add($X500Proxy)
        }
        if($UserInfo.ProxyAddresses -notcontains ("X500:"+$UserInfo.LegacyDN))
        {
            $X500Proxy = "x500:" + $UserInfo.LegacyDN
            Write-Host "Adding $X500Proxy to EmailAddresses" -ForegroundColor Green
            $UserInfo.ProxyAddresses.Add($X500Proxy)
        }

        #Disable Mailbox
        Write-Host "Disabling Mailbox" -ForegroundColor Green
        Disable-Mailbox -Identity $UserInfo.OnPremiseEmailAddress -DomainController $DomainController -Confirm:$false

        #Mail Enable
        Write-Host "Enabling Mailbox" -ForegroundColor Green
        Enable-MailUser  -Identity $UserInfo.Identity -ExternalEmailAddress $UserInfo.CloudEmailAddress -DomainController $DomainController

        #Disable RUS
        Write-Host "Disabling RUS" -ForegroundColor Green
        Set-MailUser -Identity $UserInfo.Identity -EmailAddressPolicyEnabled $false -DomainController $DomainController

        #Add Proxies and Mail
        Write-Host "Adding EmailAddresses and WindowsEmailAddress" -ForegroundColor Green
        Set-MailUser -Identity $UserInfo.Identity -EmailAddresses $UserInfo.ProxyAddresses -WindowsEmailAddress $UserInfo.Mail -DomainController $DomainController

        #Set Mailbox GUID. Need to do this via S.DS as Set-MailUser doesn't expose this property.
        $ADPath = "LDAP://" + $DomainController + "/" + $UserInfo.DistinguishedName
        $ADUser = New-Object -TypeName System.DirectoryServices.DirectoryEntry -ArgumentList $ADPath
        $MailboxGUID = New-Object -TypeName System.Guid -ArgumentList $UserInfo.MailboxGUID
        [Void]$ADUser.psbase.invokeset('msExchMailboxGUID',$MailboxGUID.ToByteArray())
        Write-Host "Setting Mailbox GUID" $UserInfo.MailboxGUID -ForegroundColor Green
        $ADUser.psbase.CommitChanges()

        Write-Host "Migration Complete for" $UserInfo.OnPremiseEmailAddress -ForegroundColor Green
        Write-Host ""
        Write-Host ""
    }
}

function LookupADInformationFromSMTPAddress($CSV)
{
    $Mailbox = Get-Mailbox $CSV.OnPremiseEmailAddress -ErrorAction SilentlyContinue

    if($Mailbox -eq $null)
    {
        Write-Host "Get-Mailbox failed for" $CSV.OnPremiseEmailAddress -ForegroundColor Red
        continue
    }

    $UserInfo = New-Object System.Object

    $UserInfo | Add-Member -Type NoteProperty -Name OnPremiseEmailAddress -Value $CSV.OnPremiseEmailAddress
    $UserInfo | Add-Member -Type NoteProperty -Name CloudEmailAddress -Value $CSV.CloudEmailAddress
    $UserInfo | Add-Member -Type NoteProperty -Name CloudLegacyDN -Value $CSV.LegacyExchangeDN
    $UserInfo | Add-Member -Type NoteProperty -Name LegacyDN -Value $Mailbox.LegacyExchangeDN
    $ProxyAddresses = New-Object Microsoft.Exchange.Data.ProxyAddressCollection
    $ProxyAddresses = $Mailbox.EmailAddresses
    $UserInfo | Add-Member -Type NoteProperty -Name ProxyAddresses -Value $ProxyAddresses
    $UserInfo | Add-Member -Type NoteProperty -Name Mail -Value $Mailbox.WindowsEmailAddress
    $UserInfo | Add-Member -Type NoteProperty -Name MailboxGUID -Value $CSV.MailboxGUID
    $UserInfo | Add-Member -Type NoteProperty -Name Identity -Value $Mailbox.Identity
    $UserInfo | Add-Member -Type NoteProperty -Name DistinguishedName -Value (Get-User $Mailbox.Identity).DistinguishedName

    $UserInfo
}
Main
```

## Setup steps to convert on-premises mailboxes to MEUs

Follow these steps to complete the process.

1. Copy ExportO365UserInfo.ps1, Exchange2007MBtoMEU.ps1, and the CSV file used to run the migration batch to the same directory in your on-premises server.

2. Rename the migration CSV file to migration.csv.

3. In the Exchange Management Shell, run the following command. The script assumes that the CSV file is in the same directory and is named migration.csv.

   ```PowerShell
   .\ExportO365UserInfo.ps1
   ```

   You will be prompted to use the existing session or open a new session.

4. Type n and press **Enter** to open a new session.

    The script runs and then saves the Cloud.csv file to the current working directory.

5. Enter the administrator credentials for your cloud-based organization and then click **OK.**

6. Run the following command in a new Exchange Management Shell session. This command assumes that ExportO365UserInfo.ps1 and Cloud.csv are located in the same directory.

   ```PowerShell
   .\Exchange2007MBtoMEU.ps1 <FQDN of on-premises domain controller>
   ```

   For example:

   ```PowerShell
   .\Exchange2007MBtoMEU.ps1 DC1.contoso.com
   ```

   The script converts on-premises mailboxes to MEUs for all users included in the Cloud.csv.

7. Verify that the new MEUs have been created. In Active Directory Users and Computers, do the following:

   1. Click Action \> Find

   2. Click the Exchange tab

   3. Select **Show only Exchange recipients**, and then select **Users with external email address**.

   4. Click **Find Now**.

      The mailboxes that were converted to MEUs are listed under **Search results**.

8. Use Active Directory Users and Computers, ADSI Edit, or Ldp.exe to verify that the following MEU properties are populated with the correct information.

   - legacyExchangeDN
   - mail
   - msExchMailboxGuid
   - proxyAddresses
   - targetAddress