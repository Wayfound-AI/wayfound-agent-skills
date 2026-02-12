# Wayfound Agent Skills

Open-source agent skills by [Wayfound](https://wayfound.ai), published to [ClawHub](https://clawhub.ai) for the [OpenClaw](https://github.com/openclaw/openclaw) ecosystem.

## Skills

### wayfound

Lightweight self-supervision that piggybacks on your existing memory system. No new infrastructure — just a rubric in SOUL.md and a daily review cron job.

- **~200 tokens/day** — One isolated cron job, cheap model, low thinking
- **No parallel systems** — Reviews live in `memory/` alongside everything else
- **Compounds naturally** — Findings feed back into SOUL.md and MEMORY.md through normal memory maintenance
- **Zero ceremony** — No onboarding wizard, no JSON schemas, no grading tiers

To install, just ask OpenClaw:

> "Install the wayfound skill from ClawHub"

Skill files: [`clawhub/`](clawhub/)

## About Wayfound

Wayfound is the world's first AI agent supervision platform. This skill brings lightweight self-review to individual agents. For fully independent evaluation by a dedicated AI Supervisor, multi-agent fleet management, shared organizational learning, and enterprise compliance, visit [wayfound.ai](https://wayfound.ai).

## License

MIT
