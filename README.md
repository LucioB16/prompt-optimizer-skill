# Prompt Optimizer Skill (Lyra)

[![Release](https://img.shields.io/github/v/release/LucioB16/prompt-optimizer-skill?label=release)](https://github.com/LucioB16/prompt-optimizer-skill/releases)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](./LICENSE)
[![Codex](https://img.shields.io/badge/Codex-compatible-blue)](https://github.com/LucioB16/prompt-optimizer-skill)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-orange)](https://github.com/LucioB16/prompt-optimizer-skill)

Portable Agent Skill for both Codex and Claude Code.

This repository keeps a single source of truth in:

- `prompt-optimizer/SKILL.md`

## Repository Layout

- `prompt-optimizer/SKILL.md` - skill logic (shared by both platforms)
- `prompt-optimizer/agents/openai.yaml` - Codex UI metadata

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

## Why One Repo

Keeping Codex and Claude in one repository avoids drift and makes versioning, issues, and releases easier.

## License

MIT
