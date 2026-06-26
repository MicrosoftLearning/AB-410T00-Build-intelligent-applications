---
lab:
    title: 'Build the Contoso Field Services data model in Dataverse'
    description: 'Create a solution, build the Work Order table with custom columns, and add sample records in Microsoft Dataverse'
    duration: '30 minutes'
    level: 200
    islab: true
---

# Build the Contoso Field Services data model in Dataverse

In this exercise, you create the data model for the Contoso Field Services solution — building the solution, table, and columns that store Work Order data, and adding sample records to test with in later labs.

This exercise should take approximately **30** minutes to complete.

## Scenario

Your Plans blueprint identified the core data need for Contoso Field Services: a **Work Order** table to track customer issues and the technicians assigned to them.

In this exercise, you create the solution container and build that table. The columns you define here will be used by every app, flow, and portal you build in later exercises.

## Task 1: Create a solution

All components you build in this course — tables, apps, flows, and pages — belong to a single solution. Working inside a solution keeps everything organized, makes it easy to move your work between environments, and is required for some features such as business process flows.

1. Open [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com` and sign in with your Microsoft account.

1. Confirm you are in the **Dev One** environment using the environment picker in the top-right corner.

1. In the left navigation, select **Solutions**.

1. Select **+ New solution**.

1. Configure the solution as follows, then select **Create**:
    - **Display name**: `Contoso Field Services`
    - **Name**: `ContosoFieldServices` (auto-filled)
    - **Publisher**: Select **Contoso (contoso)**
    - **Version**: `1.0.0.0`

1. The solution opens. All tables, apps, flows, and pages you create in Labs 3–10 will be added here.

## Task 2: Create the Work Order table

1. Confirm you are in the **Contoso Field Services** solution.

1. Select **+ New** > **Table** > **Tables**.

1. Select **Start from blank**.

1. Select the table name (it will likely say **Table1**) and rename it to `Work Order`.

> [!NOTE]
> Pay extra attention to the naming of this table. In Lab 2, the Plans designer likely already created a table called **Service Request** as part of its suggested data model, so we want to make sure this table has a distinct name. Even though that table lives in a different solution, all tables in a Dataverse environment share the same namespace — meaning tables from different solutions are visible across the environment in selectors, connectors, and security roles. Naming this table **Work Order** keeps the two distinct and avoids confusion when you're choosing tables later in the course. Functionally, it serves the same purpose: tracking customer issues and the technicians assigned to resolve them. **Work Order** is also the industry-standard term used in Dynamics 365 Field Service, so it's a realistic name for this scenario. 

    If Plans actually used the term **Work order** for the table it created, let your instructor know—they will walk you through renaming that table.

## Task 3: Add columns to the Work Order table

Now you add the columns needed to capture the details of each Work Order.

1. On the **Work Order** table card, select the ellipsis (**...**) and then select **View data**. The table opens in the data editor.

1. Select **New column** and then **Edit column**.

1. Configure the column as follows, then select **Save**:
    - **Display name**: `Customer Name`
    - **Data type**: Single line of text
    - **Format**: Text
    - **Required**: On

1. Select **Update** to save the column.

> [!NOTE]
> You'll notice that **Customer Name** appears on the Work Order table card with a key icon next to it. This means it's the **primary column** — the field Dataverse uses to identify and display a record throughout the system. When other tables or apps reference a Work Order, they display the primary column value as the record's label. Every Dataverse table has exactly one primary column, and it's set when the table is created. In this case, we're using Customer Name as the primary column so that Work Orders are identified by the customer they belong to.

1. Add a second column with the following settings, then select **Save**:
    - **Display name**: `Customer Email`
    - **Data type**: Text > Email
    - **Required**: Off

1. Add a third column with the following settings, then select **Save**:
    - **Display name**: `Issue Description`
    - **Data type**: Text > Text Area
    - **Required**: On

1. Add a fourth column with the following settings, then select **Save**:
    - **Display name**: `Priority`
    - **Data type**: Choice > Choice
    - **Choices**: `Low`, `Normal`, `High`, `Critical`
    - **Default choice**: `Normal`
    - **Required**: On

1. Add a fifth column with the following settings, then select **Save**:
    - **Display name**: `Request Status`
    - **Data type**: Choice > Choice
    - **Choices**: `New`, `Assigned`, `In Progress`, `Resolved`, `Closed`
    - **Default choice**: `New`
    - **Required**: On

1. Add a sixth column with the following settings, then select **Save**:
    - **Display name**: `Resolved Date`
    - **Data type**: Date and time > Date only
    - **Required**: Off

1. Before adding the final column, you need to add the **User** table to your solution so it's available as a lookup target. Select **+ Existing table** from the top menu, search for **User**, select it, and then select **Add Selected**. 

1. Return to the **Work Order** table data editor. Add a seventh column with the following settings, then select **Save**:
    - **Display name**: `Assigned Technician`
    - **Data type**: Lookup
    - **Related table**: User
    - **Required**: Off

> [!NOTE]
> A lookup column creates a relationship between the Work Order table and the User table. When a manager assigns a technician in the app, they'll pick from a list of real users in the environment rather than typing a name. The column stores a reference to the selected user record.

1. Review the columns you've added. Your Work Order table should now have the primary column (**Customer Name**) plus six additional columns: Customer Email, Issue Description, Priority, Request Status, Resolved Date, and Assigned Technician. System columns such as **Created On** may be present but were added automatically by Dataverse. You should also see a **Relationship** connecting the Work Order table to the User table.

1. Select **Save and exit**. (You may need to select **Save and exit** again to confirm.)

## Task 4: Add sample Work Order records

You'll need some data in the Work Order table to test the canvas app you build in the next lab. Add a few sample records now.

1. Open [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com` and confirm you are in the **Dev One** environment.

1. In the left navigation, select **Solutions**, then open the **Contoso Field Services** solution.

1. Select **Tables**, then select the **Work Order** table.

1. Select **Edit** to open the table data editor.

> [!TIP] 
   >The data editor may not show all columns by default. If a column such as **Request Status** or **Priority** is missing, select the **+ (number) more** button at the right end of the column headers to add it to the view.

1. Select **+ New row** and enter the following values:
    - **Customer Name**: `Adatum Corporation`
    - **Issue Description**: `Air conditioning unit is making loud noise and not cooling properly`
    - **Priority**: `High`
    - **Request Status**: `New`

1. Select **+ New row** and enter the following values:
    - **Customer Name**: `Tailwind Traders`
    - **Issue Description**: `Elevator panel buttons are unresponsive on floors 3 and 4`
    - **Priority**: `Critical`
    - **Request Status**: `Assigned`

1. Select **+ New row** and enter the following values:
    - **Customer Name**: `Fabrikam Inc`
    - **Issue Description**: `Exterior lighting not turning on at dusk`
    - **Priority**: `Normal`
    - **Request Status**: `In Progress`

1. Confirm that all three records appear in the table data editor and the data was saved successfully.

## Verify your work

Before moving on, confirm the following:

- The **Contoso Field Services** solution exists in the **Dev One** environment
- The **Work Order** table exists with custom columns including Priority (Choice) and Request Status (Choice)
- A relationship exists between **Work Order** and **User** tables
- The **Priority** column has the choices: Low, Normal, High, Critical
- The **Request Status** column has the choices: New, Assigned, In Progress, Resolved, Closed
- 3 sample rows exist in the **Work Order** table
