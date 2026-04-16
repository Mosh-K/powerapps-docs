---
title: Button modern control in canvas apps - Power Apps
description: Learn about the details, properties, and examples of the Button modern control in Power Apps.
author: yogeshgupta698
ms.topic: reference
ms.custom: canvas
ms.date: 04/14/2026
ms.subservice: canvas-maker
ms.author: yogupt
ms.reviewer: mkaur
search.audienceType:
  - maker
---

# Button modern control in canvas apps

Trigger actions in your canvas app with a clickable button.

## Description

The **Button** modern control provides a clickable element that triggers app logic when selected. Use it for form submissions, navigation, confirmations, and any interaction that requires user input. The control supports icon and text combinations, full font styling, and multiple appearance styles via Fluent theming. Key properties for this control are **Text**, **Appearance**, and **OnSelect**.

> [!NOTE]
> This article describes the updated Button modern control. For information about what changed from the previous version, see [Recent updates](#recent-updates).

## General

**Text** – The label displayed on the button. Supports any text or Power Fx formula that evaluates to a string.

**Visible** – Whether the control appears or is hidden. Use a Power Fx formula to show or hide the button based on app state.

## Behavior

**OnSelect** – How the app responds when the user selects (clicks or taps) the button. The control is accessible: **OnSelect** also triggers when the user presses Enter or Space while the control has keyboard focus.

**DisplayMode** – Whether the control allows user input (**Edit**), only displays data (**View**), or is disabled (**Disabled**). When **Disabled**, the button is visually dimmed and **OnSelect** does not fire.

## Size and position

**[X](../properties-size-location.md)** – Distance between the left edge of the control and the left edge of its parent container (screen if no parent container).

**[Y](../properties-size-location.md)** – Distance between the top edge of the control and the top edge of its parent container (screen if no parent container).

**Width** – Distance between the control's left and right edges. Default is **96**.

**Height** – Distance between the control's top and bottom edges. Default is **32**.

**Align** – The horizontal alignment of the button content within the control. Accepts `Align` enum values: `Align.Left`, `Align.Center`, `Align.Right`, `Align.Justify`.

**VerticalAlign** – The vertical alignment of the button content. Accepts `VerticalAlign` enum values: `VerticalAlign.Top`, `VerticalAlign.Middle`, `VerticalAlign.Bottom`.

**PaddingTop** – Distance between the button content and the top edge of the control.

**PaddingBottom** – Distance between the button content and the bottom edge of the control.

**PaddingLeft** – Distance between the button content and the left edge of the control.

**PaddingRight** – Distance between the button content and the right edge of the control.

## Style and theme

**Appearance** – The visual style of the button. Accepts `ButtonAppearance` enum values:

| Value | Description |
|-------|-------------|
| `ButtonAppearance.Primary` | Filled button using the brand color. Default. |
| `ButtonAppearance.Secondary` | Subtle filled style, secondary emphasis. |
| `ButtonAppearance.Outline` | Outlined button with no background fill. |
| `ButtonAppearance.Subtle` | Minimal styling, no border or fill. |
| `ButtonAppearance.Transparent` | No visible styling; blends with the background. |

**BasePaletteColor** – The base color used by the theme to generate the control's color palette. Change this property to apply a different theme color to the button.

**Icon** – The Fluent icon name to display alongside the button text (for example, `"Add"`, `"Save"`, `"Delete"`). Leave blank for a text-only button.

**IconStyle** – Whether the icon renders in outline or filled style. Accepts `IconStyle` enum values: `IconStyle.Outline` (default) or `IconStyle.Filled`.

**IconRotation** – Rotation in degrees applied to the icon. Default is **0**.

**Layout** – The position of the icon relative to the button text. Accepts `ButtonLayout` enum values:

| Value | Description |
|-------|-------------|
| `ButtonLayout.IconBefore` | Icon appears to the left of the text. Default. |
| `ButtonLayout.IconAfter` | Icon appears to the right of the text. |
| `ButtonLayout.IconOnly` | Only the icon is shown; no text label. |

**Font** – The font family used for the button label.

**Size** – The font size of the button label, in points.

**Color** – The color of the button label text.

**FontWeight** – The weight (thickness) of the button label. Accepts `FontWeight` enum values: `FontWeight.Bold`, `FontWeight.Semibold`, `FontWeight.Normal` (default), `FontWeight.Lighter`.

**Italic** – Whether the button label appears in italic style.

**Underline** – Whether a line appears under the button label text.

**Strikethrough** – Whether a line appears through the middle of the button label text.

**BorderColor** – The color of the control's border.

**BorderStyle** – The style of the control's border. Accepts `BorderStyle` enum values: `BorderStyle.Solid`, `BorderStyle.Dashed`, `BorderStyle.Dotted`, or `BorderStyle.None`.

**BorderThickness** – The thickness of the control's border in pixels.

**RadiusTopLeft** – The corner radius for the top-left corner of the control.

**RadiusTopRight** – The corner radius for the top-right corner of the control.

**RadiusBottomLeft** – The corner radius for the bottom-left corner of the control.

**RadiusBottomRight** – The corner radius for the bottom-right corner of the control.

## Additional properties

**AccessibleLabel** – Label read by screen readers. Provide a meaningful description when the button label alone is not sufficient (for example, when using an icon-only button).

**Tooltip** – Explanatory text that appears when the user hovers over the button.

**ContentLanguage** – The display language for the control content, if different from the app language.

## Example

The following YAML example shows a submit button and a cancel button:

```yaml
- SubmitButton:
    Control: ModernButton@1.0.0
    Properties:
      Text: ="Submit"
      Appearance: =ButtonAppearance.Primary
      Icon: ="Checkmark"
      IconStyle: =IconStyle.Outline
      Layout: =ButtonLayout.IconBefore
      AccessibleLabel: ="Submit the form"
      Tooltip: ="Submit your response"
      Width: =120
      Height: =36

- CancelButton:
    Control: ModernButton@1.0.0
    Properties:
      Text: ="Cancel"
      Appearance: =ButtonAppearance.Outline
      Width: =120
      Height: =36
```

## Recent updates

The updated version of the **Button** modern control includes the following improvements and behavior changes.

### Property renames

The following properties are renamed. Update any formulas in your app that reference the old property names.

| Previous property | New property |
|---|---|
| `FontSize` | `Size` |
| `FontColor` | `Color` |
| `FontItalic` | `Italic` |
| `FontUnderline` | `Underline` |
| `FontStrikethrough` | `Strikethrough` |
| `BorderRadius` | `RadiusTopLeft`, `RadiusTopRight`, `RadiusBottomLeft`, `RadiusBottomRight` |

### Removed properties

| Removed property | Notes |
|---|---|
| `AcceptsFocus` | Removed. The modern Button always accepts keyboard focus. No replacement needed. |

### Bug fixes and improvements

- **Updated enums**: `Appearance`, `Layout`, and `IconStyle` now use typed Power Fx enums (`ButtonAppearance`, `ButtonLayout`, `IconStyle`) instead of string values, improving IntelliSense and reducing formula errors.
- **Tooltip support**: New `Tooltip` property shows explanatory text on hover.
- **Border improvements**: Added `BorderStyle` and `BorderThickness` for more precise border control. `BorderRadius` is replaced by four corner-specific properties for independent corner control.
- **Full font control**: Font properties are now consistent with other modern controls — use `Font`, `Size`, `Color`, `Italic`, `Underline`, and `Strikethrough`.

## See also

- [Modern controls overview](overview-modern-controls.md)
- [Size and location properties](../properties-size-location.md)
