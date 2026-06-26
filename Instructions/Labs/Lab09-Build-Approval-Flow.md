---
lab:
    title: 'Build a Critical request approval flow for Contoso'
    description: 'Create an approval flow in Microsoft Power Automate that routes Critical priority Work Orders to a service manager before a technician is assigned'
    duration: '30 minutes'
    level: 300
    islab: true
---

# Build a Critical request approval flow for Contoso

In this exercise, you build an approval flow that automatically routes Critical priority Work Orders to a Contoso service manager for review before any technician is assigned.

This exercise should take approximately **30** minutes to complete.

## Scenario

Contoso's process requires manager approval before a technician can be assigned to any Critical priority Work Order � because these incidents affect production systems and carry higher liability. Currently, managers have to monitor the Work Order queue manually and catch Critical requests on their own. There's no automated way to notify them or capture their decision.

You'll build an approval flow that triggers when a Work Order's priority is set to Critical, sends an approval request to the service manager, and updates the request status based on their response.

## Task 1: Create the approval flow

1. Open [**Power Automate**](https://make.powerautomate.com) at `https://make.powerautomate.com` and sign in with your Microsoft account.

1. Confirm you are in your training environment.

1. In the left navigation, select **+ Create** > **Automated cloud flow**.

1. Configure the flow:
    - **Flow name**: `Critical Request Approval`
    - **Trigger**: Search for `When a row is added` and select **When a row is added, modified or deleted** (Microsoft Dataverse)

1. Select **Create**.

## Task 2: Look up the Priority choice value in Dataverse

Before building the condition, you need to find the integer value that Dataverse stores for **Critical** priority. Power Automate receives this integer when the trigger fires, not the display label.

1. Open a new browser tab and go to [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com`. Ensure you are in your **Dev One** environment.

1. In the left navigation, select **Tables** and open the **Work Order** table.

1. Under **Schema**, select **Columns** and open the **Priority** column.

1. In the **Choices** section, find the **Critical** option and note the **Value** shown next to it. Write this number down � you'll use it in the next task.

> [!NOTE]
> Every choice option in Dataverse has a hidden integer value behind its display label. This number is what gets stored in the database and passed to flows, reports, and integrations. Understanding this helps you debug conditions and filters that compare against choice fields.

1. Close this tab and return to Power Automate.

## Task 3: Configure the trigger and add a filter

1. Configure the Dataverse trigger:
    - **Change type**: Added or Modified
    - **Table name**: Work Orders
    - **Scope**: Organization

1. Select **+** underneath the trigger step, search for `Condition`, and select **Condition** (under Control). Configure the condition:
    - **Value** (left): Type `/` and select **Insert dynamic content**. Search for and select **Priority** from the trigger
    - **Operator**: is equal to
    - **Value** (right): The integer value you noted in Task 2 (the number may have appeared with commas in the text box, but don't include commas)

> [!NOTE]
> Dataverse stores choice column selections as integers, not text labels. Power Automate receives the integer value when the trigger fires, so your condition must match it exactly.

## Task 4: Add the approval action

1. In the **True** branch of the condition, select the plus sign to add an action.

1. Search for and select **Start and wait for an approval** (Approvals connector).

1. When prompted to create a connection, select **Create New** and follow the prompts to sign in and authorize the Approvals connector.

1. Select **Approve/Reject - First to respond** as the approval type.

1. Configure the approval:
    - **Title**: Type `Critical Work Order Requires Approval: ` then select **Customer Name** from dynamic content
    - **Assigned to**: Type your MOD Administrator email address provided by your Authorized Lab Host
    - **Details**: Build a message using dynamic content:

        ```
        A new Critical priority Work Order has been submitted and requires your approval before a technician can be assigned.

        Customer: [Customer Name]
        Issue: [Issue Description]

        Please approve to proceed with technician assignment, or reject to return the request to High priority.
        ```

    - **Item link customer**: Replace `[Customer Name]` with the **Customer Name** dynamic content.
    - **Item link issue**: Replace `[Issue Description]` with the **Issue Description** dynamic content.

1. Select **Save**.

## Task 5: Handle the approval outcome

Now you'll add actions for both the approved and rejected paths.

1. Below the **Start and wait for an approval** step, add a new **Condition**:
    - **Value** (left): Dynamic content **Outcome** from the **Start and wait for an approval** step
    - **Operator**: is equal to
    - **Value** (right): `Approve`

### If approved

1. In the **True** branch, select the plus sign and add an action: **Update a row** (Microsoft Dataverse).

1. Configure the update a row action:
    - **Table name**: Work Orders
    - **Row ID**: Type `/` and select **Insert dynamic content**. Search for and select **Work Order** with the description **Unique Identifier for entity instances**.
    - **Request Status**: `Assigned`

> [!NOTE] 
> This signals that the request has been approved and is ready for technician assignment. A more complete solution would also include a step to notify the manager's team that assignment can proceed.

### If rejected

1. In the **False** branch, select the plus sign and add an action: **Update a row** (Microsoft Dataverse).

1. Configure the update:
    - **Table name**: Work Orders
    - **Row ID**: Type `/` and select **Insert dynamic content**. Search for and select **Work Order** with the description **Unique Identifier for entity instances**.
    - **Priority**: `High`
    - **Request Status**: `New`

1. Add a second action in the **False** branch: **Send an email (V2)** (Office 365 Outlook):
    - **To**: Your email address (representing notification back to the submitting agent)
    - **Subject**: `Critical Request Rejected: ` + **Customer Name** dynamic content
    - **Body**: `The Critical priority request for [Customer Name] was not approved. The priority has been reset to High and the request is back in the New status for review.`

    Replace `[Customer Name]` with the **Customer Name** dynamic content.

1. Select **Save**.

## Task 6: Test the approval flow

1. Select **Test** > **Manually** > **Test**.

1. Open a new browser tab and go to [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com`. Ensure you are in your **Dev One** environment.

1. Open the **Contoso Service Management** model-driven app. Select **Work orders** to navigate to the Work Orders page and select **+ New** to create a new Work Order using the form:
    - **Customer Name**: `Northwind Traders`
    - **Issue Description**: `Complete electrical failure across production floor`
    - **Priority**: `Critical`
    - **Request Status**: `New`

1. Select **Save** to save the record.

1. Return to Power Automate and monitor the test run. The flow should trigger and reach the **Start and wait for an approval** step, at which point it will pause and wait for a response.

1. Check your email for the approval request. Open it, select **Approve**, optionally add a comment, and then select **Submit**.

1. Return to the flow run and confirm it completes successfully, reaching the **Update a row** step.

1. In Power Apps, verify that the **Request Status** of the Work Order has been updated to `Assigned`. (You can refresh the form if you don't see it update immediately.)

> [!NOTE]
> Approval flows pause execution and wait indefinitely for a response. In production, you would typically add a timeout action (using **Run after** settings or a parallel branch with a delay) to handle requests that are never responded to.
