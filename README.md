# Spec Driving

> 将任何长期软件项目带入规范驱动、证据驱动的交付流程。
> Turn long-lived software projects into spec-driven, evidence-based delivery systems.

`spec-driving` is a governance skill for AI-assisted software delivery. It does not teach an agent a particular framework or replace planning, debugging, testing, or review skills. Instead, it establishes the project rules, durable state, acceptance evidence, and delivery boundaries that keep those methods aligned over a long project.

`spec-driving` 是一个面向 AI 协作开发的项目治理 Skill。它不替代需求分析、计划、调试、测试或代码评审；它负责让这些方法始终围绕同一份项目规范、阶段状态、验收证据和交付边界工作。

## Why it exists / 核心优势

Most agent workflows can produce code, but long projects often lose state between conversations: requirements remain in chat, decisions are forgotten, tests prove only a happy path, and Git history does not explain what was actually verified.

Spec Driving addresses that gap:

- **Bootstrap project discipline** — create or complete `AGENTS.md` and `docs/dev_spec.md` before L2/L3 work begins.
- **阶段决策包，确认后持续推进 / Decide once per phase, then keep moving** — collect the current phase's material decisions into one recommended decision package. Confirm it, record it, and proceed; use reversible defaults for ordinary implementation details.
- **路线图先可见，再进入实现 / Show the roadmap before implementation** — after design approval, explicitly present every delivery phase, its dependencies and acceptance outcome before beginning L2/L3 work; never hide the plan behind a document link.
- **尊重本轮授权范围 / Respect the turn's authorization** — distinguish “design only” from “execution authorized,” so the agent neither acts beyond the latest instruction nor mistakes an approval wait for a blocker.
- **Keep durable project state** — scope, current phase, dependencies, acceptance criteria, decisions, risks, blockers, and next steps live in the repository rather than in chat memory.
- **Require evidence, not optimism** — HTTP 200, mock data, and code review alone do not prove delivery. Validate through real APIs, data, browser interaction, third-party test environments, or measurable performance evidence.
- **Preserve delivery boundaries** — distinguish verified work from WIP, avoid committing secrets or temporary files, and require explicit authorization for irreversible external actions.
- **Work with specialist methods** — route to available skills for brainstorming, planning, debugging, testing, review, verification, and branch completion without giving up project-specific acceptance rules.

## The model

```text
$spec-driving
  ↓
Inspect AGENTS.md + docs/dev_spec.md
  ↓
Missing or insufficient information?
  ↓
Read-only discovery → confirm the phase decision package → write decisions and assumptions into project files
  ↓
Start the next safe action in the same turn → choose the applicable planning/debugging/testing/review method
  ↓
Implement → verify with real evidence → update dev spec → commit honestly
```

The key rule is simple:

> Do not use chat memory, temporary assumptions, or unrecorded verbal decisions as a substitute for project specifications.

> 不得用聊天记忆、临时假设或未落盘的口头结论替代项目规范。

## What it governs

| Concern | Where it lives |
|---|---|
| Stable project rules, commands, safety boundaries, file routing | `AGENTS.md` |
| Current scope, phase, acceptance, decisions, evidence, blockers | `docs/dev_spec.md` |
| Detailed implementation plan | Project plan files, linked from the dev spec |
| How to brainstorm, plan, debug, test, review, and finish | Available specialist skills |
| Honest delivery history | Git commits and verification records |

## Installation

The reusable skill is the inner [`spec-driving/`](spec-driving) directory.

### Codex

Copy or symlink it into your Codex skills directory:

```bash
cp -R spec-driving ~/.codex/skills/spec-driving
```

Then start a task with:

```text
$spec-driving
Continue the current project phase with real verification.
```

The included Codex metadata disables implicit invocation by default, so high-discipline workflows are entered deliberately.

### Claude Code

Copy or symlink the same directory into:

```text
~/.claude/skills/spec-driving/
```

Then explicitly ask Claude Code to use the `spec-driving` skill for L2/L3 work, phase recovery, real integrations, acceptance, or delivery review.

### Pi

Pi supports the Agent Skills format directly. Either place the directory in:

```text
~/.pi/agent/skills/spec-driving/
```

or add your Codex skill directory to `~/.pi/agent/settings.json`:

```json
{
  "skills": ["~/.codex/skills"]
}
```

Reload Pi and invoke:

```text
/skill:spec-driving
```

## Project setup

For a new or under-specified project, explicitly invoke the skill and let it establish the minimum startup specification:

- `AGENTS.md`: command table, prohibitions, and file routing;
- `docs/dev_spec.md`: goals/non-goals, one real business loop, current status, and acceptance criteria.

The bundled references include templates for both files, handoff reports, and common failure modes.

## Scope

Use `spec-driving` for:

- phase recovery in an ongoing repository;
- L2/L3 feature work;
- real third-party, payment, order, data-sync, or migration work;
- acceptance review and delivery preparation;
- creating project conventions before substantial implementation begins.

For small L0/L1 work, use the existing project specification as context; do not turn a one-line correction into a planning ceremony.

## Compatibility

The core `SKILL.md` and `references/` follow the Agent Skills format and work in Codex, Claude Code, and Pi. The optional `agents/openai.yaml` provides Codex-specific UI metadata; other harnesses ignore it safely.

When a listed specialist skill is unavailable, `spec-driving` requires a safe fallback rather than blocking work. It can complement Superpowers-style skill collections, but does not bundle or require them.

## Repository structure

```text
spec-driving/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── agents-template.md
    ├── dev-spec-template.md
    ├── handoff-template.md
    └── anti-patterns.md
```

## License

[MIT](LICENSE)
