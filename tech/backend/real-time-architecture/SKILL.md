# Real-Time Architecture

Design real-time features: live updates, collaboration, chat, presence, and notifications.

## Research Protocol

### Web Search
- "WebSocket vs SSE vs polling [current year]"
- "real-time architecture patterns [current year]"

### WebFetch
- https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API
- https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events

---

## Context Sync Protocol

### Read Upstream
- `.claude/outputs/system-architecture.md` — infrastructure constraints
- `.claude/outputs/api-design.md` — existing API patterns

---

## Decision Tree: Real-Time Transport

```
What's the communication pattern?
├── Server → Client only (notifications, live feeds, dashboards)
│   └── Server-Sent Events (SSE)
│       → Simple, HTTP-based, auto-reconnect, works with HTTP/2
├── Bidirectional (chat, collaboration, gaming)
│   └── WebSocket
│       → Full-duplex, lower latency, needs connection management
├── Infrequent updates (email status, order tracking)
│   └── Long polling or short polling
│       → Simplest, works everywhere, higher latency
├── Collaborative editing (shared documents)
│   └── WebSocket + CRDT or OT
│       → Conflict-free resolution, offline support
└── Presence (who's online, typing indicators)
    └── WebSocket + heartbeat
        → Regular pings, timeout detection, status broadcast
```

## Architecture Patterns

| Pattern | When | Implementation |
|---------|------|----------------|
| **Pub/Sub** | Multiple subscribers per channel | Redis Pub/Sub, Supabase Realtime |
| **Fan-out** | One event → many recipients | Message broker + per-connection queues |
| **Request/Response** | Client asks, server responds in real-time | WebSocket with message IDs |
| **Event sourcing** | Full history of changes needed | Append-only log + materialized views |
| **Room-based** | Users grouped (chat rooms, game lobbies) | Channel-based subscriptions |

## Scaling Real-Time

```
Single server: Direct WebSocket connections (up to ~10K)
Multi-server: Redis Pub/Sub for cross-server message routing
Large scale: Dedicated real-time service (e.g., Ably, Pusher, Supabase Realtime)

Connection limits:
- Each WebSocket holds an open TCP connection
- Plan for: max_connections = peak_users × 1.5 (reconnections)
- Use connection pooling and sticky sessions behind load balancer
```

## Anti-Patterns

| ID | Anti-Pattern | Impact |
|----|-------------|--------|
| T32 | Polling for real-time needs | High latency, wasted bandwidth |
| T33 | No reconnection logic | Dropped connections stay dead |
| T34 | No backpressure | Server overwhelmed by fast publishers |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Transport choice** | Right transport for each use case | One transport for all | Wrong transport |
| **Reconnection** | Auto-reconnect with backoff + state recovery | Basic reconnect | No reconnect |
| **Scaling** | Horizontal scaling plan with pub/sub | Single server | No scaling plan |
| **Ordering** | Messages ordered where needed | Mostly ordered | Out-of-order issues |
| **Backpressure** | Rate limiting, buffering, flow control | Some limits | No backpressure |
| **Offline** | Queue messages, sync on reconnect | Basic queue | Lost messages offline |
| **Monitoring** | Connection count, message rate, latency | Basic metrics | No monitoring |

**28+ = Reliable real-time | 21-27 = Works but fragile | <21 = Dropped messages/connections**

## Output Protocol

Write to `.claude/outputs/real-time-architecture.md`:
- Transport chosen per feature
- Channel/room design
- Scaling approach
- Reconnection strategy
