---
# The folder name gives the slash command: /rca-writer
# description: Claude reads this to decide when to load the skill on its own.
description: Turns incident notes, logs, or triage findings for a Power BI / Fabric / gateway problem into a root cause analysis (RCA). Use when asked to write an RCA, post-incident report, or customer incident summary.
# argument-hint: shown in autocomplete. $ARGUMENTS below receives what you type.
argument-hint: "[path-to-notes-or-log] [customer|internal]"
# allowed-tools: pre-approved only for the turn that runs this skill.
# Writing the RCA file is NOT pre-approved on purpose: you'll see the permission prompt.
allowed-tools: Read Grep Glob
---

# RCA writer

Input: $ARGUMENTS
First argument is the source file or folder. Second is the audience
(`customer` or `internal`); default to `internal` if missing.

## Steps

1. If the source is a raw log or a folder of logs, delegate to the
   `log-triage` subagent first and work from its timeline. If it is already
   prose notes, read it directly.
2. Fill the template below. Every root-cause statement must cite evidence
   (a log line, timestamp, or error code from the source). If evidence is
   missing, write "Not confirmed" — never invent a cause.
3. Save to `docs/rca/<yyyy-mm-dd>-<short-slug>.md`.
4. Delegate the saved file to the `doc-reviewer` subagent. Apply its
   must-fix items, then show me its remaining suggestions.
5. Finish with the file path and a 2-line summary. Don't paste the whole RCA.

## Template

```markdown
# RCA: <one-line problem statement>

**Audience:** customer | internal  **Status:** draft
**Impact window:** <start> – <end> (timezone)

## Summary
<3 sentences: what broke, who was affected, what fixed it.>

## Timeline
| Time | Event | Evidence |
|---|---|---|

## Root cause
<Cause, with evidence. Separate confirmed facts from hypotheses.>

## Contributing factors

## Resolution

## Prevention / follow-ups
| Action | Owner | Due |
|---|---|---|
```

## Audience rules

- `customer`: no internal hostnames, ticket numbers, or engineer names. No blame.
  Explain error codes in plain words.
- `internal`: keep exact error codes, activity IDs (redacted to last 4 chars),
  and the commands used to diagnose.

<!-- TODO(shan) Module 3 checkpoint: add a "Detection" section to the template
     (how the problem was first noticed) using only this file — no code. -->
