# Claude Code curriculum — fabric-troubleshoot-copilot

Pace: 3–4 hours/week, about 10–12 weeks. Tick each box when done.
A module is finished only when its checkpoint passes. Ask the `explainer`
agent to run it: `@"explainer (agent)" run the Module N checkpoint`.

---

## Module 0 — Setup and first session

**Learn:** what Claude Code can see and touch, permission prompts, the VS Code extension.

- [ ] `claude --version` prints a version (update with `claude update` if old)
- [ ] `brew install gh && gh auth login` — GitHub CLI signed in
- [ ] Copy the starter files into `~/code/fabric-troubleshoot-copilot`, then `git init`
- [ ] Start `claude` in the folder; run `/status` and `/context` and read the output
- [ ] Ask Claude: *"Create a public GitHub repo named fabric-troubleshoot-copilot with an MIT license and a placeholder README, and push an initial commit."* Approve each prompt deliberately.
- [ ] Install the Claude Code extension in VS Code and open the same session there

**Checkpoint:** Explain in your own words (1) which files Claude loaded at startup and why, (2) what happened at each permission prompt, (3) how to undo a change Claude made (`/rewind` or `git`).

## Module 1 — CLAUDE.md and memory

**Learn:** project vs user CLAUDE.md, `CLAUDE.local.md`, `@` imports, `/memory`.

- [ ] Read `CLAUDE.md` line by line; delete or change anything you disagree with
- [ ] Create `~/.claude/CLAUDE.md` with 3–5 personal rules that apply to every repo
- [ ] Run `/memory` and open both files from there
- [ ] Test: ask *"write a function that loads settings from .env"* — Claude should leave a `TODO(shan)` and explain in 1–2 lines
- [ ] Say "do mode" and repeat — Claude should just build it

**Checkpoint:** Predict how Claude will respond to a prompt the explainer gives you, then run it. Change one CLAUDE.md line so the behavior changes, and prove it.

## Module 2 — Plan mode and the core workflow

**Learn:** Shift+Tab to plan mode; explore → plan → code → verify → commit.

- [ ] In plan mode, ask for a `uv` project skeleton: `pyproject.toml`, `src/ftc/`, `tests/`, `ruff` + `pytest` configured
- [ ] Review the plan, push back on one thing, then approve
- [ ] `uv run pytest -q` and `uv run ruff check .` both pass
- [ ] Write `src/ftc/demo.py` that loads `samples/` fixtures when `FTC_DEMO=1` — **you** write the function body; Claude only reviews
- [ ] Add README "Quick start (demo mode)" section

**Checkpoint:** Write one function and its test yourself. Explain why plan mode saved (or didn't save) rework.

## Module 3 — Skills and slash commands

**Learn:** `SKILL.md` frontmatter, `$ARGUMENTS`, `allowed-tools`, when Claude auto-loads a skill.

- [ ] Run `/rca-writer samples/notes/refresh-failure-example.md customer`
- [ ] Run it again with `internal` and compare the two outputs
- [ ] Ask *"write an RCA for the Oracle refresh failure"* without the slash — did Claude load the skill on its own? Why?
- [ ] Create a second skill yourself: `/runbook-writer` (from scratch, no copying)

**Checkpoint:** Add a "Detection" section to the RCA template by editing only `SKILL.md` (see the TODO in that file). Explain what `disable-model-invocation: true` would change.

## Module 4 — Subagents

**Learn:** separate context windows, `tools` restriction, `model` choice, `memory`.

- [ ] Read all three files in `.claude/agents/`
- [ ] `@"log-triage (agent)" analyze samples/` — watch it run on Haiku
- [ ] Try asking `log-triage` to edit a file — confirm it can't, and explain why
- [ ] Add a synthetic gateway log to `samples/logs/` and run `/rca-writer` on it end to end (triage → write → review)
- [ ] Run `/context` before and after a subagent run and compare

**Checkpoint:** For each agent, explain why it has those tools and that model. Design a 4th agent on paper; build it only if the explainer agrees it's needed.

## Module 5 — MCP as a consumer

**Learn:** `claude mcp add`, scopes (local / project / user), `/mcp`, auth flows.

- [ ] Set up a personal Power BI / Fabric test tenant (not your employer's) with one small semantic model
- [ ] Connect Microsoft's Remote Power BI MCP server to Claude Code (follow its current Microsoft Learn setup page)
- [ ] Run `/mcp` and confirm it's connected
- [ ] Ask Claude to read the schema and run one DAX query through it

**Checkpoint:** Draw the request path (you → Claude Code → MCP server → Power BI API → model) and explain where auth happens at each hop.

## Module 6 — Build your MCP server

**Learn:** MCP Python SDK, tools, input schemas, stdio transport, testing.

- [ ] Plan mode: design 3 read-only tools — `get_refresh_history`, `get_gateway_datasource_status`, `explain_refresh_error`
- [ ] MSAL device-code auth; tenant/client IDs from `.env`; `.env.example` committed
- [ ] Every tool works in demo mode from `samples/`
- [ ] `pytest` tests for each tool (demo mode, no network)
- [ ] `claude mcp add` your own server and use it from Claude Code
- [ ] README: "Connect your own tenant" + "Required permissions"

**Checkpoint:** Add a 4th tool on your own (Claude reviews only).

## Module 7 — Hooks and guardrails

**Learn:** `PreToolUse` / `PostToolUse`, exit code 2 to block, `/hooks`.

- [ ] `PostToolUse` hook: run `ruff format` after Python edits
- [ ] `PreToolUse` hook: block writes that contain GUID- or token-shaped strings outside `samples/`
- [ ] README "Security and data handling" section

**Checkpoint:** Write a hook from scratch that blocks a command you choose, and show it firing.

## Module 8 — Headless, CI, and the portfolio

**Learn:** `claude -p`, GitHub Actions, packaging, plugins.

- [ ] GitHub Actions: `pytest` + `ruff` on every push
- [ ] A `claude -p` review job (check current GitHub Actions docs and Pro plan limits first)
- [ ] Decide: PyPI package (`uvx`) and/or Claude Code plugin — check the current plugin docs
- [ ] Final README: architecture diagram, demo GIF, quick start, permissions, security
- [ ] 3-minute demo video + LinkedIn post
- [ ] Tag `v0.1.0` release

**Checkpoint:** Explain the whole system in 5 minutes as if in an interview, then answer 3 hard questions from the explainer.
