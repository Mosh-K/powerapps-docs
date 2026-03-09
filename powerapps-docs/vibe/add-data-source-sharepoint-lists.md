---
title: Use SharePoint lists as a data source in Power Apps vibe (preview)
description: Learn how to connect and add SharePoint lists as data sources in Power Apps vibe (preview). Build apps quickly while leveraging existing SharePoint workflows.
author: mduelae
ms.author: yueshu
ms.reviewer: mkaur
ms.date: 03/09/2026
ms.topic: how-to
ms.subservice: code-apps
ms.custom: vibe
ms.contributors:
- mkaur
- yueshu
---

# Add a SharePoint list to Power Apps vibe (preview)

[This article is prerelease documentation and is subject to change.]

Power Apps vibe lets you connect SharePoint lists directly as data sources when building apps. This integration allows you to work with your existing SharePoint list data without migrating to Dataverse tables.

When you connect a SharePoint list to vibe, your app can display and work with the list data in real time. This approach is ideal for scenarios where you want to:

- Leverage existing SharePoint lists. 
- Build apps quickly without data migration. 
- Maintain your current SharePoint-based workflows.

> [!IMPORTANT] 
> SharePoint lists are currently read-only in the vibe. To modify list items, make changes directly in SharePoint.

## Prerequisites

Access to the SharePoint site that contains the list you want to use.

## Add an existing SharePoint list

Select an existing SharePoint list to use with your app. Vibe doesn’t currently suggest or automatically identify available lists.

1. Sign in to [https://vibe.powerapps.com](https://vibe.powerapps.com/).
1. Open an app for editing, and then select **Data** > **Add table** > **Existing SharePoint list**.

    :::image type="content" source="media/sharepoint-data/add-sharepoint-list-vibe.png" alt-text="Connect to a SharePoint list in Power Apps vibe ":::

1. Enter the SharePoint site URL, and then select **Connect**. Alternatively, select a site from **Recent sites**.
1. Under **Select a list**, choose the list you want, and then select **Connect**.
1. Vibe connects directly to your SharePoint list. After the list is added, it appears in the data workspace with a SharePoint icon and its columns.

    :::image type="content" source="media/Sharepoint-data/sharepoint-data-added-vibe.png" alt-text="SharePoint list added to vibe":::


