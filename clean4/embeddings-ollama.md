## Recommended models

## Generate embeddings

- CLI
- cURL
- Python
- JavaScript

Generate embeddings directly from the command line:You can also pipe text to generate embeddings:Output is a JSON array.

The 

`/api/embed` endpoint returns L2‑normalized (unit‑length) vectors.
## Generate a batch of embeddings

Pass an array of strings to`input`.
- cURL
- Python
- JavaScript

## Tips

- Use cosine similarity for most semantic search use cases.
- Use the same embedding model for both indexing and querying.