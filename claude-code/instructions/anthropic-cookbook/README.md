# Anthropic Cookbook — Quick Reference for Claude Code

## Source

- **Repo**: https://github.com/anthropics/anthropic-cookbook (40k+ stars, MIT)
- **Docs**: https://docs.anthropic.com
- **Courses**: https://github.com/anthropics/courses
- **AWS integration**: https://github.com/aws-samples/anthropic-on-aws

## What's inside

Official Anthropic notebooks with copy-paste patterns for the Claude API. Files in this folder extract the most useful patterns so Claude Code can reference them instantly without cloning the repo.

## Files in this folder

| File | Content |
|------|---------|
| `README.md` | This index |
| `prompt-caching.md` | Prompt caching patterns (90% cost reduction) |
| `json-mode.md` | Reliable JSON output techniques |
| `sql-generation.md` | Natural language → SQL with Claude |
| `tool-use.md` | Function calling / tool use patterns |
| `api-patterns.md` | Client setup, streaming, error handling, batch API |

## Install the SDK

```bash
pip install anthropic
```

## Quick client setup

```python
from anthropic import Anthropic

client = Anthropic()  # reads ANTHROPIC_API_KEY from env
```

## Cookbook index (full repo)

| Category | Notebook | Direct link |
|----------|----------|-------------|
| **Caching** | Prompt caching | [prompt_caching.ipynb](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/prompt_caching.ipynb) |
| **JSON** | JSON mode | [how_to_enable_json_mode.ipynb](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/how_to_enable_json_mode.ipynb) |
| **SQL** | SQL queries | [how_to_make_sql_queries.ipynb](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/how_to_make_sql_queries.ipynb) |
| **Tools** | Tool use basics | [tool_use/](https://github.com/anthropics/anthropic-cookbook/tree/main/tool_use) |
| **RAG** | Retrieval augmented gen | [capabilities/retrieval_augmented_generation/](https://github.com/anthropics/anthropic-cookbook/tree/main/capabilities/retrieval_augmented_generation) |
| **Classification** | Text classification | [capabilities/classification/](https://github.com/anthropics/anthropic-cookbook/tree/main/capabilities/classification) |
| **Summarization** | Summarization | [capabilities/summarization/](https://github.com/anthropics/anthropic-cookbook/tree/main/capabilities/summarization) |
| **Vision** | Image analysis | [multimodal/](https://github.com/anthropics/anthropic-cookbook/tree/main/multimodal) |
| **Sub-agents** | Haiku as sub-agent | [using_sub_agents.ipynb](https://github.com/anthropics/anthropic-cookbook/blob/main/multimodal/using_sub_agents.ipynb) |
| **Evals** | Automated evaluations | [building_evals.ipynb](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/building_evals.ipynb) |
| **Moderation** | Content filter | [building_moderation_filter.ipynb](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/building_moderation_filter.ipynb) |
| **PDF** | PDF upload + summarize | [pdf_upload_summarization.ipynb](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/pdf_upload_summarization.ipynb) |
