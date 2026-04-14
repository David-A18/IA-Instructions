# API Patterns — Client Setup, Streaming, Errors, Batch

## Client setup

```bash
pip install anthropic
export ANTHROPIC_API_KEY="sk-ant-..."
```

```python
from anthropic import Anthropic

client = Anthropic()  # reads ANTHROPIC_API_KEY from env

# Or explicit:
client = Anthropic(api_key="sk-ant-...")
```

## Basic message

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    system="You are a FinOps expert.",
    messages=[
        {"role": "user", "content": "Analyze this cost report."}
    ],
)
text = response.content[0].text
```

## Streaming

For long responses, stream token by token:

```python
with client.messages.stream(
    model="claude-sonnet-4-6",
    max_tokens=4096,
    messages=[{"role": "user", "content": prompt}],
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)
```

## Error handling

```python
from anthropic import (
    APIError,
    APIConnectionError,
    RateLimitError,
    APIStatusError,
)

try:
    response = client.messages.create(...)
except RateLimitError:
    # Back off and retry (SDK has built-in retries)
    time.sleep(60)
except APIConnectionError:
    # Network issue
    pass
except APIStatusError as e:
    print(f"Status {e.status_code}: {e.message}")
```

The SDK auto-retries on:
- 408 Request Timeout
- 409 Conflict
- 429 Rate Limit
- 5xx Server Errors

Default: 2 retries with exponential backoff.

## Batch API (50% cheaper)

For non-real-time tasks, use the batch API:

```python
batch = client.messages.batches.create(
    requests=[
        {
            "custom_id": "cost-report-jan",
            "params": {
                "model": "claude-sonnet-4-6",
                "max_tokens": 2048,
                "messages": [{"role": "user", "content": "Analyze January costs..."}],
            },
        },
        {
            "custom_id": "cost-report-feb",
            "params": {
                "model": "claude-sonnet-4-6",
                "max_tokens": 2048,
                "messages": [{"role": "user", "content": "Analyze February costs..."}],
            },
        },
    ],
)

# Poll for results
import time
while batch.processing_status != "ended":
    time.sleep(10)
    batch = client.messages.batches.retrieve(batch.id)

# Get results
for result in client.messages.batches.results(batch.id):
    print(result.custom_id, result.result.message.content[0].text)
```

**Batch pricing**: 50% cheaper than real-time. Results within 24 hours.

## Models quick reference

| Model | Best for | Speed | Cost |
|-------|----------|-------|------|
| `claude-sonnet-4-6` | Default, balanced | Fast | $3/$15 per 1M |
| `claude-opus-4-1` | Complex reasoning | Slower | $15/$75 per 1M |
| `claude-haiku-3-5-20241022` | Fast, cheap tasks | Fastest | $0.80/$4 per 1M |

## Counting tokens before sending

```python
count = client.messages.count_tokens(
    model="claude-sonnet-4-6",
    messages=[{"role": "user", "content": large_text}],
)
print(f"This will use {count.input_tokens} input tokens")
```

## Relevance to our project

- **Streaming**: for CLI real-time output when Claude analyzes costs
- **Batch API**: process multiple CUR reports at 50% cost
- **Token counting**: budget estimation before expensive analysis calls
- **Error handling**: robust boto3-like retry pattern for production
