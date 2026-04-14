# SQL Generation — Natural Language to SQL

Source: [how_to_make_sql_queries.ipynb](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/how_to_make_sql_queries.ipynb)

## Pattern

1. Pass database schema as context
2. Ask question in natural language
3. Claude returns SQL query
4. Execute the query

## Implementation

```python
from anthropic import Anthropic

client = Anthropic()

def natural_language_to_sql(question: str, schema: str) -> str:
    """Convert natural language question to SQL query."""
    response = client.messages.create(
        model="claude-sonnet-4-6",
        max_tokens=2048,
        messages=[
            {
                "role": "user",
                "content": f"""Here is the database schema:

{schema}

Given this schema, output a SQL query to answer the following question.
Only output the SQL query and nothing else.

Question: {question}""",
            }
        ],
    )
    return response.content[0].text
```

## Example with our CUR schema

```python
CUR_SCHEMA = """
CREATE TABLE cur (
    account_id      VARCHAR,
    usage_date      VARCHAR,
    product_code    VARCHAR,
    product_name    VARCHAR,
    usage_type      VARCHAR,
    region          VARCHAR,
    unblended_cost  DOUBLE,
    tag_environment VARCHAR,
    tag_team        VARCHAR,
    service_category VARCHAR,
    cost_owner      VARCHAR
)
"""

query = natural_language_to_sql(
    "What are the top 5 most expensive services this month?",
    CUR_SCHEMA,
)
# Returns: SELECT product_name, SUM(unblended_cost) AS total_cost
#          FROM cur GROUP BY product_name ORDER BY total_cost DESC LIMIT 5
```

## Execute against DuckDB

```python
from finops.ingestion.local_store import LocalStore

store = LocalStore("finops.duckdb")
sql = natural_language_to_sql("Which team spent the most last week?", CUR_SCHEMA)
results = store.query(sql)
```

## Safety: always validate before execution

```python
ALLOWED_KEYWORDS = {"SELECT", "FROM", "WHERE", "GROUP", "ORDER", "LIMIT", "JOIN", "HAVING"}
FORBIDDEN_KEYWORDS = {"DROP", "DELETE", "INSERT", "UPDATE", "ALTER", "CREATE", "TRUNCATE"}

def is_safe_query(sql: str) -> bool:
    upper = sql.upper()
    return (
        any(kw in upper for kw in ALLOWED_KEYWORDS)
        and not any(kw in upper for kw in FORBIDDEN_KEYWORDS)
    )
```

## Relevance to our project

Directly applicable to FinOps Autopilot:
- `finops/ingestion/local_store.py` already has DuckDB with CUR schema
- This pattern enables a "natural language cost query" CLI command
- Schema is already defined, just pass it to Claude
