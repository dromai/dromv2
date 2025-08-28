---
title: SIP trunking
subtitle: >-
  Connect your existing phone system with ElevenLabs conversational AI agents
  using SIP trunking
---

## Overview

SIP (Session Initiation Protocol) trunking allows you to connect your existing telephony infrastructure directly to ElevenLabs conversational AI agents.
This integration enables enterprise customers to use their existing phone systems while leveraging ElevenLabs' advanced voice AI capabilities.

With SIP trunking, you can:

- Connect your Private Branch Exchange (PBX) or SIP-enabled phone system to ElevenLabs' voice AI platform
- Route calls to AI agents without changing your existing phone infrastructure
- Handle both inbound and outbound calls
- Leverage encrypted TLS transport and media encryption for enhanced security

## How SIP trunking works

SIP trunking establishes a direct connection between your telephony infrastructure and the ElevenLabs platform:

1. **Inbound calls**: Calls from your SIP trunk are routed to the ElevenLabs platform using your configured SIP INVITE address.
2. **Outbound calls**: Calls initiated by ElevenLabs are routed to your SIP trunk using your configured hostname, enabling your agents to make outgoing calls.
3. **Authentication**: Connection security for the signaling is maintained through either digest authentication (username/password) or Access Control List (ACL) authentication based on the signaling source IP.
4. **Signaling and Media**: The initial call setup (signaling) supports multiple transport protocols including TLS for encrypted communication. Once the call is established, the actual audio data (RTP stream) can be encrypted based on your media encryption settings.

## Requirements

Before setting up SIP trunking, ensure you have:

1. A SIP-compatible PBX or telephony system
2. Phone numbers that you want to connect to ElevenLabs
3. Administrator access to your SIP trunk configuration
4. Appropriate firewall settings to allow SIP traffic
5. **TLS Support**: For enhanced security, ensure your SIP trunk provider supports TLS transport
6. **Audio codec compatibility**:
   Your system must support either G711 or G722 audio codecs or be capable of resampling audio on your end. ElevenLabs' SIP deployment outputs and receives audio at this sample rate. This is independent of any audio format configured on the agent for direct websocket connections.

## Setting up SIP trunking

<Steps>
  <Step title="Navigate to Phone Numbers">
    Go to the [Phone Numbers section](https://elevenlabs.io/app/conversational-ai/phone-numbers) in the ElevenLabs Conversational AI dashboard.
  </Step>
  <Step title="Import SIP Trunk">
    Click on "Import a phone number from SIP trunk" button to open the configuration dialog.

    <Frame background="subtle">
      <img src="file:1e0407b7-071e-4bff-afa3-93ba98d1cda5" alt="Select SIP trunk option" />
    </Frame>

    <Frame background="subtle">
      <img src="file:51d03d61-a302-4ccb-9fce-2cc17cd5ac2a" alt="SIP trunk configuration dialog" />
    </Frame>

  </Step>
  <Step title="Enter basic configuration">
    Complete the basic configuration with the following information:

    - **Label**: A descriptive name for the phone number
    - **Phone Number**: The E.164 formatted phone number to connect (e.g., +15551234567)

    <Frame background="subtle">
      <img src="file:da45e650-5a73-495b-af67-778e30ca0a4f" alt="SIP trunk basic configuration" />
    </Frame>

  </Step>
    <Step title="Configure transport and encryption">
    Configure the transport protocol and media encryption settings for enhanced security:

    - **Transport Type**: Select the transport protocol for SIP signaling:
      - **TCP**: Standard TCP transport
      - **TLS**: Encrypted TLS transport for enhanced security
    - **Media Encryption**: Configure encryption for RTP media streams:
      - **Disabled**: No media encryption
      - **Allowed**: Permits encrypted media streams
      - **Required**: Enforces encrypted media streams

    <Frame background="subtle">
      <img src="file:6e8c1cbc-ad07-48b5-bfdd-1b5cec089f3b" alt="Select TLS or TCP transport" />
    </Frame>

    <Frame background="subtle">
      <img src="file:2df08faf-b84e-47f5-9aa4-5fbe08f507b8" alt="Select media encryption setting" />
    </Frame>

    <Tip>
      **Security Best Practice**: Use TLS transport with Required media encryption for maximum security. This ensures both signaling and media are encrypted end-to-end.
    </Tip>

  </Step>
  <Step title="Configure outbound settings">
    Configure where ElevenLabs should send calls for your phone number:

    - **Address**: Hostname or IP address where the SIP INVITE is sent (e.g., `sip.telnyx.com`). This should be a hostname or IP address only, not a full SIP URI.
    - **Transport Type**: Select the transport protocol for SIP signaling:
      - **TCP**: Standard TCP transport
      - **TLS**: Encrypted TLS transport for enhanced security
    - **Media Encryption**: Configure encryption for RTP media streams:
      - **Disabled**: No media encryption
      - **Allowed**: Permits encrypted media streams
      - **Required**: Enforces encrypted media streams

    <Frame background="subtle">
      <img src="file:7b8d98ee-971c-4b8f-970c-3ea9174d50c8" alt="SIP trunk outbound configuration" />
    </Frame>

    <Tip>
      **Security Best Practice**: Use TLS transport with Required media encryption for maximum security. This ensures both signaling and media are encrypted end-to-end.
    </Tip>

    <Note>
      The **Address** field specifies where ElevenLabs will send outbound calls from your AI agents. Enter only the hostname or IP address without the `sip:` protocol prefix.
    </Note>

  </Step>
  <Step title="Add custom headers (optional)">
    If your SIP trunk provider requires specific headers for call routing or identification:

    - Click "Add Header" to add custom SIP headers
    - Enter the header name and value as required by your provider
    - You can add multiple headers as needed

    Custom headers are included with all outbound calls and can be used for:
    - Call routing and identification
    - Billing and tracking purposes
    - Provider-specific requirements

  </Step>
  <Step title="Configure authentication (optional)">
    Provide digest authentication credentials if required by your SIP trunk provider:

    - **SIP Trunk Username**: Username for SIP digest authentication
    - **SIP Trunk Password**: Password for SIP digest authentication

    If left empty, Access Control List (ACL) authentication will be used, which requires you to allowlist ElevenLabs IP addresses in your provider's settings.

    <Info>
      **Authentication Methods**:
      - **Digest Authentication**: Uses username/password credentials for secure authentication (recommended)
      - **ACL Authentication**: Uses IP address allowlisting for access control

      **Digest Authentication is strongly recommended** as it provides better security without relying on IP allowlisting, which can be complex to manage with dynamic IP addresses.
    </Info>

  </Step>
  <Step title="Complete Setup">
    Click "Import" to finalize the configuration.
  </Step>
</Steps>

## Client Data and Personalization

To ensure proper forwarding and traceability of call metadata, include the following custom SIP headers in your webhook payload and SIP INVITE request:

- **X-CALL-ID**: Unique identifier for the call
- **X-CALLER-ID**: Identifier for the calling party

These headers enable the system to associate call metadata with the conversation and provide context for personalization.

### Fallback Header Support

If the standard headers above are not present, the system will automatically look for the Twilio-specific SIP header:

- **sip.twilio.callSid**: Twilio's unique call identifier

This fallback ensures compatibility with Twilio's Elastic SIP Trunking without requiring configuration changes.

### Custom Provider Headers

If you're using a SIP provider other than Twilio and your platform uses different headers for call or caller identification please contact our support team.

### Processing Flow

Once the relevant metadata is received through any of the supported headers, the `caller_id` and/or `call_id` are available in the [pre-call webhook](/docs/conversational-ai/customization/personalization/twilio-personalization#how-it-works) and as [system dynamic variables](/docs/conversational-ai/customization/personalization/dynamic-variables#system-dynamic-variables).

## Assigning Agents to Phone Numbers

After importing your SIP trunk phone number, you can assign it to a conversational AI agent:

1. Go to the Phone Numbers section in the Conversational AI dashboard
2. Select your imported SIP trunk phone number
3. Click "Assign Agent"
4. Select the agent you want to handle calls to this number

## Troubleshooting

<AccordionGroup>

  <Accordion title="Connection Issues">
    If you're experiencing connection problems: 
    
    1. Verify your SIP trunk configuration on both the ElevenLabs side and your provider side
    2. Check that your firewall allows SIP signaling traffic on the configured transport protocol and port (5060 for TCP, 5061 for TLS) and ensure there is no whitelisting applied
    3. Confirm that your address hostname is correctly formatted and accessible
    4. Test with and without digest authentication credentials
    5. If using TLS transport, ensure your provider's TLS certificates are valid and properly configured
    6. Try different transport types (TCP only, as UDP is not currently available) to isolate TLS-specific issues
    
    **Important Network Architecture Information:**
    
    - ElevenLabs runs multiple SIP servers behind the load balancer `sip.rtc.elevenlabs.io`
    - The SIP servers communicate directly with your SIP server, bypassing the load balancer
    - SIP requests may come from different IP addresses due to our distributed infrastructure
    - If your security policy requires whitelisting inbound traffic, please contact our support team for assistance.
    
  </Accordion>
  <Accordion title="Authentication Failures">
    If calls are failing due to authentication issues:

    1. Double-check your SIP trunk username and password if using digest authentication
    2. Check your SIP trunk provider's logs for specific authentication error messages
    3. Verify that custom headers, if configured, match your provider's requirements
    4. Test with simplified configurations (no custom headers) to isolate authentication issues

  </Accordion>
  <Accordion title="TLS and Encryption Issues">
    If you're experiencing issues with TLS transport or media encryption:

    1. Verify that your SIP trunk provider supports TLS transport on port 5061
    2. Check certificate validity, expiration dates, and trust chains
    3. Ensure your provider supports SRTP media encryption if using "Required" media encryption
    4. Test with "Allowed" media encryption before using "Required" to isolate encryption issues
    5. Try TCP transport to isolate TLS-specific problems (UDP is not currently available)
    6. Contact your SIP trunk provider to confirm TLS and SRTP support

  </Accordion>
  <Accordion title="Custom Headers Issues">
    If you're having problems with custom headers:

    1. Verify the exact header names and values required by your provider
    2. Check for case sensitivity in header names
    3. Ensure header values don't contain special characters that need escaping
    4. Test without custom headers first, then add them incrementally
    5. Review your provider's documentation for supported custom headers

  </Accordion>
  <Accordion title="No Audio or One-Way Audio">
    If the call connects but there's no audio or audio only flows one way:

    1. Verify that your firewall allows UDP traffic for the RTP media stream (typically ports 10000-60000)
    2. Since RTP uses dynamic IP addresses, ensure firewall rules are not restricted to specific static IPs
    3. Check for Network Address Translation (NAT) issues that might be blocking the RTP stream
    4. If using "Required" media encryption, ensure both endpoints support SRTP
    5. Test with "Disabled" media encryption to isolate encryption-related audio issues

  </Accordion>
  <Accordion title="Audio Quality Issues">
    If you experience poor audio quality:

    1. Ensure your network has sufficient bandwidth (at least 100 Kbps per call) and low latency/jitter for UDP traffic
    2. Check for network congestion or packet loss, particularly on the UDP path
    3. Verify codec settings match on both ends
    4. If using media encryption, ensure both endpoints efficiently handle SRTP processing
    5. Test with different media encryption settings to isolate quality issues

  </Accordion>

</AccordionGroup>

## Limitations and Considerations

- Support for multiple concurrent calls depends on your subscription tier
- Call recording and analytics features are available but may require additional configuration
- Outbound calling capabilities may be limited by your SIP trunk provider
- **TLS Support**: Ensure your SIP trunk provider supports TLS 1.2 or higher for encrypted transport
- **Media Encryption**: SRTP support varies by provider; verify compatibility before requiring encryption
- **Audio format**: ElevenLabs' SIP deployment outputs and receives audio in G711 8kHz or G722 16kHz audio codecs. This is independent of any audio format configured on the agent for direct websocket connections. Your SIP trunk system must either support this format natively or perform resampling to match your system's requirements

## FAQ

<AccordionGroup>
  <Accordion title="Can I use my existing phone numbers with ElevenLabs?">
    Yes, SIP trunking allows you to connect your existing phone numbers directly to ElevenLabs'
    conversational AI platform without porting them.
  </Accordion>

<Accordion title="What SIP trunk providers are compatible with ElevenLabs?">
  ElevenLabs is compatible with most standard SIP trunk providers including Twilio, Vonage,
  RingCentral, Sinch, Infobip, Telnyx, Exotel, Plivo, Bandwidth, and others that support SIP
  protocol standards. TLS transport and SRTP media encryption are supported for enhanced security.
</Accordion>

<Accordion title="Should I use TLS transport for better security?">
  Yes, TLS transport is highly recommended for production environments. It provides encrypted SIP
  signaling which enhances security for your calls. Combined with required media encryption, it
  ensures comprehensive protection of your communications. Always verify your SIP trunk provider
  supports TLS before enabling it.
</Accordion>

<Accordion title="What's the difference between transport types?">
  - **TCP**: Reliable but unencrypted signaling - **TLS**: Encrypted and reliable signaling
  (recommended for production)
  <Note>
    UDP transport is not currently available. For security-critical applications, always use TLS
    transport.
  </Note>
</Accordion>

<Accordion title="What are custom headers used for?">
  Custom SIP headers allow you to include provider-specific information with outbound calls. Common
  uses include call routing, billing codes, caller identification, and meeting specific provider
  requirements.
</Accordion>

<Accordion title="How many concurrent calls are supported?">
  The number of concurrent calls depends on your subscription plan. Enterprise plans typically allow
  for higher volumes of concurrent calls.
</Accordion>

  <Accordion title="Can I route calls conditionally to different agents?">
    Yes, you can use your existing PBX system's routing rules to direct calls to different phone
    numbers, each connected to different ElevenLabs agents.
  </Accordion>
</AccordionGroup>

## Next steps

- [Learn about creating conversational AI agents](/docs/conversational-ai/quickstart)
---
title: Batch calling
subtitle: >-
  Initiate multiple outbound calls simultaneously with your Conversational AI
  agents.
---

<iframe
  width="100%"
  height="400"
  src="https://www.youtube.com/embed/TIOnL1TwzBs"
  title="Batch Calling Tutorial"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
></iframe>

<Note>
  When conducting outbound call campaigns, ensure compliance with all relevant regulations,
  including the [TCPA (Telephone Consumer Protection Act)](/docs/conversational-ai/legal/tcpa) and
  any applicable state laws.
</Note>

## Overview

Batch Calling enables you to initiate multiple outbound calls simultaneously using your configured Conversational AI agents. This feature is ideal for scenarios such as sending notifications, conducting surveys, or delivering personalized messages to a large list of recipients efficiently.
This feature is available for both phone numbers added via the [native Twilio integration](/docs/conversational-ai/phone-numbers/twilio-integration/native-integration) and [SIP trunking](/docs/conversational-ai/phone-numbers/sip-trunking).

### Key features

- **Upload recipient lists**: Easily upload recipient lists in CSV or XLS format.
- **Dynamic variables**: Personalize calls by including dynamic variables (e.g., `user_name`) in your recipient list as separate columns.
- **Agent selection**: Choose the specific Conversational AI agent to handle the calls.
- **Scheduling**: Send batches immediately or schedule them for a later time.
- **Real-time monitoring**: Track the progress of your batch calls, including overall status and individual call status.
- **Detailed reporting**: View comprehensive details of completed batch calls, including individual call recipient information.

## Concurrency

When batch calls are initiated, they automatically utilize up to 70% of your plan's concurrency limit.
This leaves 30% of your concurrent capacity available for other conversations, including incoming calls and calls via the widget.

## Requirements

- An ElevenLabs account with an [agent setup](/app/conversational-ai).
- A phone number imported

## Creating a batch call

Follow these steps to create a new batch call:

<Steps>

<Step title="Navigate to Batch Calling">
  Access the [Outbound calls interface](https://elevenlabs.io/app/conversational-ai/batch-calling)
  from the Conversational AI dashboard
</Step>

<Step title="Initiate a new batch call">
  Click on the "Create a batch call" button. This will open the "Create a batch call" page.

  <Frame background="subtle" caption="The 'Create a batch call' interface.">
    <img
      src="file:6d24d167-5f91-4b22-b99a-394475558148"
      alt="Create a batch call page showing fields for batch name, phone number, agent selection, recipient upload, and timing options."
    />
  </Frame>
</Step>

<Step title="Configure batch details">

- **Batch name**: Enter a descriptive name for your batch call (e.g., "Delivery notice", "Weekly Update Notifications").
- **Phone number**: Select the phone number that will be used to make the outbound calls.
- **Select agent**: Choose the pre-configured Conversational AI agent that will handle the conversations for this batch.

</Step>

<Step title="Upload recipients">

- **Upload File**: Upload your recipient list. Supported file formats are CSV and XLS.
- **Formatting**:
  - The `phone_number` column is mandatory in your uploaded file (if your agent has a `phone_number` dynamic variable that also has to be set, please rename it).
  - You can include other columns (e.g., `name`, `user_name`) which will be passed as dynamic variables to personalize the calls.
  - A template is available for download to ensure correct formatting.

<Note title="Setting overrides">
  The following column headers are special fields that are used to override an agent's initial
  configuration:
    - language
    - first_message
    - system_prompt
    - voice_id

The batch call will fail if those fields are passed but are not set to be overridable in the agent's security settings. See more
[here](/docs/conversational-ai/customization/personalization/overrides).

</Note>

</Step>

<Step title="Set timing">
  - **Send immediately**: The batch call will start processing as soon as you submit it. -
  **Schedule for later**: Choose a specific date and time for the batch call to begin.
</Step>

<Step title="Submit the batch call">
  - You may "Test call" with a single recipient before submitting the entire batch. - Click "Submit
  a Batch Call" to finalize and initiate or schedule the batch.
</Step>

</Steps>

## Managing and monitoring batch calls

Once a batch call is created, you can monitor its progress and view its details.

### Batch calling overview

The Batch Calling overview page displays a list of all your batch calls.

<Frame
  background="subtle"
  caption="Overview of batch calls, displaying status, progress, and other details for each batch."
>
  <img
    src="file:14351b4d-7aa7-4922-bdb3-5597bbf24c84"
    alt="Batch Calling overview page listing several batch calls with their status, recipient count, and progress."
  />
</Frame>

### Viewing batch call details

Clicking on a specific batch call from the overview page will take you to its detailed view, from where you can view individual conversations.

<Frame
  background="subtle"
  caption="Detailed view of a specific batch call, showing summary statistics and a list of call recipients with their individual statuses."
>
  <img
    src="file:cfe9d5bb-4832-40f4-9bb4-010b4bc13233"
    alt="Batch call details page showing a summary (status, total recipients, started, progress) and a list of call recipients with phone number, dynamic variables, and status."
  />
</Frame>

## API Usage

You can also manage and initiate batch calls programmatically using the ElevenLabs API. This allows for integration into your existing workflows and applications.

- [List batch calls](/docs/api-reference/batch-calling/list) - Retrieve all batch calls in your workspace
- [Create batch call](/docs/api-reference/batch-calling/create) - Submit a new batch call with agent, phone number, and recipient list
---
title: Twilio native integration
subtitle: Learn how to configure inbound calls for your agent with Twilio.
---

## Overview

This guide shows you how to connect a Twilio phone number to your conversational AI agent to handle both inbound and outbound calls.

You will learn to:

- Import an existing Twilio phone number.
- Link it to your agent to handle inbound calls.
- Initiate outbound calls using your agent.

## Phone Number Types & Capabilities

ElevenLabs supports two types of Twilio phone numbers with different capabilities:

### Purchased Twilio Numbers (Full Support)

- **Inbound calls**: Supported - Can receive calls and route them to agents
- **Outbound calls**: Supported - Can make calls using agents
- **Requirements**: Number must be purchased through Twilio and appear in your "Phone Numbers" section

### Verified Caller IDs (Outbound Only)

- **Inbound calls**: Not supported - Cannot receive calls or be assigned to agents
- **Outbound calls**: Supported - Can make calls using agents
- **Requirements**: Number must be verified in Twilio's "Verified Caller IDs" section
- **Use case**: Ideal for using your existing business number for outbound AI calls

Learn more about [verifying caller IDs at scale](https://www.twilio.com/docs/voice/api/verifying-caller-ids-scale) in Twilio's documentation.

<Note>
  During phone number import, ElevenLabs automatically detects the capabilities of your number based
  on its configuration in Twilio.
</Note>

## Guide

### Prerequisites

- A [Twilio account](https://twilio.com/).
- Either:
  - A purchased & provisioned Twilio [phone number](https://www.twilio.com/docs/phone-numbers) (for inbound + outbound)
  - OR a [verified caller ID](https://www.twilio.com/docs/voice/make-calls#verify-your-caller-id) in Twilio (for outbound only)

<Steps>

<Step title="Import a Twilio phone number">

In the Conversational AI dashboard, go to the [**Phone Numbers**](https://elevenlabs.io/app/conversational-ai/phone-numbers) tab.

<Frame background="subtle">

![Conversational AI phone numbers page](file:811d8f1e-6e65-4756-aab8-c0cc80f8d6f4)

</Frame>

Next, fill in the following details:

- **Label:** A descriptive name (e.g., `Customer Support Line`).
- **Phone Number:** The Twilio number you want to use.
- **Twilio SID:** Your Twilio Account SID.
- **Twilio Token:** Your Twilio Auth Token.

<Note>

You can find your account SID and auth token [**in the Twilio admin console**](https://www.twilio.com/console).

</Note>

<Tabs>

<Tab title="Conversational AI dashboard">

<Frame background="subtle">
  ![Phone number configuration](file:7c4e1256-8b9c-4b9d-894c-7ee9650e0da9)
</Frame>

</Tab>

<Tab title="Twilio admin console">
  Copy the Twilio SID and Auth Token from the [Twilio admin
  console](https://www.twilio.com/console).
  <Frame background="subtle">
    ![Phone number details](file:79736a9c-e201-4fcc-a036-271c46d68b70)
  </Frame>
</Tab>

</Tabs>

<Note>ElevenLabs automatically configures the Twilio phone number with the correct settings.</Note>

<Accordion title="Applied settings">
  <Frame background="subtle">
    ![Twilio phone number configuration](file:57b6794f-acc3-47dc-8088-952c71129f60)
  </Frame>
</Accordion>

<Info>
**Phone Number Detection**: ElevenLabs will automatically detect whether your number supports:
- **Inbound + Outbound**: Numbers purchased through Twilio
- **Outbound Only**: Numbers verified as caller IDs in Twilio

If your number is not found in either category, you'll receive an error asking you to verify it exists in your Twilio account.

</Info>

</Step>

<Step title="Assign your agent (Inbound-capable numbers only)">

If your phone number supports inbound calls, you can assign an agent to handle incoming calls.

<Frame background="subtle">
  ![Select agent for inbound calls](file:18161bb6-582e-4964-9b00-c1b8e28c8048)
</Frame>

<Note>
  Numbers that only support outbound calls (verified caller IDs) cannot be assigned to agents and
  will show as disabled in the agent dropdown.
</Note>

</Step>

</Steps>

Test the agent by giving the phone number a call. Your agent is now ready to handle inbound calls and engage with your customers.

<Tip>
  Monitor your first few calls in the [Calls History
  dashboard](https://elevenlabs.io/app/conversational-ai/history) to ensure everything is working as
  expected.
</Tip>

## Making Outbound Calls

<Info>
  Both purchased Twilio numbers and verified caller IDs can be used for outbound calls. The outbound
  call button will be disabled for numbers that don't support outbound calling.
</Info>

Your imported Twilio phone number can also be used to initiate outbound calls where your agent calls a specified phone number.

<Steps>

<Step title="Initiate an outbound call">

From the [**Phone Numbers**](https://elevenlabs.io/app/conversational-ai/phone-numbers) tab, locate your imported Twilio number and click the **Outbound call** button.

<Frame background="subtle">
  ![Outbound call button](file:353d9f6b-e078-4e11-93ea-c055f1b7a15f)
</Frame>

</Step>

<Step title="Configure the call">

In the Outbound Call modal:

1. Select the agent that will handle the conversation
2. Enter the phone number you want to call
3. Click **Send Test Call** to initiate the call

<Frame background="subtle">
  ![Outbound call configuration](file:e3843bc7-3ee9-4975-a758-c38e5922c0e4)
</Frame>

</Step>

</Steps>

Once initiated, the recipient will receive a call from your Twilio number. When they answer, your agent will begin the conversation.

<Tip>
  Outbound calls appear in your [Calls History
  dashboard](https://elevenlabs.io/app/conversational-ai/history) alongside inbound calls, allowing
  you to review all conversations.
</Tip>

<Note>
  When making outbound calls, your agent will be the initiator of the conversation, so ensure your
  agent has appropriate initial messages configured to start the conversation effectively.
</Note>
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
