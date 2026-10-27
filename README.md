# Lyra · Prompt Optimizer Skill

**Turn a rough request into a precise, copy-ready prompt for the model or agent you're using.**

A portable Agent Skill that clarifies goals, constraints, output formats and missing assumptions across research, coding and everyday AI workflows. One shared `prompt-optimizer/SKILL.md` is the source of truth for supported installations.

[![Release](https://img.shields.io/github/v/release/LucioB16/prompt-optimizer-skill?label=release)](https://github.com/LucioB16/prompt-optimizer-skill/releases)
[![MIT](https://img.shields.io/badge/license-MIT-466a59)](LICENSE)

| Research | Coding agents | Everyday prompts |
| :-- | :-- | :-- |
| Define evidence, sources and comparison criteria. | Clarify scope, acceptance checks and constraints. | Specify intent, audience, tone and output. |

## Try it

After installing the skill, ask your agent to optimize the prompt you have. For example:

> Use Lyra to improve this prompt for a coding agent. Keep the technical constraints, resolve ambiguities, and return one copy-ready version.

## Install in Codex

Copy the skill folder to your global Codex skills directory:

```powershell
Copy-Item -Path .\prompt-optimizer -Destination "$HOME\.codex\skills\prompt-optimizer" -Recurse -Force
```

## Install in Claude Code

Copy the same `SKILL.md` into Claude skills:

```bash
mkdir -p ~/.claude/skills/prompt-optimizer
cp prompt-optimizer/SKILL.md ~/.claude/skills/prompt-optimizer/SKILL.md
```

For shared project-level setup, add this file to your repo:

- `.claude/skills/prompt-optimizer/SKILL.md`

## Repository Layout

- `prompt-optimizer/SKILL.md` - skill logic (shared by both platforms)
- `prompt-optimizer/agents/openai.yaml` - Codex UI metadata

## Why One Repo

Keeping Codex and Claude in one repository avoids drift and makes versioning, issues, and releases easier.

## License

MIT
