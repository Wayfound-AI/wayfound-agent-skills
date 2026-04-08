# Wayfound Agent Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a single `agent-skills/wayfound/SKILL.md` that teaches AI coding agents how to integrate Wayfound session reporting into a developer's AI agent application.

**Architecture:** One markdown file following the agentskills.io SKILL.md spec — YAML frontmatter + 8 content sections covering the Wayfound Python SDK, JavaScript SDK, and REST API.

**Tech Stack:** Markdown (agentskills.io format), Wayfound Python SDK (v2.6+), Wayfound JavaScript SDK, Wayfound REST API v2

---

### Task 1: Create SKILL.md with frontmatter and sections 1-3

**Files:**
- Create: `agent-skills/wayfound/SKILL.md`

- [ ] **Step 1: Create the directory**

```bash
mkdir -p agent-skills/wayfound
```

- [ ] **Step 2: Write the SKILL.md with frontmatter, intro, prerequisites, and message format**

Create `agent-skills/wayfound/SKILL.md` with the following content:

````markdown
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
````

- [ ] **Step 3: Verify the frontmatter is valid YAML**

```bash
head -15 agent-skills/wayfound/SKILL.md
```

Expected: clean YAML frontmatter with `name`, `description`, and `metadata` fields between `---` delimiters.

- [ ] **Step 4: Commit**

```bash
git add agent-skills/wayfound/SKILL.md
git commit -m "feat: add wayfound agent skill with intro, prereqs, and message format"
```

---

### Task 2: Add Python SDK section

**Files:**
- Modify: `agent-skills/wayfound/SKILL.md` (append after message format section)

- [ ] **Step 1: Append the Python SDK section**

Add the following after the Message Format section:

````markdown
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
````

- [ ] **Step 2: Commit**

```bash
git add agent-skills/wayfound/SKILL.md
git commit -m "feat: add Python SDK integration section to wayfound skill"
```

---

### Task 3: Add JavaScript SDK section

**Files:**
- Modify: `agent-skills/wayfound/SKILL.md` (append after Python SDK section)

- [ ] **Step 1: Append the JavaScript SDK section**

Add the following after the Python SDK section:

````markdown
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
````

- [ ] **Step 2: Commit**

```bash
git add agent-skills/wayfound/SKILL.md
git commit -m "feat: add JavaScript SDK integration section to wayfound skill"
```

---

### Task 4: Add REST API section

**Files:**
- Modify: `agent-skills/wayfound/SKILL.md` (append after JavaScript SDK section)

- [ ] **Step 1: Append the REST API section**

Add the following after the JavaScript SDK section:

````markdown
## REST API

For languages without an SDK, use the REST API directly.

### Create a session

```bash
curl -X POST https://app.wayfound.ai/api/v2/sessions \
  -H "Authorization: Bearer $WAYFOUND_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "agentId": "your-agent-id",
    "messages": [
      {
        "timestamp": "2025-01-15T10:00:00Z",
        "event_type": "assistant_message",
        "attributes": {"content": "Hello! How can I help?"}
      },
      {
        "timestamp": "2025-01-15T10:00:05Z",
        "event_type": "user_message",
        "attributes": {"content": "What is the status of Project Alpha?"}
      }
    ]
  }'
```

Response:

```json
{
  "id": "9f71c6ef-d423-4757-b39f-7118fe87adf6",
  "createdAt": "2025-01-15T10:01:00.000Z"
}
```

### Append to a session

```bash
curl -X PUT https://app.wayfound.ai/api/v2/sessions/SESSION_ID \
  -H "Authorization: Bearer $WAYFOUND_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [
      {
        "timestamp": "2025-01-15T10:05:00Z",
        "event_type": "user_message",
        "attributes": {"content": "Follow-up question here"}
      }
    ]
  }'
```

### Optional parameters

Include these in the create request body as needed:

| Parameter | Type | Description |
|-----------|------|-------------|
| `async` | boolean | Set to `false` to block until analysis completes (default: `true`) |
| `visitorId` | string | Unique identifier for the end user |
| `visitorDisplayName` | string | Display name for the end user |
| `accountId` | string | Unique identifier for the user's account/org |
| `accountDisplayName` | string | Display name for the account |
| `applicationId` | string | Groups sessions by application in multi-agent setups |
| `metadata` | object | Arbitrary key-value pairs attached to the session |
````

- [ ] **Step 2: Commit**

```bash
git add agent-skills/wayfound/SKILL.md
git commit -m "feat: add REST API integration section to wayfound skill"
```

---

### Task 5: Add Key Concepts and Common Patterns sections

**Files:**
- Modify: `agent-skills/wayfound/SKILL.md` (append after REST API section)

- [ ] **Step 1: Append Key Concepts and Common Patterns sections**

Add the following after the REST API section:

````markdown
## Key Concepts

**Async vs sync processing:** By default, session creation returns immediately with a session ID while analysis runs in the background. Set `async: false` (REST/JavaScript) or `is_async=False` (Python) to wait for analysis results including compliance checks.

**Appending messages:** Use the session ID from the create response to append additional messages later. This is useful for long-running conversations where you want to send messages incrementally rather than waiting for the full conversation to complete.

**Visitor and account tracking:** Attach user identity (`visitor_id`, `visitor_display_name`) and organization identity (`account_id`, `account_display_name`) to sessions. These appear in Wayfound's Visitors tab for tracking interactions across sessions.

**Session metadata:** Attach arbitrary key-value pairs to sessions for filtering and analysis in the Wayfound dashboard (e.g., `{"source": "web", "environment": "production", "model": "gpt-4"}`).

**Application ID:** In multi-agent architectures, use `application_id` to group sessions by application. This links sessions to a specific application context in Wayfound.

**Test mode:** Use `POST /api/v2/sessions/test/completed` (same request body as the create endpoint) to submit test sessions during development. Test sessions are analyzed but flagged separately in the dashboard.

**Message size limit:** The messages array is limited to **750 KB** per request. For very long conversations, send messages incrementally using the append endpoint.

## Common Patterns

### Sending a completed conversation

The simplest integration — collect all messages during the conversation, then send them to Wayfound after the session ends:

```python
from wayfound import Session

def run_agent_session(user_input):
    messages = []
    # ... your agent conversation loop ...
    # Each turn, append to messages with timestamp, event_type, and attributes

    # After the conversation ends, send to Wayfound
    session = Session()
    session.create(messages=messages)
```

### Streaming messages during a conversation

For long-running sessions, create the session with the first batch of messages, then append as the conversation continues:

```python
from wayfound import Session

session = Session()

# After the first exchange
result = session.create(messages=initial_messages)
# session.session_id is now set from the response

# As the conversation continues
session.append_to_session(new_messages)

# After another exchange
session.append_to_session(more_messages)
```
````

- [ ] **Step 2: Check total line count**

```bash
wc -l agent-skills/wayfound/SKILL.md
```

Expected: 250-350 lines. If over 500, consider splitting reference content into `references/` files.

- [ ] **Step 3: Commit**

```bash
git add agent-skills/wayfound/SKILL.md
git commit -m "feat: add key concepts and common patterns to wayfound skill"
```

---

### Task 6: Update README.md

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Add the new skill to the README**

In the `## Skills` section of `README.md`, after the existing `### wayfound` entry under `clawhub/`, add a new entry for the agent-skills version. The exact content should describe the new skill's purpose (helping developers integrate Wayfound into their AI agent applications) and point to `agent-skills/wayfound/`.

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "docs: add agent-skills wayfound skill to README"
```

---

### Task 7: Final review

- [ ] **Step 1: Read the complete SKILL.md and verify**

Read `agent-skills/wayfound/SKILL.md` end to end. Check:
- Frontmatter is valid YAML with `name: wayfound`
- `name` field matches the parent directory name (`wayfound`)
- All 8 sections from the spec are present
- Code examples use consistent message format across Python, JavaScript, and REST
- No placeholder text (TBD, TODO, etc.)
- Line count is under 500

- [ ] **Step 2: Verify directory structure matches agentskills.io spec**

```bash
find agent-skills/wayfound -type f
```

Expected: just `agent-skills/wayfound/SKILL.md`
