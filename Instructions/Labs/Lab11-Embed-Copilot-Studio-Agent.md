---
lab:
    title: 'Embed a Copilot Studio agent in the Contoso service management app'
    description: 'Build a Copilot Studio agent grounded on the Work Order table and embed it directly in the model-driven app for service managers'
    duration: '45 minutes'
    level: 400
    islab: true
---

# Embed a Copilot Studio agent in the Contoso service management app

In this exercise, you build a Copilot Studio agent grounded on the Contoso Work Order data and embed it directly inside the model-driven app — giving service managers an AI assistant that knows the business data and can answer questions, surface insights, and guide actions without leaving the app.

This exercise should take approximately **45** minutes to complete.

## Scenario

Service managers at Contoso Field Services spend significant time manually searching for information: which technician is overloaded, which customers have open Critical requests, and what was the resolution history for a specific account. The Copilot panel you enabled in Lab 7 helps with simple queries, but managers need a more guided, conversational assistant that understands Contoso-specific context.

In this exercise, you build a custom Copilot Studio agent grounded in the Work Order table, configure it with a description and topics relevant to field service management, and embed it in the **Contoso Service Management** model-driven app.

## Task 1: Create a new agent in Copilot Studio

1. Open [Microsoft Copilot Studio](https://copilotstudio.microsoft.com){:target="_blank"} at `https://copilotstudio.microsoft.com` and sign in with your Microsoft account.

1. Confirm you are in the **Dev One** environment using the environment picker in the top-right corner.

1. In the left navigation, select **+ Create**.

1. Select **New agent**.

1. In the agent creation panel, configure the agent:
    - **Name**: `Contoso Service Assistant`
    - **Description**: `An AI assistant for Contoso service managers. Answers questions about Work Orders, technician assignments, and resolution status using live Dataverse data.`
    - **Instructions**: Enter the following:

        ```
        You are a service management assistant for Contoso Field Services. You help service managers find information about open Work Orders, technician workloads, and customer histories. Always be concise and professional. When referencing Work Orders, include the customer name, priority, and current status. If you can't find the information requested, say so clearly and suggest how the manager might find it themselves.
        ```

1. Select **Create**.

## Task 2: Ground the agent on the Work Order table

Grounding connects the agent to your Dataverse data so it can answer questions using real records rather than general knowledge.

1. In the agent designer, select **Knowledge** in the left navigation (or find the **Knowledge** tab in the agent configuration).

1. Select **+ Add knowledge**.

1. Select **Dataverse** as the knowledge source.

1. Search for and select the **Work Order** table (`contoso_workorder`).

1. Select **Add**. The agent is now grounded on your Work Order data.

    > **Note**: When grounded on a Dataverse table, the agent can retrieve and summarize records using natural language queries. It respects Dataverse security — it only returns records that the signed-in user has permission to see based on their assigned security role.

## Task 3: Test the agent in Copilot Studio

Before embedding the agent, test it to confirm it can answer questions about your data.

1. In the agent designer, select **Test** (in the top-right area) to open the test chat panel.

1. Try the following questions in the test chat:
    - `How many open Work Orders do we have?`
    - `Show me all Critical priority requests`
    - `Which requests don't have an assigned technician?`

1. Review the responses. The agent should return answers based on the records in your Work Order table.

    > **Note**: If your environment doesn't have any Work Order records yet, create a few test records in the model-driven app from Lab 7 before testing. Use different customers, priorities, and statuses to get useful responses.

1. If responses are inaccurate or overly generic, select the response and use the **Feedback** controls to help improve the agent.

## Task 4: Publish the agent

The agent must be published before it can be embedded in another app.

1. In the agent designer, select **Publish** in the top-right corner.

1. Select **Publish** in the confirmation dialog.

1. Wait for the publish to complete. You should see a confirmation that the agent is live.

    > **Note**: Publishing makes the agent available for use in other Microsoft 365 and Power Platform surfaces. You can re-publish at any time after making changes.

## Task 5: Embed the agent in the model-driven app

Now you'll add the agent as a component inside the **Contoso Service Management** model-driven app.

1. Open [Power Apps](https://make.powerapps.com){:target="_blank"} at `https://make.powerapps.com` and navigate to the **Contoso Field Services** solution.

1. Open the **Contoso Service Management** model-driven app in the app designer.

1. In the app designer, select **Settings** (the gear icon).

1. In the **Settings** panel, locate the **Copilot** or **Agent** section.

1. Under **Custom agent**, select **+ Add agent** (or **Select agent**, depending on your environment version).

1. Search for and select **Contoso Service Assistant** — the agent you published in Task 4.

1. Select **Add**, then select **Save**.

1. Select **Publish** to publish the updated app.

## Task 6: Test the embedded agent

1. Open the published **Contoso Service Management** app.

1. Look for the **Copilot** or agent icon in the top-right corner of the app. Select it to open the agent panel.

1. Try the following questions:
    - `Which customers have Critical requests right now?`
    - `What's the status of the most recent Work Order?`
    - `Are there any requests that have been open for more than a week?`

1. Confirm that the agent responds with accurate, data-driven answers based on your Work Order records.

1. Try a question outside the agent's scope, such as `What's the weather today?` and observe that the agent declines gracefully and stays on topic.

    > **Note**: The agent's behavior is shaped by the instructions you wrote in Task 1. If responses drift off-topic or are too verbose, return to Copilot Studio, refine the instructions, and republish.

## Verify your work

Before moving on, confirm the following:

- The **Contoso Service Assistant** agent exists in Copilot Studio and is published
- The agent is grounded on the **Work Order** (`contoso_workorder`) Dataverse table
- The agent is embedded in the **Contoso Service Management** model-driven app
- The agent responds to natural language questions about Work Order data from within the app
