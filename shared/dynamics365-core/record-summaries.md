The **Row summary** feature in apps provides users with a concise, AI-generated overview of key details about a record. Summaries help users quickly understand essential information without manually scanning fields, related records, or activity timelines. You see summaries as a paragraph or bullet points, so they're easy to read.

Summaries can be accessed in two ways:

- From the **Insights** bar at the top of main forms. 
- From view pages by selecting the inline **Summary** action in the grid, which opens the summary for a record in a modal popup.

The feature enhances user efficiency by delivering context-rich insights directly within the form. Users can interact with summaries to copy content, regenerate updated information, and provide feedback on their relevance, improving both usability and accuracy.

## Prerequisite

Each of the following settings must be turned on to see the row summaries for apps.

- The [AI insight cards](/power-platform/admin/settings-features#ai-insight-cards-preview) toggle is turned on for the Power Platform environment. 

## Feature details

### Accessing record summaries

View the record summary from a form or view when a [table is configured to display summaries](/power-apps/maker/data-platform/configure-form-row-summary#create-a-row-summary).

- **Forms**: When viewing a record in a main form, the summary appears in the insights bar at the top of the main form.
  :::image type="content" source="/power-apps/user/media/row_summary_expanded.png" alt-text="Screenshot that shows a row summary card in the insights bar on a form." lightbox="/power-apps/user/media/row_summary_expanded.png":::

> [!NOTE]
> You see the record summary after you save a new record. If you haven't saved the record, the summary area stays hidden.

- **Views**: When browsing records in a view, select the inline Summary action next to a record to open the summary in a modal popup.
  :::image type="content" source="/power-apps/user/media/row_summary_gridEntry.png" alt-text="Screenshot that shows a row summary card accessed from a grid row." lightbox="/power-apps/user/media/row_summary_gridEntry.png":::

### Interacting with summaries 

Here are some actions you can take with summaries:

- **Copy**: Select to copy the summary content directly to your clipboard for easy sharing or documentation. The summary copies in markdown format, so when you paste it into other applications, the formatting might not appear exactly as it appears in the app.
- **Feedback**: Use the thumbs up or thumbs down icons to rate the summary's usefulness. Your feedback helps improve future summaries so they better meet your expectations and needs.
- **Refresh** (forms only): Select the **Refresh** button to regenerate the summary so it reflects the latest updates to the record.
- **Expand/Collapse** (forms only): In forms, the insights bar is collapsed by default and shows a one-line peek of the summary. Expand the insights bar to see more details.
  :::image type="content" source="/power-apps/user/media/row_summary_collapsed.png" alt-text="Screenshot that shows a collapsed row summary." lightbox="/power-apps/user/media/row_summary_collapsed.png":::

## Licensing requirements

The Row summary feature requires specific user licenses based on the product. If the user does not have the license, the feature will be hidden. This license enforcement will gradually rollout following the feature general availability.

- **Power Apps model driven app**: the user must have a Power Apps premium license with details in the [Power Platform License Guide](https://go.microsoft.com/fwlink/?linkid=2085130)
- **Dynamics 365 model driven app**: the user must have a Dynamics 365 enterprise or premium license as outlined in [Dynamics 365 License Guide](https://go.microsoft.com/fwlink/?LinkId=866544)

## Admin control

The primary admin control for row summary is moving to the Power Platform admin center under Copilot > Settings > Power Apps > Summary Agent > Row summaries. Learn more about the Copilot hub [here](/power-platform/admin/copilot/copilot-hub). 

The existing feature control in Power Platform admin center environment **Settings** > **Product** > **Feature** [link](/power-platform/admin/settings-features) leverages the app setting **AI insight cards on forms** (EnableFormInsights) and **AI insight cards on view pages** (EnableGridInsights). These app setting will be removed from the Settings > Product > Feature page and rely on the app setting in either the [Model App Designer Setting](/power-apps/maker/model-driven-apps/app-properties) or [Solution Explorer App Settings](/power-apps/maker/data-platform/create-edit-configure-settings#updating-a-setting-definition).
