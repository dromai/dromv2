---
title: Conversation flow
subtitle: >-
  Configure how your assistant handles timeouts and interruptions during
  conversations.
---

## Overview

Conversation flow settings determine how your assistant handles periods of user silence and interruptions during speech. These settings help create more natural conversations and can be customized based on your use case.

<CardGroup cols={2}>
  <Card title="Timeouts" icon="clock" href="#timeouts">
    Configure how long your assistant waits during periods of silence
  </Card>
  <Card title="Interruptions" icon="hand" href="#interruptions">
    Control whether users can interrupt your assistant while speaking
  </Card>
</CardGroup>

## Timeouts

Timeout handling determines how long your assistant will wait during periods of user silence before prompting for a response.

### Configuration

Timeout settings can be configured in the agent's **Advanced** tab under **Turn Timeout**.

The timeout duration is specified in seconds and determines how long the assistant will wait in silence before prompting the user. Turn timeouts must be between 1 and 30 seconds.

#### Example Timeout Settings

<Frame background="subtle">
  ![Timeout settings](file:e02474c3-cb43-4779-8f49-272faea46f6b)
</Frame>

<Note>
  Choose an appropriate timeout duration based on your use case. Shorter timeouts create more
  responsive conversations but may interrupt users who need more time to respond, leading to a less
  natural conversation.
</Note>

### Best practices for timeouts

- Set shorter timeouts (5-10 seconds) for casual conversations where quick back-and-forth is expected
- Use longer timeouts (10-30 seconds) when users may need more time to think or formulate complex responses
- Consider your user context - customer service may benefit from shorter timeouts while technical support may need longer ones

## Interruptions

Interruption handling determines whether users can interrupt your assistant while it's speaking.

### Configuration

Interruption settings can be configured in the agent's **Advanced** tab under **Client Events**.

To enable interruptions, make sure interruption is a selected client event.

#### Interruptions Enabled

<Frame background="subtle">
  ![Interruption allowed](file:645a6bed-002a-4f9f-8fa9-e0d69c537a5c)
</Frame>

#### Interruptions Disabled

<Frame background="subtle">
  ![Interruption ignored](file:f772a22b-1d08-4e96-958f-aaf537eefee9)
</Frame>

<Note>
  Disable interruptions when the complete delivery of information is crucial, such as legal
  disclaimers or safety instructions.
</Note>

### Best practices for interruptions

- Enable interruptions for natural conversational flows where back-and-forth dialogue is expected
- Disable interruptions when message completion is critical (e.g., terms and conditions, safety information)
- Consider your use case context - customer service may benefit from interruptions while information delivery may not

## Recommended configurations

<AccordionGroup>
  <Accordion title="Customer service">
    - Shorter timeouts (5-10 seconds) for responsive interactions - Enable interruptions to allow
    customers to interject with questions
  </Accordion>
  <Accordion title="Legal disclaimers">
    - Longer timeouts (15-30 seconds) to allow for complex responses - Disable interruptions to
    ensure full delivery of legal information
  </Accordion>
  <Accordion title="Conversational EdTech">
    - Longer timeouts (10-30 seconds) to allow time to think and formulate responses - Enable
    interruptions to allow students to interject with questions
  </Accordion>
</AccordionGroup>

---
title: Authentication
subtitle: Learn how to secure access to your conversational AI agents
---

## Overview

When building conversational AI agents, you may need to restrict access to certain agents or conversations. ElevenLabs provides multiple authentication mechanisms to ensure only authorized users can interact with your agents.

## Authentication methods

ElevenLabs offers two primary methods to secure your conversational AI agents:

<CardGroup cols={2}>
  <Card title="Signed URLs" icon="signature" href="#using-signed-urls">
    Generate temporary authenticated URLs for secure client-side connections without exposing API
    keys.
  </Card>
  <Card title="Allowlists" icon="list-check" href="#using-allowlists">
    Restrict access to specific domains or hostnames that can connect to your agent.
  </Card>
</CardGroup>

## Using signed URLs

Signed URLs are the recommended approach for client-side applications. This method allows you to authenticate users without exposing your API key.

<Note>
  The guides below uses the [JS client](https://www.npmjs.com/package/@elevenlabs/client) and
  [Python SDK](https://github.com/elevenlabs/elevenlabs-python/).
</Note>

### How signed URLs work

1. Your server requests a signed URL from ElevenLabs using your API key.
2. ElevenLabs generates a temporary token and returns a signed WebSocket URL.
3. Your client application uses this signed URL to establish a WebSocket connection.
4. The signed URL expires after 15 minutes.

<Warning>Never expose your ElevenLabs API key client-side.</Warning>

### Generate a signed URL via the API

To obtain a signed URL, make a request to the `get_signed_url` [endpoint](/docs/conversational-ai/api-reference/conversations/get-signed-url) with your agent ID:

<CodeBlocks>
```python
# Server-side code using the Python SDK
from elevenlabs.client import ElevenLabs
async def get_signed_url():
    try:
        elevenlabs = ElevenLabs(api_key="your-api-key")
        response = await elevenlabs.conversational_ai.conversations.get_signed_url(agent_id="your-agent-id")
        return response.signed_url
    except Exception as error:
        print(f"Error getting signed URL: {error}")
        raise
```

```javascript
import { ElevenLabsClient } from '@elevenlabs/elevenlabs-js';

// Server-side code using the JavaScript SDK
const elevenlabs = new ElevenLabsClient({ apiKey: 'your-api-key' });
async function getSignedUrl() {
  try {
    const response = await elevenlabs.conversationalAi.conversations.getSignedUrl({
      agentId: 'your-agent-id',
    });

    return response.signed_url;
  } catch (error) {
    console.error('Error getting signed URL:', error);
    throw error;
  }
}
```

```bash
curl -X GET "https://api.elevenlabs.io/v1/convai/conversation/get-signed-url?agent_id=your-agent-id" \
-H "xi-api-key: your-api-key"
```

</CodeBlocks>

The curl response has the following format:

```json
{
  "signed_url": "wss://api.elevenlabs.io/v1/convai/conversation?agent_id=your-agent-id&conversation_signature=your-token"
}
```

### Connecting to your agent using a signed URL

Retrieve the server generated signed URL from the client and use the signed URL to connect to the websocket.

<CodeBlocks>

```python
# Client-side code using the Python SDK
from elevenlabs.conversational_ai.conversation import (
    Conversation,
    AudioInterface,
    ClientTools,
    ConversationInitiationData
)
import os
from elevenlabs.client import ElevenLabs
api_key = os.getenv("ELEVENLABS_API_KEY")

elevenlabs = ElevenLabs(api_key=api_key)

conversation = Conversation(
  client=elevenlabs,
  agent_id=os.getenv("AGENT_ID"),
  requires_auth=True,
  audio_interface=AudioInterface(),
  config=ConversationInitiationData()
)

async def start_conversation():
  try:
    signed_url = await get_signed_url()
    conversation = Conversation(
      client=elevenlabs,
      url=signed_url,
    )

    conversation.start_session()
  except Exception as error:
    print(f"Failed to start conversation: {error}")

```

```javascript
// Client-side code using the JavaScript SDK
import { Conversation } from '@elevenlabs/client';

async function startConversation() {
  try {
    const signedUrl = await getSignedUrl();
    const conversation = await Conversation.startSession({
      signedUrl,
    });

    return conversation;
  } catch (error) {
    console.error('Failed to start conversation:', error);
    throw error;
  }
}
```

</CodeBlocks>

### Signed URL expiration

Signed URLs are valid for 15 minutes. The conversation session can last longer, but the conversation must be initiated within the 15 minute window.

## Using allowlists

Allowlists provide a way to restrict access to your conversational AI agents based on the origin domain. This ensures that only requests from approved domains can connect to your agent.

### How allowlists work

1. You configure a list of approved hostnames for your agent.
2. When a client attempts to connect, ElevenLabs checks if the request's origin matches an allowed hostname.
3. If the origin is on the allowlist, the connection is permitted; otherwise, it's rejected.

### Configuring allowlists

Allowlists are configured as part of your agent's authentication settings. You can specify up to 10 unique hostnames that are allowed to connect to your agent.

### Example: setting up an allowlist

<CodeBlocks>

```python
from elevenlabs.client import ElevenLabs
import os
from elevenlabs.types import *

api_key = os.getenv("ELEVENLABS_API_KEY")
elevenlabs = ElevenLabs(api_key=api_key)

agent = elevenlabs.conversational_ai.agents.create(
  conversation_config=ConversationalConfig(
    agent=AgentConfig(
      first_message="Hi. I'm an authenticated agent.",
    )
  ),
  platform_settings=AgentPlatformSettingsRequestModel(
  auth=AuthSettings(
    enable_auth=False,
    allowlist=[
      AllowlistItem(hostname="example.com"),
      AllowlistItem(hostname="app.example.com"),
      AllowlistItem(hostname="localhost:3000")
      ]
    )
  )
)
```

```javascript
async function createAuthenticatedAgent(client) {
  try {
    const agent = await elevenlabs.conversationalAi.agents.create({
      conversationConfig: {
        agent: {
          firstMessage: "Hi. I'm an authenticated agent.",
        },
      },
      platformSettings: {
        auth: {
          enableAuth: false,
          allowlist: [
            { hostname: 'example.com' },
            { hostname: 'app.example.com' },
            { hostname: 'localhost:3000' },
          ],
        },
      },
    });

    return agent;
  } catch (error) {
    console.error('Error creating agent:', error);
    throw error;
  }
}
```

</CodeBlocks>

## Combining authentication methods

For maximum security, you can combine both authentication methods:

1. Use `enable_auth` to require signed URLs.
2. Configure an allowlist to restrict which domains can request those signed URLs.

This creates a two-layer authentication system where clients must:

- Connect from an approved domain
- Possess a valid signed URL

<CodeBlocks>

```python
from elevenlabs.client import ElevenLabs
import os
from elevenlabs.types import *
api_key = os.getenv("ELEVENLABS_API_KEY")
elevenlabs = ElevenLabs(api_key=api_key)
agent = elevenlabs.conversational_ai.agents.create(
  conversation_config=ConversationalConfig(
    agent=AgentConfig(
      first_message="Hi. I'm an authenticated agent that can only be called from certain domains.",
    )
  ),
platform_settings=AgentPlatformSettingsRequestModel(
  auth=AuthSettings(
    enable_auth=True,
    allowlist=[
      AllowlistItem(hostname="example.com"),
      AllowlistItem(hostname="app.example.com"),
      AllowlistItem(hostname="localhost:3000")
    ]
  )
)
```

```javascript
async function createAuthenticatedAgent(elevenlabs) {
  try {
    const agent = await elevenlabs.conversationalAi.agents.create({
      conversationConfig: {
        agent: {
          firstMessage: "Hi. I'm an authenticated agent.",
        },
      },
      platformSettings: {
        auth: {
          enableAuth: true,
          allowlist: [
            { hostname: 'example.com' },
            { hostname: 'app.example.com' },
            { hostname: 'localhost:3000' },
          ],
        },
      },
    });

    return agent;
  } catch (error) {
    console.error('Error creating agent:', error);
    throw error;
  }
}
```

</CodeBlocks>

## FAQ

<AccordionGroup>
  <Accordion title="Can I use the same signed URL for multiple users?">
    This is possible but we recommend generating a new signed URL for each user session.
  </Accordion>
  <Accordion title="What happens if the signed URL expires during a conversation?">
    If the signed URL expires (after 15 minutes), any WebSocket connection created with that signed
    url will **not** be closed, but trying to create a new connection with that signed URL will
    fail.
  </Accordion>
  <Accordion title="Can I restrict access to specific users?">
    The signed URL mechanism only verifies that the request came from an authorized source. To
    restrict access to specific users, implement user authentication in your application before
    requesting the signed URL.
  </Accordion>
  <Accordion title="Is there a limit to how many signed URLs I can generate?">
    There is no specific limit on the number of signed URLs you can generate.
  </Accordion>
  <Accordion title="How do allowlists handle subdomains?">
    Allowlists perform exact matching on hostnames. If you want to allow both a domain and its
    subdomains, you need to add each one separately (e.g., "example.com" and "app.example.com").
  </Accordion>
  <Accordion title="Do I need to use both authentication methods?">
    No, you can use either signed URLs or allowlists independently based on your security
    requirements. For highest security, we recommend using both.
  </Accordion>
  <Accordion title="What other security measures should I implement?">
    Beyond signed URLs and allowlists, consider implementing:

    - User authentication before requesting signed URLs
    - Rate limiting on API requests
    - Usage monitoring for suspicious patterns
    - Proper error handling for auth failures

  </Accordion>
</AccordionGroup>

---
title: Agent Workflows
subtitle: Build sophisticated conversation flows with visual graph-based workflows
---

## Overview

Agent Workflows provide a powerful visual interface for designing complex conversation flows in Conversational AI. Instead of relying on linear conversation paths, workflows enable you to create sophisticated, branching conversation graphs that adapt dynamically to user needs.

<Frame background="subtle">
  ![Workflow Overview](file:5f813fb4-6496-43f7-9a30-8a53606995cc)
</Frame>

## Node Types

Workflows are composed of different node types, each serving a specific purpose in your conversation flow.

<Frame background="subtle">
  ![Node Types](file:6701ec51-fdfa-4a5f-845e-cd09e12fd4fd)
</Frame>

### Subagent Nodes

Subagent nodes allow you to modify agent behavior at specific points in your workflow. These modifications are applied on top of the base agent configuration, or can override the current agent's config completely, giving you fine-grained control over each conversation phase.
Any of an agent's configuration, tools available, and attached knowledge base items can be updated/overwitten.

<Tabs>
  <Tab title="General">
    <Frame background="subtle" style={{ maxWidth: 420, maxHeight: 500, overflow: "hidden" }}>
      <img
        src="file:be8c545c-e6b4-4c9c-881d-908a19f22ff3"
        alt="Subagent Extra Agent Config"
        style={{ width: "100%", height: "auto", maxHeight: 500, objectFit: "contain", display: "block", margin: "0 auto" }}
      />
    </Frame>
    
    Modify core agent settings for this specific node:
    
    - **System Prompt**: Append or override system instructions to guide agent behavior
    - **LLM Selection**: Choose a different language model (e.g., switch from Gemini 2.0 Flash to a more powerful model for complex reasoning tasks)
    - **Voice Configuration**: Change voice settings including speed, tone, or even switch to a different voice
    
    **Use Cases:**
    - Use a more powerful LLM for complex decision-making nodes
    - Apply stricter conversation guidelines during sensitive information gathering
    - Change voice characteristics for different conversation phases
    - Modify agent personality for specific interaction types
  </Tab>

  <Tab title="Knowledge Base">

    <Frame background="subtle" style={{ maxWidth: 420, maxHeight: 500, overflow: "hidden" }}>
      <img
        src="file:3b5e047b-974b-4bb9-a299-9b59e8f24793"
        alt="Subagent Extra Knowledge Base"
        style={{ width: "100%", height: "auto", maxHeight: 400, objectFit: "contain", display: "block", margin: "0 auto" }}
      />
    </Frame>

    Add node-specific knowledge without affecting the global knowledge base:

    - **Include Global Knowledge Base**: Toggle whether to include the agent's main knowledge base
    - **Additional Documents**: Add documents specific to this conversation phase
    - **Dynamic Knowledge**: Inject contextual information based on workflow state

    **Use Cases:**
    - Add product-specific documentation during sales conversations
    - Include compliance guidelines during authentication
    - Provide troubleshooting guides for support flows
    - Add pricing information only after qualification

  </Tab>

  <Tab title="Tools">

    <Frame background="subtle" style={{ maxWidth: 420, maxHeight: 500, overflow: "hidden" }}>
      <img
        src="file:1ae3d3f0-2659-4866-b237-a213682c6a42"
        alt="Subagent Extra Tools"
        style={{ width: "100%", height: "auto", maxHeight: 400, objectFit: "contain", display: "block", margin: "0 auto" }}
      />
    </Frame>

    Manage which tools are available to the agent at this node:

    - **Include Global Tools**: Toggle whether to include tools from the main agent configuration
    - **Additional Tools**: Add tools specific to this workflow node (e.g., webhook tools like `book_meeting`)
    - **Tool Type**: Specify whether tools are webhooks, API calls, or other integrations

    **Use Cases:**
    - Add authentication tools only after initial qualification
    - Enable payment processing tools at checkout nodes
    - Provide CRM access after user verification
    - Add scheduling tools for appointment booking phases
    - Include webhook tools for specific actions like booking meetings

  </Tab>
  
</Tabs>

### Dispatch Tool Node

Tool nodes execute a specific tool call during conversation flow. Unlike tools within subagents, tool nodes are dedicated execution points that guarantee the tool is called.

<Frame background="subtle">
  ![Tool Node Result Edges](file:06294994-f05c-4623-9fc5-c8b572dcc530)
</Frame>

**Special Edge Configuration:**
Tool nodes have a unique edge type that allows routing to a new node based on the tool execution result. You can define:

- **Success path**: Where to route when the tool executes successfully
- **Failure path**: Where to route when the tool fails or returns an error

In future, futher branching conditions will be provided.

### Agent Transfer Node

Agent transfer node facilitate handoffs the conversation between different conversational agents, learn more [here](/docs/conversational-ai/customization/tools/system-tools/agent-transfer).

### Transfer to number node

Transfer to number nodes transitions from a conversation with an AI agent to a human agent via phone systems, learn more [here](/docs/conversational-ai/customization/tools/system-tools/transfer-to-human)

### End Node

End call nodes terminate the conversation flow gracefully, learn more [here](/docs/conversational-ai/customization/tools/system-tools/transfer-to-human#:~:text=System%20tools-,End%20call,-Language%20detection)

## Edges and Flow Control

Edges define how conversations flow between nodes in your workflow. They support sophisticated routing logic that enables dynamic, context-aware conversation paths.

<Frame background="subtle">
  ![Workflow Edges](file:22265c6d-c691-4f9a-8b05-62466a9edcd7)
</Frame>

<Tabs>
  <Tab title="Forward Edges">
    Forward edges move the conversation to subsequent nodes in the workflow. They represent the primary flow of your conversation.

    <Frame background="subtle">
      ![Forward Edge Configuration](file:8e870854-60c3-4e0c-b718-50dfa3fca876)
    </Frame>

    **Configuration Options:**
    - **Transition Type**: Choose between LLM conditions or unconditional transition
    - **Label**: Human-readable description of the edge condition
    - **LLM Condition**: Natural language condition evaluated by the LLM

  </Tab>

  <Tab title="Backward Edges">
    Backward edges allow conversations to loop back to previous nodes, enabling iterative interactions and retry logic.

    <Frame background="subtle">
      ![Backward Edge Configuration](file:025f0403-3634-483c-ad90-138543feccae)
    </Frame>

    **Use Cases:**
    - Retry failed authentication attempts
    - Loop back for additional information gathering
    - Re-qualification after changes in user requirements
    - Iterative troubleshooting processes

  </Tab>
</Tabs>
