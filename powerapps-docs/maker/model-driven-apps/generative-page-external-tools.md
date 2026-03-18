---
title: Create and edit generative pages with AI code generation tools
description: Learn how to use AI code generation tools like Claude Code to create and edit generative pages for model-driven apps in Power Apps.
author: jasongre
ms.author: jasongre
ms.reviewer: matp
ms.date: 02/24/2026
ms.topic: how-to
ms.service: powerapps
ms.subservice: mda-maker
search.audienceType:
- maker
- developer
ms.collection:
- bap-ai-copilot
applies_to:
- PowerApps
---

# Create and edit generative pages with AI code generation tools (preview)

[!INCLUDE [cc-beta-prerelease-disclaimer](../../includes/cc-beta-prerelease-disclaimer.md)]

This article describes how to use AI code generation tools, such as GitHub Copilot CLI and Claude Code, to create and edit generative pages for model-driven apps in Power Apps. This approach allows you to integrate advanced code generation capabilities directly into your development workflow, allowing you to create new generative pages or iterate on existing ones using natural language instructions.

> [!IMPORTANT]
>
> - This is a preview feature.
> - [!INCLUDE [cc-preview-features-definition](../../includes/cc-preview-features-definition.md)]

Using AI code generation tools with generative pages provides an alternative development approach that complements the UI-based experience in Power Apps (make.powerapps.com). This code-first approach is designed for developers and technical makers who prefer working with local development tools and CLI-based workflows.

## What you can do with code generation tools

- *Create new generative pages* using plain-language requirements
- *Update existing generative pages* by requesting changes or enhancements through your AI tool
- *Deploy directly* to your Power Apps environment using PAC CLI commands
- *Work locally* with your preferred IDE and development tools

### How it works

1. You describe what you want to build in natural language, for example, "Create a generative page dashboard showing top accounts by revenue."
2. The AI code generation tool uses installed generative page skills or other context about generative pages to ask clarifying questions about your requirements.
3. The tool generates production-ready TypeScript and React code for your generative page.
4. The tool deploys the code to your environment using [generative page PAC CLI commands](/power-platform/developer/cli/reference/model).
5. Your generative page appears in your model-driven app.

## Prerequisites

Before you start, ensure you have the required software and permissions described here.

### Software requirements

| Component | Minimum version | More information |
|-----------|-----------------|------------------|
| Node.js | 18.0 or later | [Download Node.js](https://nodejs.org/) |
| Power Platform CLI (PAC CLI) | Latest | [Install PAC CLI](/power-platform/developer/cli/introduction?tabs=windows#install-microsoft-power-platform-cli) |
| GitHub Copilot CLI, Claude Code, or other code generation tool | Latest | [GitHub Copilot CLI](https://github.com/features/copilot/cli/) or [Claude Code](https://claude.ai/code) |

### Additional requirements

- A Power Platform environment with a model-driven app to deploy pages.
- An *authenticated PAC CLI session* connected to your target environment.
   - Go to [Authenticate Power Platform CLI](/power-platform/developer/cli/reference/auth) for more details on getting connected.

> [!NOTE]
> Your Power Platform environment must be located in the US region. This capability is coming to other regions soon.

### Install the plugin

Run the installer to set up all Power Platform plugins in either PowerShell or a Windows command window.

```powershell
iwr https://raw.githubusercontent.com/microsoft/power-platform-skills/main/scripts/install.js -OutFile install.js; node install.js; del install.js
```

```dos
curl -fsSL https://raw.githubusercontent.com/microsoft/power-platform-skills/main/scripts/install.js | node
```

The installer automatically:

- Detects available tools (Claude Code, GitHub Copilot CLI)
- Registers the plugin marketplace and installs all plugins  
- Enables auto-update so plugins stay current

After installation, restart your AI tool if needed.

### Install only the generative page plugin

To install only the generative page plugin for GitHub Copilot CLI or Claude Code:

1. Add the Power Platform Skills marketplace plugin: `/plugin marketplace add microsoft/power-platform-skills`
2. Install the Power Apps plugin: `/plugin install model-apps@power-platform-skills`

> [!NOTE]
> For Claude Code, you can install the plugin with different scopes, such as global, local, or user. Depending on the scope, you need to be in the correct directory for Claude Code to use the plugin. Go to [Extend Claude with skills](https://code.claude.com/docs/en/skills#share-skills)

Once installed, you can use the plugin by either:

- Running the `/genpage` command explicitly.
- Describing the page you want to create. The tool automatically detects and uses the plugin.

> [!TIP]
> Turn on auto-update to automatically receive updates to the marketplace and skills. Use the `/plugin` command, navigate to **Marketplaces**, choose the marketplace, and turn on auto-update.

### Using other AI code generation tools

For other AI code generation tools, ensure your tool has access to the generative page resources from the [Power Platform skills](https://github.com/microsoft/power-platform-skills/tree/main/plugins/model-apps) GitHub repository. The model-apps plugin folder includes component documentation, sample code, PAC CLI command reference, and workflow instructions necessary to create code adhering to generative page requirements. Consult the repository [readme](https://github.com/microsoft/power-platform-skills/blob/main/plugins/model-apps/README.md) for information on accessing and using these resources with your preferred tool.

## Skills overview

The Power Apps plugin provides this skill for working with generative pages.

| Skill | Command | Description | 
|-------|-------------|---------|
| Generative pages |  `/genpage` | Create code for generative pages (for creation or editing scenarios) |

This skill enables you to describe what you want to build and have the AI tool generate complete TypeScript and React code for your generative page, then deploy it directly to your Power Apps environment.

## Create a new generative page

Follow this workflow when building a new page from scratch.

1. Start a conversation with your AI tool. Describe what you want to create, including what data you want to include (which Dataverse tables or whether to create sample mock data). Be as specific as you want—the more vague you are with the request, the more details the agent tries to fill in itself. You can also attach or provide an image or other materials to help guide the visuals, theming, and layout. For example:
   - "Create a generative page dashboard showing our top 10 accounts by revenue using the Account table"
   - "Build a generative page form for creating and editing contact records with sample data"
   - "Make a generative page for displaying incident reports on a map using the Incident table"
   - "Create a generative page for a sales pipeline visualization with opportunities using the modern blue theme"

   If your page needs to support multiple languages, include that in your description — see [Localize a generative page](#localize-a-generative-page).

2. Answer clarifying questions. The AI tool asks questions to understand your requirements. Be specific about business needs and data requirements, identify mobile requirements early, and mention any specific UI components or layout preferences.

3. Review the implementation plan. The AI tool presents a plan describing the components to be built, Dataverse tables and columns to be used, key features and interactions, and data retrieval approach. Confirm the plan meets your requirements or request changes.

4. Review code and deploy. The AI tool generates complete TypeScript code. Ask the tool to publish or deploy your page when you're ready, optionally specifying a sitemap name (the tool generates a meaningful name by default).

5. Test and iterate. Open your model-driven app in Power Apps and navigate to the new page using the sitemap. If you need to make changes, return to your AI tool and describe the updates using natural language.

> [!NOTE]
> You can change the name or position of the generative page in the sitemap at any time from the model-driven app designer.

## Edit an existing generative page

Use this workflow to update a page that already exists in your environment.

1. Retrieve the existing page. In your AI code generation tool, request to retrieve the existing generative page by providing the page ID (GUID) or the page name in the sitemap and the app it's in. For example, "I want to update the Pet Adoption generative page from the Demo app."

1. Describe your updates. Tell the AI tool what changes you want to make. For example:
   - "Add a filter to show only active records"
   - "Change the layout to display cards in a grid instead of a list"
   - "Add a chart showing adoption trends over time"
   - "Update the form to include the new custom field for pet temperament"

1. Review, publish, test, and iterate. The AI tool generates updated TypeScript code based on your requested changes. Follow the same review, publish, and test process described in the "Create a new generative page" section. Continue iterating with natural language instructions until the page meets your requirements.

## Localize a generative page

If your Power Apps environment is configured with multiple languages, you can localize your generative page to display content in the user's preferred language and format numbers, dates, and currency according to their regional settings.

Use the following guidance to instruct your AI code generation tool to implement localization, or to manually add it to existing generated code.

### Step 1: Identify configured languages

Run the following PAC CLI command to see which languages are configured in your environment:

```powershell
pac model list-languages
```

Note the language names and their locale IDs (LCIDs) from the output — you need these in the steps that follow.

### Step 2: Add language detection

Ask your AI tool to add language detection based on the user's Xrm context, using the LCIDs from the previous step. For example:

> "Add language detection to this generative page. My environment has English (1033), French (1036), and Arabic (1025) configured. Use `Xrm.Utility?.getGlobalContext()?.userSettings?.languageId` to read the current user's language, defaulting to 1033 if unavailable."

The generated code reads the current user's language preference at runtime and falls back to English if no match is found:

```typescript
const language = React.useMemo(() => {
  const uiLanguageId = (typeof Xrm !== "undefined" &&
    Xrm.Utility?.getGlobalContext()?.userSettings?.languageId) || 1033;
  const langMap: Record<number, { code: string; name: string; isRtl: boolean }> = {
    1033: { code: "en-US", name: "English", isRtl: false },
    1036: { code: "fr-FR", name: "French", isRtl: false },
    1025: { code: "ar-SA", name: "Arabic", isRtl: true },
  };
  return langMap[uiLanguageId] || { code: "en-US", name: "English", isRtl: false };
}, []);
```

### Step 3: Add a translations dictionary

Ask your AI tool to create a translations dictionary for all user-visible text. For example:

> "Add a translations dictionary for English, French, and Arabic for all the labels and text in this page."

The pattern looks like this:

```typescript
const translations: Record<string, Record<string, string>> = {
  "en-US": {
    title: "Dashboard",
    save: "Save",
    cancel: "Cancel",
  },
  "fr-FR": {
    title: "Tableau de bord",
    save: "Enregistrer",
    cancel: "Annuler",
  },
  "ar-SA": {
    title: "لوحة القيادة",
    save: "حفظ",
    cancel: "إلغاء",
  },
};
```

### Step 4: Support right-to-left (RTL) languages

If your environment includes Arabic or Hebrew, ask your AI tool to add RTL layout support:

> "Add RTL support for Arabic."

### Step 5: Use user settings for number, date, and currency formatting

Dataverse stores each user's regional formatting preferences in the `usersettings` table. Ask your AI tool to fetch these and use them for all formatted values:

> "Fetch user formatting preferences from the `usersettings` Dataverse table using `dataApi.retrieveRow`, using the current user's ID from `Xrm.Utility?.getGlobalContext()?.userSettings?.userId`. Retrieve these columns: `decimalsymbol`, `numberseparator`, `currencysymbol`, `dateformatstring`, `dateseparator`. Use these to build formatting helpers for all dates, numbers, and currency values in the page. Do not use `Intl.NumberFormat` with hardcoded locale or currency codes, and do not hardcode any currency symbols."

## Troubleshooting

### Page fails to load in Power Apps

If you navigate to your generative page and observe an error message or blank screen:

1. Open browser developer tools (F12 in most browsers).
2. Select the **Console** tab.
3. Copy the complete error message, including the stack trace.
4. Return to your AI code generation tool and paste the error with context:

   "I'm getting this error when opening the page: [paste error here]. Please fix the issue."

   The AI tool analyzes the error, identifies the root cause, and generates a fix.
5. Review the fix and ask the tool to republish the page.

### Reverting to a working version

If recent changes broke your page or made issues worse, you can ask the AI tool to roll back to a previous working version:

"The recent changes broke the page. Please revert to the last working version."

The AI tool then:

1. Identifies the changes that were made
1. Restores the previous working code
1. Redeploys the stable version

## Best practices

- Start simple. Begin with a basic version of your page and iterate to add complexity.
- Test frequently. Deploy and test your page after each significant change.
- Be specific. Provide detailed requirements to get better initial results.
- Use existing patterns. Reference similar pages or UI patterns when describing your requirements.
- Validate generated code. Always review the generated code to ensure it meets your organization's standards and compliance requirements.

> [!IMPORTANT]
> While AI code generation tools make a best-effort attempt to generate complete, production-ready code with accessibility and security best practices, you're ultimately responsible for validating the code. Ensure the generated code meets your organization's standards, policies, and compliance requirements.

## Limitations

The limitations for generative pages created with AI code generation tools are the same as those for generative pages created in the Power Apps maker portal:

- Your page can connect to only Dataverse tables.
- Localization support when creating generative pages through the Power Apps maker portal is limited to US English. For multi-language environments, see [Localize a generative page](#localize-a-generative-page).
- Collaboration isn't supported—ensure only one maker is working on a generative page at a time.
- Only these data types are supported: Choice, Currency, Customer, Date and Time, Date Only, Decimal Number, Floating Point Number, Image, Lookup, Multiline Text, Status, Status Reason, Text, Whole Number, Yes/No, Unique Identifier.

## Related content

- [Generate a page using natural language with model-driven apps](/power-apps/maker/model-driven-apps/generative-pages)
- [dataApi object methods for generative pages](/power-apps/developer/model-driven-apps/generative-page/data-api/)
- [Power Platform CLI reference](/power-platform/developer/cli/reference/index)
