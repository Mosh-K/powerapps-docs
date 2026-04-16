---
title: Icon modern control in canvas apps - Power Apps
description: Learn about the details, properties, and examples of the Icon modern control in Power Apps.
author: yogeshgupta698
ms.topic: reference
ms.custom: canvas
ms.date: 04/15/2026
ms.subservice: canvas-maker
ms.author: yogupt
ms.reviewer: mkaur
search.audienceType:
  - maker
---

# Icon modern control in canvas apps

Display a Fluent icon that can optionally respond to user interaction.

## Description

The **Icon** modern control displays a Fluent icon from the Fluent icon library. It can act as a purely decorative element or, when **OnSelect** is set, become an interactive element that responds to clicks and keyboard input. Use it for toolbar actions, status indicators, inline navigation, or anywhere you need a lightweight visual element. Key properties for this control are **Icon**, **IconStyle**, and **OnSelect**.

> [!NOTE]
> This article describes the updated Icon modern control. For information about what changed from the previous version, see [Recent updates](#recent-updates).

## General

**Icon** – The name of the Fluent icon to display (for example, `"Add"`, `"Delete"`, `"Edit"`). Accepts any valid Fluent icon name as a string.

**Visible** – Whether the control appears or is hidden.

## Behavior

**OnSelect** – Actions to perform when the user selects the icon. When this property contains an active formula, the icon renders with button semantics for keyboard and screen reader accessibility.

**DisplayMode** – Whether the control allows user input (**Edit**), only displays data (**View**), or is disabled (**Disabled**).

## Size and position

**[X](../properties-size-location.md)** – Distance between the left edge of the control and the left edge of its parent container (screen if no parent container).

**[Y](../properties-size-location.md)** – Distance between the top edge of the control and the top edge of its parent container (screen if no parent container).

**Width** – Distance between the control's left and right edges. Default is **32**.

**Height** – Distance between the control's top and bottom edges. Default is **32**.

**Rotation** – The angle in degrees to rotate the icon clockwise. Default is **0**.

**PaddingTop** – Distance between the icon and the top edge of the control.

**PaddingBottom** – Distance between the icon and the bottom edge of the control.

**PaddingLeft** – Distance between the icon and the left edge of the control.

**PaddingRight** – Distance between the icon and the right edge of the control.

## Style and theme

**IconStyle** – Whether the icon renders in outline or filled style. Accepts `IconStyle` enum values:

| Value | Description |
|-------|-------------|
| `IconStyle.Outline` | Renders the icon in outline (unfilled) style. Default. |
| `IconStyle.Filled` | Renders the icon in filled style. |

**IconColor** – The color of the icon.

**BasePaletteColor** – The base color used by the theme to generate the control's color palette. Use this to apply a different theme color to the icon.

**Fill** – The background fill color of the control.

**BorderColor** – The color of the control's border.

**BorderStyle** – The style of the control's border. Accepts `BorderStyle` enum values: `BorderStyle.Solid`, `BorderStyle.Dashed`, `BorderStyle.Dotted`, or `BorderStyle.None`.

**BorderThickness** – The thickness of the control's border in pixels.

**RadiusTopLeft** – The corner radius for the top-left corner of the control.

**RadiusTopRight** – The corner radius for the top-right corner of the control.

**RadiusBottomLeft** – The corner radius for the bottom-left corner of the control.

**RadiusBottomRight** – The corner radius for the bottom-right corner of the control.

## Additional properties

**AccessibleLabel** – Label read by screen readers. When **OnSelect** is set, always provide a meaningful description of the action (for example, `"Delete item"`). This is important for icon-only interactive controls.

**Tooltip** – Explanatory text that appears when the user hovers over the control.

**ContentLanguage** – The display language for the control content, if different from the app language.

## Example

The following YAML example shows an interactive delete icon:

```yaml
- DeleteIcon:
    Control: ModernIcon@1.1.0
    Properties:
      AccessibleLabel: ="Delete"
      Icon: ="Delete"
      IconColor: =RGBA(176, 0, 32, 1)
      IconStyle: =IconStyle.Filled
      OnSelect: =
      Tooltip: ="Delete this item"
```

## Recent updates

The updated version of the **Icon** modern control includes the following improvements and behavior changes.

### Property renames

| Previous property | New property | Notes |
|---|---|---|
| `Style` | `IconStyle` | Renamed for clarity; also changed from string to typed enum. |

### Removed properties

| Removed property | Notes |
|---|---|
| `TabIndex` | Removed. Focus management is handled automatically by the control. |

### Bug fixes and improvements

- **OnSelect support**: The icon can now trigger actions when selected. When `OnSelect` is set to an active formula, the control renders with button semantics, making it fully accessible via keyboard and screen readers.
- **AccessibleLabel**: New property for screen readers — especially important for interactive icons where the glyph alone does not convey meaning.
- **Updated enum**: `IconStyle` now uses a typed Power Fx enum (`IconStyle.Outline`, `IconStyle.Filled`) instead of a plain string, improving IntelliSense and reducing formula errors.
- **Full border and fill control**: New `Fill`, `BorderColor`, `BorderStyle`, `BorderThickness`, and four corner radius properties (`RadiusTopLeft`, `RadiusTopRight`, `RadiusBottomLeft`, `RadiusBottomRight`) provide complete styling options.
- **Padding support**: New `PaddingTop`, `PaddingBottom`, `PaddingLeft`, `PaddingRight` properties control spacing between the icon glyph and the control boundary.
- **BasePaletteColor**: Apply a custom theme color to the icon.

## See also

- [Modern controls overview](overview-modern-controls.md)
- [Size and location properties](../properties-size-location.md)
