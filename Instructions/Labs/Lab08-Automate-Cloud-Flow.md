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

1. Open [**Power Automate**](https://make.powerautomate.com) at `https://make.powerautomate.com` and sign in with your Microsoft account.

1. Confirm you are in your **Dev One** environment using the environment picker.

1. In the left navigation, select **+ Create**.

1. Select **Automated cloud flow**.

1. In the **Build an automated cloud flow** dialog, configure the following:
    - **Flow name**: `Notify Technician on New Work Order`
    - **Choose your flow's trigger**: Search for `When a row is added` and select **When a row is added, modified or deleted** (Microsoft Dataverse)

1. Select **Create**.

## Task 2: Configure the Dataverse trigger

1. The trigger step should already be added to the flow canvas. Select it to configure it:
    - **Change type**: Added
    - **Table name**: Work Orders
    - **Scope**: Organization

1. Below the **When a row is added, modified or deleted** trigger, select **+** to add an action.

1. Search for `Get a row by ID` and select **Get a row by ID** under **Microsoft Dataverse**.

1. In the **Get a row by ID** step, configure the following settings:
    -   **Table name:** Work Orders
    -   **Row ID:** In **Dynamic content**, search for and select **Work Order**.

1. Select **+** to add the next action underneath it.

## Task 3: Add a condition to check for an assigned technician

Not all new Work Orders will have a technician assigned yet — some will be unassigned when first created. You'll add a condition so the email only sends when a technician name is present.

1. Search for and select **Condition** (under Control).

1. Configure the condition:
    - **Value** (left side): Type `/` and select **Insert dynamic content**. Search for and select **Assigned Technician (Value)**.
    - **Operator**: is not equal to
    - **Value** (right side): leave blank (empty string)

1. With the condition still selected, open the **Copilot** pane by selecting the Copilot icon in the upper-right corner of the flow designer. Enter the following prompt:

    `Explain what this condition does.`

    Copilot should return a response similar to: *"The condition checks if the field contoso_assignedtechnician_value is not empty. If a technician is assigned, the actions inside the True branch will run. If no technician is assigned, the actions inside the False branch will run."*

## Task 4: Add the email notification action

1. In the **True** branch of the condition, select the plus sign to add an action.

1. Search for and select **Get a row by ID** (Microsoft Dataverse).

1. Configure the action:
    - **Table name**: Users
    - **Row ID**: Type `/` and select **Insert dynamic content**. Search for and select **Assigned Technician (Value)**.

   > [!NOTE]
   > This action retrieves the full User record for the assigned technician so you can access their email address in the next step.

1. Select the plus sign below the **Get a row by ID** action to add another action.

1. Search for and select **Send an email (V2)** (Office 365 Outlook).

1. When prompted, select **Sign in** and sign in with your Administrator email address provided by your Authorized Lab Host. Accept the permissions. If the sign-in window doesn't appear, check that your pop-up blocker is turned off.

1. Configure the email:
    - **To**: Select the gear icon above the field and select **Use dynamic content**. Then start typing `/` and select **Insert dynamic content.** Search for and select **Primary Email** (from the **Get a row by ID** step).

    - **Subject**: Type `New Work Order Assigned: `, then type `/` and select **Insert dynamic content**. Search for and select **Customer Name**.

    - **Body**: Build the following message using a mix of text and dynamic content fields:

        ```
        Hello,

        You have been assigned a new Work Order. Please review the details below:

        Customer: [Customer Name]
        Issue: [Issue Description]
        Priority: [Priority]

        Please log in to the Contoso Technician App to view full details and update your job status.

        Thank you,
        Contoso Field Services
        ```

        For each bracketed placeholder, delete the bracket text, then type `/` and select **Insert dynamic content**. Search for and select the matching field from the Dataverse trigger. For **Priority**, add the following expression: `body('Get_a_row_by_ID')?['contoso_priority@OData.Community.Display.V1.FormattedValue']` to display the priority label instead of the numeric value.

1. Select **Save** in the top toolbar.

## Task 5: Test the flow

1. Select **Test** in the top-right corner.

1. Select **Manually** and then **Test**.

1. Open a new browser tab and go to [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com`. Ensure you are in your **Dev One** environment.

1. Open the **Contoso Service Management** model-driven app. Select **Work orders** to navigate to the Work Orders page and select **+ New** to create a new Work Order using the form:
    - **Customer Name**: `Fabrikam Industries`
    - **Issue Description**: `Cooling system failure in Building A`
    - **Priority**: `High`
    - **Request Status**: `Assigned`
    - **Assigned Technician**: Select your own user account from the lookup (type `MOD` to find the MOD Administrator account)

1. Select **Save** to save the record.

   > [!NOTE]
   >  Create the test record through the model-driven app rather than directly in the table editor. The app form ensures the lookup field value is properly committed when the record is saved, so the flow trigger receives a valid Assigned Technician ID. Creating a record directly in the table editor can result in the lookup value not being passed to the trigger correctly.

1. Return to Power Automate and check the test results. The flow should have triggered and show a successful run.

1. Open a new browser tab and go to [**Outlook**](https://outlook.office.com) at `https://outlook.office.com`. Sign in with your MOD Administrator email address provided by your Authorized Lab Host and check your inbox for the notification email.

   > [!NOTE]
   > If the flow run shows an error, select the failed step to see the error details. Common issues include connection problems (you may need to sign in to the Outlook connector) or dynamic content mapping errors.

## Task 6: Review the flow run history

1. Close the test panel and select **Back** to return to the flow detail page.

1. Scroll down to **28 day run history**. You should see the test run listed with a **Succeeded** status.

1. Select the run to see a detailed view of each step, the inputs, and the outputs.

   > [!NOTE]
   > The run history is your primary debugging tool in Power Automate. Each step shows exactly what data it received and what it returned, making it straightforward to identify where a flow went wrong.
