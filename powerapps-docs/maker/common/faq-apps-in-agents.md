---
title: FAQ for Power Apps in Agents
description: This FAQ provides information about Power Apps in Agents with key considerations and details about how AI is used, how it was tested and evaluated, and any specific limitations.
ms.date: 3/4/2026
ms.update-cycle: 180-days
author: clromano-microsoft
ms.author: mkaur
ms.reviewer: mkaur
ms.topic: faq
ms.subservice: common
ms.custom:
  - transparency-note
  - responsible-ai-faqs
ms.collection:
  - bap-ai-copilot
---
# FAQ for Power Apps in Agents

## What is Power Apps in Agents?

Power Apps in Agents enables users to interact with data from Power Apps model-driven apps directly in Microsoft 365 Copilot through natural language. When makers deploy a declarative agent for a model-driven app, users can query records, view data in grids, create and edit records, and receive AI-generated summaries — all through a Copilot conversation, without switching to the app.

The agent connects to the model-driven app's underlying Dataverse tables and uses the same data exploration capabilities that power the Grid view experience in model-driven apps.

---

## What are the system's capabilities?

The system can:

- Query and filter records across tables configured in the model-driven app using natural language
- Display results as an interactive grid, consistent with the Grid view in the app
- Show individual record details using a Form, consistent with the Form experience in model-driven apps
- Create and update records using the Form widget in Copilot
- Hand off to the full model-driven app for actions that require the complete app experience

---

## What is the system's intended use?

Power Apps in Agents serves two audiences:

**For makers** — The feature is intended to extend the reach of an existing Power Apps model-driven app by making its data accessible to users directly in Microsoft 365 Copilot. Makers deploy a declarative agent for their app. No additional app development is required beyond the initial setup.

**For end users** — The feature is intended to help business users access and act on their organization's data without leaving Microsoft 365 Copilot, reducing context-switching and enabling faster decisions in the flow of work. It is not intended to replace the full model-driven app experience, but to make common data tasks — querying, reviewing, and updating records — accessible through Microsoft 365 Copilot. When more complex actions are needed, it also serves as a handoff point directly to the model-driven app.

---

## How was Power Apps in Agents evaluated? What metrics are used to measure performance?

The system was evaluated across a range of query types, table configurations, record volumes, and user scenarios. Evaluation focused on query accuracy, widget render fidelity, and the correctness of information presented in them.

If you encounter issues, submit feedback through Microsoft 365 Copilot. Your feedback is used to improve Microsoft products and services. For more information, see [Data, privacy, and security for Azure OpenAI Service](https://learn.microsoft.com/en-us/legal/cognitive-services/openai/data-privacy).

---

## What are the limitations of Power Apps in Agents? How can users minimize the impact of these limitations?

**Query accuracy** — Data retrieval results may not always match the user's intent on the first attempt. Users can iterate using natural language to refine results — for example, by guiding Copilot on which column or field they meant in their original ask.

**Unsupported field types in forms** — Certain field types are not supported in the Form Fill experience. For those fields, users will need to open the full model-driven app to complete or edit the record.

**English only** — Power Apps in Agents currently supports English-language queries and responses only. Users interacting in other languages may receive inaccurate or unexpected results.

---

## What operational factors and settings allow for effective and responsible use of the system?

- **Data retrieval is powered by the [Data Exploration Agent](https://learn.microsoft.com/en-us/power-apps/user/find-data-with-ai).** Power Apps in Agents leverages the Data Exploration Agent, the same underlying AI capability that powers the Grid view experience in Power Apps model-driven apps. As a result, the quality and accuracy of data retrieval is benchmarked against and consistent with the data exploration experience users already have in model-driven apps.

- **Form filling is powered by [Form Fill Assistance](https://learn.microsoft.com/en-us/power-apps/user/form-filling-assistance).** Power Apps in Agents leverages Form Fill Assistance, the same underlying AI capability that powers field suggestions in Power Apps model-driven app forms. As a result, the quality and accuracy of prefilled values is benchmarked against and consistent with the form-filling experience users already have in model-driven apps. Users should review prefilled values before saving.

---

## How do I provide feedback?

Use the thumbs up or thumbs down controls in Microsoft 365 Copilot to provide feedback on individual responses. Microsoft uses this feedback to improve products and services.

---

## Related information

- [Set up App Skills in Copilot](maker-setup.md)
- [Grid and Form widgets reference](widgets.html)
- [Use grid filters and views in model-driven apps](https://learn.microsoft.com/en-us/power-apps/user/grid-filters)
- [FAQ for Copilot chat in model-driven apps](https://learn.microsoft.com/en-us/power-apps/maker/common/faqs-copilot-model-driven-app)
- [Responsible AI FAQs for Microsoft Power Platform](https://learn.microsoft.com/en-us/power-platform/responsible-ai-overview)
- [Data, privacy, and security for Azure OpenAI Service](https://learn.microsoft.com/en-us/legal/cognitive-services/openai/data-privacy)
