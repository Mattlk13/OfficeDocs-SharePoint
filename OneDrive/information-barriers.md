---
title: "Use information barriers with OneDrive"
description: "Learn about associating segments with a OneDrive, and what happens when segments are associated with a OneDrive."
ms.author: robmazz
author: robmazz
manager: laurawi
ms.reviewer: nibandyo
audience: Admin
f1.keywords:
- NOCSH
ms.topic: article
ms.service: one-drive
localization_priority: Normal
ms.collection: 
- Strat_OD_admin
- M365-collaboration
search.appverid:
- ODB160
- ODB150
- MET150
---

# Use information barriers with OneDrive

Information barriers are policies in Microsoft 365 that a compliance admin can configure to prevent users from communicating and collaborating with each other. This solution is useful if, for example, one division is handling information that shouldn't be shared with specific other divisions, or a division needs to be prevented, or isolated, from collaborating with all users outside of the division. Information barriers are often used in highly regulated industries and those organizations with compliance requirements, such as finance, legal, and government. [Learn more about information barriers](/microsoft-365/compliance/information-barriers).

The following image illustrates three segments in an organization: HR, Sales, and Research. An information barrier policy has been defined that blocks communication and collaboration between the Sales and Research segments.

![Example of segments in an organization](/sharepoint/sharepointonline/media/info-barriers-segments-example.png)

With information barriers in OneDrive, when a segment is applied to a user, within 24 hours that segment is automatically associated with the user's OneDrive. Other segments that are compatible with the user's segment and with each other will also get associated with the OneDrive. A OneDrive can have up to 100 segments associated with it. A global or SharePoint admin can manage these segments using PowerShell, as described later in the section [Associate or remove additional segments on a user's OneDrive](#associate-or-remove-segments-on-a-users-onedrive).

In the above example, the HR segment is compatible with both Sales and Research. However, the Sales and Research segments are incompatible. In this case, the OneDrive for a user in Sales will have the Sales and HR segments, and the OneDrive for a user in Research will have the Research and HR segments. The OneDrive of a user in HR will have only the HR segment because Sales and Research are incompatible.

When these segments are associated with the OneDrive, content can be shared with and accessed by only users who have a matching segment.

## Prerequisites

1. Make sure you meet the [licensing requirements for information barriers](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-tenantlevel-services-licensing-guidance/microsoft-365-security-compliance-licensing-guidance#information-barriers).
2. [Create information barrier policies](/office365/securitycompliance/information-barriers-policies) that allow or block communication between the segments, and then set them to active. Create segments and define the users in each.
3. After you've configured and activated your information barrier policies, wait 24 hours for the changes to propagate through your organization.
4. Complete the steps in the following sections to enable and manage SharePoint and OneDrive information barriers in your organization.

## Enable SharePoint and OneDrive information barriers in your organization

Enabling information barriers for SharePoint and OneDrive are configured in a single action. Information barriers for the services cannot be enabled separately. To enable information barriers for OneDrive, see [Enable SharePoint and OneDrive information barriers in your organization](/sharepoint/information-barriers#enable-sharepoint-and-onedrive-information-barriers-in-your-organization). After you've enabled information barriers for SharePoint and OneDrive, continue with the OneDrive guidance in this article.  

## Sharing files from a OneDrive that has segments associated

When a segment is associated with a OneDrive:

- The option to share with "Anyone with the link" is disabled.
- Files and folders can be shared only with users whose segment matches that of the OneDrive. In the above example, users in the Sales segment can share OneDrive content with other users in either the Sales or HR segment whereas users in the HR segment can share their OneDrive content with other users in the HR segment only.

When a OneDrive has no segments associated:

- The user can share files and folders based on the information barrier policy applied to the user and the sharing setting for the OneDrive.

## Accessing shared files from a OneDrive that has segments associated

For a user to access content in a OneDrive that has segments associated:

- The user's segment must match a segment that is associated with the OneDrive.

    AND

- The files must be shared with the user.

Non-segment users can access shared OneDrive files only from other non-segment users. They can't access shared OneDrive files from users who have a segment applied.

## Use PowerShell to view the segments associated with a OneDrive

A global or SharePoint admin can view and change the segments associated with a user's OneDrive.

1. Connect to the [Security & Compliance Center PowerShell](/powershell/exchange/office-365-scc/connect-to-scc-powershell/connect-to-scc-powershell) as a global admin.

2. Run the following command to get the list of segments and their GUIDs.

    ```PowerShell
    Get-OrganizationSegment | ft Name, EXOSegmentID
    ```

3. Save the list of segments.

    |**Name**|**EXOSegmentId**|
    |:-------|:---------------|
    | Sales | a9592060-c856-4301-b60f-bf9a04990d4d |
    | Research | 27d20a85-1c1b-4af2-bf45-a41093b5d111 |
    | HR | a17efb47-e3c9-4d85-a188-1cd59c83de32 |

4. If not previously completed, [download](https://go.microsoft.com/fwlink/p/?LinkId=255251) and install the latest SharePoint Online Management Shell. If you installed a previous version of the SharePoint Online Management Shell, follow the instructions in the [Enable SharePoint and OneDrive information barriers in your organization](/sharepoint/information-barriers#enable-sharepoint-and-onedrive-information-barriers-in-your-organization) article.

5. Connect to SharePoint as a [global admin or SharePoint admin](/sharepoint/sharepoint-admin-role) in Microsoft 365. To learn how, see [Getting started with SharePoint Online Management Shell](/powershell/sharepoint/sharepoint-online/connect-sharepoint-online).

6. Run the following command:

    ```PowerShell
    Get-SPOSite -Identity <site URL> | Select InformationSegment 
    ```

    Example: `Get-SPOSite -Identity https://contoso-my.sharepoint.com/personal/John_contoso_onmicrosoft_com | Select InformationSegment`

## Associate or remove segments on a user's OneDrive

> [!WARNING]
> If the segments associated with a user's OneDrive don't match the segment applied to the user, the user won't be able to access their OneDrive. Be careful not to associate any segments with the OneDrive of a non-segment user.

> [!NOTE]
> Any changes you make will be overwritten if the user's segment changes.

To associate a segment with a OneDrive, run the following command in the SharePoint Online Management Shell.

```PowerShell
Set-Sposite -Identity <site URL> -AddInformationSegment <segment GUID> 
 ```

Example: `Set-SPOSite -Identity https://contoso-my.sharepoint.com/personal/John_contoso_onmicrosoft_com  
-AddInformationSegment 27d20a85-1c1b-4af2-bf45-a41093b5d111`

An error will appear if you attempt to associate a segment that isn't compatible with the existing segments on the OneDrive.

To remove segment from a OneDrive, run the following command.  

```PowerShell
Set-Sposite -Identity <site URL> -RemoveInformationSegment <segment GUID>
 ```

Example: `Set-SPOSite -Identity https://contoso-my.sharepoint.com/personal/John_contoso_onmicrosoft_com  
-RemoveInformationSegment 27d20a85-1c1b-4af2-bf45-a41093b5d111`

## Effects of changes to user segments or information barrier policies

If a user's segment changes, the segment associated with their OneDrive will be automatically updated to match within 24 hours, and any compatible segments will be added.

If a compliance administrator changes an existing policy, the change may impact the compatibility of the segments associated with the OneDrive. For example, segments that were once compatible may no longer be compatible. A SharePoint admin must change the segments associated with an affected site accordingly. Learn how to create an [information barriers policy compliance report in PowerShell](/sharepoint/info-barriers-report).

If a policy changes after files are shared, the sharing links will work only if the user attempting to access the shared files has a segment applied that matches a segment associated with the OneDrive.

## Example

The example at the beginning of this article illustrates an organization with three segments: HR, Sales, and Research. An information barriers policy blocks communication and collaboration between Sales and Research. The segment HR has no restriction. In addition, the organization has users with no segments applied. The following table shows the effects of this configuration.

|**Components**|**HR users**|**Sales users**|**Research users**|**Non-segment users**|
|:-------------|:-----------|:--------------|:-----------------|:--------------------|
| Segments associated with OneDrive | HR | Sales, HR | Research, HR | None |
| OneDrive content can be shared with | HR only | Sales and HR | Research and HR | Anyone based on the sharing settings selected |
| OneDrive content can be accessed by | HR only | Sales and HR | Research and HR | Anyone with whom the content has been shared |

## Auditing

After you enable information barriers for OneDrive, audit events are available in the Office 365 audit log to help you monitor information barrier OneDrive activities. Audit events are logged whenever the following activities occur:

- Segments are added to a user OneDrive
- Segments are changed on a user OneDrive
- Segments are removed from a user OneDrive

For more information about OneDrive segment auditing in Office 365, see [Search the audit log in the compliance center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#information-barriers-activities).

## How to suspend SharePoint and OneDrive information barriers in your tenant

If your organization would like to temporarily suspend information barriers on SharePoint, you must use SharePoint Online Management Shell and the [Set-Spotenant](/powershell/module/sharepoint-online/set-spotenant) cmdlet.

To suspend information barriers, run the following command:

```PowerShell
Set-Spotenant -InformationBarriersSuspension $true
```

>[!NOTE]
>If you have Microsoft 365 Multi-Geo, you must run this command for each of your geo-locations.

## See also

- [Information barriers in Microsoft Teams](/microsoftteams/information-barriers-in-teams)
- [Information barriers in SharePoint](/sharepoint/information-barriers)
