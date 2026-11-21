---
id: collect-261121-veille-ia/gemini-google/structured-outputs-ollama
title: "Structured Outputs - Ollama"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["structured", "outputs", "json", "python", "javascript", "schema", "format", "ollama"]
source: https://docs.ollama.com/capabilities/structured-outputs
source_anchor: ""
source_lines: [1, 28]
section: "Gemini_Google"
---
Ollama’s Cloud currently does not support structured outputs.

## Generating structured JSON

- cURL
- Python
- JavaScript

## Generating structured JSON with a schema

Provide a JSON schema to the`format` field.
It is ideal to also pass the JSON schema as a string in the prompt to ground the model’s response.

- cURL
- Python
- JavaScript

## Example: Extract structured data

Define the objects you want returned and let the model populate the fields:
## Example: Vision with structured outputs

Vision models accept the same`format` parameter, enabling deterministic descriptions of images:
## Tips for reliable structured outputs

- Define schemas with Pydantic (Python) or Zod (JavaScript) so they can be reused for validation.
- Lower the temperature (e.g., set it to `0` ) for more deterministic completions.
- Structured outputs work through the OpenAI-compatible API via `response_format`
