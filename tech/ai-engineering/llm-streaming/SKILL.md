# LLM Streaming

Implement token-by-token streaming for responsive AI interfaces.

## Research Protocol
### Web Search
- "LLM streaming SSE implementation [current year]"
- "structured output streaming JSON [current year]"

## Decision Tree: Streaming Pattern

```
What's the output format?
├── Free-form text (chat, generation)
│   └── Token-by-token SSE streaming
│       → Stream directly to UI, render as received
├── Structured JSON (extraction, classification)
│   └── Streaming JSON parser (partial JSON handling)
│       → Buffer tokens, parse incrementally, update UI on complete fields
├── Tool calls / Function calling
│   └── Stream tool call detection, execute on complete
│       → Buffer until tool call complete, then execute
├── Multi-part (text + tool calls + text)
│   └── Content block streaming
│       → Handle each content block type as it arrives
└── Long generation with progress
    └── Streaming + progress indicators
        → Show token count, estimated remaining, cancel button
```

## Implementation Pattern

```
Server:
  Set headers: Content-Type: text/event-stream
  For each token from LLM:
    Send: data: {"type":"text","content":"token"}\n\n
  On complete:
    Send: data: {"type":"done"}\n\n

Client:
  const source = new EventSource('/api/chat')
  source.onmessage = (event) => {
    const data = JSON.parse(event.data)
    if (data.type === 'text') appendToUI(data.content)
    if (data.type === 'done') source.close()
  }

Cancellation:
  Client: source.close() + AbortController on fetch
  Server: Detect disconnect, cancel LLM request
```

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Responsiveness** | First token visible <500ms | <2s | Wait for full response |
| **Error handling** | Mid-stream errors handled gracefully | Basic error | Crashes on error |
| **Cancellation** | User can cancel, server stops generation | Client cancel | No cancel |
| **Structured output** | Incremental JSON parsing | Wait for complete JSON | No structured streaming |
| **Reconnection** | Auto-reconnect on disconnect | Manual retry | Lost on disconnect |
| **UI rendering** | Smooth token rendering, no flicker | Basic append | Janky rendering |
| **Backpressure** | Handle slow clients, buffer management | Some buffering | No backpressure |

**28+ = Smooth streaming UX | 21-27 = Functional | <21 = Laggy or broken**
