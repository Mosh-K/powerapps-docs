---
title: Understand data cards in canvas apps
description: Learn about how to use cards to collect and display information from a data source in canvas apps.
author: gregli-msft

ms.topic: how-to
ms.custom: canvas
ms.reviewer: mkaur
ms.date: 01/13/2026
ms.subservice: canvas-maker
ms.author: gregli
search.audienceType: 
  - maker
contributors:
  - mduelae
  - gregli-msft
---
# Understand data cards in canvas apps

Data cards are the pieces inside a form that show and collect values for individual fields, such as Name, Title, or Security Code. If you're new to Power Apps forms, this article explains what a data card is, how to change the control users type into, when to unlock a card, and how the form saves changes back to Dataverse.

- Identify the parts of a data card, including the label, input, required indicator, and error message.
- Reorder fields and change a card's control type, such as from single-line to multi-line text.
- Unlock a card to customize the layout or add extra controls, like icons and labels.
- Understand how data flows into the form (defaults) and out of the form (submit).

A **Display form** shows one record, and an **Edit form** lets users update or create a record. Inside the form, each **data card** connects to **one field** (one column) in your data source, which is often a Dataverse table. The card is the "wrapper" that handles the field's label, required and validation behavior, and the input control that users interact with.

- **Required indicator (asterisk or star)**: visually shows when a value is mandatory.
- **Title**: the field label shown to the user.
- **Data card value**: the input control, such as a text box or dropdown, where the user enters or edits data.
- **Error message**: displays validation errors, typically when the form is submitted.

## Prerequisites

If you're new to forms, start by reading [add-form.md](add-form.md) and [working-with-forms.md](working-with-forms.md).

## Change a card type

You can immediately try customizing cards in any app. Power Apps offers predefined cards for strings, numbers, and other data types.

**Tip: Rearrange fields in the form**. Select the **Edit form**, then in the **Properties** pane select **Edit fields** and drag fields into the order you want. This changes the layout without unlocking any cards.

:::image type="content" source="./media/working-with-cards/selected-card.png" alt-text="Screenshot of a selected card in Power Apps.":::

**To change a card’s control type**

1. Open your app for editing in [Power Apps Studio](power-apps-studio.md).
1. In the tree view, select the **Edit form** control that contains the field you want to change.
1. In the right pane, select **Edit fields**, and then select the field you want.
1. Change the **Control type**. For example, switch from **Edit single-line text** to **Edit multi-line text**.

In the right pane, you see the available types and can change the card for a field.

:::image type="content" source="./media/working-with-cards/first-screen.png" alt-text="Screenshot of an Edit form control in an app built from a list named Assets. The form displays several fields that you can customize.":::

In this example, a single-line text card is selected, but the value is longer than what fits on one line. Change this card to a multiline text card so users have more space to edit. For example, when capturing longer job titles or descriptions.

:::image type="content" source="./media/working-with-cards/multiline-edit.png" alt-text="Screenshot of a multiline text card edit in Power Apps.":::

Several fields in this data source aren't shown, but you can show or hide a field by selecting its checkbox. This example shows how to show the **SecurityCode** field.

## Customize a card

A data card contains the controls that users see - usually a label such as the title, an input control such as a text input or dropdown, and an error message label. To customize how a field looks, select the control *inside* the card for example, the text input and adjust its size, position, or properties.

Next, you practice editing the controls inside a card without changing what field the card connects to.

1. In your form, select the data card for the **SecurityCode** field.

   :::image type="content" source="./media/working-with-cards/select-security-code.png" alt-text="Screenshot of selecting the SecurityCode data card in Power Apps.":::

1. Inside the card, select the **Text input** control.

   :::image type="content" source="./media/working-with-cards/select-text-input.png" alt-text="Screenshot of selecting the Text input control inside a data card in Power Apps.":::

1. Drag to move the text input within the card, and use the handles to resize it. This method improves spacing and readability without unlocking the card.

You can move and resize controls in a locked card, but some changes like deleting a control or adding a brand-new one require unlocking the card first.

## Unlock a card

When you add a field to a form, Power Apps creates a data card for you and sets up the basic formulas that connect the card to the data source. By default, Power Apps locks these cards so you don't accidentally break that connection. If you need more control, such as a custom layout, extra controls, or different formulas, you can unlock the card.

:::image type="content" source="./media/working-with-cards/advanced-locked.png" alt-text="Screenshot of the Advanced tab showing a locked data card in Power Apps.":::

The key setting is **DataField**. It tells Power Apps which field (column) this card is responsible for. When the form submits, Power Apps uses the card's **DataField** value to know what field to update.

To unlock a card, select the card, go to the **Advanced** tab in the right pane, and then select the lock banner or the lock icon next to properties such as **DataField**, **DisplayName**, or **Required**. After you unlock the card, you can edit the generated formulas and add or remove controls inside the card.

:::image type="content" source="./media/working-with-cards/lock-icons.png" alt-text="Screenshot of lock icons in the Power Apps card properties pane.":::

Select the banner at the top to unlock the card so that you can modify these properties:

:::image type="content" source="./media/working-with-cards/unlocked-card.png" alt-text="Screenshot of an unlocked data card in Power Apps.":::

For example, after unlocking, you can change **DisplayName** so the label reads **Asset ID** instead of **AssetID**. This small change shows that the card is now under your control and not fully managed by the form.

:::image type="content" source="./media/working-with-cards/change-display-name.png" alt-text="Screenshot of changing the DisplayName property in a data card in Power Apps.":::

After you unlock and start editing the generated formulas and controls, the card becomes a **custom card**. That means you gain flexibility, but you might lose some "one-click" features, such as switching the card back and forth between predefined card types such as single-line vs. multi-line in the right pane.

> [!IMPORTANT]
> You can't relock a card after you unlock it. To get a card back to a locked state, remove it, and reinsert it in the right-hand pane.

Example: Add a custom required indicator. After unlocking, you can insert extra controls into the card. For instance, go to **Insert** > **Icons** and add a star shape, then position it where you want.

:::image type="content" source="./media/working-with-cards/add-star.png" alt-text="Screenshot of adding a star icon to a data card in Power Apps.":::

After you unlock a card, you can insert additional controls directly inside it, such as icons, labels, or other UI elements. These custom elements become part of the card, so if you later reorder fields or move the card, the added controls move with it.

The star is now a part of the card and travels with it if, for example, you reorder the cards within the form.

Another example: show an image preview. Unlock the **ImageURL** card, and then insert an **Image** control into the card from the **Insert** tab.

:::image type="content" source="./media/working-with-cards/add-image.png" alt-text="Screenshot of adding an image control to a data card in Power Apps.":::

Then set the Image control's **Image** property to *TextBox*.**Text** in the formula bar, where *TextBox* is the name of the text input that contains the URL. This setting makes the Image control display whatever URL the user types.

:::image type="content" source="./media/working-with-cards/show-image.png" alt-text="Screenshot of a data card showing an image preview in Power Apps.":::

Now the user can edit the URL and immediately see the image update. If you used **Parent.Default** for the Image property, it would show the starting value but might not refresh as the user types a new URL.

You can use the same idea in a **Display form** (read-only) screen. Because users aren't editing the URL there, you might hide the label by setting the label control's **Visible** property to **false** which hides the label but not the whole card.

:::image type="content" source="./media/working-with-cards/show-image-display.png" alt-text="Screenshot of a display form showing an image in Power Apps.":::

## Interact with a form

After you unlock a card, you can control how values move between the form and the controls inside the card.

A helpful way to think about an Edit form is: **data flows in** to show the current record (defaults), and **data flows out** when you submit the form updates. The card sits in the middle - its properties tell the form which field it represents and what value should be saved.

### DataField property

The card's **DataField** property is the "this card edits this field" setting. It helps Power Apps decide what to validate, what value is required, and what field should be updated when you submit the form.

### Information flowing in

When a form shows a record, it exposes that record as **ThisItem**. Think of **ThisItem** as the current row you're editing. Tt contains every field for that record.

In Dataverse-backed forms, the card's **DataField** typically matches the field's logical name. The card's **Default** formula commonly references the current record for example, **ThisItem.FieldName**, and the input control inside the card usually reads that value through **Parent.Default**. This pattern keeps the input control independent of the data source and lets the card encapsulate how the field value flows in.

Most cards set their **Default** property to the current record's value, like **ThisItem.FieldName**. You can optionally transform that value for example, format text before it shows in the input.

Inside the card, the input control usually uses **Parent.Default** so it always shows whatever value the card provides. This approach keeps the card "self-contained". If you later change the card's default formula, you don't have to rewrite formulas inside the card.

By default, the data source's metadata sets the **DefaultValue** and **controls/control-card.md** properties based on the **controls/control-card.md** property. You can override these formulas with your own logic by using the **functions/function-datasourceinfo.md** function to integrate the data source's metadata.

### Information flowing out

When the user selects **Save**, you typically call **SubmitForm**. The form then gathers values from each data card and writes them back to the data source. It uses each card’s **DataField** to know *which* field to update.

To save changes from an **Edit form**, trigger **SubmitForm(FormName)**. For example, from a **Save** button). If you want to clear the inputs after a successful submit, follow with **ResetForm(FormName)**. To create a new record instead of editing an existing one, set the form’s **DefaultMode** property to **FormMode.New** before the user starts entering data.

The form also reads each card’s **Update** property - this is the value that gets saved for that field. If you need to clean up a value before saving. For example, remove extra spaces, convert text to a number, or reverse a formatting change you made in **Default**. **Update** is usually the right place to do it.

**Valid** is basically "is this field okay to submit?" Power Apps uses the data source rules and the card’s **Required** setting to decide this. If the value isn't valid, the card's **Error** property contains a message that you can show to the user often through the **Error Message** label in the card.

If a card’s **DataField** is blank, the card isn't tied to any field - it's just a container you can use for layout. In that case, its **Update** and **Valid** values don't affect what gets saved when you submit the form.

## Dissecting an example

Take a closer look at what's inside a typical data-entry card. The following screenshots spread the controls out so you can see each piece clearly.

:::image type="content" source="./media/working-with-cards/dissect-card1.png" alt-text="Screenshot of a data-entry card in Power Apps with controls spread out for clarity.":::

In the next image, the controls inside the card are labeled so you can match what you see on the canvas to what’s in the tree view.

:::image type="content" source="./media/working-with-cards/dissect-card2.png" alt-text="Screenshot of labeled controls inside a data card in Power Apps.":::

Here are the main controls you usually see inside a card:

| Name | Type | What it does |
|---|---|---|
| **TextRequiredStar** | **Label** control | Shows a star or asterisk when the field is required. |
| **TextFieldDisplayName** | **Label** control | Shows the friendly field name the user sees. This is often different from the internal schema name. |
| **InputText** | Text input control | Shows the current value and lets the user type a new one. |
| **TextErrorMessage** | **Label** control | Shows a message if the value can’t be submitted such as missing required data). |

These controls usually don't connect to Dataverse directly. Instead, they read simple values from the **parent card** using **Parent**, and the card handles the connection to the data source. The following formulas are common examples.

| Control property | Formula | Beginner explanation |
|---|---|---|
| **TextRequiredStar.Visible** | **Parent.Required** | Only show the star when the field is required. |





[!INCLUDE[footer-include](../../includes/footer-banner.md)]
