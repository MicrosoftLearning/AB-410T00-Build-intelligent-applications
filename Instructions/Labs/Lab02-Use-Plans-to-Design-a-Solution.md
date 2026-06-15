---
lab:
    title: 'Use AI-powered Plans to design the Contoso Field Services solution'
    description: 'Use Microsoft Power Apps Plans to translate a business need into a structured solution blueprint'
    duration: '30 minutes'
    level: 200
    islab: true
---

# Use AI-powered Plans to design the Contoso Field Services solution

In this exercise, you use AI-powered Plans in Microsoft Power Apps to describe Contoso's business challenge and generate a structured solution blueprint, identifying the data model, apps, and automations you'll build in later exercises.

This exercise should take approximately **30** minutes to complete.

## Scenario

Contoso Field Services needs a centralized solution to manage service requests from customers who report equipment failures. Today, requests arrive by phone and email and are tracked manually in spreadsheets. Technicians don't always know which jobs they've been assigned to, customers have no way to check their request status, and managers struggle to prioritize escalations.

You've been asked to design an AI-first Power Platform solution that addresses these problems. Rather than designing from scratch, you use AI-powered Plans to describe the business need and let the AI generate the solution blueprint.

## Task 1: Describe the business problem to Plans

1. Open [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com` and sign in with your Microsoft account.

1. Ensure you are in the correct environment. Select the environment picker in the top-right corner and confirm or select your training environment.

1. Select **Plans** in the left navigation. 

1. Select **Create a plan**.

1. In the description box, enter the following description of the business problem:

    ```
    Contoso Field Services needs to manage service requests from business customers whose industrial equipment has failed. When a customer reports an issue, a service request should be created. Service requests should have a priority field — Low, Normal, High, and Critical. A service manager needs to view and prioritize all open requests and assign a technician. Requests marked Critical should automatically trigger a manager approval before a technician is assigned. The assigned technician needs a mobile app to view their jobs and update the status. Customers should be able to submit new requests and check the status of existing ones online. When a request is submitted, the assigned technician should receive a notification automatically.
    ```

1. Select **Generate** (or press **Enter**) and wait for Plans to process your description.

## Task 2: Review the generated solution blueprint

1. Review the **Requirements**. The Requirements Agent summarizes the business problem it understood from your description. Confirm that the key needs are captured — service request management, technician assignment, customer portal, notifications, and Critical request approvals.

1. If everything looks correct, select **Looks good.** The Process Agent will now generate the processes.

1. Review the **Processes** section. The Process Agent surfaces the key business processes it identified, such as submitting a service request, assigning a technician, and routing Critical requests for approval. Confirm that the main workflows from your description are represented. You can select **View process** to view a branching flow chart of the business process.

1. If everything looks correct, select **Looks good**.

1. Review the **Data** section. Note which tables Plans suggests (for example, Service Requests, Customers, Technicians). When you're satisfied, select **Looks good.**

1. Review the **Technology** section. This combines apps, automations, and pages into a single view. Confirm that you see references to a canvas app for technicians, a model-driven app for managers, flows for notifications and approvals, and a Power Pages site for external customers. When you're satisfied, select **Looks good.**

   > [!NOTE] 
   > Plans uses your description to suggest solution components — the exact output may vary. What matters is the overall shape of the solution: data → apps → automation → portal. This is the same structure you'll build across Labs 3 through 10.

## Task 3: Review the blueprint as your build guide

Take a few minutes to review the full blueprint and connect it to the exercises ahead.

1. Identify the **tables and columns** Plans recommends — you'll create these in Lab 3.

1. Identify the **canvas app** for field technicians — you'll build this in Lab 5.

1. Identify the **model-driven app** for service managers — you'll build this in Lab 6.

1. Identify the **customer portal** — you'll create this in Lab 7.

1. Identify the **automated flow** for technician notification — you'll build this in Lab 8.

1. Identify the **approval flow** for Critical requests — you'll build this in Lab 9.

1. Note where AI-generated responses from your Dataverse data could add value for agents — you'll build this in Lab 10.

   > [!NOTE] 
   > We won't build on these tables and objects at the moment. Plans was a design tool — it helped you understand *what* to build and *why*. In the remaining labs, you'll build each component yourself from scratch, so you understand exactly how everything works. You'll start by creating a new solution in Lab 3.
