# fabric-troubleshoot-copilot

An open-source AI troubleshooting copilot for Power BI and Microsoft Fabric.

> **Status: early development.** Nothing to install yet.

## What it will do

- An MCP server that reads refresh history, gateway and data source status, and
  failure details from your own tenant, using **read-only** permissions.
- Claude Code skills and subagents that turn those findings into root cause
  analyses (RCAs) and runbooks.
- A demo mode (`FTC_DEMO=1`) that runs on synthetic sample data, so you can try
  it without a tenant.

## Repo layout

- `samples/` — synthetic incident notes and logs (Contoso/Fabrikam, fake IDs)
- `.claude/skills/` and `.claude/agents/` — Claude Code skills and subagents
- `docs/` — design notes and the build curriculum

## License

[MIT](LICENSE)
