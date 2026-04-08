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
