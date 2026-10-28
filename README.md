# Prompt Optimizer Skill

[![Release](https://img.shields.io/github/v/release/LucioB16/prompt-optimizer-skill?label=release)](https://github.com/LucioB16/prompt-optimizer-skill/releases)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](./LICENSE)
[![Codex](https://img.shields.io/badge/Codex-compatible-blue)](https://github.com/LucioB16/prompt-optimizer-skill)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-orange)](https://github.com/LucioB16/prompt-optimizer-skill)

Portable, persona-free prompt-optimization skill for coding agents, including Codex and Claude Code. It can create copy-ready prompts for coding, research, evaluation, and other AI workflows. It optimizes the prompt; it does not execute the underlying task unless expressly asked.

## Repository layout

- `prompt-optimizer/SKILL.md` — authoritative workflow, mode/model selection, and output rules.
- `prompt-optimizer/references/Prompt_Optimization_Reference.md` — detailed method and examples.
- `prompt-optimizer/references/Project_Bootstrap_Mode.md` — coding-agent project bootstrap, artifacts, execution pacing, and Git decisions.
- `prompt-optimizer/agents/openai.yaml` — Codex UI metadata.

## Modes

BASIC, DETAIL, RESEARCH, AGENT, PROJECT_BOOTSTRAP, EVAL, MULTIMODAL, and PLATFORM-SPECIFIC. Combine relevant modes when useful (for example AGENT + RESEARCH).

PROJECT_BOOTSTRAP is for starting a coding-agent project with a short kickoff prompt plus a ZIP containing `PLAN.md`, `AGENTS.md`, and `CLAUDE.md`. It asks about autonomous `/goal`-style execution and Git use before finalizing artifacts; neither choice is assumed. Creating a ZIP requires file-generation capability in the destination environment.

## Install in Codex

Copy the complete skill directory into your global Codex skills directory:

```powershell
New-Item -ItemType Directory -Force -Path "$HOME\.codex\skills\prompt-optimizer" | Out-Null
Copy-Item -Path .\prompt-optimizer\* -Destination "$HOME\.codex\skills\prompt-optimizer" -Recurse -Force
```

## Install in Claude Code

Copy the **whole directory**, including `references/` (copying only SKILL.md would omit required bootstrap guidance):

```bash
mkdir -p ~/.claude/skills/prompt-optimizer
cp -R prompt-optimizer/. ~/.claude/skills/prompt-optimizer/
```

For project-level installation, copy the entire `prompt-optimizer/` directory to `.claude/skills/prompt-optimizer/`.

## Maintenance

Keep the core rules in SKILL.md and detailed guidance in references. Avoid conflicting or duplicated instructions; verify behavior in the target coding agents before publishing a new release.

## License

MIT
