---
title: Troubleshooting ECP Health Set
TOCTitle: Troubleshooting ECP Health Set
ms:assetid: 0a1cfcd5-585c-4a0a-9d3c-28dc49e16a6c
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.scom.ecp(v=EXCHG.150)
ms:contentKeyID: 49720722
ms.reviewer:
manager: serdars
ms.author: dmaguire
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Troubleshooting ECP Health Set

_**Applies to:** Exchange Server 2013_

The Exchange Control Panel (ECP) health set monitors the overall health of the Exchange admin center (EAC) and of the Outlook Web App (OWA) user setting service. The ECP health set is closely related to the following health set:

[Troubleshooting ECP.Proxy Health Set](troubleshooting-ecp-proxy-health-set.md)

If you receive an alert that specifies that the ECP health set is unhealthy, this indicates an issue that may prevent users from accessing the EAC.

## Explanation

The EAC service is monitored by using the following probes and monitors.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Probe</th>
<th>Health Set</th>
<th>Dependencies</th>
<th>Associated Monitors</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>EacSelfTestProbe</p></td>
<td><p>ECP</p></td>
<td><p>Active Directory</p></td>
<td><p>EacSelfTestMonitor</p></td>
</tr>
<tr class="even">
<td><p>EacDeepTestProbe</p></td>
<td><p>ECP</p></td>
<td><p>Active Directory</p></td>
<td><p>EacDeepTestMonitor</p></td>
</tr>
</tbody>
</table>

For more information about probes and monitors, see [Server health and performance](../../server-health-and-performance-exchange-2013-help.md).

## User Action

When you receive an alert from a health set, the email message contains the following information:

- Name of the server that sent the alert

- Time and date when the alert occurred

- Authentication and credential information

- Full exception trace of the last error, including diagnostic data and specific HTTP header information

  **Note**: You can use the information in the full exception trace to help troubleshoot the issue.

It's possible that the service recovered after it issued the alert. Therefore, when you receive an alert that specifies that the health set is unhealthy, first verify that the issue still exists. If the issue does exist, perform the appropriate recovery actions outlined in the following sections.

## Verifying the issue still exists

1. Identify the health set name and the server name in the alert.

2. The message details provide information about the exact cause of the alert. In most cases, the message details provide sufficient troubleshooting information to identify the root cause. If the message details are not clear, follow these steps:

   1. Open the Exchange Management Shell, and then run the following command to retrieve the details of the health set that issued the alert:

      ```powershell
      Get-ServerHealth -Identity <ServerName> -HealthSet <HealthSetName>
      ```

      For example, to retrieve the ECP health set details about server1.contoso.com, run the following command:

      ```powershell
      Get-ServerHealth -Identity server1.contoso.com -HealthSetName ECP
      ```

   2. Review the command output to determine which monitor reported the error. The **AlertValue** value for the monitor that issued the alert will be `Unhealthy`.

   3. Rerun the associated probe for the monitor that's in an unhealthy state. Refer to the table in the Explanation section to find the associated probe. To do this, run the following command:

      ```powershell
      Invoke-MonitoringProbe <HealthSetName>\<ProbeName> -Server <ServerName> | Format-List
      ```

      For example, assume that the failing monitor is **EacSelfTestMonitor**. The probe associated with that monitor is **EacSelfTestProbe**. To run that probe on server1.contoso.com, run the following command:

      ```powershell
      Invoke-MonitoringProbe ECP\EacSelfTestProbe -Server server1.contoso.com | Format-List
      ```

   4. In the command output, review the **Result** value of the probe. If the value is **Succeeded**, the issue was a transient error, and it no longer exists. Otherwise, refer to the recovery steps outlined in the following sections.

## EacSelfTestMonitor and EacDeepTestMonitor Recovery Actions

1. Start IIS Manager, and then connect to the server that's reporting the issue. Click **Application Pools**, and then recycle the ECP application pool named **MSExchangeECPAppPool**.

2. Rerun the associated probe as shown in step 2c in the Verifying the issue still exists section.

3. If the issue still exists, recycle the entire IIS service by using the IISReset utility.

4. Rerun the associated probe as shown in step 2c in the Verifying the issue still exists section.

5. If the probe fails, restart the server.

6. After the server restarts, rerun the associated probe as shown in step 2c in the Verifying the issue still exists section.

7. If the probe continues to fail, you may need assistance to resolve this issue. Contact a Microsoft Support professional to resolve this issue. To contact a Microsoft Support professional, visit [Support for business](https://support.microsoft.com/supportforbusiness/productselection) and then select **Servers** \> **Exchange Server**. Because your organization may have a specific procedure for directly contacting Microsoft Product Support Services, be sure to review your organization's guidelines first.

## For More Information

[What's new in Exchange 2013](../../what-s-new-in-exchange-2013-exchange-2013-help.md)

[Exchange PowerShell](/powershell/exchange/)

[Exchange admin center in Exchange 2013](../../exchange-admin-center-in-exchange-2013-exchange-2013-help.md)