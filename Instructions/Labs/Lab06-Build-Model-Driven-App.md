---
lab:
    title: 'Build a model-driven app for Contoso service managers'
    description: 'Create a model-driven app in Microsoft Power Apps that service managers use to track, prioritize, and manage all open Work Orders'
    duration: '45 minutes'
    level: 300
    islab: true
---

# Build a model-driven app for Contoso service managers

In this exercise, you build a model-driven app that Contoso service managers use to view and manage all open Work Orders, configure views for different scenarios, and work with the data model you created in Lab 3.

This exercise should take approximately **45** minutes to complete.

## Scenario

Contoso service managers need a full-featured desktop interface to manage the incoming queue of Work Orders. Unlike field technicians (who only need to see their own assigned jobs), managers need to see all requests across all customers, filter by priority, reassign technicians, and track resolution progress.

A model-driven app is the right tool for this — it's built directly on your Dataverse data model, and it automatically inherits the forms and views you've already configured.

## Task 1: Create the model-driven app

1. Open [Power Apps](https://make.powerapps.com){:target="_blank"} at `https://make.powerapps.com` and sign in with your Microsoft account.

1. Confirm you are in your **Dev One** environment.

1. In the left navigation, select **Solutions**.

1. Open the **Contoso Field Services** solution.

1. In the left menu, select **Objects.**

1. On the command bar, select **+ New** > **App** > **Model-driven app**.

1. In the **New model-driven app** dialog, configure the app:
    - **Name**: `Contoso Service Management`
    - **Description**: `Internal app for service managers to track and manage all Work Orders`

1. Select **Create**. The model-driven app opens in the modern app designer.

## Task 2: Add the Work Order table to the app

1. In the app designer, select **+ Add page**.

1. Select **Dataverse table**.

1. Search for and select **Work Order** (`contoso_workorder`), then select **Add**.

1. Confirm that **Work Orders** now appears as a page in the left navigation of the app designer. You should also see the three sample Work Orders you created in Lab 3 (Adatum Corporation, Tailwind Traders, Fabrikam Inc) displayed in the view in the center of the app designer.

1. In the left **Pages** pane, select **New Group** — the group that Work Orders is nested under. In the right properties pane, change the **Title** to `Service Management`, then select **Save**.

## Task 3: Configure views for the app

Views control which records are shown and which columns are displayed in the list. By default, all views for the table are included in the app — you'll trim this down to the two most useful ones for service managers.

1. In the left **Pages** pane of the app designer, select **Work Orders view** (not the form).

1. On the right properties pane (if you don't see it, you might need to expand it), select the **Views** tab. You'll see views listed under **In this app**.

1. Under **In this app**, you'll see two default views: **Active Work Orders** and **Inactive Work Orders**. Select **...** next to **Inactive Work Orders** and select **Remove** — service managers only need to see active records. You should see **Inactive Work Orders** under **Not in this app** after the change.

1. Select **Save**.

## Task 4: Configure the Work Order form

Forms control what fields are shown when a manager opens a specific record.

1. Under **Work Orders** in the app designer, select **Work Orders form**.

1. Select the pencil icon next to the form name to open the form designer.

1. In the **Table columns** pane, unselect **Show only unused table columns.**

1. Review the form layout. Confirm the following fields are visible on the form. If they are not, drag fields from the right panel and around in the form so they appear in the following order:
    - Customer Name
    - Customer Email
    - Issue Description
    - Priority
    - Request Status
    - Assigned Technician
    - Resolved Date
    - Owner

1. Add a **section** to organize the form more clearly:
    - Select **Component**.
    - Select **1-column section**. The section will be added below the Owner field.
    - Label the new section `Resolution Details` from the **Properties** pane on the right.
    - Drag the **Resolved Date** field into this section.

1. Select **Save and publish** to publish the form changes.

1. Close the form designer tab and return to the app designer.

## Task 5: Enable Copilot and AI assistance

> **⚠️ NEEDS RETESTING**: This task needs to be verified in a clean training environment before the course runs. In testing, Copilot chat appeared to be available in the published app without explicitly enabling it — which may mean it's on by default. If so, this task should be rewritten to focus on **using** Copilot chat rather than configuring it. Skip or present this task as a demo for now.

Power Apps model-driven apps include built-in AI features you can enable with just a few settings, no custom development required.

1. In the app designer for **Contoso Service Management**, select **Settings** (the gear icon in the top-right corner of the designer).

1. In the **Settings** panel, select the **Features** section.

1. Find **Copilot** and select **On** from the dropdown menu. This adds a Copilot chat panel to the app that lets users ask natural language questions about their data, such as filtering records or summarizing trends, without writing any queries.

    > **Note**: There are two Copilot features available. **Copilot** (Copilot chat) works with a standard Power Apps premium license and is what you're enabling here. **Enable M365 Copilot in model-driven apps** is a separate, more powerful option that requires both a Power Apps premium license and a Microsoft 365 Copilot license — your training environment likely won't have this.

    > **Important**: The app-level setting enables the feature for your app, but Copilot chat also requires an **environment-level** setting to be turned on by an administrator. If the Copilot icon doesn't appear in the running app, the environment setting may not be enabled. Ask your instructor — they can enable it in the Power Platform admin center under **Environments** > **Settings** > **Features** > **Allow users to analyze data using an AI-powered chat experience in canvas and model-driven apps**.

1. Find **Form fill assist toolbar** and select **On** from the dropdown menu. This AI feature suggests field values as managers fill in records, helping them complete forms faster and more consistently.

1. Select **Save**, then select **Publish**. (Wait for your publish to complete.)

1. Select **Play** to open the published app in a new tab. Look for the **Copilot** icon (sparkle icon) in the top-right corner of the app.

1. Select the Copilot icon to open the panel. Try asking:
    - `Show me all Critical Work Orders`
    - `How many requests are currently In Progress?`
    - `Which requests are assigned to no technician?`

    > **Note**: The Copilot panel is powered by the data in your Dataverse tables and the views configured in your app. It works best when your views and column names are clear and descriptive. Results depend on the data in your environment.

## Task 6: Create a custom view for high-priority requests

Views let you define exactly which records appear and which columns are shown when a manager opens the app. You'll create a filtered view that surfaces only the most urgent Work Orders, giving managers a focused queue without having to manually filter every time.

1. Open a new browser tab and navigate to [Power Apps](https://make.powerapps.com){:target="_blank"} at `https://make.powerapps.com`. Keep the app designer tab open so you can return to it later.

1. In the left navigation, select **Solutions** and open the **Contoso Field Services** solution.

1. Select **Objects**, expand **Tables**, and expand the **Work Order** table.

1. Select **Views**, then select **+ New view**. Configure the new view:
    - **Name**: `High Priority Work Orders`
    - **Description**: `Shows only High and Critical priority Work Orders for manager triage`

    Select **Create**. The view designer opens.

1. Configure the columns for this view. Select **+ View column** to add the following columns, removing any others that aren't needed:
    - **Customer Name**
    - **Issue Description**
    - **Priority**
    - **Request Status**
    - **Assigned Technician**

1. Before you can filter by Priority, you need to make it available as a filter option. By default, columns are not always enabled for Advanced Find, which is the underlying search and filter engine used by views. Select the dropdown arrow next to the **Priority** column header in the view designer and select **Edit column**.

1. In the column properties panel, check the box next to **Enable for Advanced Find**, then select **Save**. Priority will now appear as an option when configuring view filters.

1. Select **Edit filters** in the right pane to add a filter condition.

1. Select **+ Add row**. In the column picker, type `Priority` to search for it and select it. Set the operator to **Equals**. In the value picker, select both **High** and **Critical**.

1. Select **Ok**.

1. You should see the High and Critical Work Orders from your sample data populated in the view.

1. Select **Save and publish**.

1. Return to the tab with your model-driven app designer. In the left **Pages** pane, select **Work Orders view**.

1. In the right **Views** tab, look for **High Priority Work Orders** under **In this app**. If it doesn't appear there, find it under **Not in this app**, select **...** next to it, and select **Add**.

1. Select **Save**.

## Task 7: Create a chart for the Work Order table

Charts give managers a visual snapshot of data without any code. You'll create a pie chart that shows how Work Orders are distributed across priority levels — a quick health check at a glance. Charts are defined at the table level and can then be embedded in dashboards.

1. Return to the **Contoso Field Services** solution tab. Select **Objects**, expand **Tables**, and expand the **Work Order** table.

1. Select **Charts**, then select **+ New chart**. The chart designer opens.

    > **Note**: The chart designer opens in the classic Unified Interface layout, which looks different from the modern app designer you've been using. This is expected.

1. Name the chart `Requests by Priority`.

1. Select **Pie chart** as the chart type.

1. Configure the chart data:
    - **Legend Entries (Series)**: Set to **Work Orders** and **Count:All**. This counts the number of records in each group.
    - **Horizontal (Category) Axis Labels**: Set to **Priority.** This groups the records by their Priority value.

1. In the **Description** field, enter `Pie chart showing the distribution of Work Orders by priority level`.

1. Select **Save and close**.

1. Back in the charts page of the Solution, select **Done.**

## Task 8: Add a dashboard page

Dashboards let managers see their most important data at a glance in a single page. You'll build a dashboard that combines the **High Priority Work Orders** view and the **Requests by Priority** chart side by side, then add it to the app as a page.

1. Return to the **Contoso Field Services** solution page.

1. Select **+ New** > **Dashboard** from the command bar. A layout picker appears.

    > **Note**: The dashboard designer opens in the classic Unified Interface layout, which looks different from the modern app designer you've been using. This is expected.

1. Select **2-Column Regular Dashboard** and select **Create**. This is a standard dashboard layout that supports List and Chart components. Avoid the "Overview" or stream-based layouts — those only support Charts and Streams, not Lists.

1. Name the dashboard `Service Manager Dashboard`.

1. In the left column component, select the **List** icon (the grid icon in the center of the panel). Configure it:
    - **Record type**: Work Orders (select the one with the **contoso_** prefix)
    - **Default view**: High Priority Work Orders

1. In the right column component, select the **Chart** icon. Configure it:
    - **Record type**: Work Orders (with the **contoso_** prefix)
    - **Default view**: Active Work Orders
    - **Chart name**: Requests by Priority

1. Select **Save**, then select **Close** to return to the solution.

1. On the solution command bar, select **Publish all customizations** and wait for it to complete. This ensures the dashboard is available to add to the app — customizations created in the classic designer aren't visible in the modern app designer until published.

1. Return to the app designer tab. Select **+ Add page**.

1. Select **Dashboard**, then select **Next**.

1. Find and select **Service Manager Dashboard**, then select **Add**.

    > **Note**: If **Service Manager Dashboard** doesn't appear in the list, close the app designer tab and reopen the app from the solution. The modern app designer sometimes caches the available components and needs a full reload to pick up dashboards published from the classic designer.

1. In the **Pages** pane, select the ellipses on **Service Manager Dashboard**. Then select **Move up.** The dashboard will now be the first item users see when they open the app.

1. Select **Save**.

## Task 9: Test the app

1. Select **Publish** to ensure all changes are published.

1. Select **Play** to open the app in a new tab.

1. Navigate to **Work Orders** in the left navigation and confirm the **High Priority Work Orders** view is available in the view selector.

1. Select **+ New** to create a test Work Order:
    - **Customer Name**: `Alpine Equipment Co.`
    - **Issue Description**: `Office printer on second floor making occasional noise`
    - **Priority**: `Low`
    - **Request Status**: `New`

1. Select **Save & Close** and confirm the new record appears in the view.

1. Select **Service Manager Dashboard.** Your pie chart should have updated to show a Low priority work order.
