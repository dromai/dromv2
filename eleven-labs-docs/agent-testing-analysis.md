---
title: Agent Testing
subtitle: Build confidence in your agent's behavior with automated testing
---

The agent testing framework enables you to move from slow, manual phone calls to a fast, automated, and repeatable testing process. Create comprehensive test suites that verify both conversational responses and tool usage, ensuring your agents behave exactly as intended before deploying to production.

## Overview

The framework consists of two complementary testing approaches:

- **Scenario Testing (LLM Evaluation)** - Validates conversational abilities and response quality
- **Tool Call Testing** - Ensures proper tool usage and parameter validation

Both test types can be created from scratch or directly from existing conversations, allowing you to quickly turn real-world interactions into repeatable test cases.

## Scenario Testing (LLM Evaluation)

Scenario testing evaluates your agent's conversational abilities by simulating interactions and assessing responses against defined success criteria.

### Creating a Scenario Test

<Frame background="subtle">
  <img
    src="file:325e8e19-6152-4280-87a7-e639d137244b"
    alt="Scenario Testing Interface"
  />
</Frame>

<Steps>
  <Step title="Define the scenario">
    Create context for the text. This can be multiple turns of interaction that sets up the specific scenario you want to evaluate. Our testing framework currently only supports evaluating a single next step in the conversation. For simulating entire conversations, see our [simulate conversation endpoint](/docs/api-reference/agents/simulate-conversation) and [conversation simulation guide](/docs/conversational-ai/guides/simulate-conversations).
    
    **Example scenario:**
    ```
    User: "I'd like to cancel my subscription. I've been charged twice this month and I'm frustrated."
    ```
  </Step>

  <Step title="Set success criteria">
    Describe in plain language what the agent's response should achieve. Be specific about the
    expected behavior, tone, and actions.

    **Example criteria:**
    - The agent should acknowledge the customer's frustration with empathy
    - The agent should offer to investigate the duplicate charge
    - The agent should provide clear next steps for cancellation or resolution
    - The agent should maintain a professional and helpful tone

  </Step>

  <Step title="Provide examples">
    Supply both success and failure examples to help the evaluator understand the nuances of your
    criteria.

    **Success Example:**
    > "I understand how frustrating duplicate charges can be. Let me look into this right away for you. I can see there were indeed two charges this month - I'll process a refund for the duplicate charge immediately. Would you still like to proceed with cancellation, or would you prefer to continue once this is resolved?"

    **Failure Example:**
    > "You need to contact billing department for refund issues. Your subscription will be cancelled."

  </Step>

  <Step title="Run the test">
    Execute the test to simulate the conversation with your agent. An LLM evaluator compares the
    actual response against your success criteria and examples to determine pass/fail status.
  </Step>
</Steps>

### Creating Tests from Conversations

Transform real conversations into test cases with a single click. This powerful feature creates a feedback loop for continuous improvement based on actual performance.

<Frame background="subtle">
  <img
    src="file:d44904dd-a1ab-4386-afcb-260854309881"
    alt="Creating test from conversation"
  />
</Frame>

When reviewing call history, if you identify a conversation where the agent didn't perform well:

1. Click "Create test from this conversation"
2. The framework automatically populates the scenario with the actual conversation context
3. Define what the correct behavior should have been
4. Add the test to your suite to prevent similar issues in the future

## Tool Call Testing

Tool call testing verifies that your agent correctly uses tools and passes the right parameters in specific situations. This is critical for actions like call transfers, data lookups, or external integrations.

### Creating a Tool Call Test

<Frame background="subtle">
  <img
    src="file:230bf5aa-c626-4b81-b15d-82ca7f29c1de"
    alt="Tool Call Testing Interface"
  />
</Frame>

<Steps>
  <Step title="Select the tool">
    Choose which tool you expect the agent to call in the given scenario (e.g.,
    `transfer_to_number`, `end_call`, `lookup_order`).
  </Step>
  <Step title="Define expected parameters">
    Specify what data the agent should pass to the tool. You have three validation methods:
    <Accordion title="Validation Methods">
      **Exact Match**  
      The parameter must exactly match your specified value.

      ```
      Transfer number: +447771117777
      ```

      **Regex Pattern**
      The parameter must match a specific pattern.

      ```
      Order ID: ^ORD-[0-9]{8}$
      ```

      **LLM Evaluation**
      An LLM evaluates if the parameter is semantically correct based on context.
      ```
      Message: "Should be a polite message mentioning the connection"
      ```
    </Accordion>

  </Step>
  <Step title="Configure dynamic variables">
    When testing in development, use dynamic variable values that match those that would be actual
    values in production. Example: `{{ customer_name }}` or `{{ order_id }}`
  </Step>
  <Step title="Run and validate">
    Execute the test to ensure the agent calls the correct tool with proper parameters.
  </Step>
</Steps>

### Critical Use Cases

Tool call testing is essential for high-stakes scenarios:

- **Emergency Transfers**: Ensure medical emergencies always route to the correct number
- **Data Security**: Verify sensitive information is never passed to unauthorized tools
- **Business Logic**: Confirm order lookups use valid formats and authentication

## Development Workflow

The framework supports an iterative development cycle that accelerates agent refinement:

<Steps>
  <Step title="Write tests first">
    Define the desired behavior by creating tests for new features or identified issues.
  </Step>
  <Step title="Test and iterate">
    Run tests instantly without saving changes. Watch them fail, then adjust your agent's prompts or
    configuration.
  </Step>
  <Step title="Refine until passing">
    Continue tweaking and re-running tests until all pass. The framework provides immediate feedback
    without requiring deployment.
  </Step>
  <Step title="Save with confidence">
    Once tests pass, save your changes knowing the agent behaves as intended.
  </Step>
</Steps>

## Batch Testing and CI/CD Integration

### Running Test Suites

Execute all tests at once to ensure comprehensive coverage:

1. Select multiple tests from your test library
2. Run as a batch to identify any regressions
3. Review consolidated results showing pass/fail status for each test

### CLI Integration

Integrate testing into your development pipeline using the ElevenLabs CLI:

```bash
# Run all tests for an agent
convai test --agent-id YOUR_AGENT_ID
```

This enables:

- Automated testing on every code change
- Prevention of regressions before deployment
- Consistent agent behavior across environments

## Best Practices

<Cards>
  <Card title="Evaluate agent persona consistency" icon="duotone shield-check">
    Test that your agent maintains its defined personality, tone, and behavioral boundaries across
    diverse conversation scenarios and emotional contexts.
  </Card>
  <Card title="Verify complex multi-turn reasoning" icon="duotone phone-volume">
    Create scenarios that test the agent's ability to maintain context, follow conditional logic,
    and handle state transitions across extended conversations.
  </Card>
  <Card title="Test against prompt injection attempts" icon="duotone list-check">
    Evaluate how your agent responds to attempts to override its instructions or extract sensitive
    system information through adversarial inputs.
  </Card>
  <Card title="Assess ambiguous intent resolution" icon="duotone flask">
    Test how effectively your agent clarifies vague requests, handles conflicting information, and
    navigates situations where user intent is unclear.
  </Card>
</Cards>

## Next Steps

- [View CLI Documentation](/docs/conversational-ai/libraries/agents-cli) for automated testing setup
- [Explore Tool Configuration](/docs/conversational-ai/customization/tools) to understand available tools
- [Read the Prompting Guide](/docs/conversational-ai/best-practices/prompting-guide) for writing testable prompts
---
title: Agent Analysis
subtitle: >-
  Analyze conversation quality and extract structured data from customer
  interactions.
---

Agent analysis provides powerful tools to systematically evaluate conversation performance and extract valuable information from customer interactions. These LLM-powered features help you measure agent effectiveness and gather actionable business insights.

<CardGroup cols={2}>
  <Card
    title="Success Evaluation"
    icon="chart-line"
    href="/docs/conversational-ai/customization/agent-analysis/success-evaluation"
  >
    Define custom criteria to assess conversation quality, goal achievement, and customer
    satisfaction.
  </Card>
  <Card
    title="Data Collection"
    icon="database"
    href="/docs/conversational-ai/customization/agent-analysis/data-collection"
  >
    Extract structured information from conversations such as contact details and business data.
  </Card>
</CardGroup>

## Overview

The Conversational AI platform provides two complementary analysis capabilities:

- **Success Evaluation**: Define custom metrics to assess conversation quality, goal achievement, and customer satisfaction
- **Data Collection**: Extract specific data points from conversations such as contact information, issue details, or any structured information

Both features process conversation transcripts using advanced language models to provide actionable insights that improve agent performance and business outcomes.

## Key Benefits

<AccordionGroup>
  <Accordion title="Performance Measurement">
    Track conversation success rates, customer satisfaction, and goal completion across all interactions to identify improvement opportunities.
  </Accordion>

    <Accordion title="Automated Data Extraction">
    Capture valuable business information without manual processing, reducing operational overhead and
    improving data accuracy.
    </Accordion>


    <Accordion title="Quality Assurance">
    Ensure agents follow required procedures and maintain consistent service quality through
    systematic evaluation.
    </Accordion>

  <Accordion title="Business Intelligence">
    Gather structured insights about customer preferences, behavior patterns, and interaction outcomes for strategic decision-making.
  </Accordion>
</AccordionGroup>

## Integration with Platform Features

Agent analysis integrates seamlessly with other Conversational AI capabilities:

- **[Post-call Webhooks](/docs/conversational-ai/workflows/post-call-webhooks)**: Receive evaluation results and extracted data via webhooks for integration with external systems
- **[Analytics Dashboard](/docs/conversational-ai/dashboard)**: View aggregated performance metrics and trends across all conversations
- **[Agent Transfer](/docs/conversational-ai/customization/tools/agent-transfer)**: Use evaluation criteria to determine when conversations should be escalated

## Getting Started

<Steps>
  <Step title="Choose your analysis approach">
    Determine whether you need success evaluation, data collection, or both based on your business objectives.
  </Step>

<Step title="Configure evaluation criteria">
  Set up [Success
  Evaluation](/docs/conversational-ai/customization/agent-analysis/success-evaluation) to measure
  conversation quality and goal achievement.
</Step>

<Step title="Set up data extraction">
  Configure [Data Collection](/docs/conversational-ai/customization/agent-analysis/data-collection)
  to capture structured information from conversations.
</Step>

  <Step title="Monitor and optimize">
    Review results regularly and refine your criteria and extraction rules based on performance data.
  </Step>
</Steps>
---
title: Success Evaluation
subtitle: >-
  Define custom criteria to assess conversation quality, goal achievement, and
  customer satisfaction.
---

Success evaluation allows you to define custom goals and success metrics for your conversations. Each criterion is evaluated against the conversation transcript and returns a result of `success`, `failure`, or `unknown`, along with a detailed rationale.

## Overview

Success evaluation uses LLM-powered analysis to assess conversation quality against your specific business objectives. This enables systematic performance measurement and quality assurance across all customer interactions.

### How It Works

Each evaluation criterion analyzes the conversation transcript using a custom prompt and returns:

- **Result**: `success`, `failure`, or `unknown`
- **Rationale**: Detailed explanation of why the result was chosen

### Types of Evaluation Criteria

<Tabs>
  <Tab title="Goal Prompt Criteria">
    **Goal prompt criteria** pass the conversation transcript along with a custom prompt to an LLM to verify if a specific goal was met. This is the most flexible type of evaluation and can be used for complex business logic.

    **Examples:**
    - Customer satisfaction assessment
    - Issue resolution verification
    - Compliance checking
    - Custom business rule validation

  </Tab>
</Tabs>

## Configuration

<Steps>
  <Step title="Access agent settings">
    Navigate to your agent's dashboard and select the **Analysis** tab to configure evaluation criteria.

    <Frame background="subtle">
      ![Analysis settings](file:f262a2e8-f9b8-4730-8292-d7dba1f0711c)
    </Frame>

  </Step>

  <Step title="Add evaluation criteria">
    Click **Add criteria** to create a new evaluation criterion.

    Define your criterion with:
    - **Identifier**: A unique name for the criterion (e.g., `user_was_not_upset`)
    - **Description**: Detailed prompt describing what should be evaluated

    <Frame background="subtle">
      ![Setting up evaluation criteria](file:de335e90-d8c5-4e6c-b233-e117323fb91b)
    </Frame>

  </Step>

  <Step title="View results">
    After conversations complete, evaluation results appear in your conversation history dashboard. Each conversation shows the evaluation outcome and rationale for every configured criterion.

    <Frame background="subtle">
      ![Evaluation results in conversation history](file:cde5eb6a-18c4-4ab4-9c09-d8ab56e43291)
    </Frame>

  </Step>
</Steps>

## Best Practices

<AccordionGroup>
  <Accordion title="Writing effective evaluation prompts">
    - Be specific about what constitutes success vs. failure
    - Include edge cases and examples in your prompt
    - Use clear, measurable criteria when possible
    - Test your prompts with various conversation scenarios
  </Accordion>

<Accordion title="Common evaluation criteria">
  - **Customer satisfaction**: "Mark as successful if the customer expresses satisfaction or their
  issue was resolved" - **Goal completion**: "Mark as successful if the customer completed the
  requested action (booking, purchase, etc.)" - **Compliance**: "Mark as successful if the agent
  followed all required compliance procedures" - **Issue resolution**: "Mark as successful if the
  customer's technical issue was resolved during the call"
</Accordion>

  <Accordion title="Handling ambiguous results">
    The `unknown` result is returned when the LLM cannot determine success or failure from the transcript. This often happens with:
    - Incomplete conversations
    - Ambiguous customer responses
    - Missing information in the transcript
    
    Monitor `unknown` results to identify areas where your criteria prompts may need refinement.
  </Accordion>
</AccordionGroup>

## Use Cases

<CardGroup cols={2}>
  <Card title="Customer Support Quality" icon="headset">
    Measure issue resolution rates, customer satisfaction, and support quality metrics to improve
    service delivery.
  </Card>

    <Card title="Sales Performance" icon="chart-up">
    Track goal achievement, objection handling, and conversion rates across sales conversations.
    </Card>


    <Card title="Compliance Monitoring" icon="shield-check">
    Ensure agents follow required procedures and capture necessary consent or disclosure
    confirmations.
    </Card>

  <Card title="Training & Development" icon="graduation-cap">
    Identify coaching opportunities and measure improvement in agent performance over time.
  </Card>
</CardGroup>

## Troubleshooting

<AccordionGroup>

  <Accordion title="Evaluation criteria returning unexpected results">
    - Review your prompt for clarity and specificity
    - Test with sample conversations to validate logic
    - Consider edge cases in your evaluation criteria
    - Check if the transcript contains sufficient information for evaluation
  </Accordion>
  <Accordion title="High frequency of 'unknown' results">
    - Ensure your prompts are specific about what information to look for - Consider if conversations
    contain enough context for evaluation - Review transcript quality and completeness - Adjust
    criteria to handle common edge cases
  </Accordion>
  <Accordion title="Performance considerations">
    - Each evaluation criterion adds processing time to conversation analysis
    - Complex prompts may take longer to evaluate
    - Consider the trade-off between comprehensive analysis and response time
    - Monitor your usage to optimize for your specific needs
  </Accordion>
</AccordionGroup>

<Info>
  Success evaluation results are available through [Post-call
  Webhooks](/docs/conversational-ai/workflows/post-call-webhooks) for integration with external
  systems and analytics platforms.
</Info>
---
title: Data Collection
subtitle: >-
  Extract structured information from conversations such as contact details and
  business data.
---

Data collection automatically extracts structured information from conversation transcripts using LLM-powered analysis. This enables you to capture valuable data points without manual processing, improving operational efficiency and data accuracy.

## Overview

Data collection analyzes conversation transcripts to identify and extract specific information you define. The extracted data is structured according to your specifications and made available for downstream processing and analysis.

### Supported Data Types

Data collection supports four data types to handle various information formats:

- **String**: Text-based information (names, emails, addresses)
- **Boolean**: True/false values (agreement status, eligibility)
- **Integer**: Whole numbers (quantity, age, ratings)
- **Number**: Decimal numbers (prices, percentages, measurements)

## Configuration

<Steps>
  <Step title="Access data collection settings">
    In the **Analysis** tab of your agent settings, navigate to the **Data collection** section.

    <Frame background="subtle">
      ![Setting up data collection](file:d37d9e73-84d4-4d90-be7a-a38b3f0a8e72)
    </Frame>

  </Step>

  <Step title="Add data collection items">
    Click **Add item** to create a new data extraction rule.

    Configure each item with:
    - **Identifier**: Unique name for the data field (e.g., `email`, `customer_rating`)
    - **Data type**: Select from string, boolean, integer, or number
    - **Description**: Detailed instructions on how to extract the data from the transcript

    <Info>
      The description field is passed to the LLM and should be as specific as possible about what to extract and how to format it.
    </Info>

  </Step>

  <Step title="Review extracted data">
    Extracted data appears in your conversation history, allowing you to review what information was captured from each interaction.

    <Frame background="subtle">
      ![Data collection results in conversation history](file:9c264b22-8d6c-4a49-a150-09691c33f069)
    </Frame>

  </Step>
</Steps>

## Best Practices

<AccordionGroup>
  <Accordion title="Writing effective extraction prompts">
    - Be explicit about the expected format (e.g., "email address in the format user@domain.com")
    - Specify what to do when information is missing or unclear
    - Include examples of valid and invalid data
    - Mention any validation requirements
  </Accordion>

  <Accordion title="Common data collection examples">
    **Contact Information:**
    - `email`: "Extract the customer's email address in standard format (user@domain.com)"
    - `phone_number`: "Extract the customer's phone number including area code"
    - `full_name`: "Extract the customer's complete name as provided"

    **Business Data:**
    - `issue_category`: "Classify the customer's issue into one of: technical, billing, account, or general"
    - `satisfaction_rating`: "Extract any numerical satisfaction rating given by the customer (1-10 scale)"
    - `order_number`: "Extract any order or reference number mentioned by the customer"

    **Behavioral Data:**
    - `was_angry`: "Determine if the customer expressed anger or frustration during the call"
    - `requested_callback`: "Determine if the customer requested a callback or follow-up"

  </Accordion>

  <Accordion title="Handling missing or unclear data">
    When the requested data cannot be found or is ambiguous in the transcript, the extraction will return null or empty values. Consider:
    - Using conditional logic in your applications to handle missing data
    - Creating fallback criteria for incomplete extractions
    - Training agents to consistently gather required information
  </Accordion>
</AccordionGroup>

## Data Type Guidelines

<Tabs>
  <Tab title="String">
    Use for text-based information that doesn't fit other types.
    
    **Examples:**
    - Customer names
    - Email addresses 
    - Product categories
    - Issue descriptions
    
    **Best practices:**
    - Specify expected format when relevant
    - Include validation requirements
    - Consider standardization needs
  </Tab>

  <Tab title="Boolean">
    Use for yes/no, true/false determinations.
    
    **Examples:**
    - Customer agreement status
    - Eligibility verification
    - Feature requests
    - Complaint indicators
    
    **Best practices:**
    - Clearly define what constitutes true vs. false
    - Handle ambiguous responses
    - Consider default values for unclear cases
  </Tab>
  <Tab title="Integer">
    Use for whole number values.
    
    **Examples:**
    - Customer age
    - Product quantities
    - Rating scores
    - Number of issues
    
    **Best practices:**
    - Specify valid ranges when applicable
    - Handle non-numeric responses
    - Consider rounding rules if needed
  </Tab>
  <Tab title="Number">
    Use for decimal or floating-point values.
    
    **Examples:**
    - Monetary amounts
    - Percentages
    - Measurements
    - Calculated scores
    
    **Best practices:**
    - Specify precision requirements
    - Include currency or unit context
    - Handle different number formats
  </Tab>
</Tabs>

## Use Cases

<CardGroup cols={2}>

<Card title="Lead Qualification" icon="user-check">
  Extract contact information, qualification criteria, and interest levels from sales conversations.
</Card>

<Card title="Customer Intelligence" icon="brain">
  Gather structured data about customer preferences, feedback, and behavior patterns for strategic
  insights.
</Card>

<Card title="Support Analytics" icon="chart-line">
  Capture issue categories, resolution details, and satisfaction scores for operational
  improvements.
</Card>

<Card title="Compliance Documentation" icon="clipboard-check">
  Extract required disclosures, consents, and regulatory information for audit trails.
</Card>

</CardGroup>

## Troubleshooting

<AccordionGroup>

  <Accordion title="Data extraction returning empty values">
    - Verify the data exists in the conversation transcript
    - Check if your extraction prompt is specific enough
    - Ensure the data type matches the expected format
    - Consider if the information was communicated clearly during the conversation
  </Accordion>
  <Accordion title="Inconsistent data formats">
    - Review extraction prompts for format specifications 
    - Add validation requirements to prompts 
    - Consider post-processing for data standardization 
    - Test with various conversation scenarios
  </Accordion>
  <Accordion title="Performance considerations">
    - Each data collection rule adds processing time
    - Complex extraction logic may take longer to evaluate
    - Monitor extraction accuracy vs. speed requirements
    - Optimize prompts for efficiency when possible
  </Accordion>
</AccordionGroup>

<Info>
  Extracted data is available through [Post-call
  Webhooks](/docs/conversational-ai/workflows/post-call-webhooks) for integration with CRM systems,
  databases, and analytics platforms.
</Info>
