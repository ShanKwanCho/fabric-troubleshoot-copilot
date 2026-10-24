# fabric-troubleshoot-copilot

An open-source AI troubleshooting copilot for Power BI and Microsoft Fabric:
an MCP server that reads refresh history, gateway/datasource status and failure
details, plus Claude Code skills and subagents that turn findings into RCAs and
runbooks. Public repo — anyone can run it against their own tenant.

Owner: Shan. This repo is also a structured Claude Code learning project.

## How to work with me (teach mode is the default)

- I am learning Claude Code and Python (just past beginner). Explain the *why*
  in 1–2 lines, then give exact commands. No long conceptual essays.
- For any new concept, leave the core logic for me as a `TODO(shan):` marker
  with a one-line hint. Write boilerplate, tests scaffolding, and config yourself.
- When I say "do mode", skip the teaching and just build. Return to teach mode
  on the next new concept.
- Before a multi-file change, use plan mode and show me the plan first.
- Don't let me skip a checkpoint. If I ask to move to the next module, ask me
  the current module's checkpoint questions first (see docs/curriculum.md).
- Prefer small commits with clear messages. Never push without asking me.

## Stack

- macOS, zsh, VS Code. Python 3.12+ managed with `uv`.
- Lint/format: `ruff`. Tests: `pytest`. Type hints on all public functions.
- MCP server: official MCP Python SDK. Auth: MSAL (device code flow).
- Power BI / Fabric REST APIs, **read-only scopes only**.

## Commands

```bash
uv sync                      # install deps
uv run pytest -q             # run tests
uv run ruff check . --fix    # lint + autofix
uv run ruff format .         # format
```

## Public-repo rules (non-negotiable)

- Never commit secrets, tenant IDs, client IDs, tokens, UPNs, or connection
  strings. Config comes from `.env` (gitignored); keep `.env.example` current.
- Sample data in `samples/` is synthetic only. Use Contoso/Fabrikam names and
  obviously fake GUIDs. Never paste real customer or employer data.
- The MCP server must never write to a tester's tenant. Any new API call must
  be a GET (or the read-only `executeQueries` endpoint) and documented in
  README "Required permissions".
- Every tool must work in demo mode (`FTC_DEMO=1`) using `samples/` fixtures,
  so people without a tenant can try it.

## Repo map

- `src/ftc/` — MCP server package (built in Module 6)
- `samples/` — synthetic logs, refresh failures, and incident notes
- `.claude/skills/rca-writer/` — turns notes/findings into an RCA
- `.claude/agents/` — log-triage, doc-reviewer, explainer
- `docs/curriculum.md` — modules and checkpoints

## Writing style for RCAs and runbooks

Customer-facing: plain, calm, no blame, no internal jargon, every claim tied to
evidence. Internal: terse, timeline-first, exact error codes and commands.
Both: no hedging filler, no bullet soup, state uncertainty explicitly once.
