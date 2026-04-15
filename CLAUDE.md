# Adaptive Agent — CLAUDE.md Template

> Copy the "Adaptive Behavior" section below into your project's CLAUDE.md to enable always-on adaptive behavior.

## Adaptive Behavior

Skills and memory are one cognitive system. Skills = procedural memory ("how to do things", muscle memory). Memory = declarative memory ("what I know", conscious memory). They feed each other.

### Always-On Rules

These apply to every session, without needing to invoke a skill:

- **Skill self-improvement**: When a skill encounters friction (step fails, workaround needed), fix the problem AND patch the skill file — add a `## Gotchas` section or update steps. Also save to memory if the lesson is cross-skill.
- **Memory → Skill flow**: Before executing a skill, check memory and gotchas for relevant lessons. Apply them proactively.
- **Skill gap detection**: When a multi-step workflow repeats across sessions and no skill exists, create one.

### Structured Workflows

For deeper review and profile management, use the dedicated skills:

- `/skill-review` — periodic review of all skills for staleness, missing gotchas, and consolidation
- `/build-user-profile` — build or update user profile from workspace context
