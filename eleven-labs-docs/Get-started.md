---
title: Conversational AI overview
headline: Introduction - Conversational voice AI agents
subtitle: 'Deploy customized, conversational voice agents in minutes.'
---

<div style={{ position: 'relative', width: '100%', paddingBottom: '56.25%' }}>
  <iframe
    src="https://player.vimeo.com/video/1029660636"
    frameBorder="0"
    style={{ position: 'absolute', top: '0', left: '0', width: '100%', height: '100%' }}
    className="aspect-video w-full rounded-lg"
    allow="autoplay; fullscreen; picture-in-picture"
    allowFullScreen
  />
</div>

## What is Conversational AI?

ElevenLabs [Conversational AI](https://elevenlabs.io/conversational-ai) is a platform for deploying customized, conversational voice agents. Built in response to our customers' needs, our platform eliminates months of development time typically spent building conversation stacks from scratch. It combines these building blocks:

<CardGroup cols={2}>
  <Card title="Speech to text">
    Our fine tuned ASR model that transcribes the caller's dialogue.
  </Card>
  <Card title="Language model">
    Choose from Gemini, Claude, OpenAI and more, or bring your own.
  </Card>
  <Card title="Text to speech">
    Our low latency, human-like TTS across 5k+ voices and 31 languages.
  </Card>
  <Card title="Turn taking model">
    Our custom turn taking model that understands when to speak, like a human would.
  </Card>
</CardGroup>

Altogether it is a highly composable AI Voice agent solution that can scale to thousands of calls per day. With [server](/docs/conversational-ai/customization/tools/server-tools) & [client side](/docs/conversational-ai/customization/tools/client-tools) tools, [knowledge](/docs/conversational-ai/customization/knowledge-base) bases, [dynamic](/docs/conversational-ai/customization/personalization/dynamic-variables) agent instantiation and [overrides](/docs/conversational-ai/customization/personalization/overrides), plus built-in monitoring, it's the complete developer toolkit.

<Card title="Pricing" horizontal>
  15 minutes to get started on the free plan. Get 13,750 minutes included on the Business plan at
  \$0.08 per minute on the Business plan, with extra minutes billed at \$0.08, as well as
  significantly discounted pricing at higher volumes.
  <br />
  **Setup & Prompt Testing**: billed at half the cost.
</Card>

<Note>
  Usage is billed to the account that created the agent. If authentication is not enabled, anybody
  with your agent's id can connect to it and consume your credits. To protect against this, either
  enable authentication for your agent or handle the agent id as a secret.
</Note>

## Pricing tiers

<Tabs>
  <Tab title="In Minutes">
  
  | Tier     | Price   | Minutes included | Cost per extra minute              |
  | -------- | ------- | ---------------- | ---------------------------------- |
  | Free     | \$0     | 15               | Unavailable                        |
  | Starter  | \$5     | 50               | Unavailable                        |
  | Creator  | \$22    | 250              | ~\$0.12                            |
  | Pro      | \$99    | 1100             | ~\$0.11                            |
  | Scale    | \$330   | 3,600            | ~\$0.10                            |
  | Business | \$1,320 | 13,750           | \$0.08 (annual), \$0.096 (monthly) |

  </Tab>
  <Tab title="In Credits">
  
  | Tier     | Price   | Credits included | Cost in credits per extra minute |
  | -------- | ------- | ---------------- | -------------------------------- |
  | Free     | \$0     | 10,000           | Unavailable                      |
  | Starter  | \$5     | 30,000           | Unavailable                      |
  | Creator  | \$22    | 100,000          | 400                              |
  | Pro      | \$99    | 500,000          | 454                              |
  | Scale    | \$330   | 2,000,000        | 555                              |
  | Business | \$1,320 | 11,000,000       | 800                              |

  </Tab>
</Tabs>

In multimodal text + voice mode, text message pricing per message. LLM costs are passed through separately, see here for estimates of [LLM cost](/docs/conversational-ai/customization/llm#supported-llms).

| Plan       | Price per text message |
| ---------- | ---------------------- |
| Free       | 0.4 cents              |
| Starter    | 0.4 cents              |
| Creator    | 0.3 cents              |
| Pro        | 0.3 cents              |
| Scale      | 0.3 cents              |
| Business   | 0.3 cents              |
| Enterprise | Custom pricing         |

### Pricing during silent periods

When a conversation is silent for longer than ten seconds, ElevenLabs reduces the inference of the turn-taking model and speech-to-text services until voice activity is detected again. This optimization means that extended periods of silence are charged at 5% of the usual per-minute cost.

This reduction in cost:

- Only applies to the period of silence.
- Does not apply after voice activity is detected again.
- Can be triggered at multiple times in the same conversation.

## Models

Currently, the following models are natively supported and can be configured via the agent settings:

| Provider      | Model                 |
| ------------- | --------------------- |
| **Google**    | Gemini 2.5 Flash      |
|               | Gemini 2.0 Flash      |
|               | Gemini 2.0 Flash Lite |
|               | Gemini 1.5 Flash      |
|               | Gemini 1.5 Pro        |
| **OpenAI**    | GPT-4.1               |
|               | GPT-4.1 Mini          |
|               | GPT-4.1 Nano          |
|               | GPT-4o                |
|               | GPT-4o Mini           |
|               | GPT-4 Turbo           |
|               | GPT-4                 |
|               | GPT-3.5 Turbo         |
| **Anthropic** | Claude Sonnet 4       |
|               | Claude 3.5 Sonnet     |
|               | Claude 3.5 Sonnet v1  |
|               | Claude 3.7 Sonnet     |
|               | Claude 3.0 Haiku      |

Using your own Custom LLM is also supported by specifying the endpoint we should make requests to and providing credentials through our secure secret storage.

<Note>
  With EU data residency enabled, a small number of older Gemini and Claude LLMs are not available
  in Conversational AI to maintain compliance with EU data residency. Custom LLMs and OpenAI LLMs
  remain fully available. For more infomation please see [GDPR and data
  residency](/docs/product-guides/administration/data-residency).
</Note>

![Supported models](file:2f402037-cd90-4126-984e-e9b5a467463b)

You can start with our [free tier](https://elevenlabs.io/app/sign-up), which includes 15 minutes of conversation per month.

Need more? Upgrade to a [paid plan](https://elevenlabs.io/pricing/api) instantly - no sales calls required. For enterprise usage (6+ hours of daily conversation), [contact our sales team](https://elevenlabs.io/contact-sales) for custom pricing tailored to your needs.

## Popular applications

Companies and creators use our Conversational AI orchestration platform to create:

- **Customer service**: Assistants trained on company documentation that can handle customer queries, troubleshoot issues, and provide 24/7 support in multiple languages.
- **Virtual assistants**: Assistants trained to manage scheduling, set reminders, look up information, and help users stay organized throughout their day.
- **Retail support**: Assistants that help customers find products, provide personalized recommendations, track orders, and answer product-specific questions.
- **Personalized learning**: Assistants that help students learn new topics & enhance reading comprehension by speaking with books and [articles](https://elevenlabs.io/blog/time-brings-conversational-ai-to-journalism).
- **Multi-character storytelling**: Interactive narratives with distinct voices for different characters, powered by our new [multi-voice support](/docs/conversational-ai/customization/voice/multi-voice-support) feature.

<Note>
  Ready to get started? Check out our [quickstart guide](/docs/conversational-ai/quickstart) to
  create your first AI agent in minutes.
</Note>

## FAQ

<AccordionGroup>
  <Accordion title="Concurrency limits">
Plan limits

Your subscription plan determines how many calls can be made simultaneously.

| Plan       | Concurrency limit |
| ---------- | ----------------- |
| Free       | 4                 |
| Starter    | 6                 |
| Creator    | 10                |
| Pro        | 20                |
| Scale      | 30                |
| Business   | 30                |
| Enterprise | Elevated          |

    <Note>
      To increase your concurrency limit [upgrade your subscription plan](https://elevenlabs.io/pricing/api)
      or [contact sales](https://elevenlabs.io/contact-sales) to discuss enterprise plans.
    </Note>

  </Accordion>
  <Accordion title="Supported audio formats">
    The following audio output formats are supported in the Conversational AI platform:

    - PCM (8 kHz / 16 kHz / 22.05 kHz / 24 kHz / 44.1 kHz)
    - μ-law 8000Hz

  </Accordion>
</AccordionGroup>



---
title: Quickstart
subtitle: Build your first conversational AI voice agent in 5 minutes.
---

In this guide, you'll learn how to create your first Conversational AI voice agent. This will serve as a foundation for building conversational workflows tailored to your business use cases.

## Getting started

Conversational AI agents are managed through the [ElevenLabs dashboard](https://elevenlabs.io/app/conversational-ai). This is used to:

- Create and manage AI assistants
- Configure voice settings and conversation parameters
- Equip the agent with [tools](/docs/conversational-ai/customization/tools) and a [knowledge base](/docs/conversational-ai/customization/knowledge-base)
- Review conversation analytics and transcripts
- Manage API keys and integration settings

<Note>
  The web dashboard uses our [Web SDK](/docs/conversational-ai/libraries/react) under the hood to
  handle real-time conversations.
</Note>

<Tabs>
  <Tab title="Build a support agent">
    ## Overview
    
    In this guide, we'll create a conversational support assistant capable of answering questions about your product, documentation, or service. This assistant can be embedded into your website or app to provide real-time support to your customers.
    
    <Frame
      caption="The assistant at the bottom right corner of this page is capable of answering questions about ElevenLabs, navigating pages & taking you to external resources."
      background="subtle"
    >
      ![Conversational AI widget](file:aae5a0cb-cc44-43cc-885d-1cac73461838)
    </Frame>
    
    ### Prerequisites
    
    - An [ElevenLabs account](https://www.elevenlabs.io)
    
    ### Assistant setup
    
    <Steps>
      <Step title="Sign in to ElevenLabs">
        Go to [elevenlabs.io](https://elevenlabs.io/sign-up) and sign in to your account.
      </Step>
      <Step title="Create a new assistant">
        In the **ElevenLabs Dashboard**, create a new assistant by entering a name and selecting the `Blank template` option.
        <Frame caption="Creating a new assistant" background="subtle">
          ![Dashboard](file:2aaef0bc-441e-490c-83d7-1b823812cbdd)
        </Frame>
      </Step>
      <Step title="Configure the assistant behavior">
       Go to the **Agent** tab to configure the assistant's behavior. Set the following:
        <Steps>
          <Step title="First message">
            This is the first message the assistant will speak out loud when a user starts a conversation.
    
            ```plaintext First message
            Hi, this is Alexis from <company name> support. How can I help you today?
            ```
          </Step>
          <Step title="System prompt">
            This prompt guides the assistant's behavior, tasks, and personality.
    
            Customize the following example with your company details:
            ```plaintext System prompt
            You are a friendly and efficient virtual assistant for [Your Company Name]. Your role is to assist customers by answering questions about the company's products, services, and documentation. You should use the provided knowledge base to offer accurate and helpful responses.
    
            Tasks:
            - Answer Questions: Provide clear and concise answers based on the available information.
            - Clarify Unclear Requests: Politely ask for more details if the customer's question is not clear.
    
            Guidelines:
            - Maintain a friendly and professional tone throughout the conversation.
            - Be patient and attentive to the customer's needs.
            - If unsure about any information, politely ask the customer to repeat or clarify.
            - Avoid discussing topics unrelated to the company's products or services.
            - Aim to provide concise answers. Limit responses to a couple of sentences and let the user guide you on where to provide more detail.
            ```
          </Step>
        </Steps>
    
      </Step>
      <Step title="Add a knowledge base">
        Go to the **Knowledge Base** section to provide your assistant with context about your business. 
        
        This is where you can upload relevant documents & links to external resources:
    
        - Include documentation, FAQs, and other resources to help the assistant respond to customer inquiries.
        - Keep the knowledge base up-to-date to ensure the assistant provides accurate and current information.
    
      </Step>
    </Steps>
    
    ### Configure the voice
    
    <Steps>
      <Step title="Select a voice">
        In the **Voice** tab, choose a voice that best matches your assistant from the [voice library](https://elevenlabs.io/community):
        <Frame background="subtle">
          ![Voice settings](file:382819e3-a8bc-43d9-b3ae-fd36ca4b3157)
        </Frame>
       <Note> Using higher quality voices, models, and LLMs may increase response time. For an optimal customer experience, balance quality and latency based on your assistant's expected use case.</Note>
    
      </Step>
      <Step title="Testing your assistant">
         Press the **Test AI agent** button and try conversing with your assistant.
      </Step>
    </Steps>
    
    ### Analyze and collect conversation data
    
    Configure evaluation criteria and data collection to analyze conversations and improve your assistant's performance.
    
    <Steps>
      <Step title="Configure evaluation criteria">
        Navigate to the **Analysis** tab in your assistant's settings to define custom criteria for evaluating conversations.
    
        <Frame background="subtle">
          ![Analysis settings](file:f262a2e8-f9b8-4730-8292-d7dba1f0711c)
        </Frame>
    
        Every conversation transcript is passed to the LLM to verify if specific goals were met. Results will either be `success`, `failure`, or `unknown`, along with a rationale explaining the chosen result.
    
        Let's add an evaluation criteria with the name `solved_user_inquiry`:
    
        ```plaintext Prompt
        The assistant was able to answer all of the queries or redirect them to a relevant support channel.
    
        Success Criteria:
        - All user queries were answered satisfactorily.
        - The user was redirected to a relevant support channel if needed.
        ```
    
      </Step>
    
      <Step title="Configure data collection">
        In the **Data Collection** section, configure details to be extracted from each conversation.
    
        Click **Add item** and configure the following:
    
        1. **Data type:** Select "string"
        2. **Identifier:** Enter a unique identifier for this data point: `user_question`
        3. **Description:** Provide detailed instructions for the LLM about how to extract the specific data from the transcript:
    
        ```plaintext Prompt
        Extract the user's questions & inquiries from the conversation.
        ```
        <Tip>Test your assistant by posing as a customer. Ask questions, evaluate its responses, and tweak the prompts until you're happy with how it performs.</Tip>
    
      </Step>
      <Step title="View conversation history">
        View evaluation results and collected data for each conversation in the **Call history** tab.
        <Frame background="subtle">
          ![Conversation history](file:f1587272-4dcc-48bd-b84e-e5b9f2bfe72f)
        </Frame>
        <Tip>Regularly review conversation history to identify common issues and patterns.</Tip>
      </Step>
    </Steps>
    
    Your assistant is now configured. Embed the widget on your website to start providing real-time support to your customers.
    
  </Tab>
  <Tab title="Build a restaurant ordering agent">
    ## Overview
    
    In this guide, we’ll create a conversational ordering assistant for Pierogi Palace, a Polish restaurant that takes food orders, addressing their challenge of managing high call volumes.
    
    The assistant will guide customers through menu selection, order details, and delivery.
    
    ### Prerequisites
    
    - An [ElevenLabs account](https://www.elevenlabs.io)
    
    ### Assistant setup
    
    <Steps>
      <Step title="Sign in to ElevenLabs">
        Go to [elevenlabs.io](https://elevenlabs.io/sign-up) and sign in to your account.
      </Step>
      <Step title="Create a new assistant">
        In the **ElevenLabs Dashboard**, create a new assistant by entering a name and selecting the `Blank template` option.
        <Frame caption="Creating a new assistant" background="subtle">
          ![Dashboard](file:2aaef0bc-441e-490c-83d7-1b823812cbdd)
        </Frame>
      </Step>
      <Step title="Configure the assistant behavior">
       Go to the **Agent** tab to configure the assistant's behavior. Set the following:
        <Steps>
          <Step title="First message">
            This is the first message the assistant will speak out loud when a user starts a conversation.
    
            ```plaintext First message
            Welcome to Pierogi Palace! I'm here to help you place your order. What can I get started for you today?
            ```
          </Step>
          <Step title="System prompt">
            This prompt guides the assistant's behavior, tasks, and personality:
    
            ```plaintext System prompt
            You are a friendly and efficient virtual assistant for Pierogi Palace, a modern Polish restaurant specializing in pierogi. It is located in the Zakopane mountains in Poland.
            Your role is to help customers place orders over voice conversations. You have comprehensive knowledge of the menu items and their prices.
    
            Menu Items:
    
            - Potato & Cheese Pierogi – 30 Polish złoty per dozen
            - Beef & Onion Pierogi – 40 Polish złoty per dozen
            - Spinach & Feta Pierogi – 30 Polish złoty per dozen
    
            Your Tasks:
    
            1. Greet the Customer: Start with a warm welcome and ask how you can assist.
            2. Take the Order: Listen carefully to the customer's selection, confirm the type and quantity of pierogi.
            3. Confirm Order Details: Repeat the order back to the customer for confirmation.
            4. Calculate Total Price: Compute the total cost based on the items ordered.
            5. Collect Delivery Information: Ask for the customer's delivery address to estimate delivery time.
            6. Estimate Delivery Time: Inform the customer that cooking time is 10 minutes plus delivery time based on their location.
            7. Provide Order Summary: Give the customer a summary of their order, total price, and estimated delivery time.
            8. Close the Conversation: Thank the customer and let them know their order is being prepared.
    
            Guidelines:
    
            - Use a friendly and professional tone throughout the conversation.
            - Be patient and attentive to the customer's needs.
            - If unsure about any information, politely ask the customer to repeat or clarify.
            - Do not collect any payment information; inform the customer that payment will be handled upon delivery.
            - Avoid discussing topics unrelated to taking and managing the order.
            ```
          </Step>
        </Steps>
    
      </Step>
    </Steps>
    
    ### Configure the voice
    
    <Steps>
      <Step title="Select a voice">
        In the **Voice** tab, choose a voice that best matches your assistant from the [voice library](https://elevenlabs.io/community):
        <Frame background="subtle">
          ![Voice settings](file:382819e3-a8bc-43d9-b3ae-fd36ca4b3157)
        </Frame>
       <Note> Using higher quality voices, models, and LLMs may increase response time. For an optimal customer experience, balance quality and latency based on your assistant's expected use case.</Note>
    
      </Step>
      <Step title="Testing your assistant">
         Press the **Test AI agent** button and try ordering some pierogi.
      </Step>
    </Steps>
    
    ### Analyze and collect conversation data
    
    Configure evaluation criteria and data collection to analyze conversations and improve your assistant's performance.
    
    <Steps>
      <Step title="Configure evaluation criteria">
        Navigate to the **Analysis** tab in your assistant's settings to define custom criteria for evaluating conversations.
    
        <Frame background="subtle">
          ![Analysis settings](file:f262a2e8-f9b8-4730-8292-d7dba1f0711c)
        </Frame>
    
        Every conversation transcript is passed to the LLM to verify if specific goals were met. Results will either be `success`, `failure`, or `unknown`, along with a rationale explaining the chosen result.
    
        Let's add an evaluation criteria with the name `order_completion`:
    
        ```plaintext Prompt
        Evaluate if the conversation resulted in a successful order.
        Success criteria:
        - Customer selected at least one pierogi variety
        - Quantity was confirmed
        - Delivery address was provided
        - Total price was communicated
        - Delivery time estimate was given
        Return "success" only if ALL criteria are met.
        ```
    
      </Step>
    
      <Step title="Configure data collection">
        In the **Data Collection** section, configure details to be extracted from each conversation.
    
        Click **Add item** and configure the following:
    
        1. **Data type:** Select "string"
        2. **Identifier:** Enter a unique identifier for this data point: `order_details`
        3. **Description:** Provide detailed instructions for the LLM about how to extract the specific data from the transcript:
    
        ```plaintext Prompt
        Extract order details from the conversation, including:
        - Type of order (delivery, pickup, inquiry_only)
        - List of pierogi varieties and quantities ordered in the format: "item: quantity"
        - Delivery zone based on the address (central_zakopane, outer_zakopane, outside_delivery_zone)
        - Interaction type (completed_order, abandoned_order, menu_inquiry, general_inquiry)
        If no order was placed, return "none"
        ```
        <Tip>Test your assistant by posing as a customer. Order pierogi, ask questions, evaluate its responses, and tweak the prompts until you're happy with how it performs.</Tip>
    
      </Step>
      <Step title="View conversation history">
        View evaluation results and collected data for each conversation in the **Call history** tab.
        <Frame background="subtle">
          ![Conversation history](file:f1587272-4dcc-48bd-b84e-e5b9f2bfe72f)
        </Frame>
        <Tip>Regularly review conversation history to identify common issues and patterns.</Tip>
      </Step>
    </Steps>
    
    Your assistant is now configured & ready to take orders.
    
  </Tab>
</Tabs>

## Next steps

<CardGroup cols={2}>

<Card title="Customize your agent" href="/docs/conversational-ai/customization">
  Learn how to customize your agent with tools, knowledge bases, dynamic variables and overrides.
</Card>

<Card title="Integration quickstart" href="/docs/conversational-ai/guides/quickstarts">
  Learn how to integrate Conversational AI into your app using the SDK for advanced configuration.
</Card>

</CardGroup>

---
title: Conversational AI dashboard
subtitle: Monitor and analyze your agents' performance effortlessly.
---

## Overview

The Agents Dashboard provides real-time insights into your Conversational AI agents. It displays performance metrics over customizable time periods. You can review data for individual agents or across your entire workspace.

## Analytics

You can monitor activity over various daily, weekly, and monthly time periods.

<Frame caption="Dashboard view for Last Day" background="subtle">
  <img
    src="file:1214b90f-40c8-419b-8a29-a8699cb734c9"
    alt="Dashboard view showing last day metrics"
  />
</Frame>

<Frame caption="Dashboard view for Last Month" background="subtle">
  <img
    src="file:438c49fc-d3d9-4742-adbc-b08d989425d3"
    alt="Dashboard view showing last month metrics"
  />
</Frame>

The dashboard can be toggled to show different metrics, including: number of calls, average duration, total cost, and average cost.

## Language Breakdown

A key benefit of Conversational AI is the ability to support multiple languages.
The Language Breakdown section shows the percentage of calls (overall, or per-agent) in each language.

<Frame caption="Language Breakdown" background="subtle">
  <img
    src="file:89eb7e1c-8acf-4969-80c3-1c6543a5152b"
    alt="Language breakdown showing percentage of calls in each language"
  />
</Frame>

## Active Calls

At the top left of the dashboard, the current number of active calls is displayed. This real-time counter reflects ongoing sessions for your workspace's agents, and is also accessible via the API.

