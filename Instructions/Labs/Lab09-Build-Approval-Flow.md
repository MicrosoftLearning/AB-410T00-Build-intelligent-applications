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

Contoso's process requires manager approval before a technician can be assigned to any Critical priority Work Order — because these incidents affect production systems and carry higher liability. Currently, managers have to monitor the Work Order queue manually and catch Critical requests on their own. There's no automated way to notify them or capture their decision.

You'll build an approval flow that triggers when a Work Order's priority is set to Critical, sends an approval request to the service manager, and updates the request status based on their response.

## Task 1: Create the approval flow

1. Open [Power Automate](https://make.powerautomate.com){:target="_blank"} at `https://make.powerautomate.com` and sign in with your Microsoft account.

1. Confirm you are in your training environment.

1. In the left navigation, select **+ Create** > **Automated cloud flow**.

1. Configure the flow:
    - **Flow name**: `Critical Request Approval`
    - **Trigger**: Search for `When a row is added` and select **When a row is added, modified or deleted** (Microsoft Dataverse)

1. Select **Create**.

## Task 2: Configure the trigger and add a filter

1. Configure the Dataverse trigger:
    - **Change type**: Added or Modified
    - **Table name**: Work Orders
    - **Scope**: Organization

1. Select **+ New step** and add a **Condition**:
    - **Value** (left): Dynamic content field **Priority** from the trigger
    - **Operator**: is equal to
    - **Value** (right): `Critical`

    > **Note**: Choice field values in conditions must match exactly as stored in Dataverse — typically the label text or the underlying integer value. If `Critical` doesn't work, check the stored value in the table's choice column definition.

## Task 3: Add the approval action

1. In the **Yes** branch of the condition, select **Add an action**.

1. Search for and select **Start and wait for an approval** (Approvals connector).

1. Select **Approve/Reject - First to respond** as the approval type.

1. Configure the approval:
    - **Title**: Type `Critical Work Order Requires Approval: ` then select **Customer Name** from dynamic content
    - **Assigned to**: Enter your own email address (representing the service manager for testing)
    - **Details**: Build a message using dynamic content:

        ```
        A new Critical priority Work Order has been submitted and requires your approval before a technician can be assigned.

        Customer: [Customer Name]
        Issue: [Issue Description]
        Priority: [Priority]

        Please approve to proceed with technician assignment, or reject to return the request to Normal priority.
        ```

    - **Item link**: Leave blank for now
    - **Item link description**: Leave blank for now

1. Select **Save**.

## Task 4: Handle the approval outcome

Now you'll add actions for both the approved and rejected paths.

1. Below the **Start and wait for an approval** step, add a new **Condition**:
    - **Value** (left): Dynamic content **Outcome** from the approval step
    - **Operator**: is equal to
    - **Value** (right): `Approve`

### If approved

1. In the **Yes** branch, add an action: **Update a row** (Microsoft Dataverse).

1. Configure the update:
    - **Table name**: Work Orders
    - **Row ID**: Dynamic content **Row ID** from the trigger
    - **Request Status**: `Assigned`

    > **Note**: This signals that the request has been approved and is ready for technician assignment. A more complete solution would also include a step to notify the manager's team that assignment can proceed.

### If rejected

1. In the **No** branch, add an action: **Update a row** (Microsoft Dataverse).

1. Configure the update:
    - **Table name**: Work Orders
    - **Row ID**: Dynamic content **Row ID** from the trigger
    - **Priority**: `High`
    - **Request Status**: `New`

1. Add a second action in the **No** branch: **Send an email (V2)** (Office 365 Outlook):
    - **To**: Your email address (representing notification back to the submitting agent)
    - **Subject**: `Critical Request Rejected: ` + **Customer Name** dynamic content
    - **Body**: `The Critical priority request for [Customer Name] was not approved. The priority has been reset to High and the request is back in the New status for review.`

1. Select **Save**.

## Task 5: Test the approval flow

1. Select **Test** > **Manually** > **Test**.

1. Open a new browser tab and go to [Power Apps](https://make.powerapps.com){:target="_blank"}.

1. Navigate to **Tables** > **Work Orders** and create a new record:
    - **Customer Name**: `Northwind Traders`
    - **Issue Description**: `Complete electrical failure across production floor`
    - **Priority**: `Critical`
    - **Request Status**: `New`

1. Save the record.

1. Return to Power Automate and monitor the test run. The flow should trigger and reach the **Start and wait for an approval** step — at which point it will pause and wait for a response.

1. Check your email for the approval request. Open it and select **Approve**.

1. Return to the flow run and confirm it completes successfully, reaching the **Update a row** step.

1. In Power Apps, verify that the **Request Status** of the Work Order has been updated to `Assigned`.

    > **Note**: Approval flows pause execution and wait indefinitely for a response. In production, you would typically add a timeout action (using **Run after** settings or a parallel branch with a delay) to handle requests that are never responded to.
