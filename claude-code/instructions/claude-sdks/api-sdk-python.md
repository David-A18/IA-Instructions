# API SDK Python — `anthropic`

Repo: https://github.com/anthropics/anthropic-sdk-python

## Install

```bash
pip install anthropic
export ANTHROPIC_API_KEY="sk-ant-..."
```

## Basic message

```python
from anthropic import Anthropic

client = Anthropic()  # reads ANTHROPIC_API_KEY from env

response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    system="You are a FinOps expert.",
    messages=[{"role": "user", "content": "Analyze this cost report."}],
)
print(response.content[0].text)
```

## Streaming (sync)

```python
with client.messages.stream(
    model="claude-sonnet-4-6",
    max_tokens=4096,
    messages=[{"role": "user", "content": prompt}],
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)

final = stream.get_final_message()
```

## Streaming (async)

```python
import asyncio
from anthropic import AsyncAnthropic

client = AsyncAnthropic()

async def main():
    async with client.messages.stream(
        model="claude-sonnet-4-6",
        max_tokens=1024,
        messages=[{"role": "user", "content": "Hello"}],
    ) as stream:
        async for event in stream:
            if event.type == "text":
                print(event.text, end="", flush=True)

    final = await stream.get_final_message()

asyncio.run(main())
```

### Stream event types

| Event | Content |
|-------|---------|
| `text` | Text deltas (`.text` and `.snapshot`) |
| `input_json` | JSON deltas for tool use |
| `content_block_stop` | Block finished |
| `message_stop` | Full message complete |

### Stream helper methods

| Method | Returns |
|--------|---------|
| `stream.text_stream` | Iterator of text deltas only |
| `stream.get_final_message()` | Full accumulated Message |
| `stream.get_final_text()` | Concatenated text content |
| `stream.close()` | Abort the request |

## Tool use (function calling)

```python
tools = [
    {
        "name": "get_cost_by_service",
        "description": "Get AWS cost for a service over a date range",
        "input_schema": {
            "type": "object",
            "properties": {
                "service": {"type": "string"},
                "start_date": {"type": "string", "description": "YYYY-MM-DD"},
                "end_date": {"type": "string", "description": "YYYY-MM-DD"},
            },
            "required": ["service", "start_date", "end_date"],
        },
    },
]

response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    tools=tools,
    messages=[{"role": "user", "content": "How much did EC2 cost last week?"}],
)

# Process tool calls
for block in response.content:
    if block.type == "tool_use":
        result = your_function(block.name, block.input)
        # Send result back (see anthropic-cookbook/tool-use.md for full loop)
```

## Prompt caching

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=300,
    cache_control={"type": "ephemeral"},  # automatic caching
    system=large_static_prompt,
    messages=[{"role": "user", "content": question}],
)
# See anthropic-cookbook/prompt-caching.md for full guide
```

## Batch API (50% cheaper)

```python
batch = client.messages.batches.create(
    requests=[
        {
            "custom_id": "report-jan",
            "params": {
                "model": "claude-sonnet-4-6",
                "max_tokens": 2048,
                "messages": [{"role": "user", "content": "Analyze Jan costs..."}],
            },
        },
    ],
)
# Results within 24 hours. 50% discount vs real-time.
```

## Token counting

```python
count = client.messages.count_tokens(
    model="claude-sonnet-4-6",
    messages=[{"role": "user", "content": text}],
)
print(f"Will use {count.input_tokens} input tokens")
```

## Error handling

```python
from anthropic import RateLimitError, APIConnectionError, APIStatusError

try:
    response = client.messages.create(...)
except RateLimitError:
    time.sleep(60)  # SDK auto-retries 2x with backoff
except APIConnectionError:
    pass  # network issue
except APIStatusError as e:
    print(f"HTTP {e.status_code}: {e.message}")
```

Auto-retry on: 408, 409, 429, 5xx. Default 2 retries, exponential backoff.

## Models

| Model | Speed | Input / Output per 1M |
|-------|-------|----------------------|
| `claude-sonnet-4-6` | Fast | $3 / $15 |
| `claude-opus-4-1` | Slow, smartest | $15 / $75 |
| `claude-haiku-3-5-20241022` | Fastest | $0.80 / $4 |
