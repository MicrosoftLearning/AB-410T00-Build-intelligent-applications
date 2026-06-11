---
lab:
    title: 'Automate technician notification with a cloud flow'
    description: 'Create an automated cloud flow in Microsoft Power Automate that notifies a field technician when a new Work Order is assigned to them'
    duration: '30 minutes'
    level: 300
    islab: true
---

# Automate technician notification with a cloud flow

In this exercise, you create an automated cloud flow that triggers when a new Work Order is created in Dataverse and sends an email notification to the assigned technician.

This exercise should take approximately **30** minutes to complete.

## Scenario

When a Contoso service manager assigns a technician to a Work Order, the technician has no way of knowing unless someone tells them manually. Managers are currently sending texts and making phone calls — which is time-consuming and unreliable.

You'll create a cloud flow that runs automatically whenever a new Work Order record is created in Dataverse, checks whether a technician has been assigned, and sends them an email with the job details.

## Task 1: Create the automated cloud flow

1. Open [Power Automate](https://make.powerautomate.com){:target="_blank"} at `https://make.powerautomate.com` and sign in with your Microsoft account.

1. Confirm you are in your training environment using the environment picker.

1. In the left navigation, select **+ Create**.

1. Select **Automated cloud flow**.

1. In the **Build an automated cloud flow** dialog:
    - **Flow name**: `Notify Technician on New Work Order`
    - **Choose your flow's trigger**: Search for `When a row is added` and select **When a row is added, modified or deleted** (Microsoft Dataverse)

1. Select **Create**.

## Task 2: Configure the Dataverse trigger

1. The trigger step should already be added to the flow canvas. Configure it:
    - **Change type**: Added
    - **Table name**: Work Orders
    - **Scope**: Organization

1. Select **+ New step** to add the next action.

## Task 3: Add a condition to check for an assigned technician

Not all new Work Orders will have a technician assigned yet — some will be unassigned when first created. You'll add a condition so the email only sends when a technician name is present.

1. Search for and select **Condition** (under Control).

1. Configure the condition:
    - **Value** (left side): Select the dynamic content field **Assigned Technician** from the trigger
    - **Operator**: is not equal to
    - **Value** (right side): leave blank (empty string)

    > **Note**: This condition checks whether the Assigned Technician field contains any value. If the field is empty, the flow will follow the **No** path and stop without sending an email.

## Task 4: Add the email notification action

1. In the **Yes** branch of the condition, select **Add an action**.

1. Search for and select **Send an email (V2)** (Office 365 Outlook).

1. Configure the email:
    - **To**: Select the dynamic content field **Assigned Technician** from the trigger

        > **Note**: In a real implementation, you would use a lookup to the technician's email address from a related table. Since your Assigned Technician column is a text field, you'll type a test email address here instead.

    - **To**: Type your own email address (for testing purposes)

    - **Subject**: Use the following — mix static text with dynamic content:

        `New Work Order Assigned: ` then select **Customer Name** from dynamic content

    - **Body**: Build the following message using a mix of text and dynamic content fields:

        ```
        Hello,

        You have been assigned a new Work Order. Please review the details below:

        Customer: [Customer Name]
        Issue: [Issue Description]
        Priority: [Priority]
        Request Status: [Request Status]

        Please log in to the Contoso Technician App to view full details and update your job status.

        Thank you,
        Contoso Field Services
        ```

        Replace each bracketed item with the corresponding dynamic content field from the Dataverse trigger.

1. Select **Save** in the top toolbar.

## Task 5: Test the flow

1. Select **Test** in the top-right corner.

1. Select **Manually** and then **Test**.

1. Open a new browser tab and go to [Power Apps](https://make.powerapps.com){:target="_blank"}.

1. Navigate to **Tables** > **Work Orders** and create a new record:
    - **Customer Name**: `Fabrikam Industries`
    - **Issue Description**: `Cooling system failure in Building A`
    - **Priority**: `High`
    - **Request Status**: `Assigned`
    - **Assigned Technician**: `Alex Rivera`

1. Save the record.

1. Return to Power Automate and check the test results. The flow should have triggered and show a successful run.

1. Check your email inbox for the notification email.

    > **Note**: If the flow run shows an error, select the failed step to see the error details. Common issues include connection problems (you may need to sign in to the Outlook connector) or dynamic content mapping errors.

## Task 6: Review the flow run history

1. Close the test panel and return to the flow detail page.

1. Scroll down to **28 day run history**. You should see the test run listed with a **Succeeded** status.

1. Select the run to see a detailed view of each step, the inputs, and the outputs.

    > **Note**: The run history is your primary debugging tool in Power Automate. Each step shows exactly what data it received and what it returned, making it straightforward to identify where a flow went wrong.
