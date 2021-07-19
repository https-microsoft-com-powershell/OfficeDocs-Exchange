---
localization_priority: Normal
description: 'Summary: Track and prevent migration data loss with DataConsistencyScore'
ms.topic: overview
author: msdmaguire
ms.author: jhendr
ms.assetid: 
ms.reviewer: 
title: Track and Prevent Migration Data Loss
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Track and prevent migration data loss

When migrating to Exchange Online, the migration process might reveal inconsistencies that pose a risk of data loss. Such inconsistencies can occur during almost any migration, whether from on-premises Exchange Server, Public Folders, PST file imports, Google Workspace (formerly G Suite), or third-party IMAP servers. The migration process tracks and reports on any possible instances of data loss by generating a **DataConsistencyScore**.

## Migration and DataConsistencyScore

When you attempt a migration, any inconsistencies between the source and target data stores will count towards the DataConsistencyScore. This score is then used to determine whether an Exchange Online migration will complete successfully or if intervention is needed.

There are 4 possible grades that are derived from the DataConsistencyScore.

|Grade|Description|Approval Required?|
|---|---|---|
|**Perfect**|No inconsistencies noted during migration.|No approval is required.|
|**Good**|At least 1 inconsistency noted, but the data loss was not impactful. For example, if only metadata or folder permissions were lost during migration.|No approval is required|
|**Investigate**|A small amount of noticeable data loss was detected, caused by some common inconsistency types.|Approval of skipped items is required for migration types that have a built-in finalization phase, such as Hybrid migrations or Google Workspace onboarding.|
|**Poor**|Major data loss was detected.|Contact Microsoft Support for assistance. Approval of skipped items is required for migration types that have a built-in finalization phase.|

You can view the DataConsistencyScore for your migration in the classic Exchange admin center at a per-user and per-batch level. You can also find it using PowerShell cmdlets; the *DataConsistencyScore* property exists on **MigrationBatch**, **MigrationUser**, and **RequestStatistics** objects.

## How the DataConsistencyScore is calculated

There are various thresholds used to determine the DataConsistencyScore. Microsoft is constantly tuning these thresholds to ensure that problematic data loss does not occur during migrations. The details of these thresholds are not presented to Exchange Online administrators.

For batches, the DataConsistencyScore is equal to the worst DataConsistencyScore of any user within that batch. This behavior helps administrators know immediately whether there is any data loss that should be investigated.

## Guidance for grades of Investigate or Poor

For migrations from third-party IMAP servers, PST file imports, or on-premises Exchange Server using Cutover or Staged Exchange Migration, there is no need to approve the set of skipped items.  Approval of migrations with a score of Investigate or worse is required for the completion of Remote Move, Public Folder, and Google Workspace migrations.

If the migration receives a grade of **Investigate**, then you can approve skipped items manually to allow the migration to succeed.

If you are using batches, then run:

```PowerShell
Set-MigrationBatch -ApproveSkippedItems or Set-MigrationUser -ApproveSkippedItems
```

If you are using MoveRequests directly, then run:

```PowerShell
Set-MoveRequest -SkippedItemApprovalTime $([DateTime]::UtcNow)
```

For a batch scored as **Investigate**, approving the migration allows you to complete all migrations in the batch with a score of Perfect, Good, or Investigate.

For a batch scored as **Poor**, approving the migration allows you to complete all migrations in the batch with a score of Perfect, Good, or Investigate, but will not approve any migration in the batch with a score of Poor.

If the migration fails with a grade of **Poor**, you cannot force the migration to succeed. Please contact Microsoft Support for assistance.

## How to see which items were not migrated

### In the Classic Exchange admin center (Classic EAC)

1. In the Exchange Admin center, click **recipients**, and then click **migration**.

2. Select the batch you would like to inspect. Click **View details** in the information pane on the right.

3. Select the user you would like to inspect. Click **Skipped item details** in the information pane on the right.

### Using Exchange Online PowerShell

1. [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell).

2. Determine the identity of the user you wish to investigate.

3. Run the following commands:

   ```PowerShell
   $userStats = Get-MigrationUserStatistics -Identity user@fabrikaminc.net -IncludeSkippedItems
   $userStats.SkippedItems | ft -a Subject, Sender, DateSent, ScoringClassifications
   ```

4. Inspect the information about skipped items provided in the SkippedItems property on the userStats object.

## How to opt in or opt out of using DataConsistencyScore

Through March 2021, the [BadItemLimit and LargeItemLimit parameters](/powershell/module/exchange/new-moverequest) will remain available as options. You can specify a value for the *BadItemLimit* and *LargeItemLimit* parameters when using cmdlets or you can fill in the BadItemLimit or LargeItemLimit box in the EAC UI. When you specify a BadItemLimit or LargeItemLimit, the old migration method is used and the DataConsistencyScore is not calculated.

If neither the *BadItemLimit* parameter nor the *LargeItemLimit* parameter is specified, or if the boxes in the classic Exchange admin center wizard are left blank, then the new migration method and DataConsistencyScore are used.

> [!NOTE]
> The BadItemLimit and LargeItemLimit parameters will be completely replaced by DataConsistencyScore in January 2021.