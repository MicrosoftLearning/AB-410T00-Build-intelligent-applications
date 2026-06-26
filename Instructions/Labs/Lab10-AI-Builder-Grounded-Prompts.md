---
lab:
    title: 'Query Contoso service history with AI Builder grounded prompts'
    description: 'Create an AI Builder prompt grounded in Dataverse data that lets Contoso service agents ask natural language questions about customer service history, and configure a row summary to surface AI-generated record summaries in the model-driven app'
    duration: '45 minutes'
    level: 400
    islab: true
---

# Query Contoso service history with AI Builder grounded prompts

In this exercise, you create an AI Builder grounded prompt that uses the Contoso Work Order table as a knowledge source, allowing service agents to ask natural language questions and receive AI-generated responses based on real business data. You also configure a row summary so Copilot can generate an instant AI summary of any Work Order record in the model-driven app.

This exercise should take approximately **45** minutes to complete.

## Scenario

Contoso service agents frequently need to look up a customer's service history before responding to a new inquiry — questions like "Has this customer had this problem before?" or "What was done to resolve their last Critical request?" Currently, agents search through the model-driven app manually, which is slow.

You'll explore two different ways to surface AI-generated insights from Contoso's Work Order data:

- **A standalone grounded prompt** — an interactive prompt that searches across all Work Order records and answers natural language questions in real time. Agents can run it on demand to look up a customer's history.
- **A row summary** — a Copilot configuration on the Work Order table that tells the model-driven app which fields to use when generating an on-demand AI summary of a record. Agents can open any Work Order and instantly see a concise summary in the Copilot panel, with no manual action required.

Both patterns are useful in different situations. The standalone prompt is flexible and query-driven. The row summary is always available and requires no extra steps from the agent.

## Task 1: Open AI Builder and create a new prompt

1. Open [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com` and sign in with your Microsoft account.

1. Confirm you are in your training environment.

1. In the left navigation, select **AI hub**.

> [!NOTE]
> If you don't see **AI hub**, look for **AI Builder** in the left navigation instead. The entry point may vary depending on your environment.

1. Select **Prompts** from the AI hub menu.

1. Select **Build your own prompt**.

1. Name the prompt `Contoso Service History Assistant`.

## Task 2: Write the prompt instruction

The prompt instruction tells the AI what it should do and how it should behave. This is similar to the system prompt you wrote in Lab 1.

1. In the **Prompt** field, enter the following instruction:

    ```
    You are a helpful service assistant for Contoso Field Services. You have access to the company's Work Order records. When an agent describes a customer situation or asks about a customer's history, use the available Work Order data to provide a clear, accurate, and concise response.

    If a customer has had similar issues before, summarize the previous requests and what resolved them. If no matching history is found, say so clearly and suggest the agent create a new request.

    Always respond in a professional tone suitable for internal use by service agents.
    ```

1. Select **Save** to save the prompt instruction.

## Task 3: Add the Work Order table as a data source

Grounding the prompt in Dataverse data means the AI will base its answers on actual records, not just general knowledge.

1. In the prompt editor, select **+ Add content**.

1. Select **Dataverse** as the data source type.

1. Search for and select the **Work Order** table.

1. Configure which columns the AI can use. Select the following fields:
    - Customer Name
    - Issue Description
    - Priority
    - Request Status
    - Resolved Date

1. Select **Add**.

> [!NOTE]
>  Grounding limits the AI's responses to information from your selected data source. This is critical for accuracy in business scenarios — you want the AI to answer based on Contoso's actual data, not make up plausible-sounding answers.

## Task 4: Add an input variable

Input variables let you pass dynamic values into the prompt at runtime — for example, the name of the customer the agent is asking about.

1. In the **Instructions** panel, place your cursor at the end of the prompt text and type `/`. A dropdown menu appears.

1. Under the **Input** section of the dropdown, select **Text**. A text input placeholder is inserted inline into your prompt.

1. Select the input placeholder that appears and rename it from **Text input** to `CustomerName`.

1. Optionally, add sample data to the input by selecting it and entering a test value such as `Fabrikam Industries`. This is used when you test the prompt.

1. Ensure the end of your prompt instruction reads:

    `Focus your response on Work Orders for the customer named: {CustomerName}`

> [!NOTE]
> The input variable appears as a pill in the instructions panel but is referenced as `{CustomerName}` in the prompt text. At runtime, the value entered by the user replaces this placeholder.

1. Select **Save**.

## Task 5: Test the prompt

1. In the prompt editor, select **Test**.

1. Review the response. The AI should reference any Work Order records linked to Fabrikam Industries and summarize the relevant history.

1. Test with another customer name by selecting the **CustomerName** input pill, changing the sample data to `Northwind Traders`, and selecting **Test** again.

> [!NOTE]
> The quality of grounded responses depends on how much data exists in the table. In a production environment with hundreds of records, the AI's responses become significantly more useful.

## Task 6: Configure a row summary for the Work Order table

The standalone prompt you built in Tasks 1–5 is interactive — an agent runs it on demand and asks questions. Now you'll configure a **row summary** for the Work Order table. A row summary tells Copilot in model-driven apps which fields to use when generating an AI-powered summary of a record, shown automatically when an agent opens a Work Order.

1. Open [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com` and navigate to **Tables** > **Work Order**.

1. Select **Row summary** in the Customizations section.

1. In **Row summary,** a prompt will open. It will say *Summarize Work Order with*.

1. Place your cursor after *with* and select **+ Add data**. Select **Work Order** and add the following fields as Knowledge:
    - Customer Name
    - Issue Description
    - Priority
    - Request Status
    - Resolved Date

> [!NOTE]
> The row summary uses these fields to generate a concise AI summary whenever Copilot summarizes a Work Order record. Unlike a prompt column, it does not store text on the record — the summary is generated on demand when an agent views the record.

1. Select **Test**.

1. Select **Apply to main forms.**

## Task 7: View the row summary in the model-driven app

1. Open the **Contoso Service Management** model-driven app.

1. Navigate to a **Work Order** record that has values in the **Customer Name**, **Issue Description**, **Priority**, and **Request Status** fields.

1. Copilot displays an AI-generated summary of the record based on the columns you configured. The summary updates each time you open a record with no manual action required from the agent.
