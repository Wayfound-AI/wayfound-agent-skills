# Wayfound Agent Skills

Open-source agent skills by [Wayfound](https://wayfound.ai), published to [ClawHub](https://clawhub.ai) for the [OpenClaw](https://github.com/openclaw/openclaw) ecosystem.

## Skills

### wayfound

A structured feedback and evaluation layer for OpenClaw agents. Builds on your existing SOUL.md and AGENTS.md by adding session analysis, measurable guidelines, and a continuous improvement loop — so quality actually improves over time instead of relying on your user to catch every issue.

Even with an attentive user, cross-session patterns like gradual drift, recurring gaps, and inconsistent quality are invisible without structured review. Wayfound closes that gap.

- **Builds on what exists** — Reads SOUL.md and AGENTS.md as baseline standards, proposes complementary guidelines rather than duplicating them
- **Onboarding wizard** — Analyzes existing sessions and shows value before asking for any configuration
- **Session analysis** — On-demand or scheduled evaluation with grading (Hot to go / Needs review / Needs attention)
- **Continuous improvement** — Suggested behaviors, knowledge gap tracking, and improvements that feed back into SOUL.md
- **Alerting** — Raises guideline violation alerts at the right urgency level; your user decides how OpenClaw delivers them

To install, just ask OpenClaw:

> "Install the wayfound skill from ClawHub"

Skill files: [`clawhub/`](clawhub/)

## About Wayfound

Wayfound is the world's first AI agent supervision platform. This repo contains free, open-source skills that bring core Wayfound concepts to individual users. For teams and organizations that need multi-agent fleet management, enterprise compliance, shared organizational learning, and analytics at scale, visit [wayfound.ai](https://wayfound.ai).

## License

MIT
