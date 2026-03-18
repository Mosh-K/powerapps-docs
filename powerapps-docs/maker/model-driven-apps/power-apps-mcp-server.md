---
title: "Work with Power Apps MCP Server" 
description: Learn about the tools available with the Power Apps MCP Server.
ms.date: 02/10/2026
ms.reviewer: matp
ms.topic: how-to
author: HemantGaur
ms.subservice: mda-maker
ms.author: hemantg
ms.service: powerapps
search.audienceType: 
  - maker
---
# Work with Power Apps MCP Server

The model context protocol (MCP) is an open protocol that enables seamless integration between large language model (LLM) applications and external data sources and tools. Your agent can use the Power Apps MCP Server to communicate with your Power Apps, providing right human-in-the-loop supervision or agentic workflows.

> [!IMPORTANT]
>
> - This is a preview feature.
> - Preview features aren't meant for production use and might have restricted functionality. These features are subject to [supplemental terms of use](https://go.microsoft.com/fwlink/?linkid=2216214), and are available before an official release so that customers can get early access and provide feedback.
> - This feature is available only in the English language and it replaces the earlier Microsoft Copilot Studio activity-based agent feed.
> - For information about how AI is used with this feature, go to [FAQ about Power Apps MCP Server invoke_data_entry tool](../common/faq-power-apps-mcp-server-invoke-data-entry.md).

The Power Apps MCP Server equips your agent with two types of capabilities:

- Automate repetitive app tasks:

   The Power Apps MCP Server enables agents to use advanced app tools developed in Power Apps. For example, the data‑entry agent capabilities previously available as an on‑demand AI feature are now accessible to any agent through Power Apps MCP server. To use them, you create your agent, configure the MCP tool, and direct it to unstructured content so it can generate Dataverse records with human review and approval through the enhanced agent feed.
- Supervise agent activity:

   The Power Apps MCP Server also provides specialized tools to business users to supervise any agent activity in the agent feed. Agents can now handoff control to humans for review, assistance, and steering with the MCP tools. These tools provide makers with much more control over the tasks they want to publish to the agent feed and when they need agent-human handoff.

:::image type="content" source="media/add-agents-to-app/power-apps-mcp-server.png" alt-text="Power Apps MCP server" lightbox="media/add-agents-to-app/power-apps-mcp-server.png":::

> [!NOTE]
> Access to the agent feed and supervision capabilities is limited by default to the System Administrator and System Customizer roles. To allow additional users to view the agent feed, grant organization‑level read/write permissions on the entities listed below. You can create a new security role with these permissions and assign it to multiple users as needed.
> - Agent Hub Goal(agenthubgoal)
> - Agent Hub Insight(agenthubinsight)
> - Agent Hub Metric(agenthubmetric)
> - Agent Task(agenttask)
> - Copilot(bot)

The Power Apps MCP tools improve the more you use them. For example, when you make corrections to suggestions in the agent canvas, the data entry tool improves based on your corrections. To use the enhanced agent feed capabilities, enable and configure the Power Apps MCP Server from the Microsoft Copilot Studio agent. Once configured, you can invoke Power Apps MCP Server tools from agent instructions using natural language.

More information: [Create an autonomous agent connected to Power Apps MCP Server](add-agents-to-app.md#create-an-autonomous-agent-connected-to-power-apps-mcp-server)

> [!IMPORTANT]
>
> For autonomous agent scenarios where an agent runs via a trigger, the Power Apps MCP Server must be configured to run using "Maker-provided credentials" as seen in the details section of the tool. See [Control maker-provided credentials for authentication](https://learn.microsoft.com/en-us/microsoft-copilot-studio/configure-no-maker-authentication#scope-of-enforcement-and-experience) for more details if this option is disabled.
>    :::image type="content" source="media/add-agents-to-app/copilot-studio-configure-maker-credentials.png" alt-text="Maker-provided credentials selection":::  


## List of tools

Once connected to the Power Apps MCP Server, the agent can choose from various tools in the Power Platform environment. These tools can generate agent feed items that render different user experiences, such as a side‑by‑side view for data entry agents or direct navigation to a record for `request_for_assistance` scenarios. 

| Tool | Description |
|------|-------------|
| [log_for_review](#log_for_review) | Log completed activity for passive human oversight. |
| [request_assistance](#request_assistance) | Request assistance from a human user. |
| [invoke_data_entry](#invoke_data_entry) | Create one or more records in a data source like Microsoft Dataverse, using contents from plain text or an email.|

## log_for_review

Records completed agent work to the agent feed for review. The `log_for_review` tool is intended for scenarios where an agent has sufficient information to act autonomously but the user should still be made aware of what the agent has done. This tool can be thought of as a way to passively oversee the high-confidence or low-risk actions performed by agents. It is best suited for decisions that can be easily revised or rolled back if the agent happens to perform the action incorrectly. Besides title, description, and steps you can also ask the tool to add a link to either the relevant Dataverse record or to an app-external URL. If an agent action touches multiple Dataverse records, you can instruct the agent which record it should navigate to in connection with the created task. It could be the link to the record the agent created using the Dataverse MCP server or a record link present in context like the record that triggered the agent execution. These tasks are shown in the agent feed's **Completed** tab.

### Sample instruction

When the customer makes a booking from the portal this agent must log the details for review. The review item title should be based on the booking reference number and must use the exact prefix “Review Web Booking: ”. In the review description, write a concise summary of the booking that includes main fields like Booking Reference, Booking Date, Seat Number, and Status, so a reviewer can quickly understand what was processed without opening the record. Ensure the description reads as a short paragraph and accurately reflects the current values from the booking record. Include your reasoning as steps. Also, include a link to the booking record.

:::image type="content" source="media/add-agents-to-app/log-for-review-example.png" alt-text="Log for review example":::

## request_assistance

The intended purpose of the `request_assistance` tool is to enable agents to surface errors, escalations, or exceptions to users, so they can take appropriate action. As a maker, you can define the scenarios for when to have your agent use the `request_assistance` tool. It creates an agent feed task that is populated in the **Needs Attention** section of the agent feed. This is an asynchronous operation that calls the Microsoft Copilot Studio agent that waits until the human completes the action. For details on completing the action feed activity, go to [Supervise agents in model-driven apps with agent feed (preview)](../../user/supervise-agents-with-agent-feed.md#supervise-agents-in-model-driven-apps-with-agent-feed-preview) 

You can observe the **In progress** status for the agent run in the activity tab when viewing the agent in Copilot Studio. Once the user completes the activity from agent feed, control comes back to agent via callback and agent can complete the task. 

:::image type="content" source="media/add-agents-to-app/copilot-studio-agent-in-progress.png" alt-text="In progress status in Copilot Studio":::

Like with the `log_for_review` tool, you can control the task output for title, description, and steps and can be specific when telling the agent which link to associate with a given task.

### Sample instruction

When this agent is triggered by the creation of a new support case, it should request assistance. In the request, set the title by prefixing the value of issue with “Assistance needed: ”. In the task description include the issue type, issue description, date reported, and the Resolved value. Include your reasoning steps. Also include a link to the related Dataverse issue record. Once the user completes the task, continue processing by <insert next step for agent>.

:::image type="content" source="media/add-agents-to-app/request-assistance-with-nav-example.png" alt-text="Request user assistance example":::

## invoke_data_entry

The `invoke_data_entry` tool streamlines the creation of Dataverse records by extracting structured information from unstructured inputs such as emails, messages, or documents. When invoked from a Copilot Studio agent, it automatically analyzes incoming content, fills out the appropriate form with the extracted data, and presents the proposed entry as a task in the agent feed for user review and approval. It requires review of the propsoed entry by a user before creating the record. This enables fast, reliable data capture with minimal manual effort.

### Sample instruction - shared email triggered agent

You are the travel idea generator agent. Your job is to process incoming emails and create travel idea records in Dataverse.

When an email arrives:

1. Determine if it contains travel-related information (either in the email body or attachments).
2. Use the `invoke_data_entry` tool to create a travel idea record with the extracted information in the following columns:

   - cr3ea_title
   - cr3ea_description
   - cr3ea_triptype
   - cr3ea_customername
   - cr3ea_customeremail
   - cr3ea_customerphone
   - cr3ea_destinationcity
   - cr3ea_travelstart
   - cr3ea_travelend
   - cr3ea_numberoftravelers
   - cr3ea_budgetusd
   - cr3ea_specialrequests
3. If information is missing, still create the record with available data - leave unknown fields empty.

:::image type="content" source="../../user/media/agent-supervision/agent-feed-accept-complete.png" alt-text="Agent feed accept and complete button" lightbox="../../user/media/agent-supervision/agent-feed-accept-complete.png":::

> [!NOTE]
>
> - When you write instructions for your agent, always reference Dataverse columns by their logical names as shown in the sample instruction. Clear, direct instructions help the agent reliably create records from the input. You can view a column’s logical name by opening the table in make.powerapps.com, select **Columns**, and then open the column to view the details.
> - `invoke_data_entry` tool supports .pdf, .xlsx, .docx, .jpeg, .jpg, .png, .gif and.bmp formats.
> - `invoke_data_entry` tool can populate single line of text (None format), Whole number and Decimal column types.
> - Ensure that the user has permission to create records for the target table.

### How the invoke_data_entry tool works

When you configure a Copilot Studio agent to use the Power Apps MCP Server and enable the `invoke_data_entry` tool, the agent follows this process:

1. [An agent trigger](/microsoft-copilot-studio/authoring-triggers-about) is fired based on your configuration — such as an email arriving in a monitored mailbox or new document uploaded to SharePoint.
1. The agent analyzes incoming content and your instructions to determine whether the `invoke_data_entry` tool should be used.
1. If needed, the `invoke_data_entry` tool is invoked, passing the input content and the target Dataverse table and table columns to predict.
1. The tool processes the input, extracts relevant information, and populates a Dataverse form with suggested values for each mapped column.
1. A task appears in the agent feed. Selecting it opens the data‑entry review experience. The left panel shows the original input, and the right panel displays the form populated with suggested values.
1. The user can review the extracted values, make corrections if needed, and then save the record to Dataverse.

### Provide feedback

To provide feedback about the invoke_data_entry tool:

1. Open a invoke_data_entry task in the agent feed.
1. Select the feedback button in the task header.
1. Choose to give a compliment, report an issue, or make a suggestion.

:::image type="content" source="media/add-agents-to-app/agent-feed-feedback.png" alt-text="Agent feed feedback button":::



## Related articles

[Add agents to your model-driven app (preview)](add-agents-to-app.md)

[Supervise agents in model-driven apps with agent feed (preview)](../../user/supervise-agents-with-agent-feed.md)
