# Forms Tutorial

Forms are commonly used to provide user interaction on websites and in web applications. For example, login, registering, commenting, and purchasing. This tutorial shows you how to create accessible forms. The same concepts apply to all forms, whether they are processed client or server-side.

Aside from technical considerations, users usually prefer simple and short forms. Only ask users to enter what is required to complete the transaction or process; if irrelevant or excessive data is requested, users are more likely to abandon the form.

* **Labeling Controls:** Use the `<label>` element, and, in specific cases, other mechanisms (e.g. WAI-ARIA, `title` attribute etc.), to identify each form control.
* **Grouping Controls:** Use the `<fieldset>` and `<legend>` elements to group and associate related form controls.
* **Form Instructions:** Provide instructions to help users understand how to complete the form and individual form controls.
* **Validating Input:** Validate input provided by the user and provide options to undo changes and confirm data entry.
* **User Notifications:** Notify users about successful task completion, any errors, and provide instructions to help them correct mistakes.
* **Multi-Page Forms:** Divide long forms into multiple smaller forms that constitute a series of logical steps or stages and inform users about their progress.
* **Custom Controls:** Use stylized form elements and other progressive enhancement techniques to provide custom controls.

### A note on time limits

If possible, forms should not be subject to a time limit to allow users to complete the form at their pace. If a time limit needs to be in place, for example, for security reasons, the user should have the option to turn it off or extend it. This restriction does not apply if the time limit is due to a live event, such as an auction or a game, or if the time to complete the form is essential for a valid submission.

### Why is this important?

Forms can be visually and cognitively complex and challenging to use. Accessible forms are easier to use for everyone, including people with disabilities.

* **People with cognitive disabilities** can better understand the form and how to complete it, as making forms accessible improves the layout structure, instructions, and feedback.
* **People using speech input** can use the labels via voice commands to activate controls and move the focus to the fields that they have to complete.
* **People with limited dexterity** benefit from large clickable areas that include the labels, especially for smaller controls, such as radio buttons and checkboxes.
* **People using screen readers** can identify and understand form controls more easily because they are associated with labels, field sets, and other structural elements.
