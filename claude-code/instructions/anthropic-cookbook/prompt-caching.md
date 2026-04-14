# Prompt Caching — 90% Cost Reduction

Source: [prompt_caching.ipynb](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/prompt_caching.ipynb)

## Pricing

| Type | Cost vs base input |
|------|-------------------|
| Cache write | 1.25x |
| Cache read | **0.10x** (90% cheaper) |
| Cache miss | 1.0x (normal price) |

## Minimum cacheable length

| Model | Min tokens |
|-------|-----------|
| Sonnet | 1,024 |
| Opus / Haiku 4.5 | 4,096 |

Cache TTL: **5 min** (refreshed on each hit). 1-hour TTL available at 2x base input.

## Method 1: Automatic caching (recommended)

One-line change. System manages breakpoints automatically.

```python
import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=300,
    cache_control={"type": "ephemeral"},  # <-- this is the only change
    messages=[
        {"role": "user", "content": large_context + "\n\nYour question here"}
    ],
)
```

### Results from cookbook (187k tokens context)

| Call | Time | Behavior |
|------|------|----------|
| No caching | 4.89s | Baseline |
| First call (cache write) | 4.28s | Similar to baseline |
| Second call (cache hit) | **1.48s** | **3.3x speedup** |

## Method 2: Explicit cache breakpoints

Fine-grained control. Place `cache_control` on specific content blocks.

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=300,
    system=[
        {
            "type": "text",
            "text": large_static_context,
            "cache_control": {"type": "ephemeral"},  # cache the system prompt
        },
    ],
    messages=[
        {"role": "user", "content": "Dynamic question here"}
    ],
)
```

Up to **4 explicit breakpoints** per request. Can combine with automatic caching.

## Multi-turn conversations (automatic caching)

The breakpoint moves forward automatically as conversation grows:

| Request | Cache behavior |
|---------|---------------|
| Turn 1 | System + User:A → cache write |
| Turn 2 | System + User:A read from cache; new content written |
| Turn 3 | Everything through Turn 2 read from cache |

```python
conversation = []

for question in questions:
    conversation.append({"role": "user", "content": question})
    
    response = client.messages.create(
        model="claude-sonnet-4-6",
        max_tokens=300,
        cache_control={"type": "ephemeral"},
        system=static_system_prompt,
        messages=conversation,
    )
    
    conversation.append({"role": "assistant", "content": response.content[0].text})
```

## Reading cache usage from response

```python
usage = response.usage
print(f"Input tokens: {usage.input_tokens}")
print(f"Cache write: {getattr(usage, 'cache_creation_input_tokens', 0)}")
print(f"Cache read: {getattr(usage, 'cache_read_input_tokens', 0)}")
print(f"Output tokens: {usage.output_tokens}")
```

## Key rules for cache hits

1. **Static content first, dynamic content last** — cache prefix must be identical
2. **Don't include timestamps/session IDs** in cached portions — any change = cache miss
3. **Structure**: system prompt (cached) → tool definitions (cached) → history (semi-cached) → current message (dynamic)
4. **Start with automatic caching** — switch to explicit only if you need fine-grained control

## Relevance to our project

For FinOps Autopilot API features:
- Cache the CUR schema and rules as system prompt
- Cache tool definitions for cost analysis functions
- Multi-turn: user asks follow-up questions about the same cost report
