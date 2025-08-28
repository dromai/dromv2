---
title: Knowledge base
subtitle: Enhance your conversational agent with custom knowledge.
---

**Knowledge bases** allow you to equip your agent with relevant, domain-specific information.

## Overview

A well-curated knowledge base helps your agent go beyond its pre-trained data and deliver context-aware answers.

Here are a few examples where knowledge bases can be useful:

- **Product catalogs**: Store product specifications, pricing, and other essential details.
- **HR or corporate policies**: Provide quick answers about vacation policies, employee benefits, or onboarding procedures.
- **Technical documentation**: Equip your agent with in-depth guides or API references to assist developers.
- **Customer FAQs**: Answer common inquiries consistently.

<Info>
  The agent on this page is configured with full knowledge of ElevenLabs' documentation and sitemap. Go ahead and ask it about anything about ElevenLabs.

</Info>

## Usage

Files, URLs, and text can be added to the knowledge base in the dashboard. They can also be added programmatically through our [API](https://elevenlabs.io/docs/api-reference).

<Steps>
  <Step title="File">
    Upload files in formats like PDF, TXT, DOCX, HTML, and EPUB.
    <Frame background="subtle">
      ![File upload interface showing supported formats (PDF, TXT, DOCX, HTML, EPUB) with a 21MB
      size limit](file:a31d035b-f657-46e1-8923-33255ddd0cbe)
    </Frame>
  </Step>
  <Step title="URL">
    Import URLs from sources like documentation and product pages.
    <Frame background="subtle">
      ![URL import interface where users can paste documentation
      links](file:e2364160-c6c4-4f79-9a7f-5542e12fe66c)
    </Frame>
    <Note>
      When creating a knowledge base item from a URL, we do not currently support scraping all pages
      linked to from the initial URL, or continuously updating the knowledge base over time.
      However, these features are coming soon.
    </Note>
    <Warning>Ensure you have permission to use the content from the URLs you provide</Warning>
  </Step>
  <Step title="Text">
    Manually add text to the knowledge base.
    <Frame background="subtle">
      ![Text input interface where users can name and add custom
      content](file:de359da9-e7fd-4d1f-b51d-b2ca88184e6d)
    </Frame>
  </Step>
</Steps>

## Best practices

<h4>Content quality</h4>

Provide clear, well-structured information that's relevant to your agent's purpose.

<h4>Size management</h4>

Break large documents into smaller, focused pieces for better processing.

<h4>Regular updates</h4>

Regularly review and update the agent's knowledge base to ensure the information remains current and accurate.

<h4>Identify knowledge gaps</h4>

Review conversation transcripts to identify popular topics, queries and areas where users struggle to find information. Note any knowledge gaps and add the missing context to the knowledge base.

## Enterprise features

Non-enterprise accounts have a maximum of 20MB or 300k characters.

<Info>
  Need higher limits? [Contact our sales team](https://elevenlabs.io/contact-sales) to discuss
  enterprise plans with expanded knowledge base capabilities.
</Info>
---
title: Knowledge base dashboard
subtitle: >-
  Learn how to manage and organize your knowledge base through the ElevenLabs
  dashboard
---

## Overview

The [knowledge base dashboard](https://elevenlabs.io/app/conversational-ai/knowledge-base) provides a centralized way to manage documents and track their usage across your AI agents. This guide explains how to navigate and use the knowledge base dashboard effectively.

<Frame background="subtle">
  ![Knowledge base main interface showing list of
  documents](file:ac39c8e6-3fa4-47cb-869a-b5518e8fdbfb)
</Frame>

## Adding existing documents to agents

When configuring an agent's knowledge base, you can easily add existing documents to an agent.

1. Navigate to the agent's [configuration](https://elevenlabs.io/app/conversational-ai/)
2. Click "Add document" in the knowledge base section of the "Agent" tab.
3. The option to select from your existing knowledge base documents or upload a new document will appear.

<Frame background="subtle">
  ![Interface for adding documents to an
  agent](file:7fe3c6d0-0d40-4d7e-a26d-b261cde4dba0)
</Frame>

<Tip>
  Documents can be reused across multiple agents, making it efficient to maintain consistent
  knowledge across your workspace.
</Tip>

## Document dependencies

Each document in your knowledge base includes a "Agents" tab that shows which agents currently depend on that document.

<Frame background="subtle">
  ![Dependent agents tab showing which agents use a
  document](file:9aeedb7c-4ee9-4829-a2ca-5e20c4985991)
</Frame>

It is not possible to delete a document if any agent depends on it.
---
title: Retrieval-Augmented Generation
subtitle: Enhance your agent with large knowledge bases using RAG.
---

## Overview

**Retrieval-Augmented Generation (RAG)** enables your agent to access and use large knowledge bases during conversations. Instead of loading entire documents into the context window, RAG retrieves only the most relevant information for each user query, allowing your agent to:

- Access much larger knowledge bases than would fit in a prompt
- Provide more accurate, knowledge-grounded responses
- Reduce hallucinations by referencing source material
- Scale knowledge without creating multiple specialized agents

RAG is ideal for agents that need to reference large documents, technical manuals, or extensive
knowledge bases that would exceed the context window limits of traditional prompting.
RAG adds on slight latency to the response time of your agent, around 500ms.

<iframe
  width="100%"
  height="400"
  src="https://www.youtube-nocookie.com/embed/aFeJO7W0DIk"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  allowfullscreen
></iframe>

## How RAG works

When RAG is enabled, your agent processes user queries through these steps:

1. **Query processing**: The user's question is analyzed and reformulated for optimal retrieval.
2. **Embedding generation**: The processed query is converted into a vector embedding that represents the user's question.
3. **Retrieval**: The system finds the most semantically similar content from your knowledge base.
4. **Response generation**: The agent generates a response using both the conversation context and the retrieved information.

This process ensures that relevant information to the user's query is passed to the LLM to generate a factually correct answer.

## Guide

### Prerequisites

- An [ElevenLabs account](https://elevenlabs.io)
- A configured ElevenLabs [Conversational Agent](/docs/conversational-ai/quickstart)
- At least one document added to your agent's knowledge base

<Steps>
    <Step title="Enable RAG for your agent">
        In your agent's settings, navigate to the **Knowledge Base** section and toggle on the **Use RAG** option.

        <Frame background="subtle">
        <img src="file:d0e7261c-14ab-4c45-b892-8421424bda4d" alt="Toggle switch to enable RAG in the agent settings" />
        </Frame>
    </Step>

    <Step title="Configure RAG settings (optional)">
    After enabling RAG, you'll see additional configuration options in the **Advanced** tab:
    - **Embedding model**: Select the model that will convert text into vector embeddings
    - **Maximum document chunks**: Set the maximum amount of retrieved content per query
    - **Maximum vector distance**: Set the maximum distance between the query and the retrieved chunks

    These parameters could impact latency. They also could impact LLM cost.
    For example, retrieving more chunks increases cost.
    Increasing vector distance allows for more context to be passed, but potentially less relevant context.
    This may affect quality and you should experiment with different parameters to find the best results.

    <Frame background="subtle">
        <img
        src="file:4c65b2d1-c9ab-4493-ad73-5a8b8b39c68d"
        alt="RAG configuration options including embedding model selection"
        />
    </Frame>
    </Step>

    <Step title="Knowledge base indexing">
    Each document in your knowledge base needs to be indexed before it can be used with RAG. This
    process happens automatically when a document is added to an agent with RAG enabled.
    <Info>
        Indexing may take a few minutes for large documents. You can check the indexing status in the
        knowledge base list.
    </Info>
    </Step>

    <Step title="Configure document usage modes (optional)">
        For each document in your knowledge base, you can choose how it's used:

        - **Auto (default)**: The document is only retrieved when relevant to the query
        - **Prompt**: The document is always included in the system prompt, regardless of relevance, but can also be retrieved by RAG

        <Frame background="subtle">
            <img
                src="file:1983849b-a49d-454a-908f-6360767fbccf"
                alt="Document usage mode options in the knowledge base"
            />
        </Frame>

        <Warning>
            Setting too many documents to "Prompt" mode may exceed context limits. Use this option sparingly
            for critical information.
        </Warning>
    </Step>

    <Step title="Test your RAG-enabled agent">
      After saving your configuration, test your agent by asking questions related to your knowledge base. The agent should now be able to retrieve and reference specific information from your documents.

    </Step>

</Steps>

## Usage limits

To ensure fair resource allocation, ElevenLabs enforces limits on the total size of documents that can be indexed for RAG per workspace, based on subscription tier.

The limits are as follows:

| Subscription Tier | Total Document Size Limit | Notes                                       |
| :---------------- | :------------------------ | :------------------------------------------ |
| Free              | 1MB                       | Indexes may be deleted after inactivity.    |
| Starter           | 2MB                       |                                             |
| Creator           | 20MB                      |                                             |
| Pro               | 100MB                     |                                             |
| Scale             | 500MB                     |                                             |
| Business          | 1GB                       |                                             |
| Enterprise        | Custom                    | Higher limits available based on agreement. |

**Note:**

- These limits apply to the total **original file size** of documents indexed for RAG, not the internal storage size of the RAG index itself (which can be significantly larger).
- Documents smaller than 500 bytes cannot be indexed for RAG and will automatically be used in the prompt instead.

## API implementation

You can also implement RAG through the [API](/docs/api-reference/knowledge-base/compute-rag-index):

<CodeBlocks>

```python
from elevenlabs import ElevenLabs
import time

# Initialize the ElevenLabs client
elevenlabs = ElevenLabs(api_key="your-api-key")

# First, index a document for RAG
document_id = "your-document-id"
embedding_model = "e5_mistral_7b_instruct"

# Trigger RAG indexing
response = elevenlabs.conversational_ai.knowledge_base.document.compute_rag_index(
    documentation_id=document_id,
    model=embedding_model
)

# Check indexing status
while response.status not in ["SUCCEEDED", "FAILED"]:
    time.sleep(5)  # Wait 5 seconds before checking status again
    response = elevenlabs.conversational_ai.knowledge_base.document.compute_rag_index(
        documentation_id=document_id,
        model=embedding_model
    )

# Then update agent configuration to use RAG
agent_id = "your-agent-id"

# Get the current agent configuration
agent_config = elevenlabs.conversational_ai.agents.get(agent_id=agent_id)

# Enable RAG in the agent configuration
agent_config.agent.prompt.rag = {
    "enabled": True,
    "embedding_model": "e5_mistral_7b_instruct",
    "max_documents_length": 10000
}

# Update document usage mode if needed
for i, doc in enumerate(agent_config.agent.prompt.knowledge_base):
    if doc.id == document_id:
        agent_config.agent.prompt.knowledge_base[i].usage_mode = "auto"

# Update the agent configuration
elevenlabs.conversational_ai.agents.update(
    agent_id=agent_id,
    conversation_config=agent_config.agent
)

```

```javascript
// First, index a document for RAG
async function enableRAG(documentId, agentId, apiKey) {
  try {
    // Initialize the ElevenLabs client
    const { ElevenLabs } = require('elevenlabs');
    const elevenlabs = new ElevenLabs({
      apiKey: apiKey,
    });

    // Start document indexing for RAG
    let response = await elevenlabs.conversationalAi.knowledgeBase.document.computeRagIndex(
      documentId,
      {
        model: 'e5_mistral_7b_instruct',
      }
    );

    // Check indexing status until completion
    while (response.status !== 'SUCCEEDED' && response.status !== 'FAILED') {
      await new Promise((resolve) => setTimeout(resolve, 5000)); // Wait 5 seconds
      response = await elevenlabs.conversationalAi.knowledgeBase.document.computeRagIndex(
        documentId,
        {
          model: 'e5_mistral_7b_instruct',
        }
      );
    }

    if (response.status === 'FAILED') {
      throw new Error('RAG indexing failed');
    }

    // Get current agent configuration
    const agentConfig = await elevenlabs.conversationalAi.agents.get(agentId);

    // Enable RAG in the agent configuration
    const updatedConfig = {
      conversation_config: {
        ...agentConfig.agent,
        prompt: {
          ...agentConfig.agent.prompt,
          rag: {
            enabled: true,
            embedding_model: 'e5_mistral_7b_instruct',
            max_documents_length: 10000,
          },
        },
      },
    };

    // Update document usage mode if needed
    if (agentConfig.agent.prompt.knowledge_base) {
      agentConfig.agent.prompt.knowledge_base.forEach((doc, index) => {
        if (doc.id === documentId) {
          updatedConfig.conversation_config.prompt.knowledge_base[index].usage_mode = 'auto';
        }
      });
    }

    // Update the agent configuration
    await elevenlabs.conversationalAi.agents.update(agentId, updatedConfig);

    console.log('RAG configuration updated successfully');
    return true;
  } catch (error) {
    console.error('Error configuring RAG:', error);
    throw error;
  }
}

// Example usage
// enableRAG('your-document-id', 'your-agent-id', 'your-api-key')
//   .then(() => console.log('RAG setup complete'))
//   .catch(err => console.error('Error:', err));
```

</CodeBlocks>


---
title: Personalization
subtitle: >-
  Learn how to personalize your agent's behavior using dynamic variables and
  overrides.
---

## Overview

Personalization allows you to adapt your agent's behavior for each individual user, enabling more natural and contextually relevant conversations. ElevenLabs offers multiple approaches to personalization:

1. **Dynamic Variables** - Inject runtime values into prompts and messages
2. **Overrides** - Completely replace system prompts or messages
3. **Twilio Integration** - Personalize inbound call experiences via webhooks

## Personalization Methods

<CardGroup cols={3}>
  <Card
    title="Dynamic Variables"
    icon="duotone lambda"
    href="/docs/conversational-ai/customization/personalization/dynamic-variables"
  >
    Define runtime values using `{{ var_name }}` syntax to personalize your agent's messages, system
    prompts, and tools.
  </Card>
  <Card
    title="Overrides"
    icon="duotone sliders"
    href="/docs/conversational-ai/customization/personalization/overrides"
  >
    Completely replace system prompts, first messages, language, or voice settings for each
    conversation.
  </Card>
  <Card
    title="Twilio Integration"
    icon="duotone phone-arrow-down-left"
    href="/docs/conversational-ai/customization/personalization/twilio-personalization"
  >
    Dynamically personalize inbound Twilio calls using webhook data.
  </Card>
</CardGroup>

## Conversation Initiation Client Data Structure

The `conversation_initiation_client_data` object defines what can be customized when starting a conversation:

```json
{
  "type": "conversation_initiation_client_data",
  "conversation_config_override": {
    "agent": {
      "prompt": {
        "prompt": "overriding system prompt"
      },
      "first_message": "overriding first message",
      "language": "en"
    },
    "tts": {
      "voice_id": "voice-id-here"
    }
  },
  "custom_llm_extra_body": {
    "temperature": 0.7,
    "max_tokens": 100
  },
  "dynamic_variables": {
    "string_var": "text value",
    "number_var": 1.2,
    "integer_var": 123,
    "boolean_var": true
  },
  "user_id": "your_custom_user_id"
}
```

## Choosing the Right Approach

<Table>
  <thead>
    <tr>
      <th>Method</th>
      <th>Best For</th>
      <th>Implementation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>**Dynamic Variables**</td>
      <td>
        - Inserting user-specific data into templated content - Maintaining consistent agent
        behavior with personalized details - Personalizing tool parameters
      </td>
      <td>Define variables with `{{ variable_name }}` and pass values at runtime</td>
    </tr>
    <tr>
      <td>**Overrides**</td>
      <td>
        - Completely changing agent behavior per user - Switching languages or voices - Legacy
        applications (consider migrating to Dynamic Variables)
      </td>
      <td>
        Enable specific override permissions in security settings and pass complete replacement
        content
      </td>
    </tr>
  </tbody>
</Table>

## Learn More

- [Dynamic Variables Documentation](/docs/conversational-ai/customization/personalization/dynamic-variables)
- [Overrides Documentation](/docs/conversational-ai/customization/personalization/overrides)
- [Twilio Integration Documentation](/docs/conversational-ai/customization/personalization/twilio-personalization)
---
title: Dynamic variables
subtitle: Pass runtime values to personalize your agent's behavior.
---

**Dynamic variables** allow you to inject runtime values into your agent's messages, system prompts, and tools. This enables you to personalize each conversation with user-specific data without creating multiple agents.

## Overview

Dynamic variables can be integrated into multiple aspects of your agent:

- **System prompts** to customize behavior and context
- **First messages** to personalize greetings
- **Tool parameters and headers** to pass user-specific data

Here are a few examples where dynamic variables are useful:

- **Personalizing greetings** with user names
- **Including account details** in responses
- **Passing data** to tool calls
- **Customizing behavior** based on subscription tiers
- **Accessing system information** like conversation ID or call duration

<Info>
  Dynamic variables are ideal for injecting user-specific data that shouldn't be hardcoded into your
  agent's configuration.
</Info>

## System dynamic variables

Your agent has access to these automatically available system variables:

- `system__agent_id` - Unique identifier of the agent that initiated the conversation (stays stable throughout the conversation)
- `system__current_agent_id` - Unique identifier of the currently active agent (changes after agent transfers)
- `system__caller_id` - Caller's phone number (voice calls only)
- `system__called_number` - Destination phone number (voice calls only)
- `system__call_duration_secs` - Call duration in seconds
- `system__time_utc` - Current UTC time (ISO format)
- `system__conversation_id` - ElevenLabs' unique conversation identifier
- `system__call_sid` - Call SID (twilio calls only)

System variables:

- Are available without runtime configuration
- Are prefixed with `system__` (reserved prefix)
- In system prompts: Set once at conversation start (value remains static)
- In tool calls: Updated at execution time (value reflects current state)

<Warning>Custom dynamic variables cannot use the reserved `system__` prefix.</Warning>

## Secret dynamic variables

Secret dynamic variables are populated in the same way as normal dynamic variables but indicate to our Conversational AI platform that these should
only be used in dynamic variable headers and never sent to an LLM provider as part of an agent's system prompt or first message.

We recommend using these for auth tokens or private IDs that should not be sent to an LLM. To create a secret dynamic variable, simply prefix the dynamic variable with `secret__`.

## Updating dynamic variables from tools

[Tool calls](https://elevenlabs.io/docs/conversational-ai/customization/tools) can create or update dynamic variables if they return a valid JSON object. To specify what should be extracted, set the object path(s) using dot notation. If the field or path doesn't exist, nothing is updated.

Example of a response object and dot notation:

- Status corresponds to the path: `response.status`
- The first user's email in the users array corresponds to the path: `response.users.0.email`

<CodeGroup>
```JSON title="JSON"
{
  "response": {
    "status": 200,
    "message": "Successfully found 5 users",
    "users": [
      "user_1": {
        "user_name": "test_user_1",
        "email": "test_user_1@email.com"
      }
    ]
  }
}
```
</CodeGroup>

To update a dynamic variable to be the first user's email, set the assignment like so.

<Frame background="subtle">
  ![Query parameters](file:5c2af3ca-cf0e-42b3-82d0-5ed04ff6c596)
</Frame>

Assignments are a field of each server tool, that can be found documented [here](/docs/conversational-ai/api-reference/tools/create#response.body.tool_config.SystemToolConfig.assignments).

## Guide

### Prerequisites

- An [ElevenLabs account](https://elevenlabs.io)
- A configured ElevenLabs Conversational Agent ([create one here](/docs/conversational-ai/quickstart))

<Steps>
  <Step title="Define dynamic variables in prompts">
    Add variables using double curly braces `{{variable_name}}` in your:
    - System prompts
    - First messages
    - Tool parameters

    <Frame background="subtle">
      ![Dynamic variables in messages](file:e73958cd-28c5-45b4-95e0-8af09ac17af0)
    </Frame>

    <Frame background="subtle">
      ![Dynamic variables in messages](file:c66b0a51-5669-4fc5-8cdf-0a23341c3846)
    </Frame>

  </Step>

  <Step title="Define dynamic variables in tools">
    You can also define dynamic variables in the tool configuration.
    To create a new dynamic variable, set the value type to Dynamic variable and click the `+` button.

    <Frame background="subtle">
      ![Setting placeholders](file:70815ad8-171a-41d2-95a6-ac30821dfbab)
    </Frame>

    <Frame background="subtle">
      ![Setting placeholders](file:592e770f-099a-448f-b97e-25892ba85be2)
    </Frame>

  </Step>

  <Step title="Set placeholders">
    Configure default values in the web interface for testing:

    <Frame background="subtle">
      ![Setting placeholders](file:88f45ddd-4e10-4da5-aa67-9439a0906490)
    </Frame>

  </Step>

  <Step title="Pass variables at runtime">
    When starting a conversation, provide the dynamic variables in your code:

    <Tip>
      Ensure you have the latest [SDK](/docs/conversational-ai/libraries) installed.
    </Tip>

    <CodeGroup>
    ```python title="Python" focus={10-23} maxLines=25
    import os
    import signal
    from elevenlabs.client import ElevenLabs
    from elevenlabs.conversational_ai.conversation import Conversation, ConversationInitiationData
    from elevenlabs.conversational_ai.default_audio_interface import DefaultAudioInterface

    agent_id = os.getenv("AGENT_ID")
    api_key = os.getenv("ELEVENLABS_API_KEY")
    elevenlabs = ElevenLabs(api_key=api_key)

    dynamic_vars = {
        "user_name": "Angelo",
    }

    config = ConversationInitiationData(
        dynamic_variables=dynamic_vars
    )

    conversation = Conversation(
        elevenlabs,
        agent_id,
        config=config,
        # Assume auth is required when API_KEY is set.
        requires_auth=bool(api_key),
        # Use the default audio interface.
        audio_interface=DefaultAudioInterface(),
        # Simple callbacks that print the conversation to the console.
        callback_agent_response=lambda response: print(f"Agent: {response}"),
        callback_agent_response_correction=lambda original, corrected: print(f"Agent: {original} -> {corrected}"),
        callback_user_transcript=lambda transcript: print(f"User: {transcript}"),
        # Uncomment the below if you want to see latency measurements.
        # callback_latency_measurement=lambda latency: print(f"Latency: {latency}ms"),
    )

    conversation.start_session()

    signal.signal(signal.SIGINT, lambda sig, frame: conversation.end_session())
    ```

    ```javascript title="JavaScript" focus={7-20} maxLines=25
    import { Conversation } from '@elevenlabs/client';

    class VoiceAgent {
      ...

      async startConversation() {
        try {
            // Request microphone access
            await navigator.mediaDevices.getUserMedia({ audio: true });

            this.conversation = await Conversation.startSession({
                agentId: 'agent_id_goes_here', // Replace with your actual agent ID

                dynamicVariables: {
                    user_name: 'Angelo'
                },

                ... add some callbacks here
            });
        } catch (error) {
            console.error('Failed to start conversation:', error);
            alert('Failed to start conversation. Please ensure microphone access is granted.');
        }
      }
    }
    ```

    ```swift title="Swift"
    let dynamicVars: [String: DynamicVariableValue] = [
      "customer_name": .string("John Doe"),
      "account_balance": .number(5000.50),
      "user_id": .int(12345),
      "is_premium": .boolean(true)
    ]

    // Create session config with dynamic variables
    let config = SessionConfig(
        agentId: "your_agent_id",
        dynamicVariables: dynamicVars
    )

    // Start the conversation
    let conversation = try await Conversation.startSession(
        config: config
    )
    ```

    ```html title="Widget"
    <elevenlabs-convai
      agent-id="your-agent-id"
      dynamic-variables='{"user_name": "John", "account_type": "premium"}'
    ></elevenlabs-convai>
    ```
    </CodeGroup>

  </Step>
</Steps>

## Public Talk-to Page Integration

The public talk-to page supports dynamic variables through URL parameters, enabling you to personalize conversations when sharing agent links. This is particularly useful for embedding personalized agents in websites, emails, or marketing campaigns.

### URL Parameter Methods

There are two methods to pass dynamic variables to the public talk-to page:

#### Method 1: Base64-Encoded JSON

Pass variables as a base64-encoded JSON object using the `vars` parameter:

```
https://elevenlabs.io/app/talk-to?agent_id=your_agent_id&vars=eyJ1c2VyX25hbWUiOiJKb2huIiwiYWNjb3VudF90eXBlIjoicHJlbWl1bSJ9
```

The `vars` parameter contains base64-encoded JSON:

```json
{ "user_name": "John", "account_type": "premium" }
```

#### Method 2: Individual Query Parameters

Pass variables using `var_` prefixed query parameters:

```
https://elevenlabs.io/app/talk-to?agent_id=your_agent_id&var_user_name=John&var_account_type=premium
```

### Parameter Precedence

When both methods are used simultaneously, individual `var_` parameters take precedence over the base64-encoded variables to prevent conflicts:

```
https://elevenlabs.io/app/talk-to?agent_id=your_agent_id&vars=eyJ1c2VyX25hbWUiOiJKYW5lIn0=&var_user_name=John
```

In this example, `user_name` will be "John" (from `var_user_name`) instead of "Jane" (from the base64-encoded `vars`).

### Implementation Examples

<Tabs>
  <Tab title="JavaScript URL Generation">
    ```javascript
    // Method 1: Base64-encoded JSON
    function generateTalkToURL(agentId, variables) {
      const baseURL = 'https://elevenlabs.io/app/talk-to';
      const encodedVars = btoa(JSON.stringify(variables));
      return `${baseURL}?agent_id=${agentId}&vars=${encodedVars}`;
    }

    // Method 2: Individual parameters
    function generateTalkToURLWithParams(agentId, variables) {
      const baseURL = 'https://elevenlabs.io/app/talk-to';
      const params = new URLSearchParams({ agent_id: agentId });

      Object.entries(variables).forEach(([key, value]) => {
        params.append(`var_${key}`, encodeURIComponent(value));
      });

      return `${baseURL}?${params.toString()}`;
    }

    // Usage
    const variables = {
      user_name: "John Doe",
      account_type: "premium",
      session_id: "sess_123"
    };

    const urlMethod1 = generateTalkToURL("your_agent_id", variables);
    const urlMethod2 = generateTalkToURLWithParams("your_agent_id", variables);
    ```

  </Tab>
  
  <Tab title="Python URL Generation">
    ```python
    import base64
    import json
    from urllib.parse import urlencode, quote

    def generate_talk_to_url(agent_id, variables):
        """Generate URL with base64-encoded variables"""
        base_url = "https://elevenlabs.io/app/talk-to"
        encoded_vars = base64.b64encode(json.dumps(variables).encode()).decode()
        return f"{base_url}?agent_id={agent_id}&vars={encoded_vars}"

    def generate_talk_to_url_with_params(agent_id, variables):
        """Generate URL with individual var_ parameters"""
        base_url = "https://elevenlabs.io/app/talk-to"
        params = {"agent_id": agent_id}

        for key, value in variables.items():
            params[f"var_{key}"] = value

        return f"{base_url}?{urlencode(params)}"

    # Usage
    variables = {
        "user_name": "John Doe",
        "account_type": "premium",
        "session_id": "sess_123"
    }

    url_method1 = generate_talk_to_url("your_agent_id", variables)
    url_method2 = generate_talk_to_url_with_params("your_agent_id", variables)
    ```

  </Tab>

  <Tab title="Manual URL Construction">
    ```
    # Base64-encoded method
    1. Create JSON: {"user_name": "John", "account_type": "premium"}
    2. Encode to base64: eyJ1c2VyX25hbWUiOiJKb2huIiwiYWNjb3VudF90eXBlIjoicHJlbWl1bSJ9
    3. Add to URL: https://elevenlabs.io/app/talk-to?agent_id=your_agent_id&vars=eyJ1c2VyX25hbWUiOiJKb2huIiwiYWNjb3VudF90eXBlIjoicHJlbWl1bSJ9

    # Individual parameters method
    1. Add each variable with var_ prefix
    2. URL encode values if needed
    3. Final URL: https://elevenlabs.io/app/talk-to?agent_id=your_agent_id&var_user_name=John&var_account_type=premium
    ```

  </Tab>
</Tabs>

## Supported Types

Dynamic variables support these value types:

<CardGroup cols={3}>
  <Card title="String">Text values</Card>
  <Card title="Number">Numeric values</Card>
  <Card title="Boolean">True/false values</Card>
</CardGroup>

## Troubleshooting

<AccordionGroup>
  <Accordion title="Variables not replacing">

    Verify that:

    - Variable names match exactly (case-sensitive)
    - Variables use double curly braces: `{{ variable_name }}`
    - Variables are included in your dynamic_variables object

  </Accordion>
  <Accordion title="Type errors">

    Ensure that:
    - Variable values match the expected type
    - Values are strings, numbers, or booleans only

  </Accordion>
</AccordionGroup>
---
title: Overrides
subtitle: Tailor each conversation with personalized context for each user.
---

<Warning>
  While overrides are still supported for completely replacing system prompts or first messages, we
  recommend using [Dynamic
  Variables](/docs/conversational-ai/customization/personalization/dynamic-variables) as the
  preferred way to customize your agent's responses and inject real-time data. Dynamic Variables
  offer better maintainability and a more structured approach to personalization.
</Warning>

**Overrides** enable your assistant to adapt its behavior for each user interaction. You can pass custom data and settings at the start of each conversation, allowing the assistant to personalize its responses and knowledge with real-time context. Overrides completely override the agent's default values defined in the agent's [dashboard](https://elevenlabs.io/app/conversational-ai/agents).

## Overview

Overrides allow you to modify your AI agent's behavior in real-time without creating multiple agents. This enables you to personalize responses with user-specific data.

Overrides can be enabled for the following fields in the agent's security settings:

- System prompt
- First message
- Language
- Voice ID

When overrides are enabled for a field, providing an override is still optional. If not provided, the agent will use the default values defined in the agent's [dashboard](https://elevenlabs.io/app/conversational-ai/agents). An error will be thrown if an override is provided for a field that does not have overrides enabled.

Here are a few examples where overrides can be useful:

- **Greet users** by their name
- **Include account-specific details** in responses
- **Adjust the agent's language** or tone based on user preferences
- **Pass real-time data** like account balances or order status

<Info>
  Overrides are particularly useful for applications requiring personalized interactions or handling
  sensitive user data that shouldn't be stored in the agent's base configuration.
</Info>

## Guide

### Prerequisites

- An [ElevenLabs account](https://elevenlabs.io)
- A configured ElevenLabs Conversational Agent ([create one here](/docs/conversational-ai/quickstart))

This guide will show you how to override the default agent **System prompt** & **First message**.

<Steps>
  <Step title="Enable overrides">
    For security reasons, overrides are disabled by default. Navigate to your agent's settings and
    select the **Security** tab. 
    
    Enable the `First message` and `System prompt` overrides.

    <Frame background="subtle">
      ![Enable overrides](file:bf536f41-c0be-48e8-9f3a-46b8a3270552)
    </Frame>

  </Step>

  <Step title="Override the conversation">
    In your code, where the conversation is started, pass the overrides as a parameter.

    <Tip>
      Ensure you have the latest [SDK](/docs/conversational-ai/libraries) installed.
    </Tip>

    <CodeGroup>

    ```python title="Python" focus={3-14} maxLines=14
    from elevenlabs.conversational_ai.conversation import Conversation, ConversationInitiationData
    ...
    conversation_override = {
        "agent": {
            "prompt": {
                "prompt": f"The customer's bank account balance is {customer_balance}. They are based in {customer_location}." # Optional: override the system prompt.
            },
            "first_message": f"Hi {customer_name}, how can I help you today?", # Optional: override the first_message.
            "language": "en" # Optional: override the language.
        },
        "tts": {
            "voice_id": "custom_voice_id" # Optional: override the voice.
        }
    }

    config = ConversationInitiationData(
        conversation_config_override=conversation_override
    )
    conversation = Conversation(
        ...
        config=config,
        ...
    )
    conversation.start_session()
    ```
    ```javascript title="JavaScript" focus={4-15} maxLines=15
    ...
    const conversation = await Conversation.startSession({
      ...
      overrides: {
          agent: {
              prompt: {
                  prompt: `The customer's bank account balance is ${customer_balance}. They are based in ${customer_location}.` // Optional: override the system prompt.
              },
              firstMessage: `Hi ${customer_name}, how can I help you today?`, // Optional: override the first message.
              language: "en" // Optional: override the language.
          },
          tts: {
              voiceId: "custom_voice_id" // Optional: override the voice.
          }
      },
      ...
    })
    ```

    ```swift title="Swift" focus={3-14} maxLines=14
    import ElevenLabsSDK

    let promptOverride = ElevenLabsSDK.AgentPrompt(
        prompt: "The customer's bank account balance is \(customer_balance). They are based in \(customer_location)." // Optional: override the system prompt.
    )
    let agentConfig = ElevenLabsSDK.AgentConfig(
        prompt: promptOverride, // Optional: override the system prompt.
        firstMessage: "Hi \(customer_name), how can I help you today?", // Optional: override the first message.
        language: .en // Optional: override the language.
    )
    let overrides = ElevenLabsSDK.ConversationConfigOverride(
        agent: agentConfig, // Optional: override agent settings.
        tts: TTSConfig(voiceId: "custom_voice_id") // Optional: override the voice.
    )

    let config = ElevenLabsSDK.SessionConfig(
        agentId: "",
        overrides: overrides
    )

    let conversation = try await ElevenLabsSDK.Conversation.startSession(
      config: config,
      callbacks: callbacks
    )
    ```

    ```html title="Widget"
      <elevenlabs-convai
        agent-id="your-agent-id"
        override-language="es"         <!-- Optional: override the language -->
        override-prompt="Custom system prompt for this user"  <!-- Optional: override the system prompt -->
        override-first-message="Hi! How can I help you today?"  <!-- Optional: override the first message -->
        override-voice-id="custom_voice_id"  <!-- Optional: override the voice -->
      ></elevenlabs-convai>
    ```

    </CodeGroup>

    <Note>
      When using overrides, omit any fields you don't want to override rather than setting them to empty strings or null values. Only include the fields you specifically want to customize.
    </Note>

  </Step>
</Steps>
---
title: Twilio personalization
subtitle: Configure personalization for incoming Twilio calls using webhooks.
---

## Overview

When receiving inbound Twilio calls, you can dynamically fetch conversation initiation data through a webhook. This allows you to customize your agent's behavior based on caller information and other contextual data.

<iframe
  width="100%"
  height="400"
  src="https://www.youtube-nocookie.com/embed/cAuSo8qNs-8"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  allowfullscreen
></iframe>

## How it works

1. When a Twilio call is received, the ElevenLabs Conversational AI platform will make a webhook call to your specified endpoint, passing call information (`caller_id`, `agent_id`, `called_number`, `call_sid`) as arguments
2. Your webhook returns conversation initiation client data, including dynamic variables and overrides (an example is shown below)
3. This data is used to initiate the conversation

<Tip>

The system uses Twilio's connection/dialing period to fetch webhook data in parallel, creating a
seamless experience where:

- Users hear the expected telephone connection sound
- In parallel, theConversational AI platform fetches necessary webhook data
- The conversation is initiated with the fetched data by the time the audio connection is established

</Tip>

## Configuration

<Steps>

  <Step title="Configure webhook details">
    In the [settings page](https://elevenlabs.io/app/conversational-ai/settings) of the Conversational AI platform, configure the webhook URL and add any
    secrets needed for authentication.

    <Frame background="subtle">
        ![Enable webhook](file:438558ec-ce10-43e9-97ed-70bc8937ce6d)
    </Frame>

    Click on the webhook to modify which secrets are sent in the headers.

    <Frame background="subtle">
        ![Add secrets to headers](file:32725a18-e49c-493f-b3b5-d8df12c1d9bc)
    </Frame>

  </Step>

  <Step title="Enable fetching conversation initiation data">
    In the "Security" tab of the [agent's page](https://elevenlabs.io/app/conversational-ai/agents/), enable fetching conversation initiation data for inbound Twilio calls, and define fields that can be overridden.

    <Frame background="subtle">
        ![Enable webhook](file:c8fe852d-d4c5-40fb-8b32-76c7d35a396e)
    </Frame>

  </Step>

  <Step title="Implement the webhook endpoint to receive Twilio data">
    The webhook will receive a POST request with the following parameters:

    | Parameter       | Type   | Description                            |
    | --------------- | ------ | -------------------------------------- |
    | `caller_id`     | string | The phone number of the caller         |
    | `agent_id`      | string | The ID of the agent receiving the call |
    | `called_number` | string | The Twilio number that was called      |
    | `call_sid`      | string | Unique identifier for the Twilio call  |

  </Step>

  <Step title="Return conversation initiation client data">
   Your webhook must return a JSON response containing the initiation data for the agent.
  <Info>
    The `dynamic_variables` field must contain all dynamic variables defined for the agent. Overrides
    on the other hand are entirely optional. For more information about dynamic variables and
    overrides see the [dynamic variables](/docs/conversational-ai/customization/personalization/dynamic-variables) and
    [overrides](/docs/conversational-ai/customization/personalization/overrides) docs.
  </Info>

An example response could be:

```json
{
  "type": "conversation_initiation_client_data",
  "dynamic_variables": {
    "customer_name": "John Doe",
    "account_status": "premium",
    "last_interaction": "2024-01-15"
  },
  "conversation_config_override": {
    "agent": {
      "prompt": {
        "prompt": "The customer's bank account balance is $100. They are based in San Francisco."
      },
      "first_message": "Hi, how can I help you today?",
      "language": "en"
    },
    "tts": {
      "voice_id": "new-voice-id"
    }
  }
}
```

  </Step>
</Steps>

The Conversational AI platform will use the dynamic variables to populate the conversation initiation data, and the conversation will start smoothly.

<Warning>
  Ensure your webhook responds within a reasonable timeout period to avoid delaying the call
  handling.
</Warning>

## Security

- Use HTTPS endpoints only
- Implement authentication using request headers
- Store sensitive values as secrets through the [ElevenLabs secrets manager](https://elevenlabs.io/app/conversational-ai/settings)
- Validate the incoming request parameters


---
title: Voice customization
subtitle: Learn how to customize your AI agent's voice and speech patterns.
---

## Overview

You can customize various aspects of your AI agent's voice to create a more natural and engaging conversation experience. This includes controlling pronunciation, speaking speed, and language-specific voice settings.

## Available customizations

<CardGroup cols={2}>
  <Card
    title="Multi-voice support"
    icon="microphone-lines"
    href="/docs/conversational-ai/customization/voice/multi-voice-support"
  >
    Enable your agent to switch between different voices for multi-character conversations,
    storytelling, and language tutoring.
  </Card>
  <Card
    title="Pronunciation dictionary"
    icon="microphone-stand"
    href="/docs/conversational-ai/customization/voice/pronunciation-dictionary"
  >
    Control how your agent pronounces specific words and phrases using
    [IPA](https://en.wikipedia.org/wiki/International_Phonetic_Alphabet) or
    [CMU](https://en.wikipedia.org/wiki/CMU_Pronouncing_Dictionary) notation.
  </Card>
  <Card
    title="Speed control"
    icon="waveform"
    href="/docs/conversational-ai/customization/voice/speed-control"
  >
    Adjust how quickly or slowly your agent speaks, with values ranging from 0.7x to 1.2x.
  </Card>
  <Card
    title="Language-specific voices"
    icon="language"
    href="/docs/conversational-ai/customization/language"
  >
    Configure different voices for each supported language to ensure natural pronunciation.
  </Card>
</CardGroup>

## Best practices

<AccordionGroup>
  <Accordion title="Voice selection">
    Choose voices that match your target language and region for the most natural pronunciation.
    Consider testing multiple voices to find the best fit for your use case.
  </Accordion>
  <Accordion title="Speed optimization">
    Start with the default speed (1.0) and adjust based on your specific needs. Test different
    speeds with your content to find the optimal balance between clarity and natural flow.
  </Accordion>
  <Accordion title="Pronunciation dictionaries">
    Focus on terms specific to your business or use case that need consistent pronunciation and are
    not widely used in everyday conversation. Test pronunciations with your chosen voice and model
    combination.
  </Accordion>
</AccordionGroup>

<Note>
  Some voice customization features may be model-dependent. For example, phoneme-based pronunciation
  control is only available with the Turbo v2 model.
</Note>
---
title: Multi-voice support
subtitle: >-
  Enable your AI agent to switch between different voices for multi-character
  conversations and enhanced storytelling.
---

## Overview

Multi-voice support allows your conversational AI agent to dynamically switch between different ElevenLabs voices during a single conversation. This powerful feature enables:

- **Multi-character storytelling**: Different voices for different characters in narratives
- **Language tutoring**: Native speaker voices for different languages
- **Emotional agents**: Voice changes based on emotional context
- **Role-playing scenarios**: Distinct voices for different personas

<Frame background="subtle">
  <img
    src="file:fc028c16-a1b0-4708-aeec-4419f1ef404d"
    alt="Multi-voice configuration interface"
  />
</Frame>

## How it works

When multi-voice support is enabled, your agent can use XML-style markup to switch between configured voices during text generation. The agent automatically returns to the default voice when no specific voice is specified.

<CodeBlocks>
```xml title="Example voice switching"
The teacher said, <spanish>¡Hola estudiantes!</spanish> 
Then the student replied, <student>Hello! How are you today?</student>
```

```xml title="Multi-character dialogue"
<narrator>Once upon a time, in a distant kingdom...</narrator>
<princess>I need to find the magic crystal!</princess>
<wizard>The crystal lies beyond the enchanted forest.</wizard>
```

</CodeBlocks>

## Configuration

### Adding supported voices

Navigate to your agent settings and locate the **Multi-voice support** section under the `Voice` tab.

<Steps>

### Add a new voice

Click **Add voice** to configure a new supported voice for your agent.

<Frame background="subtle">
  <img
    src="file:1b01e6b4-34ce-4865-8c11-9566a755b12d"
    alt="Multi-voice configuration interface"
  />
</Frame>

### Configure voice properties

Set up the voice with the following details:

- **Voice label**: Unique identifier (e.g., "Joe", "Spanish", "Happy")
- **Voice**: Select from your available ElevenLabs voices
- **Model Family**: Choose Turbo, Flash, or Multilingual (optional)
- **Language**: Override the default language for this voice (optional)
- **Description**: When the agent should use this voice

### Save configuration

Click **Add voice** to save the configuration. The voice will be available for your agent to use immediately.

</Steps>

### Voice properties

<AccordionGroup>
  <Accordion title="Voice label">
    A unique identifier that the LLM uses to reference this voice. Choose descriptive labels like: -
    Character names: "Alice", "Bob", "Narrator" - Languages: "Spanish", "French", "German" -
    Emotions: "Happy", "Sad", "Excited" - Roles: "Teacher", "Student", "Guide"
  </Accordion>

<Accordion title="Model family">
  Override the agent's default model family for this specific voice: - **Flash**: Fastest eneration,
  optimized for real-time use - **Turbo**: Balanced speed and quality - **Multilingual**: Highest
  quality, best for non-English languages - **Same as agent**: Use agent's default setting
</Accordion>

<Accordion title="Language override">
  Specify a different language for this voice, useful for: - Multilingual conversations - Language
  tutoring applications - Region-specific pronunciations
</Accordion>

  <Accordion title="Description">
    Provide context for when the agent should use this voice. 
    Examples: 
    - "For any Spanish words or phrases" 
    - "When the message content is joyful or excited" 
    - "Whenever the character Joe is speaking"
  </Accordion>
</AccordionGroup>

## Implementation

### XML markup syntax

Your agent uses XML-style tags to switch between voices:

```xml
<VOICE_LABEL>text to be spoken</VOICE_LABEL>
```

**Key points:**

- Replace `VOICE_LABEL` with the exact label you configured
- Text outside tags uses the default voice
- Tags are case-sensitive
- Nested tags are not supported

### System prompt integration

When you configure supported voices, the system automatically adds instructions to your agent's prompt:

```
When a message should be spoken by a particular person, use markup: "<CHARACTER>message</CHARACTER>" where CHARACTER is the character label.

Available voices are as follows:
- default: any text outside of the CHARACTER tags
- Joe: Whenever Joe is speaking
- Spanish: For any Spanish words or phrases
- Narrator: For narrative descriptions
```

### Example usage

<Tabs>

    <Tab title="Language tutoring">
        ```
        Teacher: Let's practice greetings. In Spanish, we say <Spanish>¡Hola! ¿Cómo estás?</Spanish>
        Student: How do I respond?
        Teacher: You can say <Spanish>¡Hola! Estoy bien, gracias.</Spanish> which means Hello! I'm fine, thank you.
        ```
    </Tab>

    <Tab title="Storytelling">
      ```
      Once upon a time, a brave princess ventured into a dark cave.
      <Princess>I'm not afraid of you, dragon!</Princess> she declared boldly. The dragon rumbled from
      the shadows, <Dragon>You should be, little one.</Dragon>
      But the princess stood her ground, ready for whatever came next.
      ```
    </Tab>

</Tabs>

## Best practices

<AccordionGroup>

<Accordion title="Voice selection">

- Choose voices that clearly differentiate between characters or contexts
- Test voice combinations to ensure they work well together
- Consider the emotional tone and personality for each voice
- Ensure voices match the language and accent when switching languages

</Accordion>

<Accordion title="Label naming">

- Use descriptive, intuitive labels that the LLM can understand
- Keep labels short and memorable
- Avoid special characters or spaces in labels

</Accordion>

<Accordion title="Performance optimization">

- Limit the number of supported voices to what you actually need
- Use the same model family when possible to reduce switching overhead
- Test with your expected conversation patterns
- Monitor response times with multiple voice switches

</Accordion>

  <Accordion title="Content guidelines">
    - Provide clear descriptions for when each voice should be used 
    - Test edge cases where voice switching might be unclear
     - Consider fallback behavior when voice labels are ambiguous 
     - Ensure voice switches enhance rather than distract from the conversation
  </Accordion>
  
</AccordionGroup>

## Limitations

<Note>

- Maximum of 10 supported voices per agent (including default)
- Voice switching adds minimal latency during generation
- XML tags must be properly formatted and closed
- Voice labels are case-sensitive in markup
- Nested voice tags are not supported

</Note>

## FAQ

<AccordionGroup>

    <Accordion title="What happens if I use an undefined voice label?">
        If the agent uses a voice label that hasn't been configured, the text will be spoken using the
        default voice. The XML tags will be ignored.
    </Accordion>

    <Accordion title="Can I change voices mid-sentence?">
    Yes, you can switch voices within a single response. Each tagged section will use the specified
    voice, while untagged text uses the default voice.
    </Accordion>


    <Accordion title="Do voice switches affect conversation latency?">
    Voice switching adds minimal overhead. The first use of each voice in a conversation may have
    slightly higher latency as the voice is initialized.
    </Accordion>


    <Accordion title="Can I use the same voice with different labels?">
    Yes, you can configure multiple labels that use the same ElevenLabs voice but with different model
    families, languages, or contexts.
    </Accordion>

    <Accordion title="How do I train my agent to use voice switching effectively?">
        Provide clear examples in your system prompt and test thoroughly. You can include specific
        scenarios where voice switching should occur and examples of the XML markup format.
    </Accordion>

</AccordionGroup>
---
title: Pronunciation dictionaries
subtitle: Learn how to control how your AI agent pronounces specific words and phrases.
---

## Overview

Pronunciation dictionaries allow you to customize how your AI agent pronounces specific words or phrases. This is particularly useful for:

- Correcting pronunciation of names, places, or technical terms
- Ensuring consistent pronunciation across conversations
- Customizing regional pronunciation variations

<Frame background="subtle">
  <img
    src="file:4d317077-8e88-4d9a-a1b3-19e9f93c9525"
    alt="Pronunciation dictionary settings under the Voice tab"
  />
</Frame>

## Configuration

You can find the pronunciation dictionary settings under the **Voice** tab in your agent's configuration.

<Note>
  The phoneme function of pronunciation dictionaries only works with the Turbo v2 model, while the
  alias function works with all models.
</Note>

## Dictionary file format

Pronunciation dictionaries use XML-based `.pls` files. Here's an example structure:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<lexicon version="1.0"
      xmlns="http://www.w3.org/2005/01/pronunciation-lexicon"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.w3.org/2005/01/pronunciation-lexicon
        http://www.w3.org/TR/2007/CR-pronunciation-lexicon-20071212/pls.xsd"
      alphabet="ipa" xml:lang="en-GB">
  <lexeme>
    <grapheme>Apple</grapheme>
    <phoneme>ˈæpl̩</phoneme>
  </lexeme>
  <lexeme>
    <grapheme>UN</grapheme>
    <alias>United Nations</alias>
  </lexeme>
</lexicon>
```

## Supported formats

We support two types of pronunciation notation:

1. **IPA (International Phonetic Alphabet)**

   - More precise control over pronunciation
   - Requires knowledge of IPA symbols
   - Example: "nginx" as `/ˈɛndʒɪnˈɛks/`

2. **CMU (Carnegie Mellon University) Dictionary format**
   - Simpler ASCII-based format
   - More accessible for English pronunciations
   - Example: "tomato" as "T AH M EY T OW"

<Tip>
  You can use AI tools like Claude or ChatGPT to help generate IPA or CMU notations for specific
  words.
</Tip>

## Best practices

1. **Case sensitivity**: Create separate entries for capitalized and lowercase versions of words if needed
2. **Testing**: Always test pronunciations with your chosen voice and model
3. **Maintenance**: Keep your dictionary organized and documented
4. **Scope**: Focus on words that are frequently mispronounced or critical to your use case

## FAQ

<AccordionGroup>
  <Accordion title="Which models support phoneme-based pronunciation?">
    Currently, only the Turbo v2 model supports phoneme-based pronunciation. Other models will
    silently skip phoneme entries.
  </Accordion>
  <Accordion title="Can I use multiple dictionaries?">
    Yes, you can upload multiple dictionary files to handle different sets of pronunciations.
  </Accordion>
  <Accordion title="What happens if a word isn't in the dictionary?">
    The model will use its default pronunciation rules for any words not specified in the
    dictionary.
  </Accordion>
</AccordionGroup>

## Additional resources

- [Professional Voice Cloning](/docs/product-guides/voices/voice-cloning/professional-voice-cloning)
- [Voice Design](/docs/product-guides/voices/voice-design)
- [Text to Speech API Reference](/docs/api-reference/text-to-speech)
---
title: Speed control
subtitle: Learn how to adjust the speaking speed of your conversational AI agent.
---

## Overview

The speed control feature allows you to adjust how quickly or slowly your agent speaks. This can be useful for:

- Making speech more accessible for different audiences
- Matching specific use cases (e.g., slower for educational content)
- Optimizing for different types of conversations

<Frame background="subtle">
  <img
    src="file:b4b2066b-937b-47fb-abf7-a5c278641329"
    alt="Speed control settings under the Voice tab"
  />
</Frame>

## Configuration

Speed is controlled through the [`speed` parameter](/docs/api-reference/agents/create#request.body.conversation_config.tts.speed) with the following specifications:

- **Range**: 0.7 to 1.2
- **Default**: 1.0
- **Type**: Optional

## How it works

The speed parameter affects the pace of speech generation:

- Values below 1.0 slow down the speech
- Values above 1.0 speed up the speech
- 1.0 represents normal speaking speed

<Note>
  Extreme values near the minimum or maximum may affect the quality of the generated speech.
</Note>

## Best practices

- Start with the default speed (1.0) and adjust based on user feedback
- Test different speeds with your specific content
- Consider your target audience when setting the speed
- Monitor speech quality at extreme values

<Warning>Values outside the 0.7-1.2 range are not supported.</Warning>





---
title: Language
subtitle: Learn how to configure your agent to speak multiple languages.
---

## Overview

This guide shows you how to configure your agent to speak multiple languages. You'll learn to:

- Configure your agent's primary language
- Add support for multiple languages
- Set language-specific voices and first messages
- Optimize voice selection for natural pronunciation
- Enable automatic language switching

## Guide

<Steps>

<Step title="Default agent language">
When you create a new agent, it's configured with:

- English as the primary language
- Flash v2 model for fast, English-only responses
- A default first message.

<Frame background="subtle">![](file:d9e8ea7e-294b-4822-b088-4727cde743a7)</Frame>

<Note>
  Additional languages switch the agent to use the v2.5 Multilingual model. English will always use
  the v2 model.
</Note>

</Step>

<Step title="Add additional languages">
First, navigate to your agent's configuration page and locate the **Agent** tab.

1. In the **Additional Languages** add an additional language (e.g. French)
2. Review the first message, which is automatically translated using a Large Language Model (LLM). Customize it as needed for each additional language to ensure accuracy and cultural relevance.

<Frame background="subtle">![](file:0a4207bc-9c46-4b77-968b-d80b3be302d2)</Frame>

<Note>
  Selecting the **All** option in the **Additional Languages** dropdown will configure the agent to
  support 31 languages. Collectively, these languages are spoken by approximately 90% of the world's
  population.
</Note>

</Step>

<Step title="Configure language-specific voices">
For optimal pronunciation, configure each additional language with a language-specific voice from our [Voice Library](https://elevenlabs.io/app/voice-library).

<Note>
  To find great voices for each language curated by the ElevenLabs team, visit the [language top
  picks](https://elevenlabs.io/app/voice-library/collections).
</Note>

<Tabs>
<Tab title="Language-specific voice settings">
<Frame background="subtle">![](file:8d3a1100-2b2a-4883-b0d2-2ed2446b7e96)</Frame>
</Tab>
<Tab title="Voice library">
<Frame background="subtle">![](file:8875f223-d2d8-44a4-998b-094a5cb865db)</Frame>
</Tab>

</Tabs>
</Step>

<Step title="Enable language detection">

Add the [language detection tool](/docs/conversational-ai/customization/tools/system-tools/language-detection) to your agent can automatically switch to the user's preferred language.

</Step>

<Step title="Starting a call">

Now that the agent is configured to support additional languages, the widget will prompt the user for their preferred language before the conversation begins.

If using the SDK, the language can be set programmatically using conversation overrides. See the
[Overrides](/docs/conversational-ai/customization/personalization/overrides) guide for implementation details.

<Frame background="subtle">![](file:86e76d08-61fc-4e41-b56a-3705abad55db)</Frame>

<Note>
  Language selection is fixed for the duration of the call - users cannot switch languages
  mid-conversation.
</Note>

</Step>

</Steps>

### Internationalization

You can integrate the widget with your internationalization framework by dynamically setting the language and UI text attributes.

```html title="Widget"
<elevenlabs-convai
  language="es"
  action-text={i18n["es"]["actionText"]}
  start-call-text={i18n["es"]["startCall"]}
  end-call-text={i18n["es"]["endCall"]}
  expand-text={i18n["es"]["expand"]}
  listening-text={i18n["es"]["listening"]}
  speaking-text={i18n["es"]["speaking"]}
></elevenlabs-convai>
```

<Note>
  Ensure the language codes match between your i18n framework and the agent's supported languages.
</Note>

## Best practices

<AccordionGroup>
<Accordion title="Voice selection">
  Select voices specifically trained in your target languages. This ensures:
  - Natural pronunciation
  - Appropriate regional accents
  - Better handling of language-specific nuances
</Accordion>

<Accordion title="First message customization">
While automatic translations are provided, consider:

<div>
  
 - Reviewing translations for accuracy 
 - Adapting greetings for cultural context 
 - Adjusting formal/informal tone as needed

</div>
</Accordion>
</AccordionGroup>
---
title: Language detection
subtitle: Let your agent automatically switch to the language
---

## Overview

The `language detection` system tool allows your Conversational AI agent to switch its output language to any the agent supports.
This system tool is not enabled automatically. Its description can be customized to accommodate your specific use case.

<iframe
  width="100%"
  height="400"
  src="https://www.youtube-nocookie.com/embed/YhF2gKv9ozc"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  allowfullscreen
></iframe>

<Note>
  Where possible, we recommend enabling all languages for an agent and enabling the language
  detection system tool.
</Note>

Our language detection tool triggers language switching in two cases, both based on the received audio's detected language and content:

- `detection` if a user speaks a different language than the current output language, a switch will be triggered
- `content` if the user asks in the current language to change to a new language, a switch will be triggered

## Custom LLM integration

**Purpose**: Automatically switch to the user's detected language during conversations.

**Trigger conditions**: The LLM should call this tool when:

- User speaks in a different language than the current conversation language
- User explicitly requests to switch languages
- Multi-language support is needed for the conversation

**Parameters**:

- `reason` (string, required): The reason for the language switch
- `language` (string, required): The language code to switch to (must be in supported languages list)

**Function call format**:

```json
{
  "type": "function",
  "function": {
    "name": "language_detection",
    "arguments": "{\"reason\": \"User requested Spanish\", \"language\": \"es\"}"
  }
}
```

**Implementation**: Configure supported languages in agent settings and add the language detection system tool. The agent will automatically switch voice and responses to match detected languages.


## Enabling language detection

<Steps>
    <Step title="Configure supported languages">
        The languages that the agent can switch to must be defined in the `Agent` settings tab.

        <Frame background="subtle">
            ![Agent languages](file:3cfcf4c9-bf74-4e4c-b4c0-b7a3778c2625)
        </Frame>
    </Step>

    <Step title="Add the language detection tool">
        Enable language detection by selecting the pre-configured system tool to your agent's tools in the `Agent` tab.
        This is automatically available as an option when selecting `add tool`.

        <Frame background="subtle">
            ![System tool](file:9f9ed922-c490-486e-8871-793614d40dc4)
        </Frame>
    </Step>

    <Step title="Configure tool description">
        Add a description that specifies when to call the tool

        <Frame background="subtle">
            ![Description](file:5798c3ee-8b28-4caf-afe8-a9d4f5eebb5d)
        </Frame>
    </Step>

</Steps>

### API Implementation

When creating an agent via API, you can add the `language detection` tool to your agent configuration. It should be defined as a system tool:

<CodeBlocks>

```python
from elevenlabs import (
    ConversationalConfig,
    ElevenLabs,
    AgentConfig,
    PromptAgent,
    PromptAgentInputToolsItem_System,
    LanguagePresetInput,
    ConversationConfigClientOverrideInput,
    AgentConfigOverride,
)

# Initialize the client
elevenlabs = ElevenLabs(api_key="YOUR_API_KEY")

# Create the language detection tool
language_detection_tool = PromptAgentInputToolsItem_System(
    name="language_detection",
    description=""  # Optional: Customize when the tool should be triggered
)

# Create language presets
language_presets = {
    "nl": LanguagePresetInput(
        overrides=ConversationConfigClientOverrideInput(
            agent=AgentConfigOverride(
                prompt=None,
                first_message="Hoi, hoe gaat het met je?",
                language=None
            ),
            tts=None
        ),
        first_message_translation=None
    ),
    "fi": LanguagePresetInput(
        overrides=ConversationConfigClientOverrideInput(
            agent=AgentConfigOverride(
                first_message="Hei, kuinka voit?",
            ),
            tts=None
        ),
    ),
    "tr": LanguagePresetInput(
        overrides=ConversationConfigClientOverrideInput(
            agent=AgentConfigOverride(
                prompt=None,
                first_message="Merhaba, nasılsın?",
                language=None
            ),
            tts=None
        ),
    ),
    "ru": LanguagePresetInput(
        overrides=ConversationConfigClientOverrideInput(
            agent=AgentConfigOverride(
                prompt=None,
                first_message="Привет, как ты?",
                language=None
            ),
            tts=None
        ),
    ),
    "pt": LanguagePresetInput(
        overrides=ConversationConfigClientOverrideInput(
            agent=AgentConfigOverride(
                prompt=None,
                first_message="Oi, como você está?",
                language=None
            ),
            tts=None
        ),
    )
}

# Create the agent configuration
conversation_config = ConversationalConfig(
    agent=AgentConfig(
        prompt=PromptAgent(
            tools=[language_detection_tool],
            first_message="Hi how are you?"
        )
    ),
    language_presets=language_presets
)

# Create the agent
response = elevenlabs.conversational_ai.agents.create(
    conversation_config=conversation_config
)
```

```javascript
import { ElevenLabs } from '@elevenlabs/elevenlabs-js';

// Initialize the client
const elevenlabs = new ElevenLabs({
  apiKey: 'YOUR_API_KEY',
});

// Create the agent with language detection tool
await elevenlabs.conversationalAi.agents.create({
  conversationConfig: {
    agent: {
      prompt: {
        tools: [
          {
            type: 'system',
            name: 'language_detection',
            description: '', // Optional: Customize when the tool should be triggered
          },
        ],
        firstMessage: 'Hi, how are you?',
      },
    },
    languagePresets: {
      nl: {
        overrides: {
          agent: {
            prompt: null,
            firstMessage: 'Hoi, hoe gaat het met je?',
            language: null,
          },
          tts: null,
        },
      },
      fi: {
        overrides: {
          agent: {
            prompt: null,
            firstMessage: 'Hei, kuinka voit?',
            language: null,
          },
          tts: null,
        },
        firstMessageTranslation: {
          sourceHash: '{"firstMessage":"Hi how are you?","language":"en"}',
          text: 'Hei, kuinka voit?',
        },
      },
      tr: {
        overrides: {
          agent: {
            prompt: null,
            firstMessage: 'Merhaba, nasılsın?',
            language: null,
          },
          tts: null,
        },
      },
      ru: {
        overrides: {
          agent: {
            prompt: null,
            firstMessage: 'Привет, как ты?',
            language: null,
          },
          tts: null,
        },
      },
      pt: {
        overrides: {
          agent: {
            prompt: null,
            firstMessage: 'Oi, como você está?',
            language: null,
          },
          tts: null,
        },
      },
      ar: {
        overrides: {
          agent: {
            prompt: null,
            firstMessage: 'مرحبًا كيف حالك؟',
            language: null,
          },
          tts: null,
        },
      },
    },
  },
});
```

```bash
curl -X POST https://api.elevenlabs.io/v1/convai/agents/create \
     -H "xi-api-key: YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
  "conversation_config": {
    "agent": {
      "prompt": {
        "first_message": "Hi how are you?",
        "tools": [
          {
            "type": "system",
            "name": "language_detection",
            "description": ""
          }
        ]
      }
    },
    "language_presets": {
      "nl": {
        "overrides": {
          "agent": {
            "prompt": null,
            "first_message": "Hoi, hoe gaat het met je?",
            "language": null
          },
          "tts": null
        }
      },
      "fi": {
        "overrides": {
          "agent": {
            "prompt": null,
            "first_message": "Hei, kuinka voit?",
            "language": null
          },
          "tts": null
        }
      },
      "tr": {
        "overrides": {
          "agent": {
            "prompt": null,
            "first_message": "Merhaba, nasılsın?",
            "language": null
          },
          "tts": null
        }
      },
      "ru": {
        "overrides": {
          "agent": {
            "prompt": null,
            "first_message": "Привет, как ты?",
            "language": null
          },
          "tts": null
        }
      },
      "pt": {
        "overrides": {
          "agent": {
            "prompt": null,
            "first_message": "Oi, como você está?",
            "language": null
          },
          "tts": null
        }
      },
      "ar": {
        "overrides": {
          "agent": {
            "prompt": null,
            "first_message": "مرحبًا كيف حالك؟",
            "language": null
          },
          "tts": null
        }
      }
    }
  }
}'
```

</CodeBlocks>

<Tip>Leave the description blank to use the default language detection prompt.</Tip>
