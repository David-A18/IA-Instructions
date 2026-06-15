# Gemini API Structured Output And Tools

Use this route when building with the Gemini API, tool use, structured output, or grounding.

## Design Rules

- Define the task, input variables, and output shape explicitly.
- Use structured output when application code depends on response shape.
- Keep stable instructions before dynamic user input and retrieved context.
- Use tools or grounding when the task needs fresh or external information.
- Log model, prompt version, tool calls, latency, errors, and token usage for production paths.

## Prompt Rules

- Prefer concise instructions with concrete acceptance criteria.
- Keep examples small and targeted.
- Do not mix unrelated policies in one prompt.
- Validate model output before using it in downstream code.

## Current Docs

Use official Google Gemini documentation for current model names, API fields, SDK behavior, and Code Assist capabilities.
