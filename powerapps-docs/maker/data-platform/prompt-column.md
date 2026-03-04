---
title: "Prompt columns in Microsoft Dataverse"
description: "Understand how to create, manage, and use prompt columns with Power Apps and Dataverse."
keywords: ""
ms.date: 01/09/2026
ms.custom: 
ms.topic: article
applies_to: 
  - "Dynamics 365 (online)"
  - "Dynamics 365 Version 9.x"
  - "powerapps"
author: "paulliew"
ms.subservice: dataverse-maker
ms.author: paulliew
ms.reviewer: Mattp123
ms.collection: bap-ai-copilot
search.audienceType: 
  - maker
---
# Prompt columns (preview)

[!INCLUDE [preview-banner](~/../shared-content/shared/preview-includes/preview-banner.md)]

A prompt column is an AI-powered data type in Microsoft Dataverse that enables you to define natural language prompts tied to other columns in your table. The AI model processes these prompts to generate relevant responses based on specified input columns. The result is immediately stored in the prompt column, ready to be used in apps, workflows, or reports.

The key function is that the prompt column contains generative AI results stored in the table persistently. This is how customer data is enriched using generative AI and brings value to their data.

Example use cases:

- Customer support: Automate responses to frequently asked questions by using AI prompts to generate accurate and timely replies based on customer inquiries stored in Dataverse.
- Content creation: Use prompts to assist in writing articles, reports, or marketing materials by generating text that aligns with the input provided.
- Data analysis: Use AI prompts to analyze complex datasets and generate insights, sentiments, extract, and classify contents or summaries that help in making informed decisions.
- Workflow automation: Integrate AI prompts into business workflows to automatically do tasks such as scheduling, reporting, and data entry.

[!INCLUDE [preview-banner](~/../shared-content/shared/preview-includes/preview-note-pp.md)]

## Prerequisites

- Prompt columns use the same licensing model as AI builder prompts. More information: [Prompts overview](/ai-builder/prompts-overview#prerequisites)
- Copilot and AI Prompts features turned On in the Features area for the environment settings. More information: [Manage feature settings](/power-platform/admin/settings-features?tabs=new#copilot-preview)

## Create a prompt column 

1. Go to [Power Apps](https://make.powerapps.com/). Select **Tables** on the left navigation pane.
1. Create a new table or open an existing table. More information: [Create a table](create-edit-entities-portal.md#create-a-table)
1. On the **Tables** page, select **New** > **Column**.
1. On the column properties page, enter a **Display name** and **Description** for your column.
1. Under the **Data type** dropdown list, select **Prompt**.
1. Clear the **Allow form fill assistance** checkbox.
1. Select **+Add new prompt**. You can create up to five prompt columns per table.
1. On the prompt column page, create a prompt. More information: [How to write an AI prompt for a prompt column](#how-to-write-an-ai-prompt-for-a-prompt-column)
1. Select **Save** on the prompt page.
1. Select **Save** on the column properties page to save your column.

## How to write an AI prompt for a prompt column

Crafting effective AI prompts is crucial for getting accurate and relevant responses from the AI model. You can use the prefilled prompt or write your own custom prompt.

To clear the prefilled prompt, highlight and delete it, or select the three dots to the left of **Model**, then select **Clear prompt**.

:::image type="content" source="media/prompt-columns/prompt-column-clear-prompt.png" alt-text="Clear prompt UI":::

## Write effective prompts

Here are some best practices for writing prompts:

- Write clear instructions. Ensure that your prompts are clear and specific. Avoid ambiguity by being precise about what you want the AI to do. For example, instead of writing *Tell me about the weather*, write *Provide a summary of today's weather in Paris*.
- Use structured language. Structured language helps the AI model understand the context and intent of the prompt better. For example, *Generate a list of top 10 marketing strategies for 2025* is more effective than *Top marketing strategies*.
- Include contextual information. Provide necessary context within the prompt to help the AI model generate more accurate responses. For example, *Based on the sales data from Q1, suggest ways to improve our marketing campaign*. Add an input column in each prompt.

## Add input columns

1. Replace default input text by selecting it and then select **Delete**.
1. Select **+Add content**, and then select the table.
1. The column dropdown list appears. Select the input column from the list of columns from your table, and then select **Add**. In this example, the *Customer feedback* column is selected.
   :::image type="content" source="media/prompt-columns/prompt-column-add-columns.png" alt-text="Add an input column":::

   - You can use more than one input column in each prompt.
   - Input columns can’t be formula columns, file columns, image data type, or another prompt column. If you select these data types, their input value is ignored.

1. Select **Save**.

## Test and refine prompts

Test your AI prompt. Create a test record with appropriate values in all your input columns for testing. For example, create a *Name* column in the table and enter some value that is used to identify the test record, such as *testing prompts*.

1. Create or select a prompt column.
1. While editing the prompt in a prompt column, select the input column to open the **Filter knowledge** pop-up screen.
1. Select the **Filter attribute** option. 
   :::image type="content" source="media/prompt-columns/prompt-column-filter-attribute.png" alt-text="Filter attribute option":::
1. Select the **Filter attribute** dropdown list and select a filter, such as *Name*. 
1. Enter the value of your testing record **Name**, such as *testing prompt*.
1. Select **Close**.
1. Select **Test** and review the **Model response**.
1. Select the **Knowledge used** tab to confirm that the input came from your test record.
   :::image type="content" source="media/prompt-columns/prompt-column-knowledge.png" alt-text="Knowledge used in prompt column" lightbox="media/prompt-columns/prompt-column-knowledge.png":::
1. Modify your prompt until you get the desired results.  
1. Return to the **Filter knowledge** pop-up and select **No filter** when your prompt changes are done.
1. Select **Save** to update your prompt column.

## View prompt column results

Create a model-driven app to view and validate your prompt column results.

1. Go to [Power Apps](https://make.powerapps.com/), and then open an app for editing or create a new app. More information: [Create a model-driven app](../model-driven-apps/create-model-driven-app.md)
1. In the app designer, select **Edit form** to edit the table form for the table that has the prompt columns.  
1. The form designer opens. Select all the input and prompt columns you want to add to the form. 
1. Select **Save and publish**. 
1. Go to the **Tables** area in Power Apps and select **Views**.
1. Observe the values in the records including new records that contain prompt column values.


# Prompt Column feature enhancements

**Pre-requiste:** Requires service update <> or higher for below updated features.

Prompt Columns leverage asynchronous computation so that AI-driven processing is decoupled from real-time transactions. This preserves system responsiveness, protects business critical workflows, and enables organizations to adopt AI capabilities at scale without compromising operational reliability.

### How to check if Prompts are asynchronous
When prompt column is added to an entity, a banner message appears at the top of the page.

   :::image type="content" source="media/prompt-columns/Prompt-Column-Async-Message.png" alt-text="Prompts Aysnc Message" lightbox="media/prompt-columns/prompt-column-async-message.png":::

**Note:** If your environment is not upgraded to the latest version, the banner message may not be visible. Prompt Columns created in earlier versions will continue to operate without change until the upgrade occurs. As part of the upgrade process, existing prompt definitions are updated to support asynchronous execution.

### Text Input variable is added by-default 
A default Text Input variable is automatically added to prompt definition. This variable is used as a filter on the primary column of the data source and is required to save the prompt definition.

:::image type="content" source="media/prompt-columns/prompt-column-recordid.png" alt-text="media/prompt-columns/Prompt-Column-recordid.png":::

This variable is used as a filter on the primary column of the data source and is required to save the prompt definition.


:::image type="content" source="media/prompt-columns/prompt-column-filter-attribute-value.png" alt-text="prompt-column-filter-attribute-value.png":::

**Note:** The platform requires a single Text Input variable, which is automatically added and applied as a filter on the primary column of the data source. When saving, an error is displayed if the Text Input variable is missing or not used as a filter on the primary column.


## Prompt column with Filter Condition
Filter conditions ensure Prompts run only when they add value. 

With filter based execution, prompts run only when specified conditions are satisfied. During record creation or update, Prompt Column evaluates the filter first and executes the prompt only for eligible records, helping minimize unnecessary executions and optimize credit usage.

### Adding Filters on Existing Prompt Column
1. Go to [Power Apps](https://make.powerapps.com/). Select **Tables** on the left navigation pane.
1. Click on Table that has Prompt column.
1. Click on Prompt Column > Edit Column.
1. Click on **Apply filter**.
 :::image type="content" source="media/prompt-columns/prompt-column-apply-filter.png" alt-text="prompt column apply filter.png":::

1. On Filter conditions screen > Add Filter (example below).
 :::image type="content" source="media/prompt-columns/prompt-column-add-filter-conditions.png" alt-text="prompt column apply filter.png":::
1. Press **Ok**.
1. On Edit column screen, notice Filters selected is applied
 :::image type="content" source="media/prompt-columns/prompt-column-filter-selected.png" alt-text="prompt column apply filter.png":::
1. Press **Save**.


## Prompt Column Execution Status
When a Prompt Column is created, the table automatically includes two corresponding columns—Status and Details—for that Prompt Column.

1. Go to [Power Apps](https://make.powerapps.com/). Select **Tables** on the left navigation pane.
1. Open Table that has Prompt Column created. 
1. On Table Page, go to the table columns. Press (v) chevron to view all columns. 
1. On Show existing column – search for Prompt Column Name. 
1. Select columns named as 
    * (columnName) _PromptColumnStatus
    * (columnName)_PromptColumnDetails

When prompt execution starts, these columns are populated to record the execution status and any failure details.

The example below demonstrates a Prompt Column named testSummary and its corresponding Status and Details columns.

 :::image type="content" source="media/prompt-columns/prompt-column-show-status-details-column.png" alt-text="prompt column show status details column.png":::



**Prompt column status codes:**


| Status  | Name | Description |
|---|---|---|
| 0 | NotStarted | Record created. AI analysis has not started yet. |
| 1 | InProgress | The prompt AI analysis is currently in-progress. Details column shows start Date and Time.|
| 2 | Completed | Prompt execution completed successfully. Details column shows last successful Date and Time.|
| 3 | Failed | The prompt AI generation failed. Detail column shows error details in last execution.|

### Disable Prompt column AI generation
1. Go to [Power Apps](https://make.powerapps.com/). Select **Tables** on the left navigation pane.
1. Go to Table
1. Go to column that is of data type Prompt > Edit column
1. On Edit column screen > Uncheck **Allow prompt column** execution checkbox 
  :::image type="content" source="media/prompt-columns/prompt-column-disable.png" alt-text="prompt column disable.png":::

**Note:** By default, this setting is enabled. When Allow prompt column execution is disabled or AI Prompts feature (environment settings: More information: Manage feature settings) is turned off, AI analysis is not performed for Prompt column. 

### FAQs

**Are backfills supported?** 

Prompt column generates AI results when a new record is created, or when one or more columns specified in the Prompt Column definition are updated.

Existing records are not processed by default. However, updating a column defined in the Prompt Column definition triggers AI analysis.

**Are Prompt columns audited?** 

Prompt columns are not audited.

**Can I perform on-demand AI analysis without updating a record?** 

The Prompt Column is automatically triggered when a record is updated; however, on‑demand execution for that record is not currently supported.


## Related articles

For information about how the AI is used with this feature, go to [FAQ for prompts and text generation capabilities](/ai-builder/faqs-text-generation)