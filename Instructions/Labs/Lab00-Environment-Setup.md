---
lab:
    title: 'Set up your lab environment'
    description: 'Activate your Power Platform trial and configure your environment before starting the course labs'
    duration: '20 minutes'
    level: 100
    islab: true
---

# Set up your lab environment

In this exercise, you activate your Power Platform trial and verify that your environment is ready for the hands-on labs in this course.

This exercise should take approximately **20** minutes to complete.

## About Contoso Field Services

Throughout this course, all labs follow a single, connected scenario based on a fictional company called **Contoso Field Services**.

Contoso Field Services installs and repairs industrial equipment for business customers across the region. When a customer's equipment fails — a conveyor belt stops mid-shift, a hydraulic press loses pressure, a cooling system goes offline — they need help fast.

Today, Contoso's process is entirely manual:

- Customers call or email to report issues
- Agents record requests in spreadsheets
- Managers assign technicians by phone
- Technicians receive job details verbally or by text
- Customers have no way to check their request status without calling back

Over the course of these labs, you'll build a complete Power Platform solution that transforms this process. By the end of Day 3, Contoso will have:

- A **Dataverse data model** to store and manage Work Orders
- A **canvas app** for field technicians to view and update their jobs on mobile
- A **model-driven app** for service managers to track and prioritize all open requests
- A **customer portal** built with Power Pages where customers can submit requests and check status online
- **Automated flows** that notify technicians when they're assigned and route critical requests through a manager approval
- An **AI-powered assistant** that lets agents query Contoso's service history in natural language

Every table, app, flow, and automation you build will be part of this single solution — so what you create in one lab is used in the next.

## Before you start

You will be provided with a **Microsoft 365 license** by your Authorized Lab Host (ALH). This gives you a work email address in the format `username@domain.onmicrosoft.com`. You will use this account as your identity for all Power Platform services throughout the course.

> [!NOTE]
> Do not use a personal Microsoft account for these labs. The Microsoft 365 account provided by your ALH includes the organizational permissions required to create Dataverse environments and access Power Platform features.

## Task 1: Sign in to Power Apps and start your trial

1. Open a browser and go to [**Power Apps**](https://make.powerapps.com) at `https://make.powerapps.com`.

1. Sign in using the Microsoft 365 credentials provided by your Authorized Lab Host.

1. If prompted to choose an account type, select **Work or school account**.

1. Once signed in, look for the **Build apps with Dataverse** section on the home screen. Under that header, find the environment selector and select **Dev One** from the list.

1. If prompted to **Start a free trial** or **Try Power Apps for free**, select it. This activates a 30-day Power Apps Premium trial on your account.

1. When prompted, select your **country/region** and select **Get started**.

1. Confirm that the environment picker in the top-right corner now shows **Dev One**. This is the environment you will use throughout all labs in this course.

> [!NOTE]
> Dev One includes Microsoft Dataverse, which is required for all labs in this course. If you don't see Dev One in the environment list, let your instructor know.

## Task 2: Verify your Power Automate access

1. Open a new browser tab and go to [**Power Automate**](https://make.powerautomate.com) at `https://make.powerautomate.com`.

1. Sign in with the same Microsoft 365 credentials.

1. Confirm you are in the **Dev One** environment (check the environment picker in the top-right). If it shows a different environment, select it and switch to **Dev One**.

1. In the left navigation, select **+ Create**. Confirm you can see **Automated cloud flow** as an option.

> [!NOTE]
> Labs 8 and 9 use the Dataverse connector in Power Automate, which is a premium connector. Your Power Apps trial includes premium Power Automate capabilities. If you see a message about upgrading your plan when building flows, let your instructor know.

## Task 3: Start your Power Pages trial

Power Pages is a separate product from Power Apps and requires its own trial activation. You will need this for Lab 7 (Day 3).

1. Open a new browser tab and go to [**Power Pages**](https://make.powerpages.microsoft.com) at `https://make.powerpages.microsoft.com`.

1. Sign in with the same Microsoft 365 credentials.

1. If prompted to start a trial, select **Get started** and follow the prompts.

1. Confirm that the Power Pages home screen loads and shows the option to create a new site.

> [!NOTE]
> You don't need to create a site now — just confirm that your trial is active. You'll build the Contoso customer portal in Lab 7.

## Task 4: Verify AI Builder access

AI Builder is used in Lab 10 to create a grounded prompt that queries Contoso's service history. AI Builder requires credits, which are included with your Power Platform trial.

1. Return to [Power Apps](https://make.powerapps.com) at `https://make.powerapps.com` and confirm you are in the **Dev One** environment.

1. In the left navigation, select **AI hub**.

1. Confirm that the AI hub loads and you can see options for **Prompts**, **AIModels**, and other AI Builder features.

1. If you see a message about AI Builder credits or a prompt to start a trial, select **Try AI Builder** or **Start trial** to activate your AI Builder credits.

> [!NOTE]
> AI Builder credits are consumed each time a prompt is run or a model is used. Use the **Test** feature thoughtfully during Lab 10 to avoid depleting your trial credits before completing the exercise.

## You're ready

Once you've completed all four tasks, you are ready to begin Lab 1. Keep your browser open with Power Apps and Power Automate tabs active — you'll use them throughout the course.

Your instructor will confirm that everyone's environment is set up before the first lab begins.
