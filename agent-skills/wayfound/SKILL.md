---
name: wayfound
description: >
  Integrate AI agents with Wayfound's supervision and observability platform.
  Sends session transcripts for analysis, compliance monitoring, and performance
  evaluation. Use when the user wants to add Wayfound to their agent, send
  conversations to Wayfound, set up agent supervision, or monitor AI agent
  performance. Supports Python SDK, JavaScript SDK, and REST API.
metadata:
  author: wayfound
  version: "1.0"
---

# Wayfound Integration

Wayfound is an AI agent supervision platform. It analyzes agent-user conversations for performance, compliance, and knowledge gaps. Send session transcripts via SDK or REST API, and Wayfound returns analysis including guideline violations, improvement suggestions, and performance metrics.

Docs: https://docs.wayfound.ai
API reference: https://wayfound-api.readme.io/reference

## Prerequisites

Before integrating, the developer needs:

- A Wayfound account at https://app.wayfound.ai
- An agent created on the platform with a **name**, **role**, and **goal**. The goal should have clear, verifiable success criteria — Wayfound's supervisor evaluates sessions against it.
- An **API key** — created on the Settings > Connections page (requires admin permissions)
- The **Agent ID** — found on the agent's Connection page

Set these as environment variables (both SDKs read them automatically):

```bash
export WAYFOUND_API_KEY="your-api-key"
export WAYFOUND_AGENT_ID="your-agent-id"
```

## Message Format

All integration methods use the same message schema. Define messages once, use them with any method.

Each message in the array:

```json
{
  "timestamp": "2025-01-15T10:00:00Z",
  "event_type": "assistant_message",
  "attributes": {
    "content": "Hello! How can I help you today?"
  }
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `timestamp` | string | Yes | ISO 8601 datetime of when the message occurred |
| `event_type` | string | Yes | `user_message`, `assistant_message`, `tool_call`, or custom types |
| `attributes` | object | No | Event-specific data. Use `content` for message text |
| `label` | string | No | Classification label for the event |
| `description` | string | No | Additional context about the event |

The messages array has a **750 KB size limit**.

## Python SDK

Install:

```bash
pip install wayfound
```

Requires Python 3.8+.

### Create a session

```python
from wayfound import Session

session = Session(
    wayfound_api_key="your-api-key",
    agent_id="your-agent-id"
)

messages = [
    {
        "timestamp": "2025-01-15T10:00:00Z",
        "event_type": "assistant_message",
        "attributes": {"content": "Hello! How can I help?"}
    },
    {
        "timestamp": "2025-01-15T10:00:05Z",
        "event_type": "user_message",
        "attributes": {"content": "What's the status of Project Alpha?"}
    },
    {
        "timestamp": "2025-01-15T10:00:08Z",
        "event_type": "assistant_message",
        "attributes": {"content": "Project Alpha is on track for delivery next Friday."}
    }
]

result = session.create(messages=messages)
```

If `WAYFOUND_API_KEY` and `WAYFOUND_AGENT_ID` are set as environment variables, the constructor needs no arguments:

```python
session = Session()
```

### Synchronous mode

By default, `create()` returns immediately and analysis runs in the background. Set `is_async=False` to wait for analysis results:

```python
result = session.create(messages=messages, is_async=False)

if "compliance" in result:
    violations = [item for item in result["compliance"] if not item["result"]["compliant"]]
    if violations:
        print(f"Found {len(violations)} guideline violations")
```

### Append to a session

Add more messages to an existing session:

```python
additional_messages = [
    {
        "timestamp": "2025-01-15T10:05:00Z",
        "event_type": "user_message",
        "attributes": {"content": "Can you also check Project Beta?"}
    }
]

session.append_to_session(additional_messages)
```

### Visitor and account tracking

Track who is interacting with the agent:

```python
session = Session(
    wayfound_api_key="your-api-key",
    agent_id="your-agent-id",
    visitor_id="user-123",
    visitor_display_name="Jane Smith",
    account_id="acct-456",
    account_display_name="Acme Corp",
    metadata={"source": "web", "environment": "production"}
)
```

## JavaScript SDK

Install:

```bash
npm install wayfound
```

### Create a session

```javascript
import { Session } from "wayfound";

const session = new Session({
  wayfoundApiKey: "your-api-key",
  agentId: "your-agent-id"
});

const messages = [
  {
    timestamp: "2025-01-15T10:00:00Z",
    event_type: "assistant_message",
    attributes: { content: "Hello! How can I help?" }
  },
  {
    timestamp: "2025-01-15T10:00:05Z",
    event_type: "user_message",
    attributes: { content: "What's the status of Project Alpha?" }
  },
  {
    timestamp: "2025-01-15T10:00:08Z",
    event_type: "assistant_message",
    attributes: { content: "Project Alpha is on track for delivery next Friday." }
  }
];

const result = await session.create({ messages });
```

If `WAYFOUND_API_KEY` and `WAYFOUND_AGENT_ID` are set as environment variables, the constructor needs no arguments:

```javascript
const session = new Session({});
```

### Synchronous mode

Set `async: false` to wait for analysis results:

```javascript
const result = await session.create({ messages, async: false });
```

### Append to a session

Add more messages to an existing session:

```javascript
const additionalMessages = [
  {
    timestamp: "2025-01-15T10:05:00Z",
    event_type: "user_message",
    attributes: { content: "Can you also check Project Beta?" }
  }
];

await session.appendToSession({ messages: additionalMessages });
```

### Visitor, account, and metadata tracking

```javascript
const session = new Session({
  wayfoundApiKey: "your-api-key",
  agentId: "your-agent-id",
  visitorId: "user-123",
  visitorDisplayName: "Jane Smith",
  accountId: "acct-456",
  accountDisplayName: "Acme Corp",
  metadata: { source: "web", environment: "production" }
});
```
