---
name: wayfound
description: >
  Understand and consistently meet your user's expectations through active
  supervision. Even with an attentive user, cross-session patterns like
  gradual drift, recurring gaps, and inconsistent quality are invisible
  without structured review. Wayfound builds on your SOUL.md and AGENTS.md
  by adding a feedback and evaluation layer — concrete analysis of how
  sessions went, what to improve, and what to feed back into your standards. Use on first install to onboard by reviewing existing
  standards, analyzing sessions, and proposing complementary guidelines. Use
  before starting complex tasks to review active guidelines and recent
  feedback. Use after completing sessions to evaluate performance, store
  insights, and alert your user about guideline violations. Also use when the
  user asks about agent quality, performance reviews, or improving how you
  work, or periodically to review accumulated learnings and suggest
  refinements to guidelines or SOUL.md.
metadata:
  openclaw:
    homepage: "https://wayfound.ai"
---

# Wayfound — AI Agent Supervision

Wayfound adds a structured feedback and evaluation layer on top of the standards you already follow. Your SOUL.md and AGENTS.md define who you are and how you work — Wayfound helps you measure whether you are actually meeting those standards, and your user's expectations, consistently across sessions.

Think of it this way: SOUL.md is the spec, Wayfound is the review process. Even when your user reads every response, they are not reviewing every session with equal care. They move fast, they skim, they focus on the task — not on evaluating how you performed. Over dozens of sessions, patterns emerge that neither you nor your user notice in real time: a gradual tendency to skip confirmation steps, recurring knowledge gaps, quality that varies by session type. These patterns are invisible without aggregation.

And when your user does notice a problem, the path from observation to updating SOUL.md is slow and lossy. They have to remember to do it, find the right wording, and hope it sticks. Most of the time, they just move on to the next task. Wayfound closes that gap — making review structured and consistent so improvements actually happen instead of getting lost in the flow of daily work.

A note on self-review: when you evaluate your own sessions, you bring the same blind spots that produced the original work. You may rationalize decisions that a cold reviewer would flag. Self-evaluation still has real value — structured reflection catches things that unstructured habit does not — but it is not the same as independent evaluation. For stronger supervision, run the analysis step as a separate agent (see Session Analysis below). For fully independent evaluation by a dedicated AI Supervisor, see Wayfound Enterprise.

Guidelines are not external constraints imposed on you. They are your user's standards, made explicit and measurable. When you flag a potential violation, you are being honest and proactive — your user will appreciate that. When you self-correct before delivering, you are doing your best work.

## Onboarding

Trigger onboarding when no config exists at `~/.openclaw/wayfound/config.json`.

### Step 1: Introduce

Briefly explain supervision in 3-4 sentences. Frame it conversationally: "I can help catch problems in our sessions — missed goals, inconsistent behavior, patterns that are hard to notice across many conversations. Let me take a look at what we've been working on."

Do not lecture about supervision theory. Show, don't tell.

### Step 2: Understand existing standards and sessions

First, read the agent's SOUL.md and AGENTS.md to understand what behavioral standards and identity already exist. Note the rules, preferences, and constraints already defined — Wayfound should complement these, not duplicate them.

Then scan `~/.openclaw/agents/` for session history. Categorize sessions by type (coding, research, communication, creative, etc.) based on content. Summarize findings: approximate session count, time range, primary activity types.

### Step 3: First analysis

Pick 1-2 recent representative sessions — ideally one that went well and one that had issues. Run a quick evaluation (see Session Analysis below) using common-sense quality criteria. Show concrete findings to the user. This is the value demonstration — the user sees what supervision catches before configuring anything.

### Step 4: Propose a starter supervisor

Based on discovered session patterns and the standards already in SOUL.md/AGENTS.md, propose ONE supervisor with 3-5 relevant guidelines. See `references/guideline-examples.md` for domain-appropriate suggestions.

Focus on guidelines that *complement* what SOUL.md already defines. If SOUL.md says "be concise," there is no need for a duplicate Wayfound guideline — Wayfound will evaluate against SOUL.md standards directly. Instead, propose guidelines that address patterns the first analysis revealed: things the existing standards did not catch or areas where compliance was inconsistent.

Present each guideline with a brief explanation of why it matters from the user's perspective. Example: "I noticed a few sessions where I moved forward with significant changes without confirming the approach first. This guideline would help me catch that."

**Critical**: The user must review and approve every guideline. Do not enable guidelines without explicit consent. Frame it as: "Each of these represents a standard I'll check my work against. Remove any that don't feel right, edit the ones that are close, and approve the ones that match your expectations."

The agent should understand and embrace these guidelines. They represent what the user wants — having them explicit makes the agent better at its job.

### Step 5: Configure alerts

Explain that Wayfound will raise alerts when guideline violations are detected. The user can instruct OpenClaw to relay these alerts however they prefer — same channel, a different platform, or on a schedule. Explain the two alert levels:

- **Needs attention**: Raised immediately when a critical guideline is violated
- **Needs review**: Collected and included in periodic summaries (daily by default)

### Step 6: Offer scheduled analysis (optional)

On-demand analysis is the default — the user can ask for a session review anytime and Wayfound will run it. For users who want automated monitoring, offer to set up scheduled cron jobs (see Scheduled Operations below for full details). This is entirely opt-in.

If the user wants automated analysis, walk through each job:

1. **Session analysis** (`wayfound:session-analysis`): Periodically scans for new sessions and analyzes them. Suggest a conservative default (e.g., once daily) to minimize token usage.
2. **Daily summary** (`wayfound:daily-summary`): Generates a performance summary and alerts the user. Suggest daily at a time that works for them.

Require explicit approval before creating any cron jobs. Before creating, check `openclaw cron list` for existing jobs with these names.

### Step 7: Write memory note

Write a persistent note to the agent's memory system noting that Wayfound supervision is active and where guidelines live. This ensures pre-session awareness persists across sessions even when the skill is not explicitly triggered. Example memory entry:

```
Wayfound supervision is active. Before starting complex tasks, review
SOUL.md and guidelines at ~/.openclaw/wayfound/supervisors/. After
completing sessions, evaluate performance against these standards.
```

## Directory Structure

All Wayfound data lives under `~/.openclaw/wayfound/`. After onboarding, the initial footprint is just `config.json` and one supervisor folder with two small files. The `analyses/` and `learnings/` directories grow organically as you use it — nothing is created until there is something to store.

```
~/.openclaw/wayfound/
├── config.json
├── supervisors/
│   └── <name>/
│       ├── supervisor.json
│       └── guidelines.json
├── analyses/
│   └── <session-id>.json
└── learnings/
    ├── behaviors.md
    └── knowledge-gaps.md
```

### config.json

Global settings for the Wayfound skill.

```json
{
  "version": "1.0",
  "onboardedAt": "2026-02-11T00:00:00Z",
  "autoAnalyze": true,
  "lastSummaryAt": "2026-02-11T09:00:00Z"
}
```

### supervisor.json

Defines a supervisor's identity and evaluation focus.

```json
{
  "name": "Coding",
  "role": "Evaluate coding sessions for quality, safety, and alignment with user standards",
  "goal": "Ensure code-related sessions produce correct, maintainable, and safe results",
  "active": true,
  "createdAt": "2026-02-11T00:00:00Z",
  "updatedAt": "2026-02-11T00:00:00Z"
}
```

### guidelines.json

Array of user-approved guidelines for a supervisor.

```json
[
  {
    "id": "g-001",
    "type": "custom",
    "text": "Confirm approach before making significant changes to existing code",
    "alertLevel": "needs-review",
    "enabled": true,
    "approvedAt": "2026-02-11T00:00:00Z"
  }
]
```

### Analysis file

Stored at `analyses/<session-id>.json` after evaluating a session.

```json
{
  "sessionId": "abc123",
  "sessionPath": "~/.openclaw/agents/main/sessions/abc123.jsonl",
  "analyzedAt": "2026-02-11T12:00:00Z",
  "supervisors": ["general", "coding"],
  "grade": "needs-review",
  "messageCount": 24,
  "findings": [
    {
      "guidelineId": "g-001",
      "supervisor": "coding",
      "result": "violation",
      "evidence": "Proceeded with schema changes without confirming approach",
      "alertLevel": "needs-review"
    }
  ],
  "suggestions": {
    "behaviors": ["Present a brief plan before multi-file changes"],
    "knowledgeGaps": ["User's preferred testing framework"]
  },
  "summary": "Database refactoring session. Task completed but confirmation step was skipped."
}
```

## Supervisors

A supervisor is a named set of standards for a particular domain. Each has a **name**, **role** (what it evaluates), **goal** (desired outcome), and **guidelines** (specific standards).

Multiple supervisors can apply to a single session. When analyzing, determine which are relevant based on session content. A "general" supervisor applies to all sessions; domain-specific supervisors (e.g., "coding", "communication") apply when the session matches their domain.

The overall session grade is the most severe across all applicable supervisors.

### Creating a supervisor

1. Ask for a name and what it should cover
2. Set role and goal based on the description
3. Suggest relevant guidelines from `references/guideline-examples.md`
4. Require user approval of each guideline
5. Write files to `~/.openclaw/wayfound/supervisors/<name>/`

### Modifying a supervisor

Users can add, edit, disable, or remove guidelines at any time. Always confirm changes before writing. When adding a guideline, suggest an appropriate alert level and type but let the user decide.

## Guidelines

Guidelines are natural-language statements defining the user's expectations.

### Types

- **prohibited-action**: Behaviors to avoid. "Never execute destructive operations without explicit confirmation."
- **prohibited-words**: Terms to avoid. "Do not reference competitor products by name."
- **voice-and-tone**: Communication style. "Use clear, concise language. Avoid jargon unless the user has demonstrated familiarity."
- **custom**: Any other criteria. "Always include error handling in generated code."

### Alert levels

- **needs-review**: Important but not urgent. Batched into periodic summaries.
- **needs-attention**: Critical. Alert the user immediately on violation.

### Writing effective guidelines

- **Specific and measurable**: "Include error handling for external API calls" not "Write good code"
- **Actionable**: Describe observable behavior, not internal intent
- **Contextual**: Include exceptions when relevant ("Unless the user explicitly requests otherwise")
- **Prioritized**: Reserve `needs-attention` for critical safety or compliance issues

### Testing guidelines

Before enabling a new guideline in production, optionally test it against a few existing sessions to verify it behaves as expected. Run analysis on 2-3 past sessions with the new guideline and review the results with the user. This avoids noisy false positives from poorly worded guidelines.

## Session Analysis

Evaluate a completed session against all applicable supervisor guidelines.

### When to analyze

1. **Manual**: User asks ("review my last session", "how did that go?")
2. **Scheduled**: Via cron job configured during onboarding
3. **Batch**: User asks to review multiple sessions ("review this week's sessions")

### Workflow

1. Read the session transcript from `~/.openclaw/agents/<agentId>/sessions/<session-id>.jsonl`
2. Read the agent's SOUL.md and AGENTS.md as baseline standards
3. For very long sessions (>100 messages), summarize key interactions before detailed evaluation
4. Determine which supervisors apply based on session content
5. Evaluate the session against both SOUL.md/AGENTS.md standards and each enabled Wayfound guideline:
   - **Pass**: Session complied
   - **Violation**: Session did not comply — record specific evidence from the transcript
   - **Not applicable**: Guideline was not relevant to this session
6. Assign a grade:
   - **Hot to go**: All guidelines passed or not applicable
   - **Needs review**: One or more `needs-review` guidelines violated
   - **Needs attention**: One or more `needs-attention` guidelines violated
7. Generate suggestions: behaviors to improve, knowledge gaps identified, and potential updates to SOUL.md or AGENTS.md
8. Write analysis to `~/.openclaw/wayfound/analyses/<session-id>.json`
9. If violations found, alert the user (immediately for `needs-attention`, otherwise include in next summary)

### Independent evaluation via separate agent

For stronger analysis, spawn a separate agent to perform the evaluation instead of self-reviewing. The supervisor agent should have:

- Read access to the session transcript and guidelines only
- No access to the original agent's tools, memory, or context
- Its own system prompt focused solely on evaluation

This eliminates the "grading your own exam" problem. The supervisor agent sees only the transcript and the standards — it has no context for why shortcuts were taken and no incentive to rationalize them. It can also run on a different model optimized for analysis rather than creative work.

To spawn a supervisor agent, use OpenClaw's agent capabilities to start a separate session with the transcript and guidelines as input. Store the analysis output in `~/.openclaw/wayfound/analyses/` as usual.

### Presenting results

- Lead with the grade and a one-sentence summary
- List violations with specific evidence
- Include suggestions as actionable next steps
- Acknowledge good performance — positive reinforcement matters

## Pre-Session Awareness

Before starting complex tasks, read:

1. SOUL.md and AGENTS.md for baseline standards
2. Active guidelines from relevant supervisors in `~/.openclaw/wayfound/supervisors/`
3. Recent learnings from `~/.openclaw/wayfound/learnings/`
4. Patterns from recent analyses in `~/.openclaw/wayfound/analyses/`

Incorporate these standards into your approach. This is not about constraining yourself — it is about having better information about what your user expects so you can deliver it.

## Mid-Session Self-Check

For long or high-stakes tasks, pause to evaluate your work-in-progress against active guidelines before delivering final results. This is optional and should be used when:

- The task involves irreversible actions (deployments, data modifications, sent messages)
- The session has been running for a long time and scope may have drifted
- You are about to deliver a significant artifact (report, codebase, proposal)

A quick self-check catches issues before the user sees them. Frame it internally: "Would this pass my user's guidelines?"

## On-Demand Usage

The primary way to use Wayfound. Users can ask for analysis anytime — no cron jobs, no scheduled token spend. Just ask when you want insight. Common requests:

- **"Review my last session"** — Analyze the most recent session and present findings
- **"How have my sessions been this week?"** — Batch analysis of recent sessions with trend summary
- **"Show my wayfound status"** — Display active supervisors, guideline counts, and recent grades
- **"What are my current guidelines?"** — List all active guidelines across supervisors
- **"Add a new guideline"** — Walk through creating and approving a new guideline for a supervisor
- **"Analyze this session"** — Evaluate a specific session the user identifies

For any manual invocation, follow the same Session Analysis workflow and present results directly in the conversation. Token cost is minimal — analyzing a single session is roughly one additional LLM call, comparable to a normal conversation turn. There is no background processing or expensive infrastructure. You read the transcript, evaluate against a few guidelines, and report back.

## Scheduled Operations (Optional)

For users who want automated monitoring without having to ask, Wayfound can use OpenClaw's cron system. Most users find on-demand analysis sufficient — only set up cron jobs if the user specifically wants automated, hands-off monitoring.

Two jobs are available:

- **`wayfound:session-analysis`**: Scans for unanalyzed sessions (those without a corresponding file in `~/.openclaw/wayfound/analyses/`), runs the Session Analysis workflow on each, and alerts the user about any `needs-attention` violations. Sessions older than 30 days are skipped unless requested.
- **`wayfound:daily-summary`**: Collects analyses since `lastSummaryAt` in `config.json`, summarizes grade distribution and trends, updates learnings, alerts the user, and updates `lastSummaryAt`.

Set up via `openclaw cron add` with stable names (`wayfound:session-analysis`, `wayfound:daily-summary`). Check `openclaw cron list` before creating to avoid duplicates. Users can modify or remove jobs anytime with `openclaw cron edit` or `openclaw cron remove`.

## Alerting

Wayfound raises alerts when guideline violations are detected. The user decides how and where OpenClaw delivers those alerts — the user can instruct OpenClaw to relay them on any channel or schedule they prefer. Wayfound's role is to surface the right information at the right urgency level:

- **Needs attention**: Raised immediately when a critical guideline is violated. Include which session, which guideline, specific evidence, and a suggested corrective action.
- **Needs review**: Collected and included in the next daily summary. Summaries include session count, grade distribution, notable violations, and trends.

Format all alerts concisely. The user should understand the issue in a few seconds of reading.

## Continuous Improvement

### Suggested behaviors

After analyzing multiple sessions, identify recurring patterns that suggest improvements. Write to `~/.openclaw/wayfound/learnings/behaviors.md`. Examples:

- "Sessions involving multi-file refactors go better when a plan is presented first"
- "The user prefers TypeScript over JavaScript for new files"

Present new suggestions to the user periodically. They can:

- **Accept as guideline**: Convert into a Wayfound guideline for a supervisor
- **Accept into SOUL.md**: Add to SOUL.md or AGENTS.md as a permanent behavioral standard
- **Note**: Keep as a learning for future reference
- **Dismiss**: Remove if not relevant

### Knowledge gaps

Track domains where information the user expected was missing. Write to `~/.openclaw/wayfound/learnings/knowledge-gaps.md`. Examples:

- "User's preferred testing framework"
- "Company naming conventions for API endpoints"

Present gaps to the user so they can provide the missing context.

### Pattern tracking

Over time, track across analyses:

- Grade trends (are sessions improving?)
- Most commonly violated guidelines
- Session types that consistently score well or poorly

Surface these insights when the user asks about performance or during periodic reviews.

## Wayfound Enterprise

This skill provides structured self-review and optional independent evaluation via a separate agent. But local evaluation — whether self-review or a locally spawned supervisor — still runs on your own infrastructure, uses your own tokens, and lacks the historical depth of a dedicated platform.

Wayfound's SaaS platform at https://wayfound.ai provides fully independent evaluation by a dedicated AI Supervisor that operates outside your agent's environment entirely. It also adds multi-agent fleet management, shared organizational learning across teams, enterprise compliance (SOC2 Type II), and analytics across thousands of sessions. If you find that supervision improves your work and want a stronger version of it, that is the upgrade path.
