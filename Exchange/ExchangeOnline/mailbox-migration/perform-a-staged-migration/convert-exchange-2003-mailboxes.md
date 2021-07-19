---
localization_priority: Normal
ms.topic: article
author: msdmaguire
ms.author: jhendr
ms.assetid: 5296a30b-00cb-44be-8855-ed9d14d93e17
ms.reviewer: 
description: Convert Exchange 2003 mailboxes to mail enabled users.
title: Convert Exchange 2003 mailboxes to mail-enabled users
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- MED150
- GMA150
- MBS150
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

# Convert Exchange 2003 mailboxes to mail-enabled users

After you have completed a staged migration, convert the mailboxes to mail-enabled users so that the mailboxes can automatically connect to the cloud mailbox.

## Why convert mailboxes to mail-enabled users?

If you've completed a staged Exchange migration to migrate your organization's Exchange 2003 on-premises mailboxes to Microsoft 365 or Office 365 and you want to manage cloud-based users from your on-premises organization (using Active Directory) you should convert the on-premises mailboxes to mail-enabled users (MEUs).

This article includes a Windows PowerShell script that collects information from the cloud-based mailboxes and a Visual Basic (VB) script that you can run to convert Exchange 2003 mailboxes to MEUs. When you run this script, the proxy addresses from the cloud-based mailbox are copied to the MEU, which resides in Active Directory. Also, the properties of the MEU enable the Microsoft Online Services Directory Synchronization tool (DirSync) to match the MEU with its corresponding cloud mailbox

It's recommended that you convert on-premises mailboxes to MEUs for a migration batch. After a staged Exchange migration batch is finished and you have verified that all mailboxes in the batch are successfully migrated and the initial synchronization of mailbox items to the cloud is complete, convert the mailboxes in the migration batch to MEUs.

## PowerShell script to collect data from cloud mailboxes

You can use the scripts below to collect information about the cloud-based mailboxes, and to convert the Exchange 2003 mailboxes to MEUs.

The following script collects information from your cloud mailboxes and saves it to a CSV file. Run this script first.

Copy the script below to a .txt file and then save the file and save it as ExportO365UserInfo.ps1.

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
            Write-Host "Using existing Exchange Online Powershell Session." -ForeGroundColor Green
            return
        }
        Disconnect-ExchangeOnline -Confirm:$false
    }
    Import-Module ExchangeOnlineManagement
    Connect-ExchangeOnline
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
    $MailBoxList = $MigrationCSV | %{$_.EmailAddress} | Get-Mailbox
    $Users = @()
    #Get LegacyDN, Tenant, and On-Premises Email addresses for the users
    foreach($user in $MailBoxList)
    {
        $UserInfo = New-Object System.Object
        $CloudEmailAddress = $user.EmailAddresses | ?{($_ -match 'onmicrosoft') -and ($_ -cmatch 'smtp:')}
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

The following Visual Basic script converts on-premises Exchange 2003 mailboxes to MEUs. Run this script after you have ran the script to collect information from the cloud mailboxes.

Copy the script below to a .txt file and then save the file as Exchange2003MBtoMEU.vbs.

```VB.net
'Globals/Constants
Const ADS_PROPERTY_APPEND = 3
Dim UserDN
Dim remoteSMTPAddress
Dim remoteLegacyDN
Dim domainController
Dim csvMode
csvMode = FALSE
Dim csvFileName
Dim lastADLookupFailed
Class UserInfo
    public OnPremiseEmailAddress
    public CloudEmailAddress
    public CloudLegacyDN
    public LegacyDN
    public ProxyAddresses
    public Mail
    public MailboxGUID
    public DistinguishedName
    Public Sub Class_Initialize()
        Set ProxyAddresses = CreateObject("Scripting.Dictionary")
    End Sub
End Class
'Command Line Parameters
If WScript.Arguments.Count = 0 Then
    'No parameters passed
    WScript.Echo("No parameters were passed.")
    ShowHelp()
ElseIf StrComp(WScript.Arguments(0), "-c", vbTextCompare) = 0 And WScript.Arguments.Count = 2 Then
    WScript.Echo("Missing DC Name.")
    ShowHelp()
ElseIf StrComp(WScript.Arguments(0), "-c", vbTextCompare) = 0 Then
    'CSV Mode
    csvFileName = WScript.Arguments(1)
    domainController = WScript.Arguments(2)
    csvMode = TRUE
    WScript.Echo("CSV mode detected. Filename: " & WScript.Arguments(1) & vbCrLf)
ElseIf wscript.Arguments.Count <> 4 Then
    'Invalid Arguments
    WScript.Echo WScript.Arguments.Count
    Call ShowHelp()
Else
    'Manual Mode
    UserDN = wscript.Arguments(0)
    remoteSMTPAddress = wscript.Arguments(1)
    remoteLegacyDN = wscript.Arguments(2)
    domainController = wscript.Arguments(3)
End If
Main()
'Main entry point
Sub Main
    'Check for CSV Mode
    If csvMode = TRUE Then
        UserInfoArray = GetUserInfoFromCSVFile()
    Else
        WScript.Echo "Manual Mode Detected" & vbCrLf
        Set info = New UserInfo
        info.CloudEmailAddress = remoteSMTPAddress
        info.DistinguishedName = UserDN
        info.CloudLegacyDN = remoteLegacyDN
        ProcessSingleUser(info)
    End If
End Sub
'Process a single user (manual mode)
Sub ProcessSingleUser(ByRef UserInfo)
    userADSIPath = "LDAP://" & domainController & "/" & UserInfo.DistinguishedName
    WScript.Echo "Processing user " & userADSIPath
    Set MyUser = GetObject(userADSIPath)
    proxyCounter = 1
    For Each address in MyUser.Get("proxyAddresses")
        UserInfo.ProxyAddresses.Add proxyCounter, address
        proxyCounter = proxyCounter + 1
    Next
    UserInfo.OnPremiseEmailAddress = GetPrimarySMTPAddress(UserInfo.ProxyAddresses)
    UserInfo.Mail = MyUser.Get("mail")
    UserInfo.MailboxGUID = MyUser.Get("msExchMailboxGUID")
    UserInfo.LegacyDN = MyUser.Get("legacyExchangeDN")
    ProcessMailbox(UserInfo)
End Sub
'Populate user info from CSV data
Function GetUserInfoFromCSVFile()
    CSVInfo = ReadCSVFile()
    For i = 0 To (UBound(CSVInfo)-1)
        lastADLookupFailed = false
        Set info = New UserInfo
        info.CloudLegacyDN = Split(CSVInfo(i+1), ",")(0)
        info.CloudEmailAddress = Split(CSVInfo(i+1), ",")(1)
        info.OnPremiseEmailAddress = Split(CSVInfo(i+1), ",")(2)
        WScript.Echo "Processing user " & info.OnPremiseEmailAddress
        WScript.Echo "Calling LookupADInformationFromSMTPAddress"
        LookupADInformationFromSMTPAddress(info)
        If lastADLookupFailed = false Then
            WScript.Echo "Calling ProcessMailbox"
            ProcessMailbox(info)
        End If
        set info = nothing
    Next
End Function
'Populate user info from AD
Sub LookupADInformationFromSMTPAddress(ByRef info)
    'Lookup the rest of the info in AD using the SMTP address
    Set objRootDSE = GetObject("LDAP://RootDSE")
    strDomain = objRootDSE.Get("DefaultNamingContext")
    Set objRootDSE = nothing
    Set objConnection = CreateObject("ADODB.Connection")
    objConnection.Provider = "ADsDSOObject"
    objConnection.Open "Active Directory Provider"
    Set objCommand = CreateObject("ADODB.Command")
    BaseDN = "<LDAP://" & domainController & "/" & strDomain & ">"
    adFilter = "(&(proxyAddresses=SMTP:" & info.OnPremiseEmailAddress & "))"
    Attributes = "distinguishedName,msExchMailboxGUID,mail,proxyAddresses,legacyExchangeDN"
    Query = BaseDN & ";" & adFilter & ";" & Attributes & ";subtree"
    objCommand.CommandText = Query
    Set objCommand.ActiveConnection = objConnection
    On Error Resume Next
    Set objRecordSet = objCommand.Execute
    'Handle any errors that result from the query
    If Err.Number <> 0 Then
        WScript.Echo "Error encountered on query " & Query & ". Skipping user."
        lastADLookupFailed = true
        return
    End If
    'Handle zero or ambiguous search results
    If objRecordSet.RecordCount = 0 Then
        WScript.Echo "No users found for address " & info.OnPremiseEmailAddress
        lastADLookupFailed = true
        return
    ElseIf objRecordSet.RecordCount > 1 Then
        WScript.Echo "Ambiguous search results for email address " & info.OnPremiseEmailAddress
        lastADLookupFailed = true
        return
    ElseIf Not objRecordSet.EOF Then
        info.LegacyDN = objRecordSet.Fields("legacyExchangeDN").Value
        info.Mail = objRecordSet.Fields("mail").Value
        info.MailboxGUID = objRecordSet.Fields("msExchMailboxGUID").Value
        proxyCounter = 1
        For Each address in objRecordSet.Fields("proxyAddresses").Value
            info.ProxyAddresses.Add proxyCounter, address
            proxyCounter = proxyCounter + 1
        Next
        info.DistinguishedName = objRecordSet.Fields("distinguishedName").Value
        objRecordSet.MoveNext
    End If
    objConnection = nothing
    objCommand = nothing
    objRecordSet = nothing
    On Error Goto 0
End Sub
'Populate data from the CSV file
Function ReadCSVFile()
    'Open file
    Set objFS = CreateObject("Scripting.FileSystemObject")
    Set objTextFile = objFS.OpenTextFile(csvFileName, 1, false, -1)
    'Loop through each line, putting each line of the CSV file into an array to be returned to the caller
    counter = 0
    Dim CSVArray()
    Do While NOT objTextFile.AtEndOfStream
        ReDim Preserve CSVArray(counter)
        CSVArray(counter) = objTextFile.ReadLine
        counter = counter + 1
    Loop
    'Close and return
    objTextFile.Close
    Set objTextFile = nothing
    Set objFS = nothing
    ReadCSVFile = CSVArray
End Function
'Process the migration
Sub ProcessMailbox(User)
    'Get user properties
    userADSIPath = "LDAP://" & domainController & "/" & User.DistinguishedName
    Set MyUser = GetObject(userADSIPath)
    'Add x.500 address to list of existing proxies
    existingLegDnFound = FALSE
    newLegDnFound = FALSE
    'Loop through each address in User.ProxyAddresses
    For i = 1 To User.ProxyAddresses.Count
        If StrComp(address, "x500:" & User.LegacyDN, vbTextCompare) = 0 Then
            WScript.Echo "x500 proxy " & User.LegacyDN & " already exists"
            existingLegDNFound = true
        End If
        If StrComp(address, "x500:" & User.CloudLegacyDN, vbTextCompare) = 0 Then
            WScript.Echo "x500 proxy " & User.CloudLegacyDN & " already exists"
            newLegDnFound = true
        End If
    Next
    'Add existing leg DN to proxy list
    If existingLegDnFound = FALSE Then
        WScript.Echo "Adding existing legacy DN " & User.LegacyDN & " to proxy addresses"
        User.ProxyAddresses.Add (User.ProxyAddresses.Count+1),("x500:" & User.LegacyDN)
    End If
    'Add new leg DN to proxy list
    If newLegDnFound = FALSE Then
        'Add new leg DN to proxy addresses
        WScript.Echo "Adding new legacy DN " & User.CloudLegacyDN & " to existing proxy addresses"
        User.ProxyAddresses.Add (User.ProxyAddresses.Count+1),("x500:" & User.CloudLegacyDN)
    End If
    'Dump out new list of addresses
    WScript.Echo "Original proxy addresses updated count: " & User.ProxyAddresses.Count
    For i = 1 to User.ProxyAddresses.Count
        WScript.Echo " proxyAddress " & i & ": " & User.ProxyAddresses(i)
    Next
    'Delete the Mailbox
    WScript.Echo "Opening " & userADSIPath & " as CDOEXM::IMailboxStore object"
    Set Mailbox = MyUser
    Wscript.Echo "Deleting Mailbox"
    On Error Resume Next
    Mailbox.DeleteMailbox
    'Handle any errors deleting the mailbox
    If Err.Number <> 0 Then
        WScript.Echo "Error " & Err.number & ". Skipping User." & vbCrLf & "Description: " & Err.Description & vbCrLf
        Exit Sub
    End If
    On Error Goto 0
    'Save and continue
    WScript.Echo "Saving Changes"
    MyUser.SetInfo
    WScript.Echo "Refeshing ADSI Cache"
    MyUser.GetInfo
    Set Mailbox = nothing
    'Mail Enable the User
    WScript.Echo "Opening " & userADSIPath & " as CDOEXM::IMailRecipient"
    Set MailUser = MyUser
    WScript.Echo "Mail Enabling user using targetAddress " & User.CloudEmailAddress
    MailUser.MailEnable User.CloudEmailAddress
    WScript.Echo "Disabling Recipient Update Service for user"
    MyUser.PutEx ADS_PROPERTY_APPEND, "msExchPoliciesExcluded", Array("{26491CFC-9E50-4857-861B-0CB8DF22B5D7}")
    WScript.Echo "Saving Changes"
    MyUser.SetInfo
    WScript.Echo "Refreshing ADSI Cache"
    MyUser.GetInfo
    'Add Legacy DN back on to the user
    WScript.Echo "Writing legacyExchangeDN as " & User.LegacyDN
    MyUser.Put "legacyExchangeDN", User.LegacyDN
    'Add old proxies list back on to the MEU
    WScript.Echo "Writing proxyAddresses back to the user"
    For j=1 To User.ProxyAddresses.Count
        MyUser.PutEx ADS_PROPERTY_APPEND, "proxyAddresses", Array(User.ProxyAddresses(j))
        MyUser.SetInfo
        MyUser.GetInfo
    Next
    'Add mail attribute back on to the MEU
    WScript.Echo "Writing mail attribute as " & User.Mail
    MyUser.Put "mail", User.Mail
    'Add msExchMailboxGUID back on to the MEU
    WScript.Echo "Converting mailbox GUID to writable format"
    Dim mbxGUIDByteArray
    Call ConvertHexStringToByteArray(OctetToHexString(User.MailboxGUID), mbxGUIDByteArray)
    WScript.Echo "Writing property msExchMailboxGUID to user object with value " & OctetToHexString(User.MailboxGUID)
    MyUser.Put "msExchMailboxGUID", mbxGUIDByteArray
    WScript.Echo "Saving Changes"
    MyUser.SetInfo
    WScript.Echo "Migration Complete!" & vbCrLf
End Sub
'Returns the primary SMTP address of a user
Function GetPrimarySMTPAddress(Addresses)
    For Each address in Addresses
        If Left(address, 4) = "SMTP" Then GetPrimarySMTPAddress = address
    Next
End Function
'Converts Hex string to byte array for writing to AD
Sub ConvertHexStringToByteArray(ByVal strHexString, ByRef pByteArray)
    Set FSO = CreateObject("Scripting.FileSystemObject")
    Set Stream = CreateObject("ADODB.Stream")
    Temp = FSO.GetTempName()
    Set TS = FSO.CreateTextFile(Temp)
    For i = 1 To (Len (strHexString) -1) Step 2
        TS.Write Chr("&h" & Mid (strHexString, i, 2))
    Next
    TS.Close
    Stream.Type = 1
    Stream.Open
    Stream.LoadFromFile Temp
    pByteArray = Stream.Read
    Stream.Close
    FSO.DeleteFile Temp
    Set Stream = nothing
    Set FSO = Nothing
End Sub
'Converts raw bytes from AD GUID to readable string
Function OctetToHexString (arrbytOctet)
    OctetToHexStr = ""
    For k = 1 To Lenb (arrbytOctet)
        OctetToHexString = OctetToHexString & Right("0" & Hex(Ascb(Midb(arrbytOctet, k, 1))), 2)
    Next
End Function
Sub ShowHelp()
    WScript.Echo("This script runs in two modes, CSV Mode and Manual Mode." & vbCrLf & "CSV Mode allows you to specify a CSV file from which to pull usernames." & vbCrLf& "Manual mode allows you to run the script against a single user.")
    WSCript.Echo("Both modes require you to specify the name of a DC to use in the local domain." & vbCrLf & "To run the script in CSV Mode, use the following syntax:")
    WScript.Echo("  cscript Exchange2003MBtoMEU.vbs -c x:\csv\csvfilename.csv dc.domain.com")
    WScript.Echo("To run the script in Manual Mode, you must specify the users AD Distinguished Name, Remote SMTP Address, Remote Legacy Exchange DN, and Domain Controller Name.")
    WSCript.Echo("  cscript Exchange2003MBtoMEU.vbs " & chr(34) & "CN=UserName,CN=Users,DC=domain,DC=com" & chr(34) & " " & chr(34) & "user@cloudaddress.com" & chr(34) & " " & chr(34) & "/o=Cloud Org/ou=Cloud Site/ou=Recipients/cn=CloudUser" & chr(34) & " dc.domain.com")
    WScript.Quit
End Sub
```

## What do the scripts do?

### ExportO365UserInfo.ps1

This is a Windows PowerShell script that you run in your cloud based organization to collect information about the cloud mailboxes that you migrated during the staged Exchange migration. It uses a CSV file to scope the batch of users. It's recommended that you use the same migration CSV file that you used to migrate a batch of users

When you run the ExportO365UserInfo script:

- The following properties are collected from the cloud mailboxes for the users listed in the input CSV file:

  - Primary SMTP address

  - Primary SMTP address of the corresponding on-premises mailbox

  - Other proxy addresses for the cloud mailbox

  - LegacyExchangeDN

- The collected properties are saved to a CSV file named Cloud.csv.

### Exchange2003MBtoMEU.vbs

This a VB script that you run in your on-premises Exchange 2003 organization to convert mailboxes to MEUs. It uses the Cloud.csv file, which is output by the ExportO365UserInfo script.

When you run the Exchange2003MBtoMEU.vbs script, it does the following for each mailbox listed in input CSV file:

- Collects information from the input CSV file and from the on-premises mailbox.

- Creates a list of proxy addresses from the on-premises and cloud mailbox to add to the MEU.

- Deletes the on-premises mailbox.

- Creates a MEU and populates the following properties:

  - **legacyExchangeDN**: Value from the on-premises mailbox.

  - **mail**: The primary SMTP of the cloud mailbox.

  - **msExchMailboxGuid**: Value from the on-premises mailbox.

  - **proxyAddresses**: Values from both the on-premises mailbox and the cloud mailbox.

  - **targetAddress**: Read from the on-premises mailbox; the value is the primary SMTP of the cloud mailbox.

    > [!IMPORTANT]
    > To enable off-boarding from Microsoft 365 or Office 365 to Exchange 2003, you have to replace the value of msExchMailboxGuid on the MEU with the Guid from the cloud-based mailbox. To obtain the Guids for the mailboxes in your cloud organization and save them to a CSV file, run the following PowerShell command:

    ```PowerShell
    Get-Mailbox | Select PrimarySmtpAddress, Guid | Export-csv -Path .\guid.csv
    ```

    This command extracts the primary SMTP address and Guid for all cloud mailboxes into the guid.csv file, and then saves this file to the current directory.

Instead of using the input CSV file to convert a batch of mailboxes, you can run the Exchange2003MBtoMEU.vbs script in manual mode to convert one mailbox at a time. To do this, you will need to provide the following input parameters:

- The distinguished name (DN)of the on-premises mailbox.

- The primary SMTP address of the cloud mailbox.

- The Exchange Legacy DN for the cloud mailbox.

- A domain controller name in your Exchange 2003 organization.

## Steps to convert on-premises mailboxes to MEUs

1. Run the ExportO365UserInfo in your cloud organization. Use the CSV file for the migration batch as the input file. The script creates a CSV file named Cloud.csv.

   ```PowerShell
   .\ExportO365UserInfo.ps1 <CSV input file>
   ```

   For example:

   ```PowerShell
   .\ExportO365UserInfo.ps1 .\MigrationBatch1.csv
   ```

   This example assumes that the script and input CSV file are located in the same directory.

2. Copy Exchange2003MBtoMEU.vbs and Cloud.csv to the same directory in your on-premises organization.

3. In your on-premises organization, run the following command:

    ```vbscript
    cscript Exchange2003MBtoMEU.vbs -c .\Cloud.csv <FQDN of on-premises domain controller>
    ```

    For example:

    ```vbscript
    cscript Exchange2003MBtoMEU.vbs -c .\Cloud.csv DC1.contoso.com
    ```

    To run the script in manual mode, enter the following command. Use spaces between each value.

    ```vbscript
    cscript Exchange2003MBtoMEU.vbs "<DN of on-premises mailbox>" "<Primary SMTP of cloud mailbox>" "<ExchangeLegacyDN of cloud mailbox>" <FQDN of on-premises domain controller>
    ```

    For example:

    ```vbscript
    cscript Exchange2003MBtoMEU.vbs "CN=Ann Beebe,CN=Users,DC=contoso,DC=com" "annb@contoso.onmicrosoft.com" "/o=First Organization/ou=Exchange Administrative Group (FYDIBOHF23SPDLT)/cn=Recipients/cn=d808d014cec5411ea6de1f70cc116e7b-annb" DC1.contoso.com
    ```

4. Verify that the new MEUs have been created. In Active Directory Users and Computers, do the following:

   1. Click **Action** \> **Find**.

   2. Click the **Exchange tab**.

   3. Select **Show only Exchange recipients**, and then select **Users with external email address**.

   4. Click **Find Now**.

      The mailboxes that were converted to MEUs are listed under **Search results**.

5. Use Active Directory Users and Computers, ADSI Edit, or Ldp.exe to verify that the following MEU properties are populated with the correct information.

   - legacyExchangeDN
   - mail
   - msExchMailboxGuid<sup>*</sup>
   - proxyAddresses
   - targetAddress

    <sup>*</sup> As previously explained, the Exchange2003MBtoMEU.vbs script retains the **msExchMailboxGuid** value from the on-premises mailbox. To enable off-boarding from Microsoft 365 or Office 365 to Exchange 2003, you have to replace the value for the **msExchMailboxGuid** property on the MEU with the Guid from the cloud-based mailbox.