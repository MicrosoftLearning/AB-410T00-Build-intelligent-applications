---
lab:
    title: 'Write and test prompts for Contoso Field Services'
    description: 'Practice writing and refining effective prompts for generative AI tools to support Contoso service agents'
    duration: '15 minutes'
    level: 200
    islab: true
---

# Write and test prompts for Contoso Field Services

In this exercise, you practice prompt engineering techniques by writing and refining prompts that help Contoso Field Services agents respond to customer inquiries faster and more consistently.

This exercise should take approximately **15** minutes to complete.

## Scenario

Contoso Field Services installs and repairs industrial equipment for business customers across the region. The service team handles dozens of customer calls every day — questions about request status, troubleshooting guidance, and next steps while waiting for a technician.

Agents currently answer these questions manually, searching through documentation and past cases. Your task is to write prompts that help agents use generative AI to generate accurate, professional, and helpful responses instantly.

## Task 1: Write a basic prompt

First, you write a simple prompt and observe how specificity affects the quality of the response.

1. Open [**Microsoft Copilot**](https://copilot.microsoft.com) at `https://copilot.microsoft.com` and sign in with your Microsoft account.

1. Select your **Work** account if prompted.

1. Close any welcome messages that appear.

1. In the chat box, enter the following prompt and press **Enter**:

    `A customer's equipment isn't working. What should they do?`

1. Review the response. Notice that the AI gives a generic answer — it has no context about the company, the type of equipment, or who is asking.

1. Now enter this more specific version:

    `A Contoso Field Services customer's industrial pump has stopped working mid-shift. What immediate steps should a service agent tell the customer to take while waiting for a technician?`

1. Compare the two responses. Note how adding the company name, the product type, the audience (service agent), and the situation (waiting for a technician) produces a significantly more useful answer.

   > [!NOTE]
   > The quality of an AI response is directly shaped by the quality of the prompt. Specificity is the most immediate way to improve output.

## Task 2: Set a persona and context

Next, you give the AI a defined role and background context before asking your question. This is sometimes called a *system prompt*.

1. Start a new conversation in Copilot by selecting **New chat**.

1. Enter the following prompt, which establishes who the AI is and what it knows before you ask anything:

    ```
    You are a helpful support assistant for Contoso Field Services, a company that installs and repairs industrial equipment for business customers. You help service agents respond to customer inquiries in a professional, empathetic, and concise tone. Always encourage the customer to avoid operating affected equipment until a technician has assessed it, and confirm that Contoso will follow up within 4 business hours.

    A customer calls to say their conveyor belt system stopped working during a production shift. What should the agent say?
    ```

1. Review the response. Note how the persona instruction shapes the tone, the detail, and the closing statement.

1. Ask a follow-up question in the same conversation — without repeating the persona:

    `The customer says the system tripped a safety shutoff. What should the agent advise now?`

1. Notice that Copilot maintains the Contoso context throughout the conversation without you needing to repeat it.

## Task 3: Constrain the output format

Now you practice controlling the length, format, and audience of the response.

1. Enter the following prompt, which adds specific output constraints:

    ```
    You are a Contoso Field Services support assistant.

    A customer reports that their refrigeration unit is making unusual noises and the temperature inside is rising. Write a response the agent can send to the customer. The response must:
    - Be no longer than 3 sentences
    - Acknowledge the issue
    - Give one immediate action the customer can take
    - Confirm that a technician will follow up
    ```

1. Review how the constraints shaped the output.

1. Modify the prompt to request the response in a **formal email format** instead of a short message, and submit it again. Observe how the output changes.

1. Modify the prompt once more to request the response be written for a **non-technical customer** who is unfamiliar with equipment terminology.

   > [!NOTE]
   > Adding constraints — length, format, tone, and audience — helps ensure AI output is ready to use in a real business context, not just technically accurate.

## Task 4: Use few-shot examples

Few-shot prompting gives the AI examples of the style and format you want before asking your real question.

1. Enter the following prompt:

    ```
    You are a Contoso Field Services support assistant. Use the examples below to guide your tone and format.

    Example 1:
    Customer: My air compressor keeps shutting off.
    Agent: Thank you for contacting Contoso Field Services. This can sometimes be caused by overheating. Please ensure the unit has adequate ventilation and check that the air filter is clean. A technician will follow up with you within 4 business hours.

    Example 2:
    Customer: The control panel on my equipment is showing an error code.
    Agent: Thank you for reaching out. Please note the error code displayed and avoid operating the equipment until our technician has assessed it. We will be in contact shortly to arrange a visit.

    Now respond to this inquiry:
    Customer: My hydraulic press is leaking fluid and I cannot stop production.
    ```

1. Compare the style of this response to your earlier attempts. Notice how the examples guided the format and closing statement.

1. Replace the final customer message with a different scenario of your choosing and resubmit. Observe how the AI applies the same style to the new situation.

   > [!NOTE]
   > Few-shot prompting is especially useful when you need consistent output across many interactions — for example, when building a Copilot Studio topic or an AI Builder prompt action that many agents will use.

## Summary

In this exercise, you practiced four core prompt engineering techniques:

| Technique | What it does |
|-----------|-------------|
| **Specificity** | Adding context about the company, product, and role improves relevance |
| **Persona and context** | A system-level instruction sets the AI's tone and behavior across the conversation |
| **Output constraints** | Controlling length, format, and audience makes responses immediately usable |
| **Few-shot examples** | Demonstrating the desired style produces more consistent output |

These same techniques apply directly to prompts you build in Power Platform — including Copilot Studio topics and AI Builder prompt actions — which you explore in later exercises in this course.
