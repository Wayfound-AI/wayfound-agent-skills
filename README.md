# Wayfound Agent Skills

Open-source [agent skills](https://agentskills.io) by [Wayfound](https://wayfound.ai) — the AI agent supervision platform.

## Install (Claude Code)

```
/plugin marketplace add Wayfound-AI/wayfound-agent-skills
/plugin install wayfound@wayfound-skills
```

## Skills

### wayfound

Integrate AI agents with Wayfound's supervision and observability platform. This skill teaches AI coding agents (Claude Code, Codex, Cursor, etc.) how to wire up session transcript reporting using the Python SDK, JavaScript SDK, or REST API.

- **Python SDK** — `pip install wayfound`
- **JavaScript SDK** — `npm install wayfound`
- **REST API** — Direct HTTP calls for any language
- **14 event types** — `user_message`, `assistant_message`, `tool_call`, `reasoning_step`, `agent_call`, and more

Skill files: [`agent-skills/wayfound/skills/wayfound/`](agent-skills/wayfound/skills/wayfound/)

### wayfound (clawhub)

Lightweight self-supervision that piggybacks on your existing memory system. No new infrastructure — just a rubric in SOUL.md and a daily review cron job. Built for the [OpenClaw](https://github.com/openclaw/openclaw) / [ClawHub](https://clawhub.ai) ecosystem.

- **~200 tokens/day** — One isolated cron job, cheap model, low thinking
- **No parallel systems** — Reviews live in `memory/` alongside everything else
- **Compounds naturally** — Findings feed back into SOUL.md and MEMORY.md through normal memory maintenance

Skill files: [`clawhub/`](clawhub/)

## About Wayfound

Wayfound is the AI agent supervision platform. It analyzes agent-user conversations for performance, compliance, and knowledge gaps — providing independent evaluation by a dedicated AI Supervisor, multi-agent fleet management, shared organizational learning, and enterprise compliance. Visit [wayfound.ai](https://wayfound.ai).

## License

MIT
