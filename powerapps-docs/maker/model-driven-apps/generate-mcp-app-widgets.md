---
title: Generate MCP App widgets with AI code generation tools
description: Learn how to generate MCP app widgets with AI code generation tools.
#customer intent: As a Power Apps maker, I want to know how to use AI code generation tools like GitHub Copilot CLI or Claude Code to generate interactive MCP app widgets for your model-driven Power Apps MCP tools.
author: Mattp123
ms.author: hemantg
ms.reviewer: matp
ms.date: 04/16/2026
ms.topic: how-to
ms.service: powerapps
ms.subservice: mda-maker
---
# Generate MCP app widgets with AI code generation tools

[!INCLUDE [cc-beta-prerelease-disclaimer](../../includes/cc-beta-prerelease-disclaimer.md)]

This article explains how to use AI code generation tools like GitHub Copilot CLI or Claude Code to generate interactive model context protocol (MCP) app widgets for your model-driven Power Apps MCP tools. MCP apps are self-contained HTML files that render a tool's JSON output visually—as cards, charts, dashboards, or maps inside any MCP Apps–compatible host, including Microsoft 365 Copilot, Claude, and Visual Studio Code.

If you have an MCP tool that returns JSON data, the `generate-mcp-app-ui` skill can produce a polished, theme-aware widget that displays that data in a compact visual format directly inside a chat conversation.

> [!IMPORTANT]
>
> - This is a preview feature.
> - [!INCLUDE [cc-preview-features-definition](../../includes/cc-preview-features-definition.md)]
> - MCP apps support in Microsoft 365 Copilot Chat is generally available as of March 2026. Power Apps support for MCP apps in declarative agents is currently in public preview. For the full announcement, see [MCP Apps now available in Copilot Chat](https://devblogs.microsoft.com/microsoft365dev/mcp-apps-now-available-in-copilot-chat/).

## What you can do with the generate-mcp-app skill

- **Create visual widgets** for any MCP tool by describing what you want and pasting the tool's JSON output.
- **Choose the right visual for your data**:  charts for numeric trends, cards for structured records, tables for comparisons, maps for coordinates etc.
- **Support light and dark themes automatically** through Fluent UI design tokens.
- **Add interactivity** so widgets can call your tool again at runtime (for example, a refresh button).
- **Refine iteratively** by describing changes in natural language ("make the gallery compact", "add a chart", "use a card layout").

## How it works

1. Test your MCP tool and copy its JSON output.
2. Invoke the skill and describe the visual you want, pasting the JSON into the conversation.
3. The AI tool analyzes the data structure, selects the appropriate visual components, and generates a complete HTML widget file.
4. The widget uses the [MCP Apps protocol](#mcp-apps-protocol) to receive tool data at runtime and render it.
5. You open the HTML file in a browser to preview it, then attach it to your MCP server for deployment.

You can iterate at any point by describing changes in natural language.

## Prerequisites

### Software requirements

| Component | Minimum version | More information |
|---|---|---|
| GitHub Copilot CLI, Claude Code, or other code generation tool | Latest | [Claude Code](https://claude.ai/code), [GitHub Copilot CLI](https://github.com/features/copilot/cli/) |
| A modern browser | Any | For previewing generated widgets locally |

### Additional requirements

- An MCP tool that returns JSON output. Your tool's output type must be set to **JSON**.
- A working internet connection. Widgets load Fluent UI and other libraries from CDN at runtime.

### Install the plugin

Run the following installer command from GitHub Copilot CLI or Claude Code:

```
/plugin marketplace add microsoft/power-platform-skills
```

The installer automatically detects available tools and installs all Power Platform plugins, including `generate-mcp-apps-UI`.

To install only the MCP App widget skill:

1. Add the Power Platform Skills marketplace: `/plugin marketplace add microsoft/power-platform-skills`
2. Install the plugin: `/plugin install model-apps@power-platform-skills`

> [!TIP]
> Turn on auto-update to automatically receive skill updates. Use the `/plugin` command, navigate to **Marketplaces**, choose the marketplace, and turn on auto-update.

## Skills overview

| Skill | Command | Description |
|---|---|---|
| Generate MCP App | `/generate-mcp-app` | Generate a self-contained MCP App widget (HTML file) for an MCP tool's JSON output |

The skill is also triggered by natural language phrases such as "create a widget", "build a widget for my tool", or "make an MCP App".

## Generate a widget

Follow this workflow to create a new widget for an MCP tool.

1. **Test your tool** in your MCP development environment and copy the full JSON output. Make sure the tool's output type is set to JSON.

2. **Invoke the skill** and describe what you want displayed, pasting the JSON output into the conversation:

   ```
   /generate-mcp-app Show these locations on an interactive map.

   Here's my tool's output:
   {"attractions":[{"name":"Space Needle","latitude":47.6205,"longitude":-122.3493,"description":"An iconic observation tower"},{"name":"Pike Place Market","latitude":47.6097,"longitude":-122.3425,"description":"A historic public market"}]}
   ```

3. **Review the generated file.** The skill writes a self-contained HTML file (for example, `travel-map.html`) to your working directory and tells you where it is.

4. **Preview in a browser.** Open the HTML file locally to verify appearance in both light and dark mode.

5. **Iterate.** Describe any changes directly in the chat:
   - "Make the map larger"
   - "Add a sidebar with attraction details"
   - "Use a card layout instead"

> [!NOTE]
> The skill requires actual JSON from your tool—not sample or mock data. The data shape drives the widget generation. If you paste mock data, the generated widget may not work correctly when connected to the real tool.

## Add interactivity with callServerTool

If you also provide your tool's name when invoking the skill, the generated widget includes interactive tool-call integration. This allows the widget to call your tool again at runtime—for example, through a refresh button.

```
/generate-mcp-app Show the current weather conditions with a refresh button.
Tool name: get_weather

Tool output:
{"city":"Seattle","temp_f":54,"condition":"Overcast","humidity":78,"forecast":[...]}
```

The skill wires up `app.callServerTool` in the widget so that when users click **Refresh**, the widget fetches updated data directly from your tool.

If you don't provide a tool name, the widget is read-only and renders only the data delivered through the `ontoolresult` callback.

## Deploy your widget

Once your widget is ready, register the HTML file with your MCP server so it's returned as the tool's UI response. Refer to your MCP host's documentation for how to associate a widget with a tool result:

- **Microsoft 365 Copilot Chat**: See [MCP Apps in Copilot Chat](https://devblogs.microsoft.com/microsoft365dev/mcp-apps-now-available-in-copilot-chat/) for deployment paths including sideloading for testing, deploying through the Microsoft 365 Admin Center for organizational use, and publishing to the Microsoft 365 Agent Store.
- **Power Apps declarative agents**: See [Power Apps MCP declarative agent documentation](https://learn.microsoft.com/en-us/power-apps/maker/model-driven-apps/generative-page-external-tools) for how to connect MCP tools with model-driven apps.
- **Other MCP hosts**: Consult your host's documentation for the MCP Apps widget registration process.

## Widget technical details

### MCP Apps protocol

Widgets communicate with the chat host using the `App` class from the `@modelcontextprotocol/ext-apps` package. The protocol manages:

| Callback / method | Description |
|---|---|
| `app.ontoolresult` | Fires when the host delivers tool data. **Your data is always at `result.structuredContent`**—not `result.data` or `result` itself. |
| `app.onhostcontextchanged` | Fires when the host context changes, including theme (`ctx.theme` is `'light'` or `'dark'`). |
| `app.onteardown` | Fires when the widget is removed from the conversation. |
| `app.connect()` | Establishes communication with the host. All event handlers must be registered **before** calling `connect()`. |
| `app.getHostContext()` | Returns the current host context (including initial theme) after `connect()` completes. |
| `app.callServerTool({ name, arguments })` | Calls a tool interactively. Returns `result.isError` and `result.structuredContent`. |

> [!IMPORTANT]
> `App` is a **named export**. Use `import { App } from '...'` with curly braces. A default import (`import App from '...'`) results in `App` being `undefined` at runtime.

### CDN imports

Widgets load all dependencies from CDN. No build step or local installation is required:

| Library | Import | Purpose |
|---|---|---|
| `@modelcontextprotocol/ext-apps` | ESM: `https://cdn.jsdelivr.net/npm/@modelcontextprotocol/ext-apps/+esm` | MCP Apps `App` class |
| `@fluentui/web-components@beta` | UMD script tag from `unpkg.com` | Fluent UI components (buttons, cards, badges, spinners, etc.) |
| `@fluentui/tokens` | ESM: `https://cdn.jsdelivr.net/npm/@fluentui/tokens/+esm` | `webLightTheme` / `webDarkTheme` token sets |

### Display modes

MCP Apps in Microsoft 365 Copilot support two display modes:

- **Inline (required)**: The widget renders as a compact card directly in the conversation, appearing before the model's text response. All widgets generated by this skill use inline mode.
- **Side-by-side (optional)**: An expanded workspace that opens alongside the conversation for complex workflows. This mode requires additional configuration beyond what the skill generates. See the [MCP Apps developer documentation](https://devblogs.microsoft.com/microsoft365dev/mcp-apps-now-available-in-copilot-chat/) for details.

### Fluent UI components

The following Fluent UI Web Components are available in widgets:

`<fluent-card>`, `<fluent-button>`, `<fluent-text-input>`, `<fluent-textarea>`, `<fluent-dropdown>`, `<fluent-listbox>`, `<fluent-option>`, `<fluent-checkbox>`, `<fluent-spinner>`, `<fluent-divider>`, `<fluent-badge>`, `<fluent-switch>`, `<fluent-tooltip>`

### Theme support

Widgets support light and dark themes through Fluent UI design tokens. The widget applies the correct token values when the host's theme changes via `onhostcontextchanged`. Always use token variables (for example, `var(--colorNeutralForeground1)`) rather than hardcoded color values to ensure correct rendering in both themes.

### Data type safety

MCP tool responses are JSON, but field types at runtime can differ from test data: numbers may arrive as strings, booleans as `"true"`/`"false"` strings, and absent fields as empty strings. Use these patterns when reading fields:

```javascript
// Numeric fields
const lat = parseFloat(item.latitude);
if (!isFinite(lat)) { /* treat as missing */ }

// Optional text fields
const desc = (item.description == null || String(item.description).trim() === '')
  ? null : String(item.description).trim();

// Boolean fields
const isActive = item.active === true || String(item.active).toLowerCase() === 'true';
```

### Security

Always escape user-provided content before inserting it into HTML. Prefer `textContent` over `innerHTML` wherever possible:

```javascript
function escapeHtml(text) {
  const div = document.createElement('div');
  div.textContent = text;
  return div.innerHTML;
}
```

### UMD global collision

When you load third-party UMD libraries (such as Leaflet for maps or Chart.js for charts) alongside Fluent UI Web Components, the Fluent UI UMD bundle can overwrite the third-party library's global variable. Save the library's global immediately after it loads:

```html
<script>
  (function() {
    var s = document.createElement('script');
    s.src = 'https://cdn.example.com/leaflet.js';
    s.onload = function() {
      window.L_saved = window.L; // save before Fluent overwrites it
      var f = document.createElement('script');
      f.src = 'https://unpkg.com/@fluentui/web-components@beta/dist/web-components.min.js';
      document.head.appendChild(f);
    };
    document.head.appendChild(s);
  })();
</script>
```

Then use the saved reference (`window.L_saved`) for all API calls, not the original global.

## Design guidelines

These defaults produce polished, accessible widgets. They can be adjusted through follow-up requests.

### Visual states

Every widget handles three states:

| State | Guidance |
|---|---|
| **Loading** | Show a `<fluent-spinner>` with a contextual message ("Finding attractions…" not just "Loading…"). |
| **Data** | Render the content compactly. Use the full available width. |
| **Error** | Show a friendly message and a "Try again" button. If the widget uses `callServerTool`, the button re-invokes the tool. |

### Spacing

Use only these values: 4 px, 8 px, 12 px, 16 px, 24 px, 32 px.

- Card padding: 16 px or 24 px
- Gap between items: 8 px or 12 px
- Gap between sections: 16 px or 24 px
- Border radius: 8 px (small elements), 12 px (cards/containers)
- Do **not** set `max-width` on the widget container. Use the full available width.

### Typography

Use 2–3 font sizes: 1 rem (headings, weight 600), 0.875 rem (body), 0.75 rem (captions). Do not import custom fonts.

### Color tokens

| Use | Token |
|---|---|
| Primary text | `var(--colorNeutralForeground1)` |
| Secondary text | `var(--colorNeutralForeground2)` |
| Primary background | `var(--colorNeutralBackground1)` |
| Card/hover background | `var(--colorNeutralBackground2)` |
| Brand/accent | `var(--colorBrandBackground)` |
| Text on brand surface | `var(--colorNeutralForegroundOnBrand)` |
| Borders | `var(--colorNeutralStroke1)` |
| Error text | `var(--colorStatusDangerForeground1)` |
| Success text | `var(--colorStatusSuccessForeground1)` |

Never use hardcoded hex or RGB values, and never invent token names not listed here.

### Accessibility

- Add `aria-label` to icon-only buttons and non-text interactive elements.
- Use semantic HTML: headings for sections, lists for groups.
- Wrap all animations in `@media (prefers-reduced-motion: no-preference) { }`.
- Disable action buttons while a tool call is in flight to prevent double-submission.

## Best practices

- **Provide real test data.** The skill analyzes the actual JSON structure to select the right visual. Mock data produces widgets that break when connected to the real tool.
- **Be specific about the visual.** Describe the format you want (map, chart, table, card layout). Vague descriptions lead to generic results.
- **Start with one view.** Widgets are compact conversation cards, not full applications. No tabs, page navigation, or search bars that duplicate the chat input.
- **Test with both themes.** Preview in light and dark mode to verify contrast and readability.
- **Match the visual to the data.** Maps for coordinates, charts for numeric/trend data, cards for structured records, tables for comparisons.

## Limitations

- Widgets must load all external libraries from CDN. An internet connection is required at runtime.
- Side-by-side display mode requires additional implementation beyond what the skill generates.
- The skill does not handle MCP server registration or deployment to M365 Admin Center. You must complete those steps separately.
- Authentication (OAuth 2.1, Microsoft Entra SSO) is handled by the MCP host environment, not the widget HTML itself.

## Related documentation

### Microsoft 365 developer documentation
- [MCP Apps now available in Copilot Chat](https://devblogs.microsoft.com/microsoft365dev/mcp-apps-now-available-in-copilot-chat/)
- [Microsoft 365 Agents Toolkit](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/teams-toolkit-fundamentals)

### Power Platform documentation
- [Create and edit generative pages with AI code generation tools](https://learn.microsoft.com/en-us/power-apps/maker/model-driven-apps/generative-page-external-tools)
- [Power Platform CLI reference](https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction)

### External references
- [Model Context Protocol (MCP) specification](https://modelcontextprotocol.io/)
- [Fluent UI Web Components](https://fluent2.microsoft.design/)
- [Power Platform Skills repository](https://github.com/microsoft/power-platform-skills)
- 