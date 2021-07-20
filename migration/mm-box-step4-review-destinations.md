---
title: "Review the destination paths for your Box migration with Migration Manager"
ms.reviewer: 
ms.author: jhendr
author: JoanneHendrickson
manager: serdars
audience: ITPro
f1.keywords:
- NOCSH
ms.topic: article
ms.service: sharepoint-online
localization_priority: Priority
ms.collection: 
- m365solutions-migratefileshares
- m365solution-migratetom365
- m365solution-scenario
- M365-collaboration
- SPMigration
search.appverid: MET150
description: Review your destination paths for your Box migration while using Migration Manager.
---
# Step 4:   Review destination paths

>[!Note]
>This release currently supports the migration of up to approximately 1500 Box accounts in a single migration. For larger migrations, we recommend you use the [Mover migration tool](https://Mover.io).

In this step, review the destination paths of the accounts you have moved to the migrations list, making sure they are correct. An account cannot be migrated without a destination indicated. Once you start migrating content to a destination, it cannot be modified.

## Single destination edit
If a destination is missing, highlight the row. A panel will appear to the right. Under **Destination**, select **Edit**.  You have the choice of selecting a OneDrive, SharePoint, or Teams path as a destination.


### OneDrive

1. Highlight the row. Under **Destination**, select **Edit.**
2. Select **OneDrive** as a destination.

![edit OD destination path](media/mm-box-select-od-destination.png)

3. Select a folder (optional).

![select OneDrive folder path](media/mm-box-destination-folder-onedrive.png)

4. Select **Save path**.


###  SharePoint

1. Highlight the row. Under **Destination**, select **Edit.**
2. Select **OneDrive** as a destination.

![edit SP destination path](media/mm-box-sp-destination-path.png)

3. Select a location where you want to move the content.

![select SP library path](media/mm-box-sharepoint-destination-folder.png)
4. Select **Save path**.



### Teams


1. Highlight the row. Under **Destination**, select **Edit.**
2. Select **OneDrive** as a destination.

![select teams destination](media/mm-box-teams-destination-path.png)

3. Select a channel.

![select teams channel](media/mm-box-teams-destination-channel.png)

4. Select **Save path**.


## Upload destinations using a CSV file

If you have many destinations to edit, you can choose to upload a bulk destinations CSV file.  Download the *MigrationDestinations.csv* file template to your computer and enter your destinations. Save your file as a .csv file using any name you wish. 


![upload destinations for Box accounts bulk](media/mm-box-bulk-upload-destination-panel.png)

1. From the Migrations tab, select **Upload destinations** from the menu bar.
2. Select the file to upload with your destinations.
4. Select **Save**.  


>[!Important]
>Review your destination CSV file before you upload it to make sure you have entered the values in correctly.  The destination file is not validated, and once you have migrated to these destinations it cannot be undone.


[**Step 5: Map identities**](mm-box-step5-map-identities.md)


>[!NOTE]
>Migration Manager Box isn't available for users of Office 365 operated by 21Vianet in China. It's also not available for users of Microsoft 365 with the German cloud that use the data trustee *German Telekom*. It is supported for users in Germany whose data location isn't in the German datacenter.
>
> This feature is also not supported for users of the Government Cloud, including GCC, Consumer, GCC High, or DoD.