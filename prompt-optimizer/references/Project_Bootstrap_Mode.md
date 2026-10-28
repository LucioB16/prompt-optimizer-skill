# PROJECT_BOOTSTRAP Mode

## Purpose

Use PROJECT_BOOTSTRAP when the user wants to start or prepare a coding-agent project/workspace and the most useful handoff is not one giant prompt, but a small kickoff prompt plus persistent project files the agent can read and maintain.

This mode is especially appropriate when the user asks for some combination of:
- a `PLAN.md`
- an `AGENTS.md`
- a `CLAUDE.md`
- a workspace/project folder name
- a ZIP they can extract and open directly in a coding agent
- a long-running implementation plan with checkboxes
- a short prompt that refers to the plan rather than duplicating it

If the request matches this pattern and no mode was named, infer PROJECT_BOOTSTRAP instead of generic AGENT.

## Resolve execution, `/goal`, and Git choices BEFORE finalizing artifacts

The user's explicit decisions always take precedence. Do not apply prior-conversation preferences as hard defaults. Never silently assume `/goal`, Git enablement, local-only Git, or remote creation. Reuse a decision already stated in this request or conversation without asking again. These settings are separate, but execution behavior follows the selected `/goal` mode unless the user explicitly asks for something else.

### 1. `/goal` or equivalent autonomous execution

If the user has not stated whether the kickoff prompt should be designed for an agent `/goal` function or an equivalent uninterrupted goal/autonomous execution mode, ask ONE concise question in the user's language before finalizing the bootstrap artifacts. Explain the consequence in that question: **GOAL = work through all phases autonomously; no GOAL = complete one phase, report and pause for approval before starting the next.**

If the user has answered this in the current request or conversation, never ask again. If the target platform does not literally call the feature `/goal`, preserve the user's intended semantics rather than inventing a feature.

- **GOAL requested:** the kickoff prompt directs the agent to carry out all ordered phases in `PLAN.md` in one continuous run, including verification, checkbox updates and Git commits if Git is enabled. Do not pause between ordinary phases or to request routine implementation choices. Stop only for true blockers, missing user-only information, destructive/irreversible operations requiring permission, unavailable systems, conflicting instructions, or explicitly requested approval gates.
- **GOAL declined / phase-by-phase requested:** the kickoff prompt directs the agent to execute only the next incomplete phase, update/check off verified work, commit phase progress if Git is enabled, report results, then STOP and await the user's go-ahead before starting the next phase. It must not complete all phases in one run.
- **User explicitly overrides execution semantics:** respect the instruction. If they simultaneously demand `/goal` and mandatory per-phase approval, ask a short clarification about the conflict rather than silently choosing one. Do not introduce a *separate* redundant “phase-by-phase or all at once?” question once `/goal` has already resolved the pacing.

### 2. Git and repository publication

If the user has not explicitly said whether to use Git, ask one concise question: **“¿Querés usar Git para registrar el progreso del agente?”** Do not choose Git just because it was used in earlier projects, and do not silently omit it.

- **User chooses no Git:** do not include Git initialization, commits, remotes or pushes in the plan or kickoff prompt. Keep the checkbox-based `PLAN.md` tracker regardless.
- **User chooses Git, but not the scope:** ask whether they want **(a) Git local-only, with commits for tracking and no remote** or **(b) that the agent attempt to create/configure a remote and publish progress**. Do not infer one from previous bootstraps.
- **User explicitly chose local-only:** instruct the agent to initialize or reuse the local repository when appropriate, never configure a remote, never push and never create an online repository. If the workspace already has a remote, do not delete or change it merely to meet this choice: report the discovery and avoid interacting with it unless the user requests a change.
- **User chose remote:** instruct the agent to inspect for an existing repository and remote first; if missing, attempt remote creation/configuration using available, authenticated tooling and appropriate permissions. Determine provider/account and intended repository visibility (private/public) from explicit context; otherwise ask rather than guess or accidentally publish privately held code. Do not expose tokens or assume GitHub is installed/authenticated. If remote creation/push is blocked, preserve local tracking and report the blocker; never claim publication succeeded without verification. The agent should only push when the user has authorized the remote workflow.
- **Git requested, existing repository discovered:** inspect status, branch and remote configuration; preserve unrelated work/history. Avoid destructive rewrites, forced pushes or automatic deletion/reconfiguration of existing remotes.

Clarification strategy: ask only missing material choices; combine the `/goal` and Git questions into a compact conversational message when both are unknown, and ask the Git scope follow-up only if Git was selected. Do not turn the questions into fixed defaults. If the user already specified all choices, proceed without questions.

### 3. Git progress tracking and commits (only when Git is enabled)

The generated `PLAN.md`, `AGENTS.md`, and kickoff prompt must explicitly require this commit protocol:

1. Initialize/reuse Git according to the selected scope, and establish an appropriate `.gitignore` before staging.
2. Make actionable tasks in `PLAN.md` Markdown checkboxes (`- [ ]` initially), update verified tasks to `- [x]` as work proceeds, and add newly discovered required tasks before or while executing them.
3. **Commit incrementally after every meaningful, independently verified milestone with relevant project changes**, even when the current phase is not finished. A milestone can be a working component, a passing feature/fix, completed setup, or validated research/documentation. Update all corresponding `PLAN.md` checkboxes before each milestone commit, stage the related implementation/tests/docs **and the current `PLAN.md`**, use a descriptive message, and record the short hash. This is mandatory Git progress tracking, not an optional optimization for long phases. Do not commit after every minor checkbox or trivial edit.
4. **At the end of every completed, verified phase**, create a final phase checkpoint commit for any remaining relevant changes, including the up-to-date `PLAN.md`. If the final milestone commit already captures the entire verified phase state and the working tree has no remaining project changes, that commit satisfies the phase checkpoint: do not manufacture an empty commit. Report the hash(es) and what each represents.
5. Do not mark a task or phase as complete before its required validation succeeds. When blocked or failing, leave unresolved tasks unchecked, document what happened, and do not create a misleading success commit. If useful, a truthful WIP/checkpoint commit can be made and clearly labeled; never imply the phase passed.
6. **Commit all relevant project work**, including code, configuration, tests, documentation and the current progress checkboxes. Review `git status` and the staged diff before committing; stage only project changes within scope. Do not stage secrets, local credentials, caches, unrelated pre-existing changes, user-private files or unintended large artifacts. Report any relevant uncommitted work and why it remains. Do not amend, reset or rewrite existing commits without an explicit user request.
7. In **local-only** mode, commits remain local: never push. In **remote** mode, push only to the explicitly authorized/configured remote after committing, verify the result, and report any push failure without undoing local progress.
8. With **no Git**, keep checkboxes and progress reporting exactly the same but omit all Git commands, commits and hashes.

## Output contract

Produce all of the following:

1. A concise project/workspace folder name. If the user provided one, preserve it. Otherwise choose a short, descriptive kebab-case name.
2. A downloadable ZIP named `<project-folder>-bootstrap.zip`.
3. Inside that ZIP, one top-level folder named exactly `<project-folder>/`.
4. Inside the folder, at minimum:
   - `PLAN.md`
   - `AGENTS.md`
   - `CLAUDE.md`
5. The kickoff prompt separately in the conversation, in a fenced code block. Do NOT include the kickoff prompt inside the ZIP.
6. Optional `docs/` Markdown files when the project's durable knowledge is too detailed for a compact `AGENTS.md`.

When file-generation capability is available, create the actual files and ZIP; do not merely paste pseudo-files. If artifact creation is genuinely unavailable, say so plainly and provide the complete file contents as the best fallback rather than pretending a ZIP exists.

## Kickoff prompt

Keep the kickoff prompt intentionally short because the detailed workflow lives in project files.

The kickoff prompt must:
- tell the coding agent to read and follow `AGENTS.md`
- tell it to read `PLAN.md` and use it as the execution source of truth
- tell it to update `PLAN.md` checkboxes **during execution**, marking only verified tasks complete
- tell it to keep project documentation current as it discovers durable facts
- reference the plan by the exact filename `PLAN.md`
- avoid duplicating the full plan or AGENTS rules
- make the selected execution pacing unambiguous (autonomous all-phases for GOAL, stop after each completed phase for non-GOAL unless the user explicitly overrides)
- when Git is enabled, require **incremental commits after meaningful validated milestones**, plus the final phase checkpoint covering any remaining changes and the updated `PLAN.md`; respect local-only vs authorized remote publication
- when Git is disabled, contain no commit/push obligations

A normal non-GOAL kickoff prompt must direct the agent to execute **only the first/next incomplete phase**, then stop and ask for permission before the subsequent phase.

A `/goal`-style kickoff prompt must instruct the agent to continue through **every phase** until the objective and definition of done are satisfied, without routine phase pauses, stopping only for true blockers or approval boundaries in `AGENTS.md`/`PLAN.md`.

## `PLAN.md`

`PLAN.md` is the operational execution guide and progress tracker.

Requirements:
- use Markdown task checkboxes (`- [ ]`) for **every executable task**
- organize work into clear ordered phases or milestones
- include discovery/inspection first when the environment, repository, infrastructure, or current state is not fully known
- include prerequisites and dependencies when sequencing matters
- include validation/testing steps, not only implementation steps; make validation tasks checkboxes too
- include a clear Definition of Done / acceptance criteria, preferably as verifiable checkboxes
- include explicit handoff or final verification steps when appropriate
- the agent must change completed tasks from `- [ ]` to `- [x]` as it completes them, **not only at the end**
- when new required work is discovered, add it to the correct section of the plan before or while doing it
- do not mark a checkbox complete unless its work and required validation actually succeeded
- with Git enabled, describe **mandatory intermediate commits after meaningful verified milestones** and a **phase-end validation + progress commit checkpoint**; make Git actions conditional, not tasks in a no-Git plan
- with Git enabled, **every milestone and phase checkpoint commit must include the current `PLAN.md`** if progress changed, alongside all relevant code, tests, configuration and documentation; remote pushes depend on scope
- keep the plan actionable; avoid turning it into a long essay

The plan may be detailed and long when the project warrants it. Unlike `AGENTS.md`, `PLAN.md` is allowed to carry substantial procedural detail because it is the explicit execution artifact.

Example tasks for a Git-enabled phase:

```markdown
## Phase 2 — Implementation
- [ ] Implement the feature and update relevant docs.
- [ ] Run the targeted tests and resolve failures.
- [ ] Mark verified phase tasks complete in this PLAN.md.

With Git enabled: after each meaningful validated milestone, commit its relevant changes and the updated `PLAN.md` immediately. Once all phase tasks pass validation, commit any remaining changes and the current `PLAN.md` as the phase checkpoint. If the last milestone commit already captured the complete verified phase with no remaining changes, reuse that hash rather than creating an empty commit.
```

Avoid a `commit` checkbox that would require a second commit just to check itself. Treat milestone commits as mandatory whenever a meaningful validated milestone produces project changes; the final verified commit is the phase checkpoint when nothing else remains. Report the real hashes after success. Commit only the updated, truthful state; never mark failed work complete.

## `AGENTS.md`

`AGENTS.md` contains durable operating principles, project knowledge, and navigation—not a transcript of the implementation plan.

It should normally include:
- project purpose and objective
- high-level architecture or conceptual structure, when known
- important workspace/repository structure and the purpose of major directories/files
- authoritative source-of-truth files and how they relate (`PLAN.md`, docs, config, tests, etc.)
- durable engineering principles and invariants
- safe tool/use boundaries and approval conditions relevant to the project
- validation expectations
- documentation-maintenance rules
- instruction to read `PLAN.md`, execute it, and keep its checkboxes current
- when Git is enabled: Git-scope choice (local-only vs authorized remote), **mandatory incremental commits at meaningful verified milestones and a phase-end checkpoint**, commits tracking all relevant code/config/tests/docs and the current `PLAN.md` checkboxes, safe staging, and no unintended push or remote changes
- when Git is disabled: no Git requirements whatsoever
- non-GOAL execution: stop and wait for user confirmation after a phase; GOAL execution: proceed across phases without routine approval gates
- instruction to keep `AGENTS.md` compact and move detailed durable material into `docs/`

### Context-budget discipline

Do not let `AGENTS.md` grow without bound. It is loaded frequently and should remain high-signal.

Default budget:
- target roughly 600–1,200 words
- avoid exceeding roughly 1,500 words unless the user explicitly wants a larger file and there is a strong reason

If `AGENTS.md` becomes too large:
1. keep only durable rules, critical invariants, a concise project map, and pointers
2. move detailed architecture, setup procedures, troubleshooting, research notes, conventions, schemas, or operational runbooks into focused Markdown files under `docs/`
3. add or update links/pointers in `AGENTS.md`
4. avoid duplicating the same detailed material in both places

## `docs/` structure

Create `docs/` when the project has durable knowledge that would make `AGENTS.md` too large or too detailed.

Prefer focused files such as:
- `docs/architecture.md`
- `docs/project-structure.md`
- `docs/setup.md`
- `docs/operations.md`
- `docs/decisions.md`
- `docs/research.md`
- `docs/troubleshooting.md`

Use only the files that help the specific project; do not create empty documentation bureaucracy.

When multiple docs exist, consider `docs/README.md` as a short index. `AGENTS.md` should explain that detailed durable project knowledge belongs in `docs/` and should point to the most important files.

## `CLAUDE.md`

`CLAUDE.md` must contain exactly one non-empty line:

Read and strictly follow AGENTS.md.

Do not duplicate project instructions in `CLAUDE.md`.

## Project knowledge and structure

The bootstrap files should reflect the actual project described by the user, not generic boilerplate.

If the existing repository/workspace structure is known from supplied files or context, document that structure accurately. If the workspace is new, describe the intended initial structure and make the plan's discovery phase verify/update it once the agent begins work.

Do not invent existing files, services, credentials, repositories, infrastructure, or completed setup. Distinguish what is known, what is intended, and what the coding agent must discover.

## ZIP structure example

```text
<project-folder>/
├── AGENTS.md
├── CLAUDE.md
├── PLAN.md
└── docs/                 # only when useful
    ├── README.md         # optional index
    └── ...
```

The conversational kickoff prompt is deliberately NOT part of this ZIP.

## Delivery order

A good final response normally presents:
1. project/workspace folder name
2. ZIP download link
3. short kickoff prompt in a fenced code block
4. a brief note about `/goal` behavior or assumptions only if useful

Do not bury the ZIP link or prompt under long explanatory prose.

## Relationship to other modes

PROJECT_BOOTSTRAP can combine with:
- RESEARCH when the plan must instruct the coding agent to investigate current technologies, APIs, or external systems
- EVAL when the project needs strong regression/test/evaluation requirements
- MULTIMODAL when files/images/PDFs are inputs to the coding project
- PLATFORM-SPECIFIC when the bootstrap must account for Codex, Claude Code, OpenCode, Cursor, another agent, or a particular `/goal` implementation

Keep the prompt short even when these modifiers apply. Put durable and procedural detail into `PLAN.md`, `AGENTS.md`, or `docs/` rather than inflating the kickoff prompt.
