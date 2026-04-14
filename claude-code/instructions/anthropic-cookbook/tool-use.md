# Tool Use — Function Calling with Claude

Source: [tool_use/](https://github.com/anthropics/anthropic-cookbook/tree/main/tool_use)

## Concept

Define tools (functions) that Claude can invoke. Claude returns a `tool_use` block with the function name and arguments. Your code executes the function and returns the result.

## Basic pattern

```python
from anthropic import Anthropic

client = Anthropic()

tools = [
    {
        "name": "get_cost_by_service",
        "description": "Get AWS cost for a specific service over a date range",
        "input_schema": {
            "type": "object",
            "properties": {
                "service_name": {
                    "type": "string",
                    "description": "AWS service name (e.g., AmazonEC2, AmazonS3)",
                },
                "start_date": {
                    "type": "string",
                    "description": "Start date in YYYY-MM-DD format",
                },
                "end_date": {
                    "type": "string",
                    "description": "End date in YYYY-MM-DD format",
                },
            },
            "required": ["service_name", "start_date", "end_date"],
        },
    },
]

response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    tools=tools,
    messages=[
        {"role": "user", "content": "How much did we spend on EC2 last week?"}
    ],
)
```

## Handling tool calls

```python
import json

for block in response.content:
    if block.type == "tool_use":
        tool_name = block.name
        tool_input = block.input
        tool_id = block.id
        
        # Execute the actual function
        result = execute_tool(tool_name, tool_input)
        
        # Send result back to Claude
        followup = client.messages.create(
            model="claude-sonnet-4-6",
            max_tokens=1024,
            tools=tools,
            messages=[
                {"role": "user", "content": "How much did we spend on EC2 last week?"},
                {"role": "assistant", "content": response.content},
                {
                    "role": "user",
                    "content": [
                        {
                            "type": "tool_result",
                            "tool_use_id": tool_id,
                            "content": json.dumps(result),
                        }
                    ],
                },
            ],
        )
```

## Multiple tools in one request

Claude can call multiple tools. Just define them all in the `tools` array:

```python
tools = [
    {"name": "get_cost_by_service", ...},
    {"name": "get_anomalies", ...},
    {"name": "get_rightsizing_recommendations", ...},
]
```

## Tool loop pattern (agentic)

For complex tasks where Claude needs to call tools iteratively:

```python
messages = [{"role": "user", "content": user_question}]

while True:
    response = client.messages.create(
        model="claude-sonnet-4-6",
        max_tokens=4096,
        tools=tools,
        messages=messages,
    )
    
    if response.stop_reason == "end_turn":
        break  # Claude is done, has final answer
    
    # Process tool calls
    messages.append({"role": "assistant", "content": response.content})
    tool_results = []
    for block in response.content:
        if block.type == "tool_use":
            result = execute_tool(block.name, block.input)
            tool_results.append({
                "type": "tool_result",
                "tool_use_id": block.id,
                "content": json.dumps(result),
            })
    messages.append({"role": "user", "content": tool_results})
```

## Relevance to our project

FinOps Autopilot can expose its analysis functions as tools:
- `get_cost_by_service` → `local_store.daily_cost_by_service()`
- `get_cost_by_team` → `local_store.cost_by_team()`
- `detect_anomalies` → `anomaly_detector.detect()`
- `get_recommendations` → `ec2_rightsizer.analyze()`

This enables a chat interface: "What's driving our cost increase?" → Claude calls tools → returns analysis.
