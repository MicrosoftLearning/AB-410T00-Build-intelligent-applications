---
lab:
    title: 'Configure Contoso security roles in Dataverse'
    description: 'Create Dataverse security roles for field technicians and service managers to control what each user type can see and do'
    duration: '20 minutes'
    level: 200
    islab: true
---

# Configure Contoso security roles in Dataverse

In this exercise, you create two security roles in Dataverse — one for field technicians and one for service managers — to control what each type of user can see and do with Work Order data.

This exercise should take approximately **20** minutes to complete.

## Scenario

Now that the Work Order table is in place, you need to make sure the right people have the right level of access. Contoso Field Services has two distinct user types:

- **Field technicians** should only see and update Work Orders assigned to them — not every request in the system.
- **Service managers** need full visibility and control over all Work Orders so they can assign, escalate, and report across the whole team.

You'll create a Dataverse security role for each user type and configure the appropriate table privileges.

## Task 1: Create the Field Technician security role

Security roles in Dataverse control what records each user can read, create, update, and delete. You configure privileges per table, and you can scope each privilege to the user's own records, their business unit, or the entire organization.

1. Open a new tab and navigate to the [**Power Platform Admin Center**](https://admin.powerplatform.microsoft.com) at `https://admin.powerplatform.microsoft.com`.

1. In the left navigation, select **Manage**, then select **Environments**.

1. Select your **Dev One** environment.

1. Select **Settings** in the command bar.

1. Expand **Users + permissions** and select **Security roles**.

1. Select **+ New role** to open the **Create New Role** panel.

1. Enter `Contoso Field Technician` as the role name.

1. For **Business Unit**, select your organization. It appears as a prefix in the format `orgXXXXXXXX`.

1. For **Description**, enter `Grants field technicians access to view and update their own assigned Work Orders.`

1. For **Applies to**, enter `Field Technicians`.

1. For **Summary of Core table privileges**, enter `Read, Write (User): Work Order`.

1. Select **Save**.

1. The role opens in the privilege editor. In the **Search** box, enter `Work Order` to find the table.

1. Select **Work Order** (`contoso_workorder`), then set the following permissions:
    - **Create**: None (technicians don't create new Work Orders)
    - **Read**: User (the technician can read their own assigned records)
    - **Write**: User (the technician can update their own records)
    - **Delete**: None (technicians can't delete records)
    - **Append**: User (the technician can associate notes or activities with their own records)
    - **Append To**: User (records can be appended to the technician's Work Orders)
    - **Assign**: None (technicians can't reassign records to others)
    - **Share**: None (technicians can't share records with others)

1. Select **Save** in the command bar.

1. Select **Security roles** to return to the security role list.

## Task 2: Create the Service Manager security role

1. Select **+ New role**.

1. Enter `Contoso Service Manager` as the role name.

1. For **Business Unit**, select your organization.

1. For **Description**, enter `Grants service managers full access to all Work Orders across the organization.`

1. For **Applies to**, enter `Service Managers`.

1. For **Summary of Core table privileges**, enter `Read, Write, Create, Delete (Organization): Work Order`.

1. Select **Save**.

1. In the **Search** box, enter `Work Order` to find the table. Select the **Work Order** (contoso_workorder) table.

1. Set the following permissions:
    - **Create**: Organization (the manager can create new Work Orders)
    - **Read**: Organization (the manager can read all records in the environment)
    - **Write**: Organization (the manager can update any record)
    - **Delete**: Organization (the manager can delete any record)
    - **Append**: Organization (the manager can associate notes or activities with any record)
    - **Append To**: Organization (records can be appended to any Work Order)
    - **Assign**: Organization (the manager can assign Work Orders to technicians)
    - **Share**: Organization (the manager can share records with others)

1. Select **Save + close** from the command bar.

   > [!NOTE]
   > The **Organization** scope means the user can access all records in the environment, regardless of who created or owns them. The **User** scope limits access to records the user owns or has been explicitly shared with them.

## Verify your work

Before moving on, confirm the following:

- The **Contoso Field Technician** security role exists with User-scoped Read and Write access on the `contoso_workorder` table
- The **Contoso Service Manager** security role exists with Organization-scoped Create, Read, Write, Delete, Assign, and Share access on the `contoso_workorder` table
