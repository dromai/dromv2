---
title: Post-call webhooks
subtitle: Get notified when calls end and analysis is complete through webhooks.
---

## Overview

Post-call [Webhooks](/docs/product-guides/administration/webhooks) allow you to receive detailed information about a call after analysis is complete. When enabled, ElevenLabs will send a POST request to your specified endpoint with comprehensive call data.

ElevenLabs supports two types of post-call webhooks:

- **Transcription webhooks** (`post_call_transcription`): Contains full conversation data including transcripts, analysis results, and metadata
- **Audio webhooks** (`post_call_audio`): Contains minimal data with base64-encoded audio of the full conversation

## Migration Notice: Enhanced Webhook Format

<Warning>
  **Important:** Starting August 15th, 2025, post-call transcription webhooks will be migrated to
  include additional fields for enhanced compatibility and consistency.
</Warning>

### What's Changing

From August 15th, 2025, post-call transcription webhooks will be updated to match the same format as the [GET Conversation response](/docs/api-reference/conversations/get). The webhook `data` object will include three additional boolean fields:

- `has_audio`: Boolean indicating whether the conversation has any audio available
- `has_user_audio`: Boolean indicating whether user audio is available for the conversation
- `has_response_audio`: Boolean indicating whether agent response audio is available for the conversation

### Migration Requirements

To ensure your webhook handlers continue working after the migration:

1. **Update your webhook parsing logic** to handle these three new boolean fields
2. **Test your webhook endpoints** with the new field structure before August 15th, 2025
3. **Ensure your JSON parsing** can gracefully handle additional fields without breaking

### Benefits After Migration

Once the migration is complete:

- **Unified data model**: Webhook responses will match the GET Conversation API format exactly
- **SDK compatibility**: Webhook handlers can be provided in the SDK and automatically stay up-to-date with the GET response model

## Enabling post-call webhooks

Post-call webhooks can be enabled for all agents in your workspace through the Conversational AI [settings page](https://elevenlabs.io/app/conversational-ai/settings).

<Frame background="subtle">
  ![Post-call webhook settings](file:7ad1e631-4e5f-4f9f-8152-8398906955f6)
</Frame>

<Warning>
  Post call webhooks must return a 200 status code to be considered successful. Webhooks that
  repeatedly fail are auto disabled if there are 10 or more consecutive failures and the last
  successful delivery was more than 7 days ago or has never been successfully delivered.
</Warning>

<Note>For HIPAA compliance, if a webhook fails we can not retry the webhook.</Note>

### Authentication

It is important for the listener to validate all incoming webhooks. Webhooks currently support authentication via HMAC signatures. Set up HMAC authentication by:

- Securely storing the shared secret generated upon creation of the webhook
- Verifying the ElevenLabs-Signature header in your endpoint using the shared secret

The ElevenLabs-Signature takes the following format:

```json
t=timestamp,v0=hash
```

The hash is equivalent to the hex encoded sha256 HMAC signature of `timestamp.request_body`. Both the hash and timestamp should be validated, an example is shown here:

<Tabs>
  <Tab title="Python">
    Example python webhook handler using FastAPI:

    ```python
    from fastapi import FastAPI, Request
    import time
    import hmac
    from hashlib import sha256

    app = FastAPI()

    # Example webhook handler
    @app.post("/webhook")
    async def receive_message(request: Request):
        payload = await request.body()
        headers = request.headers.get("elevenlabs-signature")
        if headers is None:
            return
        timestamp = headers.split(",")[0][2:]
        hmac_signature = headers.split(",")[1]

        # Validate timestamp
        tolerance = int(time.time()) - 30 * 60
        if int(timestamp) < tolerance
            return

        # Validate signature
        full_payload_to_sign = f"{timestamp}.{payload.decode('utf-8')}"
        mac = hmac.new(
            key=secret.encode("utf-8"),
            msg=full_payload_to_sign.encode("utf-8"),
            digestmod=sha256,
        )
        digest = 'v0=' + mac.hexdigest()
        if hmac_signature != digest:
            return

        # Continue processing

        return {"status": "received"}
    ```

  </Tab>
  <Tab title="JavaScript">
    <Tabs>
      <Tab title="Express">
        Example javascript webhook handler using node express framework:

        ```javascript
        const crypto = require('crypto');
        const secret = process.env.WEBHOOK_SECRET;
        const bodyParser = require('body-parser');

        // Ensure express js is parsing the raw body through instead of applying it's own encoding
        app.use(bodyParser.raw({ type: '*/*' }));

        // Example webhook handler
        app.post('/webhook/elevenlabs', async (req, res) => {
          const headers = req.headers['ElevenLabs-Signature'].split(',');
          const timestamp = headers.find((e) => e.startsWith('t=')).substring(2);
          const signature = headers.find((e) => e.startsWith('v0='));

          // Validate timestamp
          const reqTimestamp = timestamp * 1000;
          const tolerance = Date.now() - 30 * 60 * 1000;
          if (reqTimestamp < tolerance) {
            res.status(403).send('Request expired');
            return;
          } else {
            // Validate hash
            const message = `${timestamp}.${req.body}`;
            const digest = 'v0=' + crypto.createHmac('sha256', secret).update(message).digest('hex');
            if (signature !== digest) {
              res.status(401).send('Request unauthorized');
              return;
            }
          }

          // Validation passed, continue processing ...

          res.status(200).send();
        });
        ```
      </Tab>
      <Tab title="Next.js">
        Example javascript webhook handler using Next.js API route:

        ```javascript app/api/convai-webhook/route.js
        import { NextResponse } from "next/server";
        import type { NextRequest } from "next/server";
        import crypto from "crypto";

        export async function GET() {
          return NextResponse.json({ status: "webhook listening" }, { status: 200 });
        }

        export async function POST(req: NextRequest) {
          const secret = process.env.ELEVENLABS_CONVAI_WEBHOOK_SECRET; // Add this to your env variables
          const { event, error } = await constructWebhookEvent(req, secret);
          if (error) {
            return NextResponse.json({ error: error }, { status: 401 });
          }

          if (event.type === "post_call_transcription") {
            console.log("event data", JSON.stringify(event.data, null, 2));
          }

          return NextResponse.json({ received: true }, { status: 200 });
        }

        const constructWebhookEvent = async (req: NextRequest, secret?: string) => {
          const body = await req.text();
          const signature_header = req.headers.get("ElevenLabs-Signature");
          console.log(signature_header);

          if (!signature_header) {
            return { event: null, error: "Missing signature header" };
          }

          const headers = signature_header.split(",");
          const timestamp = headers.find((e) => e.startsWith("t="))?.substring(2);
          const signature = headers.find((e) => e.startsWith("v0="));

          if (!timestamp || !signature) {
            return { event: null, error: "Invalid signature format" };
          }

          // Validate timestamp
          const reqTimestamp = Number(timestamp) * 1000;
          const tolerance = Date.now() - 30 * 60 * 1000;
          if (reqTimestamp < tolerance) {
            return { event: null, error: "Request expired" };
          }

          // Validate hash
          const message = `${timestamp}.${body}`;

          if (!secret) {
            return { event: null, error: "Webhook secret not configured" };
          }

          const digest =
            "v0=" + crypto.createHmac("sha256", secret).update(message).digest("hex");
          console.log({ digest, signature });
          if (signature !== digest) {
            return { event: null, error: "Invalid signature" };
          }

          const event = JSON.parse(body);
          return { event, error: null };
        };
        ```
      </Tab>
    </Tabs>

  </Tab>
</Tabs>


### IP whitelisting

For additional security, you can whitelist the following static egress IPs from which all ElevenLabs webhook requests originate:

| Region       | IP Address     |
| ------------ | -------------- |
| US (Default) | 34.67.146.145  |
| US (Default) | 34.59.11.47    |
| EU           | 35.204.38.71   |
| EU           | 34.147.113.54  |
| Asia         | 35.185.187.110 |
| Asia         | 35.247.157.189 |

If you are using a [data residency region](/docs/product-guides/administration/data-residency) then the following IPs will be used:

| Region          | IP Address     |
| --------------- | -------------- |
| EU Residency    | 34.77.234.246  |
| EU Residency    | 34.140.184.144 |
| India Residency | 34.93.26.174   |
| India Residency | 34.93.252.69   |

If your infrastructure requires strict IP-based access controls, adding these IPs to your firewall allowlist will ensure you only receive webhook requests from ElevenLabs' systems.

<Note>
  These static IPs are used across all ElevenLabs webhook services and will remain consistent. Using
  IP whitelisting in combination with HMAC signature validation provides multiple layers of
  security.
</Note>

## Webhook response structure

ElevenLabs sends two distinct types of post-call webhooks, each with different data structures:

### Transcription webhooks (`post_call_transcription`)

Contains comprehensive conversation data including full transcripts, analysis results, and metadata.

#### Top-level fields

| Field             | Type   | Description                                                            |
| ----------------- | ------ | ---------------------------------------------------------------------- |
| `type`            | string | Type of event (always `post_call_transcription`)                       |
| `data`            | object | Conversation data using the `ConversationHistoryCommonModel` structure |
| `event_timestamp` | number | When this event occurred in unix time UTC                              |

#### Data object structure

The `data` object contains:

| Field                                 | Type   | Description                                   |
| ------------------------------------- | ------ | --------------------------------------------- |
| `agent_id`                            | string | The ID of the agent that handled the call     |
| `conversation_id`                     | string | Unique identifier for the conversation        |
| `status`                              | string | Status of the conversation (e.g., "done")     |
| `user_id`                             | string | User identifier if available                  |
| `transcript`                          | array  | Complete conversation transcript with turns   |
| `metadata`                            | object | Call timing, costs, and phone details         |
| `analysis`                            | object | Evaluation results and conversation summary   |
| `conversation_initiation_client_data` | object | Configuration overrides and dynamic variables |

<Note>
  As of August 15th, 2025, transcription webhooks will include the `has_audio`, `has_user_audio`,
  and `has_response_audio` fields to match the [GET Conversation
  response](/docs/api-reference/conversations/get) format exactly. Prior to this date, these fields
  are not included in webhook payloads.
</Note>

### Audio webhooks (`post_call_audio`)

Contains minimal data with the full conversation audio as base64-encoded MP3.

#### Top-level fields

| Field             | Type   | Description                               |
| ----------------- | ------ | ----------------------------------------- |
| `type`            | string | Type of event (always `post_call_audio`)  |
| `data`            | object | Minimal audio data                        |
| `event_timestamp` | number | When this event occurred in unix time UTC |

#### Data object structure

The `data` object contains only:

| Field             | Type   | Description                                                                    |
| ----------------- | ------ | ------------------------------------------------------------------------------ |
| `agent_id`        | string | The ID of the agent that handled the call                                      |
| `conversation_id` | string | Unique identifier for the conversation                                         |
| `full_audio`      | string | Base64-encoded string containing the complete conversation audio in MP3 format |

<Warning>
  Audio webhooks contain only the three fields listed above. They do NOT include transcript data,
  metadata, analysis results, or any other conversation details.
</Warning>

## Example webhook payloads

### Transcription webhook example

```json
{
  "type": "post_call_transcription",
  "event_timestamp": 1739537297,
  "data": {
    "agent_id": "xyz",
    "conversation_id": "abc",
    "status": "done",
    "user_id": "user123",
    "transcript": [
      {
        "role": "agent",
        "message": "Hey there angelo. How are you?",
        "tool_calls": null,
        "tool_results": null,
        "feedback": null,
        "time_in_call_secs": 0,
        "conversation_turn_metrics": null
      },
      {
        "role": "user",
        "message": "Hey, can you tell me, like, a fun fact about 11 Labs?",
        "tool_calls": null,
        "tool_results": null,
        "feedback": null,
        "time_in_call_secs": 2,
        "conversation_turn_metrics": null
      },
      {
        "role": "agent",
        "message": "I do not have access to fun facts about Eleven Labs. However, I can share some general information about the company. Eleven Labs is an AI voice technology platform that specializes in voice cloning and text-to-speech...",
        "tool_calls": null,
        "tool_results": null,
        "feedback": null,
        "time_in_call_secs": 9,
        "conversation_turn_metrics": {
          "convai_llm_service_ttfb": {
            "elapsed_time": 0.3704247010173276
          },
          "convai_llm_service_ttf_sentence": {
            "elapsed_time": 0.5551181449554861
          }
        }
      }
    ],
    "metadata": {
      "start_time_unix_secs": 1739537297,
      "call_duration_secs": 22,
      "cost": 296,
      "deletion_settings": {
        "deletion_time_unix_secs": 1802609320,
        "deleted_logs_at_time_unix_secs": null,
        "deleted_audio_at_time_unix_secs": null,
        "deleted_transcript_at_time_unix_secs": null,
        "delete_transcript_and_pii": true,
        "delete_audio": true
      },
      "feedback": {
        "overall_score": null,
        "likes": 0,
        "dislikes": 0
      },
      "authorization_method": "authorization_header",
      "charging": {
        "dev_discount": true
      },
      "termination_reason": ""
    },
    "analysis": {
      "evaluation_criteria_results": {},
      "data_collection_results": {},
      "call_successful": "success",
      "transcript_summary": "The conversation begins with the agent asking how Angelo is, but Angelo redirects the conversation by requesting a fun fact about 11 Labs. The agent acknowledges they don't have specific fun facts about Eleven Labs but offers to provide general information about the company. They briefly describe Eleven Labs as an AI voice technology platform specializing in voice cloning and text-to-speech technology. The conversation is brief and informational, with the agent adapting to the user's request despite not having the exact information asked for."
    },
    "conversation_initiation_client_data": {
      "conversation_config_override": {
        "agent": {
          "prompt": null,
          "first_message": null,
          "language": "en"
        },
        "tts": {
          "voice_id": null
        }
      },
      "custom_llm_extra_body": {},
      "dynamic_variables": {
        "user_name": "angelo"
      }
    }
  }
}
```

### Audio webhook example

```json
{
  "type": "post_call_audio",
  "event_timestamp": 1739537319,
  "data": {
    "agent_id": "xyz",
    "conversation_id": "abc",
    "full_audio": "SUQzBAAAAAAA...base64_encoded_mp3_data...AAAAAAAAAA=="
  }
}
```

## Audio webhook delivery

Audio webhooks are delivered separately from transcription webhooks and contain only the essential fields needed to identify the conversation along with the base64-encoded audio data.

<Note>
  Audio webhooks can be enabled or disabled using the "Send audio data" toggle in your webhook
  settings. This setting can be configured at both the workspace level (in the Conversational AI
  settings) and at the agent level (in individual agent webhook overrides).
</Note>

### Streaming delivery

Audio webhooks are delivered as streaming HTTP requests with the `transfer-encoding: chunked` header to handle large audio files efficiently.

### Processing audio webhooks

Since audio webhooks are delivered via chunked transfer encoding, you'll need to handle streaming data properly:

<CodeBlocks>

```python

import base64
import json
from aiohttp import web

async def handle_webhook(request):

    # Check if this is a chunked/streaming request
    if request.headers.get("transfer-encoding", "").lower() == "chunked":
        # Read streaming data in chunks
        chunked_body = bytearray()
        while True:
            chunk = await request.content.read(8192)  # 8KB chunks
            if not chunk:
                break
            chunked_body.extend(chunk)

        # Parse the complete payload
        request_body = json.loads(chunked_body.decode("utf-8"))
    else:
        # Handle regular requests
        body_bytes = await request.read()
        request_body = json.loads(body_bytes.decode('utf-8'))

    # Process different webhook types
    if request_body["type"] == "post_call_transcription":
        # Handle transcription webhook with full conversation data
        handle_transcription_webhook(request_body["data"])
    elif request_body["type"] == "post_call_audio":
        # Handle audio webhook with minimal data
        handle_audio_webhook(request_body["data"])

    return web.json_response({"status": "ok"})

def handle_audio_webhook(data):
    # Decode base64 audio data
    audio_bytes = base64.b64decode(data["full_audio"])

    # Save or process the audio file
    conversation_id = data["conversation_id"]
    with open(f"conversation_{conversation_id}.mp3", "wb") as f:
        f.write(audio_bytes)

```

```javascript
import fs from 'fs';

app.post('/webhook/elevenlabs', (req, res) => {
  let body = '';

  // Handle chunked/streaming requests
  req.on('data', (chunk) => {
    body += chunk;
  });

  req.on('end', () => {
    try {
      const requestBody = JSON.parse(body);

      // Process different webhook types
      if (requestBody.type === 'post_call_transcription') {
        // Handle transcription webhook with full conversation data
        handleTranscriptionWebhook(requestBody.data);
      } else if (requestBody.type === 'post_call_audio') {
        // Handle audio webhook with minimal data
        handleAudioWebhook(requestBody.data);
      }

      res.status(200).json({ status: 'ok' });
    } catch (error) {
      console.error('Error processing webhook:', error);
      res.status(400).json({ error: 'Invalid JSON' });
    }
  });
});

function handleAudioWebhook(data) {
  // Decode base64 audio data
  const audioBytes = Buffer.from(data.full_audio, 'base64');

  // Save or process the audio file
  const conversationId = data.conversation_id;
  fs.writeFileSync(`conversation_${conversationId}.mp3`, audioBytes);
}
```

</CodeBlocks>

<Note>
  Audio webhooks can be large files, so ensure your webhook endpoint can handle streaming requests
  and has sufficient memory/storage capacity. The audio is delivered in MP3 format.
</Note>

## Use cases

### Automated call follow-ups

Post-call webhooks enable you to build automated workflows that trigger immediately after a call ends. Here are some practical applications:

#### CRM integration

Update your customer relationship management system with conversation data as soon as a call completes:

```javascript
// Example webhook handler
app.post('/webhook/elevenlabs', async (req, res) => {
  // HMAC validation code

  const { data } = req.body;

  // Extract key information
  const userId = data.metadata.user_id;
  const transcriptSummary = data.analysis.transcript_summary;
  const callSuccessful = data.analysis.call_successful;

  // Update CRM record
  await updateCustomerRecord(userId, {
    lastInteraction: new Date(),
    conversationSummary: transcriptSummary,
    callOutcome: callSuccessful,
    fullTranscript: data.transcript,
  });

  res.status(200).send('Webhook received');
});
```

### Stateful conversations

Maintain conversation context across multiple interactions by storing and retrieving state:

1. When a call starts, pass in your user id as a dynamic variable.
2. When a call ends, set up your webhook endpoint to store conversation data in your database, based on the extracted user id from the dynamic_variables.
3. When the user calls again, you can retrieve this context and pass it to the new conversation into a {{previous_topics}} dynamic variable.
4. This creates a seamless experience where the agent "remembers" previous interactions

```javascript
// Store conversation state when call ends
app.post('/webhook/elevenlabs', async (req, res) => {
  // HMAC validation code

  const { data } = req.body;
  const userId = data.metadata.user_id;

  // Store conversation state
  await db.userStates.upsert({
    userId,
    lastConversationId: data.conversation_id,
    lastInteractionTimestamp: data.metadata.start_time_unix_secs,
    conversationHistory: data.transcript,
    previousTopics: extractTopics(data.analysis.transcript_summary),
  });

  res.status(200).send('Webhook received');
});

// When initiating a new call, retrieve and use the state
async function initiateCall(userId) {
  // Get user's conversation state
  const userState = await db.userStates.findOne({ userId });

  // Start new conversation with context from previous calls
  return await elevenlabs.startConversation({
    agent_id: 'xyz',
    conversation_id: generateNewId(),
    dynamic_variables: {
      user_name: userState.name,
      previous_conversation_id: userState.lastConversationId,
      previous_topics: userState.previousTopics.join(', '),
    },
  });
}
```

---
title: Transfer to human
subtitle: >-
  Seamlessly transfer the user to a human operator via phone number based on
  defined conditions.
---

## Overview

Human transfer allows a Conversational AI agent to transfer the ongoing call to a specified phone number or SIP URI when certain conditions are met. This enables agents to hand off complex issues, specific requests, or situations requiring human intervention to a live operator.

This feature utilizes the `transfer_to_number` system tool which supports transfers via Twilio and SIP trunk numbers. When triggered, the agent can provide a message to the user while they wait and a separate message summarizing the situation for the human operator receiving the call.

<Note>
  The `transfer_to_number` system tool is only available for phone calls and is not available in the
  chat widget.
</Note>

## Transfer Types

The system supports two types of transfers:

- **Conference Transfer**: Default behavior that calls the destination and adds the participant to a conference room, then removes the AI agent so only the caller and transferred participant remain.
- **SIP REFER Transfer**: Uses the SIP REFER protocol to transfer calls directly to the destination. Works with both phone numbers and SIP URIs, but only available when using SIP protocol during the conversation and requires your SIP Trunk to allow transfer via SIP REFER.

## Custom LLM integration

**Purpose**: Seamlessly hand off conversations to human operators when AI assistance is insufficient.

**Trigger conditions**: The LLM should call this tool when:

- Complex issues requiring human judgment
- User explicitly requests human assistance
- AI reaches limits of capability for the specific request
- Escalation protocols are triggered

**Parameters**:

- `reason` (string, optional): The reason for the transfer
- `transfer_number` (string, required): The phone number to transfer to (must match configured numbers)
- `client_message` (string, required): Message read to the client while waiting for transfer
- `agent_message` (string, required): Message for the human operator receiving the call

**Function call format**:

```json
{
  "type": "function",
  "function": {
    "name": "transfer_to_number",
    "arguments": "{\"reason\": \"Complex billing issue\", \"transfer_number\": \"+15551234567\", \"client_message\": \"I'm transferring you to a billing specialist who can help with your account.\", \"agent_message\": \"Customer has a complex billing dispute about order #12345 from last month.\"}"
  }
}
```

**Implementation**: Configure transfer phone numbers and conditions. Define messages for both customer and receiving human operator. Works with both Twilio and SIP trunking.


## Numbers that can be transferred to

Currently only [SIP trunking](/docs/conversational-ai/phone-numbers/sip-trunking) phone numbers support transferring to external numbers.
[Twilio phone numbers](/docs/conversational-ai/phone-numbers/twilio-integration/native-integration) currently can only transfer to phone numbers hosted on Twilio. If this is needed we recommended using Twilio numbers via [Twilio elastic SIP trunking](https://www.twilio.com/docs/sip-trunking) and our SIP trunking support, rather than via the native integration.

## Enabling human transfer

Human transfer is configured using the `transfer_to_number` system tool.

<Steps>
    <Step title="Add the transfer tool">
        Enable human transfer by selecting the `transfer_to_number` system tool in your agent's configuration within the `Agent` tab. Choose "Transfer to Human" when adding a tool.

        <Frame background="subtle" caption="Select 'Transfer to Human' tool">
            {/* Placeholder for image showing adding the 'Transfer to Human' tool */}
            <img src="file:33464d95-1f84-4495-b81e-67af24fcd284" alt="Add Human Transfer Tool" />
        </Frame>
    </Step>

    <Step title="Configure tool description (optional)">
        You can provide a custom description to guide the LLM on when to trigger a transfer. If left blank, a default description encompassing the defined transfer rules will be used.

        <Frame background="subtle" caption="Configure transfer tool description">
             {/* Placeholder for image showing the tool description field */}
             <img src="file:1fabaae8-70fe-45af-835b-a93c0fc54e22" alt="Human Transfer Tool Description" />
        </Frame>
    </Step>

    <Step title="Define transfer rules">
        Configure the specific rules for transferring to phone numbers or SIP URIs. For each rule, specify:

        - **Transfer Type**: Choose between Conference (default) or SIP REFER transfer methods
        - **Number Type**: Select Phone for regular phone numbers or SIP URI for SIP addresses
        - **Phone Number/SIP URI**: The target destination in the appropriate format:
          - Phone numbers: E.164 format (e.g., +12125551234)
          - SIP URIs: SIP format (e.g., sip:1234567890@example.com)
        - **Condition**: A natural language description of the circumstances under which the transfer should occur (e.g., "User explicitly requests to speak to a human", "User needs to update sensitive account information").

        The LLM will use these conditions, along with the tool description, to decide when and to which destination to transfer.

        <Note>
            **SIP REFER transfers** require SIP protocol during the conversation and your SIP Trunk must allow transfer via SIP REFER. Only SIP REFER supports transferring to a SIP URI.
        </Note>

        <Frame background="subtle" caption="Define transfer rules with phone number and condition">
            {/* Placeholder for image showing transfer rules configuration */}
            <img src="file:f0300fae-cdd1-421f-b846-fcf933c21901" alt="Human Transfer Rules Configuration" />
        </Frame>

        <Note>
            Ensure destinations are correctly formatted:
            - Phone numbers: E.164 format and associated with a properly configured account
            - SIP URIs: Valid SIP format (sip:user@domain or sips:user@domain)
        </Note>
    </Step>

</Steps>

## API Implementation

You can configure the `transfer_to_number` system tool when creating or updating an agent via the API. The tool allows specifying messages for both the client (user being transferred) and the agent (human operator receiving the call).

<CodeBlocks>

```python
from elevenlabs import (
    ConversationalConfig,
    ElevenLabs,
    AgentConfig,
    PromptAgent,
    PromptAgentInputToolsItem_System,
    SystemToolConfigInputParams_TransferToNumber,
    PhoneNumberTransfer,
)

# Initialize the client
elevenlabs = ElevenLabs(api_key="YOUR_API_KEY")

# Define transfer rules
transfer_rules = [
    PhoneNumberTransfer(
        transfer_destination={"type": "phone", "phone_number": "+15551234567"},
        condition="When the user asks for billing support.",
        transfer_type="conference"
    ),
    PhoneNumberTransfer(
        transfer_destination={"type": "sip_uri", "sip_uri": "sip:support@example.com"},
        condition="When the user requests to file a formal complaint.",
        transfer_type="sip_refer"
    )
]

# Create the transfer tool configuration
transfer_tool = PromptAgentInputToolsItem_System(
    type="system",
    name="transfer_to_human",
    description="Transfer the user to a specialized agent based on their request.", # Optional custom description
    params=SystemToolConfigInputParams_TransferToNumber(
        transfers=transfer_rules
    )
)

# Create the agent configuration
conversation_config = ConversationalConfig(
    agent=AgentConfig(
        prompt=PromptAgent(
            prompt="You are a helpful assistant.",
            first_message="Hi, how can I help you today?",
            tools=[transfer_tool],
        )
    )
)

# Create the agent
response = elevenlabs.conversational_ai.agents.create(
    conversation_config=conversation_config
)

# Note: When the LLM decides to call this tool, it needs to provide:
# - transfer_number: The phone number to transfer to (must match one defined in rules).
# - client_message: Message read to the user during transfer.
# - agent_message: Message read to the human operator receiving the call.
```

```javascript
import { ElevenLabs } from '@elevenlabs/elevenlabs-js';

// Initialize the client
const elevenlabs = new ElevenLabs({
  apiKey: 'YOUR_API_KEY',
});

// Define transfer rules
const transferRules = [
  {
    transferDestination: { type: 'phone', phoneNumber: '+15551234567' },
    condition: 'When the user asks for billing support.',
    transferType: 'conference'
  },
  {
    transferDestination: { type: 'sip_uri', sipUri: 'sip:support@example.com' },
    condition: 'When the user requests to file a formal complaint.',
    transferType: 'sip_refer'
  },
];

// Create the agent with the transfer tool
await elevenlabs.conversationalAi.agents.create({
  conversationConfig: {
    agent: {
      prompt: {
        prompt: 'You are a helpful assistant.',
        firstMessage: 'Hi, how can I help you today?',
        tools: [
          {
            type: 'system',
            name: 'transfer_to_number',
            description: 'Transfer the user to a human operator based on their request.', // Optional custom description
            params: {
              systemToolType: 'transfer_to_number',
              transfers: transferRules,
            },
          },
        ],
      },
    },
  },
});

// Note: When the LLM decides to call this tool, it needs to provide:
// - transfer_number: The phone number to transfer to (must match one defined in rules).
// - client_message: Message read to the user during transfer.
// - agent_message: Message read to the human operator receiving the call.
</code_block_to_apply_changes_from>
```

</CodeBlocks>
---
title: Agent transfer
subtitle: >-
  Seamlessly transfer the user between Conversational AI agents based on defined
  conditions.
---

## Overview

Agent-agent transfer allows a Conversational AI agent to hand off the ongoing conversation to another designated agent when specific conditions are met. This enables the creation of sophisticated, multi-layered conversational workflows where different agents handle specific tasks or levels of complexity.

For example, an initial agent (Orchestrator) could handle general inquiries and then transfer the call to a specialized agent based on the conversation's context. Transfers can also be nested:

<Frame background="subtle" caption="Example Agent Transfer Hierarchy">

```text
Orchestrator Agent (Initial Qualification)
│
├───> Agent 1 (e.g., Availability Inquiries)
│
├───> Agent 2 (e.g., Technical Support)
│     │
│     └───> Agent 2a (e.g., Hardware Support)
│
└───> Agent 3 (e.g., Billing Issues)

```

</Frame>

<Note>

We recommend using the `gpt-4o` or `gpt-4o-mini` models when using agent-agent transfers due to better tool calling.

</Note>

## Custom LLM integration

**Purpose**: Transfer conversations between specialized AI agents based on user needs.

**Trigger conditions**: The LLM should call this tool when:

- User request requires specialized knowledge or different agent capabilities
- Current agent cannot adequately handle the query
- Conversation flow indicates need for different agent type

**Parameters**:

- `reason` (string, optional): The reason for the agent transfer
- `agent_number` (integer, required): Zero-indexed number of the agent to transfer to (based on configured transfer rules)

**Function call format**:

```json
{
  "type": "function",
  "function": {
    "name": "transfer_to_agent",
    "arguments": "{\"reason\": \"User needs billing support\", \"agent_number\": 0}"
  }
}
```

**Implementation**: Define transfer rules mapping conditions to specific agent IDs. Configure which agents the current agent can transfer to. Agents are referenced by zero-indexed numbers in the transfer configuration.


## Enabling agent transfer

Agent transfer is configured using the `transfer_to_agent` system tool.

<Steps>
    <Step title="Add the transfer tool">
        Enable agent transfer by selecting the `transfer_to_agent` system tool in your agent's configuration within the `Agent` tab. Choose "Transfer to AI Agent" when adding a tool.

        <Frame background="subtle">
            <img src="file:0971992d-8021-4650-86b3-2f3eebf8f29b" alt="Add Transfer Tool" />
        </Frame>
    </Step>

    <Step title="Configure tool description (optional)">
        You can provide a custom description to guide the LLM on when to trigger a transfer. If left blank, a default description encompassing the defined transfer rules will be used.

        <Frame background="subtle">
             <img src="file:f389d875-cfcd-45f2-a935-6fd076d1cfca" alt="Transfer Tool Description" />
        </Frame>
    </Step>

    <Step title="Define transfer rules">
        Configure the specific rules for transferring to other agents. For each rule, specify:
        - **Agent**: The target agent to transfer the conversation to.
        - **Condition**: A natural language description of the circumstances under which the transfer should occur (e.g., "User asks about billing details", "User requests technical support for product X").
        - **Delay before transfer (milliseconds)**: The minimum delay (in milliseconds) before the transfer occurs. Defaults to 0 for immediate transfer.
        - **Transfer Message**: An optional custom message to play during the transfer. If left blank, the transfer will occur silently.
        - **Enable First Message**: Whether the transferred agent should play its first message after the transfer. Defaults to off.

        The LLM will use these conditions, along with the tool description, to decide when and to which agent (by number) to transfer.

        <Frame background="subtle">
            <img src="file:e4e97690-1630-4c73-b36e-4f3392d6a1dd" alt="Transfer Rules Configuration" />
        </Frame>

        <Note>
            Ensure that the user account creating the agent has at least viewer permissions for any target agents specified in the transfer rules.
        </Note>
    </Step>

</Steps>

## API Implementation

You can configure the `transfer_to_agent` system tool when creating or updating an agent via the API.

<CodeBlocks>

```python
from elevenlabs import (
    ConversationalConfig,
    ElevenLabs,
    AgentConfig,
    PromptAgent,
    PromptAgentInputToolsItem_System,
    SystemToolConfigInputParams_TransferToAgent,
    AgentTransfer
)

# Initialize the client
elevenlabs = ElevenLabs(api_key="YOUR_API_KEY")

# Define transfer rules with new options
transfer_rules = [
    AgentTransfer(
        agent_id="AGENT_ID_1",
        condition="When the user asks for billing support.",
        delay_ms=1000,  # 1 second delay
        transfer_message="I'm connecting you to our billing specialist.",
        enable_transferred_agent_first_message=True
    ),
    AgentTransfer(
        agent_id="AGENT_ID_2",
        condition="When the user requests advanced technical help.",
        delay_ms=0,  # Immediate transfer
        transfer_message=None,  # Silent transfer
        enable_transferred_agent_first_message=False
    )
]

# Create the transfer tool configuration
transfer_tool = PromptAgentInputToolsItem_System(
    type="system",
    name="transfer_to_agent",
    description="Transfer the user to a specialized agent based on their request.", # Optional custom description
    params=SystemToolConfigInputParams_TransferToAgent(
        transfers=transfer_rules
    )
)

# Create the agent configuration
conversation_config = ConversationalConfig(
    agent=AgentConfig(
        prompt=PromptAgent(
            prompt="You are a helpful assistant.",
            first_message="Hi, how can I help you today?",
            tools=[transfer_tool],
        )
    )
)

# Create the agent
response = elevenlabs.conversational_ai.agents.create(
    conversation_config=conversation_config
)

print(response)
```

```javascript
import { ElevenLabs } from '@elevenlabs/elevenlabs-js';

// Initialize the client
const elevenlabs = new ElevenLabs({
  apiKey: 'YOUR_API_KEY',
});

// Define transfer rules with new options
const transferRules = [
  {
    agentId: 'AGENT_ID_1',
    condition: 'When the user asks for billing support.',
    delayMs: 1000, // 1 second delay
    transferMessage: "I'm connecting you to our billing specialist.",
    enableTransferredAgentFirstMessage: true,
  },
  {
    agentId: 'AGENT_ID_2',
    condition: 'When the user requests advanced technical help.',
    delayMs: 0, // Immediate transfer
    transferMessage: null, // Silent transfer
    enableTransferredAgentFirstMessage: false,
  },
];

// Create the agent with the transfer tool
await elevenlabs.conversationalAi.agents.create({
  conversationConfig: {
    agent: {
      prompt: {
        prompt: 'You are a helpful assistant.',
        firstMessage: 'Hi, how can I help you today?',
        tools: [
          {
            type: 'system',
            name: 'transfer_to_agent',
            description: 'Transfer the user to a specialized agent based on their request.', // Optional custom description
            params: {
              systemToolType: 'transfer_to_agent',
              transfers: transferRules,
            },
          },
        ],
      },
    },
  },
});
```

</CodeBlocks>
