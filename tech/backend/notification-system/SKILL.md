# Notification System

Design multi-channel notifications: push, in-app, email, SMS with preferences and batching.

## Research Protocol

### Web Search
- "notification system architecture [current year]"
- "push notification best practices [current year]"

---

## Decision Tree: Notification Channel

```
How urgent is the notification?
├── Immediate + critical (security alert, payment failure)
│   └── Push + Email + SMS (all channels)
├── Immediate + informational (new message, mention)
│   └── Push + In-app
├── Batched (activity digest, weekly summary)
│   └── Email (batched/digest)
├── Contextual (in-app only)
│   └── In-app notification + badge count
└── Marketing
    └── Email (with unsubscribe) — separate from transactional
```

## Architecture

```
Event occurs → Notification service → Route by preference
                                    → Channel adapters:
                                       ├── Push (FCM/APNs)
                                       ├── In-app (WebSocket/database)
                                       ├── Email (Resend/Postmark)
                                       └── SMS (Twilio)

User preferences:
  notification_preferences {
    channel: { push: true, email: true, sms: false },
    types: {
      mentions: { push: true, email: false },
      marketing: { push: false, email: true },
      security: { push: true, email: true, sms: true }
    },
    quiet_hours: { start: "22:00", end: "08:00", timezone: "US/Pacific" }
  }
```

## Batching & Digests

```
Instead of: 50 individual "X liked your post" notifications
Send: "X, Y, and 48 others liked your post" (batched)

Batching rules:
- Same type + same target → batch
- Wait window: 5-15 minutes
- Max batch size: configurable
- Always send immediately for: security, payments, direct messages
```

## Anti-Patterns

| ID | Anti-Pattern | Impact |
|----|-------------|--------|
| T44 | No user preferences | Users disable all notifications |
| T45 | No batching | Notification spam |
| T46 | No quiet hours | Notifications at 3am |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Channel routing** | Per-type, per-channel preferences | Basic preferences | No preferences |
| **Batching** | Smart batching with configurable rules | Basic batching | Every event = notification |
| **Delivery** | Retry, dedup, delivery confirmation | Basic retry | Fire-and-forget |
| **Preferences** | Granular per-type, quiet hours, timezone | On/off per channel | No preferences |
| **In-app** | Real-time, read/unread, badge count | Basic list | No in-app |
| **Templates** | Consistent across channels, localized | Basic templates | Hardcoded strings |
| **Analytics** | Delivery rate, open rate, click rate | Basic tracking | No tracking |

**28+ = Helpful notifications | 21-27 = Noisy | <21 = Users turn off everything**

## Output Protocol

Write to `.claude/outputs/notification-system.md`:
- Channels per notification type
- Preference schema
- Batching rules
- Delivery architecture
