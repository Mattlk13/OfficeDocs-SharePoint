---
title: "Get a list of all user OneDrive URLs in your organization"
ms.reviewer: 
ms.author: kaarins
author: kaarins
manager: serdars
audience: Admin
f1.keywords:
- NOCSH
ms.topic: article
ms.service: one-drive
localization_priority: Normal
search.appverid:
- SPO160
- ODB160
- ODB150
- GOB150
- GOB160
- MET150
ms.collection: M365-collaboration
ms.custom:
-  seo-marvel-apr2020
ms.assetid: 8e200cb2-c768-49cb-88ec-53493e8ad80a
description: "In this article, you'll learn how to display a list of all the OneDrive URLs for the users in your organization."
---

# Get a list of all user OneDrive URLs in your organization

This article is for global and SharePoint admins in Microsoft 365.
  
## View the list of OneDrive users and URLs in your organization

1. Sign in to https://admin.microsoft.com as a global or SharePoint admin. (If you see a message that you don't have permission to access the page, you don't have Microsoft 365 admin permissions in your organization.)
    
    > [!NOTE]
    > If you have Office 365 Germany, sign in at https://portal.office.de. If you have Office 365 operated by 21Vianet (China), sign in at https://login.partner.microsoftonline.cn/. Then select the Admin tile to open the admin center.  
    
2. In the left pane, select **Reports** \> **Usage**. (You might need to select **Show all** to see the Reports option.) 
    
3. Select the **OneDrive files** tile, or select **Select a report** \> **OneDrive usage**.

    > [!NOTE]
    > If you see GUIDs in the report instead of URLs and names, in the left pane, select **Settings** > **Services & add-ins**, and then select **Reports**. Clear the box **Display anonymous identifiers instead of names in all reports**.
    
4. In the upper right of the table at the bottom, select **Export**.
    
## Create a list of all the OneDrive URLs in your organization using Microsoft PowerShell
<a name="BKMK_Step2"> </a>

The list you create in these steps will be saved to a text file.
  
1. [Download the latest SharePoint Online Management Shell](https://go.microsoft.com/fwlink/p/?LinkId=255251).

    > [!NOTE]
    > If you installed a previous version of the SharePoint Online Management Shell, go to Add or remove programs and uninstall "SharePoint Online Management Shell." 

2. Save the following text to a PowerShell file. For example, you could save it to a file named OneDriveSites.ps1.
    
     ```PowerShell
    $TenantUrl = Read-Host "Enter the SharePoint admin center URL"
    $LogFile = [Environment]::GetFolderPath("Desktop") + "\OneDriveSites.log"
    Connect-SPOService -Url $TenantUrl
    Get-SPOSite -IncludePersonalSite $true -Limit all -Filter "Url -like '-my.sharepoint.com/personal/'" | Select -ExpandProperty Url | Out-File $LogFile -Force
    Write-Host "Done! File saved as $($LogFile)."
     ```

3. Open the SharePoint Online Management Shell. Navigate to the directory where the script has been saved and run:

    ```PowerShell
    PS C:\>.\OneDriveSites.ps1
    ```

   > [!NOTE]
   > If you get an error message about being unable to run scripts, you might need to change your execution policies. For info, see [About Execution Policies](/powershell/module/microsoft.powershell.core/about/about_execution_policies). 
    
4. The script will prompt you for the SharePoint admin center URL. For example, `https://contoso-admin.sharepoint.com` is the Contoso SharePoint admin center URL.

5. You will then be prompted to sign in. Use a SharePoint admin or global admin account.

After the script successfully completes, a text file is created in the location specified by the **$LogFile** variable in the script. This file contains a list of all OneDrive Urls in your organization. The following text provides an example of how the list of Urls in this file should be formatted.
  
`https://contoso-my.sharepoint.com/personal/annb_contoso_onmicrosoft_com/
https://contoso-my.sharepoint.com/personal/carolt_contoso_onmicrosoft_com/
https://contoso-my.sharepoint.com/personal/esterv_contoso_onmicrosoft_com/  
https://contoso-my.sharepoint.com/personal/hollyh_contoso_onmicrosoft_com/`

## More information
<a name="BKMK_MoreInfo"> </a>

Once you have the URL for a user's OneDrive, you can get more info about it by using the [Get-SPOSite](/powershell/module/sharepoint-online/get-sposite) cmdlet, and change settings by using the [Set-SPOSite](/powershell/module/sharepoint-online/set-sposite) cmdlet.