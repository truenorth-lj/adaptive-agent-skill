# Adaptive Agent

[![GitHub](https://img.shields.io/github/stars/truenorth-lj/adaptive-agent-skill?style=social)](https://github.com/truenorth-lj/adaptive-agent-skill)
[![skills.sh](https://img.shields.io/badge/skills.sh-install-blue)](https://skills.sh/truenorth-lj/adaptive-agent-skill)
[![ClawHub: skill-review](https://img.shields.io/badge/ClawHub-skill--review-purple)](https://clawhub.ai/skills/adaptive-agent-skill-review)
[![ClawHub: build-user-profile](https://img.shields.io/badge/ClawHub-build--user--profile-purple)](https://clawhub.ai/skills/adaptive-agent-build-user-profile)
[![Anthropic Plugin](https://img.shields.io/badge/Anthropic_Plugin-pending_review-orange)](https://github.com/truenorth-lj/adaptive-agent-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Make your AI coding agent get stronger with use — skill self-improvement, memory-skill feedback loops, and automatic user profiling.

Inspired by [Hermes Agent](https://github.com/NousResearch/hermes-agent)'s four-layer memory architecture, adapted for Claude Code and other AI coding agents through prompt engineering.

## Install

```bash
# skills.sh (Claude Code, Codex, Gemini CLI, Copilot, 45 agents)
npx skills add truenorth-lj/adaptive-agent-skill

# ClawHub
clawhub install adaptive-agent-skill-review
clawhub install adaptive-agent-build-user-profile

# Manual (any agent)
git clone https://github.com/truenorth-lj/adaptive-agent-skill.git
cp -r adaptive-agent-skill/skills/skill-review ~/.claude/skills/
cp -r adaptive-agent-skill/skills/build-user-profile ~/.claude/skills/
```

## Directory Structure

```
adaptive-agent-skill/
├── CLAUDE.md                          # Always-on behavioral rules (paste into your project)
├── .claude-plugin/
│   └── plugin.json                    # Claude Code plugin manifest
├── skills/
│   ├── skill-review/
│   │   └── SKILL.md                   # Periodic skill audit and improvement
│   └── build-user-profile/
│       └── SKILL.md                   # Build user profile from workspace context
├── templates/
│   └── user_profile.md                # Memory template for user profile
├── meta.json                          # Skill metadata
├── LICENSE
└── README.md
```

## The Idea

AI agents treat skills and memory as separate systems. They shouldn't.

- **Skills** = procedural memory ("how to do things" — muscle memory)
- **Memory** = declarative memory ("what I know" — conscious memory)
- They should feed each other in a loop

When a skill fails, the lesson should flow back into the skill AND into memory. When memory contains a relevant gotcha, it should be applied proactively before the skill hits the same wall.

### Three-Layer Architecture

```
┌────────────────────────────────────────────────┐
│  CLAUDE.md — Always-On Rules                    │
│  Auto-patch skills on friction                  │
│  Apply memory lessons proactively               │
│  Detect skill gaps from repeated workflows      │
├────────────────────────────────────────────────┤
│  Skills — Procedural Memory                     │
│  /skill-review: audit and improve all skills    │
│  /build-user-profile: build who-you-are memory  │
├────────────────────────────────────────────────┤
│  Memory — Declarative Memory                    │
│  user_profile.md: role, strengths, preferences  │
│  (agent's memory system, auto-loaded)           │
└────────────────────────────────────────────────┘
```

## Setup

### 1. Add behavioral rules to your project

Copy the "Adaptive Behavior" section from `CLAUDE.md` into your project's CLAUDE.md. This enables the always-on rules:

- Skill self-improvement (auto-patch on friction)
- Memory → Skill flow (apply gotchas proactively)
- Skill gap detection (create skills for repeated workflows)

### 2. Build your user profile

Run `/build-user-profile` in your first session. The agent will:

1. Scan your workspace (git history, project structure, tech stack)
2. Synthesize a profile (role, strengths, work style, preferences)
3. Ask you to validate
4. Save to memory — every future session starts knowing who you are

### 3. Review skills periodically

Run `/skill-review` after a batch of tasks. The agent will:

1. Inventory all skills with last-modified dates
2. Cross-reference with memory and gotchas
3. Patch stale skills, flag gaps
4. Report summary

## How It Works

### Skill Self-Improvement (Always-On)

```
You run /deploy → a step fails (API changed)
  → Agent fixes the immediate problem
  → Agent patches the skill file (adds ## Gotchas or updates steps)
  → Agent saves lesson to memory if cross-skill
  → Next session: skill has the fix baked in
```

### Memory → Skill Flow (Always-On)

```
Memory has: "Zeabur CLI domain create needs GraphQL fallback"
You run /deploy with Zeabur step
  → Agent checks memory before executing
  → Agent applies the workaround proactively
  → You never hit the known issue
```

### User Profile (On Demand)

```
First session: /build-user-profile
  → Gathers context from git, config files, memory
  → Writes structured profile to memory
  → Every future session starts knowing who you are
```

## Background: Hermes Agent's Memory Architecture

This project is based on a [source-code analysis](https://zengineer.blog/blog/deeptech/hermes-agent-deep-dive-nous-research-ai-agents-2026/) of Hermes Agent v0.9.0, which implements a four-layer memory system:

| Layer | Hermes Implementation | Our Adaptation |
|-------|----------------------|----------------|
| L4: Procedural Memory | Skills with auto-create + auto-patch | Skills with CLAUDE.md auto-patch rules |
| L3: Semantic Memory | FTS5 cross-session search | Not possible (architecture constraint) |
| L2: Declarative Memory | MEMORY.md + USER.md (bounded) | Agent memory files (no char limit) |
| L1: External Plugins | Honcho, mem0, 11 backends | Not possible (architecture constraint) |

We can't replicate L1 and L3 (they require infrastructure changes), but L2 and L4 are achievable through prompt engineering alone. The key insight: **Opus 4.6's instruction-following ability compensates** — a smart agent that follows behavioral rules reliably is equivalent to a less-smart agent with built-in memory infrastructure.

## Constraints

| Limitation | Why | Workaround |
|-----------|-----|------------|
| No cross-session search | Agent can't search past conversations | Memory files persist key lessons |
| No background review | Can't spawn background LLM calls | Manual `/skill-review` invocation |
| No proactive memory saves | Only saves when rules trigger | CLAUDE.md rules cover common cases |
| Permission gates | Skill patches need user approval | Expected — user controls the boundary |

## Compatibility

| Agent | Status |
|-------|--------|
| Claude Code | Full support (primary target) |
| Codex CLI | Skills work, CLAUDE.md → AGENTS.md |
| Gemini CLI | Skills work, CLAUDE.md equivalent varies |
| OpenClaw | Skills work via agentskills.io standard |
| Cursor | Skills work via .cursorrules |

## License

MIT
