---
lab:
    title: 'Build a canvas app for Contoso field technicians'
    description: 'Create and customize a canvas app in Microsoft Power Apps that field technicians use to view and update their assigned Work Orders'
    duration: '60 minutes'
    level: 300
    islab: true
---

# Build a canvas app for Contoso field technicians

In this exercise, you build a canvas app that Contoso field technicians use on their mobile devices to view their assigned Work Orders and update job status while in the field.

This exercise should take approximately **60** minutes to complete.

## Scenario

Contoso field technicians spend their days traveling between customer sites. They need a simple, mobile-friendly app to see which jobs they've been assigned, view the details of each request, and update the status when work is complete — all from their phone.

You'll build this app connected to the Work Order table you created in Lab 3, with a list screen showing assigned requests and a detail screen where technicians can update status.

## Task 1: Create the canvas app

1. Open [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com` and sign in with your Microsoft account.

1. Confirm you are in your **Dev One** environment.

1. In the left navigation, select **+ Create**.

1. Select **Create from blank** and select **Phone size**.

1. Skip any welcome messages that appear.

1. Select the **gear** icon in the bottom left corner. Configure the **App name** as `Contoso Technician App`, then select **Close**.

## Task 2: Connect to the Work Order table

1. In the left panel of the canvas studio, select the **Data** icon (it looks like a grid).

1. Select **+ Add data**.

1. In the search box, type `Work Order` and select **Work Orders** (`contoso_workorder`).

1. Confirm that **Work Orders** now appears in the data sources list.

## Task 3: Build the request list screen

Now you'll set up the main screen to display a gallery of Work Orders.

1. Select the **tree view** icon from the left menu (it will look like boxes stacked on top of each other).

1. Rename the default screen from **Screen1** to `ListScreen` by double-clicking its name in the tree view panel.

1. On **ListScreen**, select **+ Insert** from the top menu, then select **Vertical gallery**.

1. When prompted to connect a data source, select **Work Orders**.

1. Resize and reposition the gallery to fill the screen, leaving space for a title bar at the top.

   > [!NOTE]
   > Once the gallery is connected to Work Orders, you should see the three sample records you created in Lab 3 (Adatum Corporation, Tailwind Traders, Fabrikam Inc) appear in the gallery. If the gallery is empty, confirm that you selected the correct Work Orders table with the **contoso_** prefix.

1. With the gallery selected, a small popup appears pinned just above the gallery. Select **Layout** in that popup and choose the layout that shows **Title, subtitle, and body**.

1. In the properties panel, select the **Fields** count (for example, 7 selected) to open the field list. The fields are shown using their schema names. Select the schema name next to each field to open the dropdown and choose the correct field:
    - **Body**: select `contoso_status`
    - **Subtitle**: select `contoso_issuedescription`
    - **Title**: select `customername`

1. Select **+ Insert** > **Text label** to add a title bar at the top of the screen.

1. Set the label **Text** property to `"My Work Orders"` in the formula bar.

   > [!NOTE]
   > The double quotes are required. Power Apps treats everything in the formula bar as an expression, not plain text. Without quotes, it tries to interpret `My Work Orders` as a formula reference and throws an error. Wrapping the value in double quotes tells Power Apps to treat it as a literal text string.

1. Format the label: set the **Background color** to a dark blue, the **Color** (text) to white, and increase the **FontSize** to `36`.

1. The text box defaults to a small size. Now that the background color is filled in, drag its edges so it stretches the full width of the screen and is tall enough to display the text clearly.

## Task 4: Set up the detail screen and form

Now you'll add a second screen where technicians can view full details and update the status of a selected request.

1. In the left panel, select the **Tree view** icon. Select **+ New screen** > **Blank**.

1. Rename the new screen to `DetailScreen` by double-clicking its name in the screens panel.

1. On **DetailScreen**, select **+ Insert** > **Edit form**.

1. When prompted to connect a data source, select **Work Orders**.

1. By default, the form is positioned at the very top of the screen, which leaves no room for a Back button. In the **Tree view**, select **Form1**, then drag it down to leave space at the top of the screen for a button.

1. A popup appears directly over the form. Select **Fields** — this surfaces a dropdown with AI-suggested fields based on your data source. Remove the ones you don't need and add the missing one:
    - **Remove**: `Customer Email`, `Status Reason`, and `Record created on` (select the ellipsis next to each and select **Remove**)
    - **Add**: select **+ Add field**, search for `Assigned Technician`, and select **Add**

    When done, the form should show: Customer Name, Issue Description, Priority, Request Status, Assigned Technician, and Resolved Date.

1. Assigned Technician will appear at the bottom of the field list. Drag it above **Resolved Date** so the fields are in this order: Customer Name, Issue Description, Priority, Request Status, Assigned Technician, Resolved Date.

## Task 5: Add navigation buttons to the detail screen

Now you'll add Back and Save buttons so technicians can navigate and submit updates.

1. Add a **Back** button at the top of the screen by selecting **+ Insert** > **Button**.

1. When the button is inserted, notice the formula bar at the top of the screen. The property dropdown on the left side already shows **OnSelect** by default.

1. Before using Copilot, clear any existing content from the formula bar — if there's already a formula there, Copilot will append to it rather than replace it.

1. Select the **Copilot** icon in the formula bar and select **Create a formula (preview)**. This button will navigate back to the previous screen, so type `go back to the previous screen` and press **Enter**. Copilot suggests the formula `Back()`. Select **Apply** to apply it.

   > [!NOTE]
   > If Copilot isn't available in your environment, or gives you a response you didn't expect, you can type `Back()` directly in the formula bar.

1. Now set the button label: select the property dropdown and choose **Text**. Type `"Back"` in the formula bar.

1. Add a **Save** button below the form by selecting **+ Insert** > **Button**.

1. With the Save button selected, confirm the property dropdown shows **OnSelect**, then type `SubmitForm(Form1)` in the formula bar (replace `Form1` with the actual name of your edit form if different).

1. Select the **Copilot** icon in the formula bar and select **Explain this formula**. Read the explanation Copilot provides for the `SubmitForm()` function.

1. Switch the property dropdown to **Text** and type `"Save"`.

## Task 6: Connect the screens with navigation

Now you'll connect the gallery on the first screen to the detail screen.

1. Return to **ListScreen** in the screens panel.

1. In the **Tree view**, expand **Gallery1** (or the name of your gallery) to see its child elements. Select **NextArrow** from the list.

1. With **NextArrow** selected, make it easier to see by updating its colors in the **Properties** panel on the right:
    - Set **Color** to a dark blue or your preferred accent color
    - Set **Disabled color** to a lighter shade of the same color
    - Set **Hover color** to a brighter or bolder shade so it's clearly visible when technicians tap it

1. Now set the **OnSelect** property of **NextArrow**. When a technician taps the arrow on a gallery item, you want the app to navigate to the detail screen for that request with a slide transition. Confirm the property dropdown in the formula bar shows **OnSelect**.

1. Clear any existing content from the formula bar — Copilot appends to whatever is already there.

1. Select the **Copilot** icon and choose **Create a formula (preview)**. Describe what you want — for example, type `navigate to the detail screen with a slide transition` and press **Enter**.

1. Review the formula Copilot suggests. It should be:

    `Navigate(DetailScreen, ScreenTransition.Cover)`

    If Copilot's suggestion matches, select **Apply**. If it doesn't match exactly, select **Discard**, then type the formula above directly in the formula bar.

1. With the navigation working, you need to pass the selected item to the detail form. Select the **Form** on **DetailScreen**.

1. The **Properties** pane will appear on the right. Select **Advanced.**

1. Set the **Item** property to:

    `Gallery1.Selected`

    (Replace `Gallery1` with the actual name of your gallery if different.)

## Task 7: Create the AI Builder flow in Power Automate

Now you'll build a Power Automate flow that uses an AI Builder prompt to analyze an issue description and suggest a priority level.

1. On **DetailScreen**, select **+ Insert** > **Button** and place it below the form, above the **Save** button.

1. With the button selected, select the **ellipsis (...)** at the bottom of the left panel to reveal more options, then select **Power Automate**.

1. Select **Create new flow**, then select **+ Create from blank**. A Power Automate flow designer opens in a panel within the canvas app studio, with a pre-configured Power Apps trigger already in place.

1. Select the default flow name at the top of the panel and rename it to `SuggestPriorityFlow`.

1. Select the trigger step to expand it and select **+ Add an input.**

1. Add a text input named `IssueDescription`.

1. Add a new step: search for **AI Builder** and select **Run a prompt**.

   > [!NOTE]
   > You may be prompted to authenticate or sign in to AI Builder at this point. Follow the on-screen steps to connect, then continue.

1. Configure the **Run a prompt** step with the following values:
    - **Prompt**: select **AI Classify** from the dropdown
    - **Input text**: select the **Input** from the PowerApps connector in the dynamic value selector (this is the IssueDescription we set up in a previous step)
    - **Input Categories**: `Low, Normal, High, Critical`
    - **AdditionalContext**: `You are classifying the urgency of a field Work Order. Choose the priority level that best matches the issue described.`

1. Select **+ New step**.

1. Add a **Respond to a Power App or flow** step. Select **+ Add an output**, choose **Text**, name it `SuggestedPriority`, and set its value to the **Text** output from the **Run a prompt** step.

1. **Save** the flow and return to the canvas app.

## Task 8: Add the Suggest Priority feature to the app

Now you'll wire the flow to the canvas app and display the AI suggestion on screen.

1. Back in the canvas app, select the button you added in the previous task.

1. Set the button **Text** property to `"Suggest Priority"`.

1. Set the button **OnSelect** property to:

    `Set(varSuggestedPriority, SuggestPriorityFlow.Run(Gallery1.Selected.contoso_issuedescription).SuggestedPriority)`

1. Select the **Copilot** button and select **Explain this formula.** You should see that Copilot is aware that the expression will set the Suggested Priority value returned by the flow.

1. Add a **Text label** below the button. In the **Properties** panel, set the **Color** to blue so the AI suggestion stands out on screen. Expand the text label so it spans the whole length of the screen.

1. Set its **Text** property to:

    `If(varSuggestedPriority = "", "", "AI suggestion: " & varSuggestedPriority)`

1. Select the **Copilot** icon in the formula bar and select **Explain this formula**. Read the explanation Copilot provides for the `If()` function and consider how it controls what the label displays before and after the flow runs.

## Task 9: Preview and test the app

1. **Save** your app.

1. Press **F5** (or select the **Play** button in the top-right corner) to preview the app.

1. You should see the request list screen with any existing Work Order records displayed.

1. Select a record in the gallery to navigate to the detail screen.

1. On the detail screen, select **Suggest Priority**. After a few seconds, the label should display a suggested priority based on the issue description.

   > [!NOTE]
   > AI Builder prompt actions require an AI Builder capacity allocation in your environment. If the flow fails with a licensing error, check with your administrator or use a trial capacity add-on. Results may vary — the AI suggestion is a starting point, not a definitive answer.

1. Change the **Request Status** value and select **Save**.

1. Select **Back** to return to the list and confirm your change was saved.

1. Press **Esc** or select the **X** to exit preview mode.

## Task 10: Save and publish the app

1. Select the **Save** icon on the top-right corner (or press **Ctrl+S**).

1. Select **Publish** and then **Publish this version** to make the app available to users.

   > [!NOTE]
   > Sharing the app with specific users and assigning security roles is covered in a separate module. For now, the app is published and accessible in your environment.
