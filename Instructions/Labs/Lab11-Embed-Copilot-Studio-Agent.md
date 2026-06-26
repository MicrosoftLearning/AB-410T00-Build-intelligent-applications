---
lab:
    title: 'Build and evaluate a Copilot Studio agent for Contoso service managers'
    description: 'Create a Copilot Studio agent grounded on the Work Order table, add suggested prompts and a custom escalation topic, and use a second agent to compare the AI-generated plan from Lab 2 to the solution you built'
    duration: '45 minutes'
    level: 400
    islab: true
---

# Build and evaluate a Copilot Studio agent for Contoso service managers

In this exercise, you build two Copilot Studio agents. The first is a practical field service assistant grounded on your Work Order data � the kind of agent a real Contoso service manager would use. The second is a learning tool built for you as a student: a Plan Comparison assistant that lets you interrogate the AI-generated plan from Lab 2 and compare it to the solution you actually built across the course.

This exercise should take approximately **45** minutes to complete.

## Scenario

Service managers at Contoso Field Services spend significant time manually searching for information: which technician is overloaded, which customers have open Critical requests, and what was the resolution history for a specific account. The Copilot panel you enabled in Lab 7 helps with simple queries, but managers need a more guided, conversational assistant that understands Contoso-specific context.

In this exercise, you build a custom Copilot Studio agent grounded in the Work Order table, configure suggested prompts and a custom escalation topic, and test it against your data. You then build a second agent grounded on the three tables Plan Designer created in Lab 2 � **Account**, **Field Technician**, and **Service Request** � and use it to compare what was planned to what you actually built.

## Task 1: Open the model-driven app and initiate agent creation

You'll create the agent from inside the model-driven app's designer. This approach opens Copilot Studio already scoped to your environment, avoiding the sign-in and loading issues that can occur when navigating to Copilot Studio directly.

1. Open [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com` and sign in with your Microsoft account.

1. Confirm you are in your **Dev One** environment.

1. In the left navigation, select **Solutions**, then open the **Contoso Field Services** solution.

1. Open the **Contoso Service Management** model-driven app and select **Edit** to open it in the app designer.

1. In the app designer, select the **Agents** tab in the left navigation pane.

1. Select **+ Create agent**. Copilot Studio opens in a new browser tab, already scoped to your Dev One environment.

1. You may be prompted to **Start a trial** when Copilot Studio opens. Select it to proceed. You may also be prompted to sign in with your email address. Follow the prompts to complete sign-in before continuing.

> [!NOTE]
> If Copilot Studio gets stuck on a loading screen after sign-in, this is a known UI bug. To work around it:
> 1. Open [**Power Automate**](https://make.powerautomate.com) in a new browser tab.
> 1. Look at the URL in the address bar. It will contain a path like `/environments/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/`. Copy the value between `/environments/` and the next `/` — that is your environment ID.
> 1. In the stuck Copilot Studio tab, replace the URL in the address bar with `https://copilotstudio.microsoft.com/environments/{your-environment-id}/home`, substituting your environment ID for `{your-environment-id}`.
> 1. Press Enter to navigate. Copilot Studio should load correctly.

## Task 2: Configure the agent in Copilot Studio

1. In Copilot Studio, skip any welcome messages that appear.

1. Confirm you are in the **Dev One** environment.

1. In the **What would you like to build?** text box on the Copilot Studio home page, enter the following description:

    ```
    An AI assistant for Contoso service managers. Answers questions about Work Orders, technician assignments, and resolution status using live Dataverse data.
    ```

1. Select the arrow or press Enter to submit. Copilot Studio generates a name, description, and instructions for the agent and opens the **Overview** page.

1. Review the generated values. Under the **Details** section, select **Edit** and update the **Name** to `Contoso Service Assistant`. The description and instructions don't have to be exact, but they should roughly match the following:
    - **Name**: `Contoso Service Assistant`
    - **Description**: `An AI assistant for Contoso service managers. Answers questions about Work Orders, technician assignments, and resolution status using live Dataverse data.`
    - **Instructions**:

        ```
        You are a service management assistant for Contoso Field Services. You help service managers find information about open Work Orders, technician workloads, and customer histories. Always be concise and professional. When referencing Work Orders, include the customer name, priority, and current status. If you can't find the information requested, say so clearly and suggest how the manager might find it themselves.
        ```

1. Select **Save** in each section to save your changes.

## Task 3: Ground the agent on the Work Order table

Grounding connects the agent to your Dataverse data so it can answer questions using real records rather than general knowledge.

> [!IMPORTANT]
> Two prerequisites are required before Dataverse knowledge sources will work:
> - **Dataverse Search** must be enabled in your environment. If you can't add a Dataverse table in the steps below, ask your administrator to enable Dataverse Search in the Power Platform admin center.
> - The agent's authentication must be set to **Authenticate with Microsoft**. To verify or set this, in the agent designer, select **Settings**, then **Security**, then **Authentication**, choose **Authenticate with Microsoft**, and select **Save**.

1. In the agent designer, select the **Knowledge** tab in the top navigation.

1. Select **+ Add knowledge**.

1. Select **Dataverse** as the knowledge source.

1. Search for and select the **Work Order** table (`contoso_workorder`).

1. Select **Add to agent**. The agent is now grounded on your Work Order data.

> [!NOTE]
> When grounded on a Dataverse table, the agent can retrieve and summarize records using natural language queries. It respects Dataverse security � it only returns records that the signed-in user has permission to see based on their assigned security role.

## Task 4: Configure suggested prompts

Suggested prompts appear at the start of a conversation, giving users ready-made questions to choose from. They help service managers get value from the agent immediately without having to think of what to ask.

1. In the agent designer, select the **Overview** tab.

1. Scroll down to the **Suggested prompts** section and select **+ Add suggested prompts**.

1. Add the following suggested prompts. For each one, enter a **Title** (the label users see) and a **Prompt** (the text sent to the agent):

    | Title | Prompt |
    |---|---|
    | Show open Work Orders | `Show me all open Work Orders` |
    | Critical requests | `Which Critical requests need attention?` |
    | Unassigned Work Orders | `Are there any Work Orders without an assigned technician?` |
    | Recent status | `What's the status of recent Work Orders?` |

1. Select **Save**.

> [!NOTE]
> Suggested prompts are not visible in the Copilot Studio test panel. They appear on the agent's welcome page only when the agent is deployed to a channel such as Teams or an embedded app. You won't see them during testing in this lab, but they would be ready in a published or deployed agent.

## Task 5: Create an escalation topic

Topics let your agent respond to specific phrases with a guided, structured response � going beyond open-ended Q&A to deliver a consistent workflow. Unlike knowledge-based answers (which are generative), topics follow a defined path you control. You'll create an escalation topic that walks a service manager through a checklist when they need to escalate a Critical Work Order.

1. In the agent designer, select the **Topics** tab in the top navigation.

1. Select **+ Add a topic** > **From blank**.

1. At the top of the canvas, name the topic `Work Order Escalation`.

1. Select the **Trigger** node. Because your agent uses generative orchestration, the trigger shows **The agent chooses** rather than a list of phrases. In the description text box, enter the following:

    ```
    Trigger this topic when the user wants to escalate a Work Order, mentions a critical issue, or asks for help with an urgent situation that needs immediate attention.
    ```

> [!NOTE]
> With generative orchestration, you describe the topic's purpose in plain language rather than listing exact phrases. The AI reads this description to decide when to route the conversation to this topic � so a clear, specific description produces more reliable triggering.

1. Below the trigger node, select **+** to add a node, then select **Send a message**. Enter the following:

    ```
    I can help you escalate this Work Order. Let's make sure everything is in order before we proceed.
    ```

1. Add another **Send a message** node with the following escalation checklist:

    ```
    Before escalating, confirm the following:
    - The Work Order has a customer name and issue description
    - Priority is set to Critical or High
    - A technician is assigned (or your escalation includes a request to assign one)
    - The requesting manager has been notified

    Update the Work Order record with any missing information before proceeding.
    ```

1. Add a final **Send a message** node:

    ```
    Once the checklist is complete, contact the assigned technician directly and notify your service lead. Document the escalation in the Work Order's resolution notes.
    ```

1. Select **Save** to save the topic.

> [!NOTE]
> Topics take priority over generative answers. When a user sends a message that matches a trigger phrase, the agent follows the topic's defined path rather than generating a free-form response. This makes topics ideal for high-stakes scenarios � like escalations � where you want consistent, predictable behavior.

## Task 6: Test the agent in Copilot Studio

Before evaluating, test the agent to confirm it responds correctly � both to Work Order data questions and to the escalation topic you created.

1. In the agent designer, select **Test** (in the top-right area) to open the test chat panel.

1. Try the following in the test chat:
    - `How many open Work Orders do we have?`
    - `Show me all Critical priority requests`
    - `Which requests don't have an assigned technician?`
    - `I need to escalate this`

1. Review the responses. The first three should return data-driven answers from your Work Order table. The last should trigger the **Work Order Escalation** topic and walk through the checklist.

> [!NOTE]
> If your environment doesn't have any Work Order records yet, create a few test records in the model-driven app from Lab 6 before testing. Use different customers, priorities, and statuses to get useful responses.

1. If responses are inaccurate or overly generic, select the response and use the **Feedback** controls to help improve the agent.

## Task 7: Explore the Plan Designer solution

In Lab 2, Plan Designer generated a solution with three tables: **Account**, **Field Technician**, and **Service Request**. Before grounding your comparison agent on them, take a moment to understand what each table represents � and how it relates to what you actually built.

1. Keep the Copilot Studio tab open. In a new tab, open [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com` and navigate to **Solutions**.

1. Locate the solution you created in Lab 2 with Plan Designer and open it. (It may be named something like **Field Service Request Manager** � if you're not sure which one it is, ask your instructor.)

1. Select **Objects** and expand **Tables**. The tables listed here are the ones the Data Agent created in Lab 2. Your tables might not match a peer's � that's fine. Take note of the **Display Name** of each table. You'll use those names in Task 8 to ground the comparison agent.

## Task 8: Create a Plan Comparison agent

Now you'll create a second agent, a **Plan Comparison Assistant**, grounded on the Plan Designer tables and your Work Order table. You'll use the test panel to ask it comparison questions that surface the architectural differences between what was planned and what you built.

1. Switch to your **Copilot Studio** browser tab. In the left navigation, select **Agents**.

1. In the Copilot text box, enter:

    ```
    An assistant that helps compare an AI-generated solution plan to the actual solution that was built. It can answer questions about what was planned, what was built, and what differences exist between the two.
    ```
    
1. Select the arrow to submit and wait for the agent to provision.

1. Review the generated name. Under the **Details** section, select **Edit** and update the name to `Plan Comparison Assistant`, then select **Save**.

1. On the **Overview** page, find the **Instructions** section and select **Edit.**

1. Update it to include the following, substituting your own plan table names where indicated:

    ```
    You have access to two sets of data. The [your **Plan** table names] tables represent an AI-generated solution plan created in Lab 2. The Work Order (contoso_workorder) table represents the solution that was actually built during this course. When asked comparison questions, always frame your answers in terms of "what the plan suggested" versus "what was built."
    ```

> [!NOTE]
> This context is critical. Without it, the agent sees all four tables equally and can't reason about which represents the plan and which represents the built solution.

1. Select **Save**.

1. Select the **Knowledge** tab, then select **+ Add knowledge**.

1. Select **Dataverse**. Search for and add each of the Plan Designer-created tables you noted in Task 7.

1. Select **Add to agent.**

1. Select **+ Add knowledge** again, select **Dataverse**, and add the **Work Order** table (`contoso_workorder`). 

1. Select **Add to agent**.

1. Select **Test** to open the test panel and try the following questions:
    - `Can you summarize the tables I built versus the tables the Plan created?`
    - `What fields does the Service Request table have?`
    - `How does the Field Technician table relate to Service Requests?`
    - `What fields does our Work Order table have?`
    - `How is the Work Order table different from the Service Request table?`

1. Review the responses and take notes on what the agent returns for each question. You'll use these observations in Task 9.

> [!NOTE]
> The quality of the agent's responses depends on how much data is in your tables. If the Plan Designer tables are mostly empty, responses will be sparse � which is itself a useful observation about the limits of plan-only data as a knowledge source.

## Task 9: Reflect and share out

The Plan Comparison agent can reason about table schemas and data, but a solution is more than tables. Cloud flows, apps, and security roles exist only as solution components, not as Dataverse records, so the agent has no visibility into them. To get the full picture, you'll browse the solution manually alongside your agent observations.

1. Open [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com` and navigate to **Solutions**. Open the solution you created in Lab 2 with Plan Designer.

1. Select **Objects** and spend 2�3 minutes browsing through the full component list. Look at each category:
    - **Tables:** compare to what the agent told you about the plan's data model
    - **Cloud flows:** did the plan generate any automation?
    - **Apps:** what app structure did the plan suggest?
    - **Security roles:** did the plan include any roles?

1. Review your notes from the Plan Comparison agent. Use these questions to guide your thinking:
    - Did your plan create a dedicated technician table? In the course labs, technician assignment was handled as a lookup field on the Work Order rather than a separate entity � did your plan do the same, or something different?
    - Did your Work Order table relate to an Account entity, or did it store customer information as a plain text field? What did the plan do?
    - Compare the columns on your plan's core service request table to the fields on your Work Order table. What concepts overlap? What's missing from one side?

1. Based on what you saw in the solution and what the Plan Comparison agent returned, identify:
    - **One thing that matched** (for example, did both solutions have a concept of a service request/work order and technician assignment?)
    - **One thing that diverged** (for example, the plan used a separate **Field Technician** table while you used a lookup field, or the plan used the **Account** entity while you used a plain text customer name field)
    - **One thing you added** (something meaningful you built that wasn't in the plan at all, like the escalation topic, security roles, the model-driven app dashboard, or the Copilot Studio agent itself)

1. Be prepared to share at least one of your findings with the class. Here are a few discussion points to get you started:
   - *"Did anyone's plan accurately predict the security role structure?"*
   - *"What did the plan get right about the data model?"*
   - *"What did you build in these labs that surprised you � things the plan never anticipated?"*
   - *"If you were starting a real field service project tomorrow, how would you use Plan Designer differently?"*
   - *"What seems easier: creating the model from scratch like we did, or editing components that the Plan Designer created?"*

> [!TIP]
> AI-assisted planning is a powerful starting point, not a finished blueprint. The gap between what a plan generates and what a real solution requires is exactly where human judgment, domain expertise, and iterative testing add value. You've experienced that gap firsthand across these labs.

## Verify your work

Before moving on, confirm the following:

- The **Contoso Service Assistant** agent exists in Copilot Studio
- The agent is grounded on the **Work Order** (`contoso_workorder`) Dataverse table
- The agent has at least four suggested prompts configured
- The **Work Order Escalation** topic exists and triggers correctly on escalation phrases
- The **Plan Comparison Assistant** agent exists in Copilot Studio and is grounded on tables you identified in Task 7 and the **Work Order** table.
- You can articulate at least one thing the plan predicted correctly, one divergence, and one thing you built beyond the plan
