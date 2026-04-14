# Context7 Usage Guide — Prompts, Library IDs, Tips

## Basic usage: add "use context7"

Append `use context7` to any prompt that involves a library:

```
How do I create a DuckDB connection and run SQL queries in Python? use context7
```

```
Show me boto3 S3 get_object with error handling. use context7
```

```
How do I define a Terraform aws_cur_report_definition resource? use context7
```

Context7 will:
1. Resolve the library → find Context7 ID
2. Fetch current docs + code examples for your question
3. Inject them into the response context

## Use a specific library ID (faster)

If you know the library, skip the resolution step with the slash syntax:

```
How do I use DuckDB Python API? use library /duckdb/duckdb for API and docs.
```

```
Show me Typer CLI command groups. use library /tiangolo/typer for API and docs.
```

### Common library IDs for our project

| Library | Context7 ID | What we use it for |
|---------|-------------|-------------------|
| boto3 | `/boto/boto3` | AWS SDK (S3, CloudWatch, CE, STS) |
| DuckDB | `/duckdb/duckdb` | Local SQL store for CUR data |
| Typer | `/tiangolo/typer` | CLI framework |
| Rich | `/textualize/rich` | Terminal UI (tables, progress) |
| PyYAML | `/yaml/pyyaml` | Config file loading |
| jsonschema | `/python-jsonschema/jsonschema` | Config validation |
| Jinja2 | `/pallets/jinja` | Report templates |
| kubernetes | `/kubernetes-client/python` | K8s API access |
| pytest | `/pytest-dev/pytest` | Testing |
| moto | `/getmoto/moto` | AWS mocking for tests |
| ruff | `/astral-sh/ruff` | Linting + formatting |
| Terraform AWS | `/hashicorp/terraform-provider-aws` | IaC resources |

## Specify a version

Mention the version in your prompt:

```
How do I use DuckDB 1.1 Python API? use context7
```

```
Show me boto3 1.35 S3 client methods. use context7
```

Context7 automatically matches the appropriate version docs.

## Real examples for our FinOps project

### CUR ingestion

```
How do I read a large CSV file with DuckDB Python API efficiently? use context7
```

### Config validation

```
How do I validate a YAML config against a JSON Schema using jsonschema Draft7Validator? use context7
```

### CLI

```
How do I create a Typer app with subcommands that have shared options? use context7
```

### Testing

```
How do I mock S3 get_object with moto in pytest? use context7
```

### Terraform

```
How do I configure aws_athena_workgroup with byte scan limits? use context7
```

### Reports

```
How do I render a Jinja2 template from a file path with variables? use context7
```

## Auto-trigger rule (optional)

Add to your CLAUDE.md or Cursor rules to trigger Context7 automatically:

```
Always use Context7 when I need library/API documentation, code generation,
setup or configuration steps without me having to explicitly ask.
```

## Why this saves tokens

| Scenario | Without Context7 | With Context7 |
|----------|-------------------|--------------|
| Hallucinated API → correction cycle | 3-5 back-and-forth messages | 1 message, correct API |
| Deprecated method → "that doesn't exist" | 2-3 extra messages | Right method first time |
| Wrong argument order → runtime error | Debug cycle (expensive) | Correct signature from docs |

Average savings: **2-4 fewer messages per library question** = hundreds of output tokens saved.
