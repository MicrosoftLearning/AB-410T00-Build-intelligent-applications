---
lab:
    title: 'Create a Contoso customer portal with Power Pages'
    description: 'Build an external-facing Power Pages site where Contoso customers can submit Work Orders and check their status'
    duration: '60 minutes'
    level: 300
    islab: true
---

# Create a Contoso customer portal with Power Pages

In this exercise, you build an external-facing portal using Microsoft Power Pages that allows Contoso customers to submit new Work Orders and check the status of existing ones — without needing a Power Platform license.

This exercise should take approximately **60** minutes to complete.

## Scenario

Contoso customers currently report equipment failures by phone. Agents then manually create Work Orders in the system. This process is slow and error-prone. Customers don't get confirmation numbers, and they have to call back to check status.

Contoso wants an external portal where customers can submit requests themselves and track progress online. You'll build this portal using Power Pages connected to the Work Order table from Lab 3.

> **A note on AI-generated output**: Because you'll use Copilot to generate this site, your portal will look different from your classmates' — and that's intentional. Copilot produces different layouts, color schemes, and page structures each time. Treat this as an opportunity to apply your own user experience design judgment: evaluate what Copilot gives you, keep what works, simplify what doesn't, and make design choices that reflect what a real Contoso customer would expect to see.

> **Working with two tabs**: This lab moves back and forth between **Power Apps** (`make.powerapps.com`) and **Power Pages** (`make.powerpages.microsoft.com`). Keep both open in separate browser tabs throughout. At the start of each task, make sure you're in the right tab before you begin.

## Task 1: Create a site with Copilot

Make sure you are in your **Power Pages** tab (`make.powerpages.microsoft.com`) before starting this task.

Power Pages includes a Copilot-powered site creation experience. Instead of starting from a blank template and building everything manually, you describe what you need in plain language and Copilot generates the site structure for you.

1. Open [Power Pages](https://make.powerpages.microsoft.com){:target="_blank"} at `https://make.powerpages.microsoft.com` and sign in with your Microsoft account. (If prompted, select **Get started** and select **Other** for Industry.)

1. Confirm you are in your **Dev One** environment using the environment picker in the top right.

1. You'll see a prompt box that says **Describe the site you want and let Copilot build it**. Enter the following prompt:

    `Contoso Field Services customer portal. Customers can submit Work Orders for equipment failures and check request status. Pages: home, submit a request, contact.`

1. Select the arrow (send) button to submit your prompt. Copilot will generate a site name, web address, and a suggested page structure based on your description.

1. Review what Copilot proposes:
    - Confirm the **Site name** is something like `Contoso Field Services Portal`
    - Adjust the **Web address** if needed — it must be unique in your environment (for example, `contoso-fs-portal-[your initials]`)

1. Select **Next.**

1. Power Pages displays a suggested site layout with the prompt **"Pick the layout that meets your needs."** Review the suggested layout. If you'd like a different option, select **Try again** to generate another. When you find a layout you like, select it.

1. Select **Next**.

1. Review the pages that Copilot suggests. You should see the following pages. Select them to add them to your site:
    - Submit a request
    - Request status
    - About us
    - Contact us

    > **Note**: Copilot may not suggest all of these pages depending on your prompt results. If any are missing, you can ask Copilot to create them once the site is provisioned — you'll do this in the next task. If you run into any issues, ask your instructor for help.

1. Select **Done** to provision the site. This may take 1–2 minutes.

    > **Note**: Copilot generates a starter site with pages, navigation, and placeholder content — all from your description. You'll review and refine it in the next task.

## Task 2: Review and refine the generated site

Make sure you are in your **Power Pages** tab before starting this task.

Once provisioning completes, Power Pages Studio opens with a preview of your generated site. Take a moment to explore what Copilot built before making any changes.

1. Look at the **Pages** panel on the left. Confirm Copilot created at least a **Home** page and a page for submitting requests.

    > **Note**: If any expected pages are missing, open the Copilot panel and ask it to create them — for example: `Add a page called "Submit a Request" for customers to submit new Work Orders.` Copilot will add the page to your site. Ask your instructor if you run into any issues.

1. Select the **Home** page to preview it. Review the generated heading and body text.

1. Locate the Copilot panel: select the **Copilot** icon in the toolbar if it isn't already open.

1. Use the Copilot panel to add a button to the home page. Enter the following prompt:

    `Add a button to the home page labeled "Submit a Work Order" that links to the Submit a Request page.`

1. Review the changes Copilot suggests and select **Keep it** (or **Apply**) to accept them.

1. Now update the home page text manually. Select the main heading on the page and change it to:

    `Contoso Field Services: Your everyday service solution`

1. Select the subheading or body text below the heading. With the text box selected, select the **Copilot** icon that appears, then select **Rewrite**. Copilot will suggest a few alternative versions of the text. Review the options and select the one that best fits a customer-facing field services portal. Select **Replace text** when you're ready.

    > **Note**: If you're not happy with any of the suggestions, select **Try again** to generate more options, or select **Edit** to tweak the text manually before accepting.

1. Select **Sync** in the top toolbar to save your changes.

## Task 3: Create a customer-facing form

Make sure you are in your **Power Apps** tab (`make.powerapps.com`) before starting this task.

Before you can add a form to the portal, you need to create a dedicated form on the Work Order table that only exposes the fields a customer should see. Forms are defined at the table level and reused across apps and portals. It's the same pattern you used in Lab 4 for the model-driven app, but for a different audience.

1. Open a new browser tab and navigate to [Power Apps](https://make.powerapps.com){:target="_blank"} at `https://make.powerapps.com`. Keep the Power Pages tab open.

1. In the left navigation, select **Solutions** and open **Contoso Field Services**.

1. Select **Objects**, expand **Tables**, and expand the **Work Order** table.

1. Select **Forms**, then select **+ New form**.

1. In the **Create a form** window, configure the following:
    - **Type of form**: Main form
    - **Form name**: `Customer Portal`
    - **Description**: `External-facing form for customers to submit new Work Orders through the portal`

    Select **Create**. The form designer opens.

1. Remove all fields from the form body except:
    - **Customer Name**
    - **Customer Email**
    - **Issue Description**

    To remove a field, select it and select the **ellipses (...)**, then choose **Delete**. For required fields that can't be deleted, choose **Hide** instead — they'll be excluded from the form without breaking validation.

    > **Note**: Designing the form this way — with only customer-facing fields — means there's no risk of accidentally exposing internal fields in the portal. It's safer and cleaner than trying to hide fields after the fact in Power Pages.

1. Select **Save and publish**, then close the form designer tab.

## Task 4: Add the form to the portal page

Make sure you are in your **Power Pages** tab before starting this task.

With the Customer Portal form published, you can now add it as a component on the Submit a Request page in Power Pages.

1. Return to the Power Pages tab and refresh it.

1. In the **Pages** panel, select the page created for submitting requests. (Copilot probably named it something like **Submit a Request**.)

1. Your page may look different depending on the layout and content Copilot generated, and that's expected. Regardless of what's already on the page, you'll add a form section. Under the first section, select **+ Add a section**, then select **1 column**.

    > **Tip**: Copilot may have generated several sections on this page that you don't need. To clean it up before adding the form, select a section you want to remove, select the **ellipses (...)** that appears on the section, and select **Delete**. Repeat until the page is as simple as you'd like.

1. Select **+ Add component** in the new section, then select **Form**. If you're prompted to describe the form, select **+New form** under **Other ways to get started**.

1. In the form configuration panel, select the **Form** tab and configure the following:
    - **Choose a table**: Search for and select **Work Order** (`contoso_workorder`)
    - **Select a form**: Choose **Customer Portal**
    - **Name your copy of the selected form**: `Submit a Work Order`

1. Select the **Data** tab and set **Data from this form** to **Creates a new record**.

1. Select the **On Submit** tab and configure the submission message:
    - **When the form is submitted**: Select **Display a message**
    - **Display this message**: `Thank you. Your Work Order has been submitted. A Contoso technician will contact you within 4 business hours.`

1. Leave the **CAPTCHA**, **Attachments**, and **AI form fill** tabs at their defaults. Select **OK**.

1. Select **Sync** to save the page.

## Task 5: Configure table permissions

Make sure you are in your **Power Pages** tab before starting this task.

Power Pages uses an explicit security model; external users cannot read or write Dataverse table data unless you grant permission. This applies even if the form is on the page. Without this step, the form will appear but submissions will fail.

1. In the Power Pages Studio left navigation, select **Security** > **Table permissions**.

1. Select **+ New permission**.

1. Configure the permission:
    - **Name**: `Work Order Access`
    - **Table**: Work Order (`contoso_workorder`)
    - **Access type**: Global access
    - **Permission to**: Create, Read
    - **Roles**: Select **+Add roles** and select **Anonymous Users**

1. Select **Save** and **Save** again to confirm.

    > **Note**: **Anonymous Users** allows anyone to submit without signing in — appropriate for a public submission form. In a production portal, you would use **Authenticated Users** and add row-level filtering so customers can only see their own requests.

## Task 6: Extend the Work Order data model

Make sure you are in your **Power Apps** tab before starting this task.

Before testing the portal, you'll add two new columns to the Work Order table: an **autonumber** column that generates a unique reference number for every submission, and a **formula column** that calculates the estimated resolution date based on the 48-hour SLA.

1. In [Power Apps](https://make.powerapps.com){:target="_blank"}, navigate to **Solutions** > **Contoso Field Services** > **Objects** > **Tables** > **Work Order** > **Columns**.

1. Select **+ New column** and configure the autonumber column:
    - **Display name**: `Work Order Number`
    - **Description:** `Unique number autogenerated for each work order.`
    - **Data type**: Autonumber
    - **Autonumber type**: String prefixed number
    - **Prefix**: `WO`
    - **Minimum number of digits**: `5`
    - **Seed value**: `1`

    Leave **Required** set to **No** — the value is generated automatically when the record is created, so it will never be empty.

    > **Note**: The seed value is the starting number for the sequence. The default is `1000`, which would produce `WO-01000` for the first record. Setting it to `1` starts the sequence at `WO-00001`.

1. Select **Save**. Every new Work Order record will now automatically receive a unique reference number like `WO-00001`.

1. Select **+ New column** again and configure the estimated resolution column:
    - **Display name**: `Estimated Resolution`
    - **Description**: `Calculated date representing the expected resolution time, based on the 48-hour SLA from record creation.`
    - **Data type**: Formula
    - **Formula**: `DateAdd(ThisRecord.'Created On', 48, TimeUnit.Hours)`
    - **Format**: Date and time

    Select **Save**. This column calculates 48 hours from the time the record was created, representing Contoso's standard SLA.

    > **Note**: Formula columns recalculate dynamically. The Estimated Resolution date will always reflect 48 hours from when the record was first created, which is the correct SLA baseline.

1. Now create a dedicated view for the portal status page. In the **Work Order** table, select **Views**, then select **+ New view**. Configure it:
    - **Name**: `Portal Status View`
    - **Description**: `Simplified view for the customer portal showing Work Order Number, Request Status, and Estimated Resolution.`

1. Select **Create**.

1. In the view designer, keep or add the following columns, removing any others:
    - **Customer Name** (this is the primary column and can't be removed — it also helps customers identify their record)
    - **Work Order Number**
    - **Request Status**
    - **Estimated Resolution**

1. Sort the view by Estimated Resolution so the most urgent requests appear first. Select **Sort by** and select **Estimated resolution.**

1. Select **Save and publish**.

    > **Note**: You may notice that the **Work Order Number** column is blank for existing records in the view. This is expected. Autonumber columns only generate values for records created *after* the column is added, so existing records are not backfilled. The Work Order Number will populate correctly for any new record submitted through the portal, including the test record you'll create next.

1. Now add the two new columns to the **Main Information** form so service managers can see them in the model-driven app. In the **Work Order** table, select **Forms** and open the **Main Information** form.

1. In the form designer, add **Work Order Number** to the form and drag it to be the first field in the form.

1. Add **Estimated Resolution** to the form and drag it to be the first field in the **Resolution Details** section.

1. Select **Save and publish**, then close the form designer tab.

## Task 7: Build the Request Status page

Make sure you are in your **Power Pages** tab before starting this task.

Now that the new columns and view are ready, you'll build the Request Status page where customers can look up their Work Order.

1. In the **Pages** panel, select the **Request Status** page (or whatever Copilot named the status-tracking page). If it doesn't exist, open the Copilot panel and enter:

    `Add a page called "Request Status" where customers can view the status of their submitted Work Orders.`

1. In the first section, select the subheading and replace the text with the following:

    `Orders are worked on in the order they are received. Please find your Work Order number in the queue below to check your current status and estimated resolution time.`

1. With the text component selected, select the **Copilot** icon and select **Change the tone.** Select **Friendly** to generate a friendlier, more customer-facing version. Review the suggestions and accept the one that fits best.

1. Add a **1-column section** below the text. Select **List** and then select **+New List.**

1. Configure the list:
    - **Choose a table**: Work Order (`contoso_workorder`)
    - **Select the data views**: Portal Status View
    - **Name your list**: `Work Order Queue`

1. Select **Done**, then select **Sync** to save the page.

1. Now reorder the site navigation so the most important pages appear first. In the **Pages** panel, drag the pages into the following order:
    1. Home
    1. Submit a request
    1. Request status
    1. Contact us (and any other pages, in whatever order you prefer)

    > **Note**: To reorder a page, select and hold the page in the **Pages** panel, then drag it to the new position. The navigation menu on the live site will reflect this order.

1. Select **Sync** to save the updated navigation.

## Task 8: Test the end-to-end scenario

Start in your **Power Pages** tab for this task — you'll switch to Power Apps partway through.

You'll now run through the complete customer journey: submit a Work Order through the portal, look up the Work Order Number in the model-driven app, and then verify it appears in the portal queue.

1. In the top toolbar, select **Preview** > **Desktop** to open the portal in a new browser tab.

    > **Note**: If prompted with a permissions request, select **Accept** to allow the preview to access your Dataverse environment. This is expected the first time you preview a Power Pages site.

1. Select **Submit a Work Order** and fill in the form:
    - **Customer Name**: `Southridge Video`
    - **Customer Email**: `support@southridgevideo.com`
    - **Issue Description**: `Video editing workstation won't power on`
    - If prompted for a CAPTCHA, fill it out.

1. Submit the form and confirm the success message appears.

1. Select the **Request Status** from the site navigation menu. Find the Southridge Video Work Order in the queue and confirm:
    - The **Work Order Number** generated and is in a WO- format
    - The **Request Status** shows **New**
    - The **Estimated Resolution** date is visible

    > **Note**: If the status hasn't updated yet, refresh the portal preview page. Portal pages pull live data from Dataverse, so changes made in the model-driven app are immediately reflected.
