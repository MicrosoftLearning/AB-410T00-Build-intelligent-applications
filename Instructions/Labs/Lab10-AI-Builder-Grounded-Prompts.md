---
lab:
    title: 'Query Contoso service history with AI Builder grounded prompts'
    description: 'Create an AI Builder prompt grounded in Dataverse data that lets Contoso service agents ask natural language questions about customer service history'
    duration: '45 minutes'
    level: 400
    islab: true
---

# Query Contoso service history with AI Builder grounded prompts

In this exercise, you create an AI Builder grounded prompt that uses the Contoso Work Order table as a knowledge source, allowing service agents to ask natural language questions and receive AI-generated responses based on real business data.

This exercise should take approximately **45** minutes to complete.

## Scenario

Contoso service agents frequently need to look up a customer's service history before responding to a new inquiry — questions like "Has this customer had this problem before?" or "What was done to resolve their last Critical request?" Currently, agents search through the model-driven app manually, which is slow.

You'll create an AI Builder prompt that is grounded in the Work Order table. Agents can describe a situation in plain language and the AI will return an intelligent response based on actual records — without the agent needing to search or filter manually.

## Task 1: Open AI Builder and create a new prompt

1. Open [Power Apps](https://make.powerapps.com){:target="_blank"} at `https://make.powerapps.com` and sign in with your Microsoft account.

1. Confirm you are in your training environment.

1. In the left navigation, select **... More** to expand the navigation, then select **AI hub**.

    > **Note**: If you don't see **AI hub**, look for **AI Builder** in the left navigation instead. The entry point may vary depending on your environment.

1. Select **Prompts** from the AI hub menu.

1. Select **+ New prompt**.

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

Grounding the prompt in Dataverse data means the AI will base its answers on actual records — not just general knowledge.

1. In the prompt editor, look for the **Data** or **Knowledge** section (this may appear as **Add data** or **Grounding data** depending on your environment version).

1. Select **+ Add data source** or **+ Add knowledge**.

1. Select **Dataverse** as the data source type.

1. Search for and select the **Work Orders** table.

1. Configure which columns the AI can use. Select the following fields:
    - Customer Name
    - Issue Description
    - Priority
    - Request Status
    - Assigned Technician
    - Resolved Date

1. Select **Save** or **Apply**.

    > **Note**: Grounding limits the AI's responses to information from your selected data source. This is critical for accuracy in business scenarios — you want the AI to answer based on Contoso's actual data, not make up plausible-sounding answers.

## Task 4: Add an input variable

Input variables let you pass dynamic values into the prompt at runtime — for example, the name of the customer the agent is asking about.

1. In the prompt editor, select **+ Add input** or look for the **Inputs** panel.

1. Add a new input:
    - **Name**: `CustomerName`
    - **Type**: Text
    - **Description**: `The name of the customer the agent is inquiring about`

1. Modify your prompt instruction to reference this input variable. Add the following line at the end of the prompt:

    `Focus your response on Work Orders for the customer named: {CustomerName}`

    > **Note**: Input variables in AI Builder prompts are referenced using curly braces: `{VariableName}`. At runtime, the value provided by the agent replaces the variable placeholder.

1. Select **Save**.

## Task 5: Test the prompt

1. In the prompt editor, locate the **Test** panel or select **Test prompt**.

1. In the **CustomerName** input field, type `Alpine Equipment Co.`

    > **Note**: If you created a test record for Alpine Equipment Co. in Lab 6, the AI will use it. If not, the AI should indicate that no records were found for that customer.

1. Select **Run** or **Generate** to execute the prompt.

1. Review the response. The AI should reference any Work Order records linked to Alpine Equipment Co. and summarize the relevant history.

1. Test with another customer name — try `Northwind Traders` (from your Lab 9 test record) or `Fabrikam Industries` (from Lab 8).

1. Try a prompt that asks a comparative question by typing in the **Custom input** area if available:

    `Have there been any Critical priority requests from this customer before, and if so, how were they resolved?`

    > **Note**: The quality of grounded responses depends on how much data exists in the table. In a production environment with hundreds of records, the AI's responses become significantly more useful.

## Task 6: Use the prompt in the model-driven app

Now you'll make this prompt available to service agents directly inside the Contoso Service Management app from Lab 6.

1. Open [Power Apps](https://make.powerapps.com){:target="_blank"} and navigate to **Apps**.

1. Select the **Contoso Service Management** app and select **Edit**.

1. In the app designer, select the **Work Orders** form.

1. Open the form designer and add a new section to the form:
    - Select **+ Component** > **1-column section**
    - Label it `AI Service History`

1. In this section, add an **AI Builder prompt** control (look under **Components** or **AI Builder** in the insert panel).

1. Configure the control to use the **Contoso Service History Assistant** prompt.

1. Map the **CustomerName** input to the **Customer Name** field on the form.

1. Select **Save and publish**.

1. Open the app, navigate to a Work Order record, and test the AI prompt control — the agent can now query service history directly from within the record.

    > **Note**: The AI Builder prompt component in model-driven apps is a relatively new feature. If it's not available in your environment, you can still use the prompt as a standalone tool or integrate it via a Power Automate flow that calls the prompt and writes the result to a field on the record.
