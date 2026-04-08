# Wayfound Agent Skill — Design Spec

## Goal

Create an agent skill (following the agentskills.io spec) that teaches AI coding agents how to integrate Wayfound's AI agent supervision platform into a developer's application. The skill provides domain expertise on Wayfound's SDKs and API so that when a developer asks a coding agent to "add Wayfound supervision," the agent knows how to wire it up.

## Target Audience

Developers building AI agent applications who want to send session transcripts to Wayfound for analysis, compliance monitoring, and performance evaluation. The skill is general-purpose (not coding-agent-specific).

## Skill Location

`agent-skills/wayfound/SKILL.md` — lives alongside the existing `clawhub/` skill in this repo.

## Approach

Single `SKILL.md` file with inline examples (Approach A). No reference files or scripts unless the file exceeds ~500 lines. Estimated length: 250-350 lines.

## Frontmatter

```yaml
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
```

## Content Sections

### 1. What is Wayfound (~3 sentences)

Wayfound is an AI agent supervision platform. It analyzes agent-user conversations for performance, compliance, and knowledge gaps. Developers send session transcripts via SDK or API, and Wayfound returns analysis including guideline violations and improvement suggestions.

### 2. Prerequisites (bullet list)

- Wayfound account at app.wayfound.ai
- An agent created on the platform (with name, role, and goal — the goal should have clear success criteria)
- API key (requires admin permissions, created on Settings > API page)
- Agent ID (found on the agent's Connection page)
- Environment variable names: `WAYFOUND_API_KEY`, `WAYFOUND_AGENT_ID`

### 3. Message Format (canonical schema, shown once)

The universal message object used across all integration methods:

- `timestamp` (string, ISO 8601, required) — when the message was created
- `event_type` (string, required) — `user_message`, `assistant_message`, `tool_call`, etc.
- `attributes` (object, optional) — event-specific data, typically includes `content`
- `label` (string, optional) — classification label
- `description` (string, optional) — additional context

### 4. Python SDK Integration

- Install: `pip install wayfound` (Python 3.8+)
- Session init with explicit credentials and with env vars
- `session.create(messages=..., is_async=True/False)`
- `session.append_to_session(messages, is_async=True/False)`
- Compliance result inspection pattern
- Visitor/account tracking via constructor params

### 5. JavaScript SDK Integration

- Install: `npm install wayfound`
- Session init with explicit credentials and with env vars
- `await session.create({ messages })`
- Metadata support via constructor
- Same message format as Python

### 6. REST API Integration

- Endpoint: `POST https://app.wayfound.ai/api/v2/sessions`
- Append: `PUT https://app.wayfound.ai/api/v2/sessions/{id}`
- Auth: `Authorization: Bearer <API_KEY>`
- curl examples for create and append
- Response shape: `{ id, createdAt }`

### 7. Key Concepts

- `async` (default true) vs sync — sync returns analysis inline
- Visitor tracking: `visitor_id`, `visitor_display_name`
- Account tracking: `account_id`, `account_display_name`
- Session metadata: arbitrary key-value pairs
- Appending to sessions: use session ID from create response
- 750 KB message array limit
- Test mode: `POST /api/v2/sessions/test/completed`
- Application ID: optional grouping for multi-agent apps

### 8. Common Patterns

1-2 brief, framework-agnostic examples showing where to hook Wayfound into a typical agent conversation loop. Focus on the integration point (after collecting messages, before/after response), not framework-specific callbacks.

## Out of Scope

- Wayfound platform configuration (guidelines, supervisor setup)
- MCP server integration (querying Wayfound, not sending to it)
- Self-supervision / self-review patterns (covered by the existing clawhub skill)
- Framework-specific integrations (LangChain, CrewAI callbacks) — keep patterns generic
