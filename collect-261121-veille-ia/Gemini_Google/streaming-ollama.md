---
id: collect-261121-veille-ia/gemini-google/streaming-ollama
title: "Streaming - Ollama"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["tool", "streaming", "each", "chunk", "thinking", "ollama", "stream", "partial"]
source: https://docs.ollama.com/capabilities/streaming
source_anchor: ""
source_lines: [1, 13]
section: "Gemini_Google"
---
`stream` parameter to `True`.
## Key streaming concepts

1. Chatting: Stream partial assistant messages. Each chunk includes the `content` so you can render messages as they arrive.
2. Thinking: Thinking-capable models emit a `thinking` field alongside regular content in each chunk. Detect this field in streaming chunks to show or hide reasoning traces before the final answer arrives.
3. Tool calling: Watch for streamed `tool_calls` in each chunk, execute the requested tool, and append tool outputs back into the conversation.

## Handling streamed chunks

 It is necessary to accumulate the partial fields in order to maintain the history of the conversation. This is particularly important for tool calling where the thinking, tool call from the model, and the executed tool result must be passed back to the model in the next request. 

- Python
- JavaScript
