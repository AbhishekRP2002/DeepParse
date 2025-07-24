# DeepParse

## Overview
DeepParse is an agentic engine to transform raw, freeform text into complex, deeply nested JSON structures, strictly following a user-supplied schema. It is designed to handle input texts of varying length and density and schemas of varying complexity, with advanced chunking, schema decomposition, and agent orchestration for robust, scalable extraction.

## Approach

1. **Agentic System**: Uses modular agents for extraction and validation, orchestrated via LangGraph for state management and control flow.
2. **Chunking**: If the input text is very long (>5k tokens), it is semantically chunked and stored in a runtime database.
3. **Schema Analysis**: The JSON schema is analyzed to identify critical/optional fields, logical groupings, and dependencies. If the schema is too large, it is decomposed into smaller sub-schemas using a dependency DAG.
4. **Matching**: For each sub-schema, the most relevant text chunks are identified for extraction.
5. **Extraction Agents**: Two types of extraction agents are used:
    - A fast, smaller model for high-precision literals/enums.
    - A larger model for complex free-text fields.
   Both use Instructor for structured output and retries.
6. **Validation Agent (Optional)**: Validates JSON fragments, checks rules, ensures cross-field consistency, and flags low-confidence or invalid fields for review.
7. **Assembly**: Extracted fragments are merged into the final JSON, with confidence scores and spans propagated.

## Folder Structure

```
deepparse/
│
├── deepparse/
│   ├── __init__.py
│   ├── agent/
│   │   ├── __init__.py
│   │   ├── extraction_agent.py
│   │   ├── validation_agent.py
│   │   └── base_agent.py
│   ├── chunking/
│   │   ├── __init__.py
│   │   ├── semantic_chunker.py
│   │   └── chunk_db.py
│   ├── schema/
│   │   ├── __init__.py
│   │   ├── schema_analyzer.py
│   │   └── schema_store.py
│   ├── matching/
│   │   ├── __init__.py
│   │   └── relevance_matcher.py
│   ├── orchestration/
│   │   ├── __init__.py
│   │   └── langgraph_workflow.py
│   ├── extraction/
│   │   ├── __init__.py
│   │   └── instructor_client.py
│   ├── utils/
│   │   ├── __init__.py
│   │   └── helpers.py
│   ├── config.py
│   └── main.py
│
├── tests/
│   └── ... (mirrors package structure)
│
├── examples/
│   └── example_usage.py
│
├── pyproject.toml
├── README.md
└── LICENSE
```

## Getting Started

1. Install dependencies with Poetry:
   ```bash
   poetry install
   ```

## References
