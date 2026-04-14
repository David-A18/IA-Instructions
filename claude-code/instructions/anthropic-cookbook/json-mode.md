# JSON Mode — Reliable Structured Output

Source: [how_to_enable_json_mode.ipynb](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/how_to_enable_json_mode.ipynb)

## Problem

Claude doesn't have constrained JSON sampling. It may add preamble text before/after the JSON.

## 4 techniques for reliable JSON

### 1. Prefill the assistant response

Force Claude to start with `{` by prefilling:

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "Return a JSON dict of AWS services and their categories."},
        {"role": "assistant", "content": "Here is the JSON requested:\n{"},
    ],
)
json_str = "{" + response.content[0].text
result = json.loads(json_str[:json_str.rfind("}") + 1])
```

### 2. Extract JSON from response text

When Claude wraps JSON in explanation:

```python
def extract_json(text: str) -> dict:
    json_start = text.index("{")
    json_end = text.rfind("}")
    return json.loads(text[json_start:json_end + 1])
```

### 3. XML tags for multiple JSON objects

When you need several JSON outputs in one response:

```python
prompt = """Analyze these costs and return:
1. A summary in <summary> tags as JSON
2. Anomalies in <anomalies> tags as JSON array
3. Recommendations in <recommendations> tags as JSON array"""

# Extract with regex
import re

def extract_between_tags(tag: str, text: str) -> list[str]:
    return re.findall(f"<{tag}>(.+?)</{tag}>", text, re.DOTALL)

summary = json.loads(extract_between_tags("summary", response)[0])
anomalies = json.loads(extract_between_tags("anomalies", response)[0])
```

### 4. System prompt + strict instructions

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    system="You are a JSON API. Always respond with valid JSON only. No explanation.",
    messages=[
        {"role": "user", "content": "List top 5 AWS services by cost."},
    ],
)
```

## Best practice summary

| Technique | When to use |
|-----------|-------------|
| Prefill with `{` | Single JSON object, most reliable |
| Extract from text | When you also want Claude's reasoning |
| XML tags | Multiple JSON outputs in one call |
| System prompt | Simple cases, API-like behavior |

## Relevance to our project

FinOps Autopilot outputs JSON reports (`finops/reports/json_reporter.py`). These patterns apply when building API endpoints that return structured cost data.
