# Wayfound Agent Skills

Open-source [agent skills](https://agentskills.io) by [Wayfound](https://wayfound.ai) — the AI agent supervision platform.

These skills teach AI coding agents how to integrate with Wayfound, so when a developer asks their agent to "add Wayfound supervision," the agent knows exactly how to wire it up.

## Getting Started

### Claude Code

This repo is a Claude Code plugin marketplace. To install:

1. Add the marketplace:
   ```
   /plugin marketplace add Wayfound-AI/wayfound-agent-skills
   ```
2. Open `/plugin`, go to the **Marketplaces** tab, select **wayfound-skills**, browse plugins, and install **wayfound**

Once installed, the skill activates automatically when you ask your agent to integrate Wayfound into a project.

### Cursor

This repo is a Cursor team marketplace. To install:

1. Go to **Dashboard > Settings > Plugins**
2. Under **Team Marketplaces**, click **Import**
3. Paste the repo URL: `https://github.com/Wayfound-AI/wayfound-agent-skills`
4. Review and install the **wayfound** plugin

### Other Agents

Any agent that supports the [agentskills.io](https://agentskills.io) format can use this skill — including Codex, Gemini CLI, Roo Code, and others. Check your agent's documentation for how it discovers and loads skills. The skill file is at [`agent-skills/wayfound/skills/wayfound/SKILL.md`](agent-skills/wayfound/skills/wayfound/SKILL.md).

## Skills

### wayfound

Integrate AI agents with Wayfound's supervision and observability platform. This skill teaches coding agents how to send session transcripts to Wayfound for analysis, compliance monitoring, and performance evaluation.

**What the skill covers:**

- **Python SDK** — `pip install wayfound` — Session creation, appending, sync/async modes
- **JavaScript SDK** — `npm install wayfound` — Same capabilities, JS/TS syntax
- **REST API** — Direct HTTP calls for any language
- **14 event types** — `user_message`, `assistant_message`, `tool_call`, `reasoning_step`, `agent_call`, `user_feedback`, and more
- **Common patterns** — Completed conversations, streaming/incremental sessions, visitor tracking

Skill file: [`agent-skills/wayfound/skills/wayfound/SKILL.md`](agent-skills/wayfound/skills/wayfound/SKILL.md)

### wayfound (clawhub)

Lightweight self-supervision that piggybacks on your existing memory system. No new infrastructure — just a rubric in SOUL.md and a daily review cron job. Built for the [OpenClaw](https://github.com/openclaw/openclaw) / [ClawHub](https://clawhub.ai) ecosystem.

- **~200 tokens/day** — One isolated cron job, cheap model, low thinking
- **No parallel systems** — Reviews live in `memory/` alongside everything else
- **Compounds naturally** — Findings feed back into SOUL.md and MEMORY.md through normal memory maintenance

Skill files: [`clawhub/`](clawhub/)

## About Wayfound

[Wayfound](https://wayfound.ai) is the AI agent supervision platform. It analyzes agent-user conversations for performance, compliance, and knowledge gaps — providing independent evaluation by a dedicated AI Supervisor, multi-agent fleet management, shared organizational learning, and enterprise compliance.

- **Docs:** [docs.wayfound.ai](https://docs.wayfound.ai)
- **API Reference:** [wayfound-api.readme.io](https://wayfound-api.readme.io/reference)
- **Python SDK:** [github.com/Wayfound-AI/wayfound-sdk-python](https://github.com/Wayfound-AI/wayfound-sdk-python)
- **JavaScript SDK:** [github.com/Wayfound-AI/wayfound-sdk-javascript](https://github.com/Wayfound-AI/wayfound-sdk-javascript)

## License

MIT
