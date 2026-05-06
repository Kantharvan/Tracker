---
project: iam-ui
last_updated: 2026-05-06T11:00
active_branch: hotfix/pin-piper-pipeline-github-v1.56.2
---

# PROJECT.md — iam-ui

## Current state
iam-ui is on a hotfix branch (`hotfix/pin-piper-pipeline-github-v1.56.2`) that pins two upstream pipeline dependencies to resolve npm artifact resolution failures. GitHub Actions Release stage (run #33941307) now progresses further (Build → Acceptance → Promote) but fails at DWC artifact promotion timeout — infrastructure-side issue unrelated to the hotfixes themselves.

## Active problems
- **DWC Release stage timeout** (`sapDwCStageRelease`, run #33941307): `context deadline exceeded` during artifact promotion. Fails after passing Build, Acceptance, Promote stages. Likely requires DWC service investigation or timeout configuration adjustment.
- **Obsidian `/clear` command broken**: Session notes not being written to vault on session close despite prior functionality. `obsidian-session-note.sh` hook appears inactive or not detecting summary file.
- **Jira ticket creation pending**: Pipeline version fix (`sap-piper` + `piper-pipeline-github` hotfix) needs Jira ticket — unclear whether it should go under `CALMSERVICES-60965` (Replication Failures parent) or be standalone.

## Recent decisions
- 2026-05-06 11:00: Use wrapper script (`boot-session-files.sh`) instead of inline variable expansion in `/boot` command — avoids Claude Code safety gate firing on shell variables and keeps allow rules static and unambiguous.
- 2026-05-06 11:00: Keep `/boot` output to ~4 lines max (branch, last worked on, open items, watch-outs) — skip empty sections to maintain scannability.
- 2026-05-06 09:19: Named session context loader command `/boot` (over `/catchup`, `/memoria`, etc.) and made it project-aware (reads only from current project subfolder).
- 2026-05-06 08:31: Converted 11 old session notes from flat format to YAML frontmatter, organized by project subfolder.
- 2026-05-06 08:29: Root cause of Release stage failure is DWC timeout, not the hotfixes — hotfixes are working (pipeline progresses further than pre-fix).

## Don't forget
- **Branch is a hotfix**: `hotfix/pin-piper-pipeline-github-v1.56.2` — two version pins already applied and validated to improve upstream dependency resolution.
- **MCP tool config quirk**: `sap-jira` tool reads `.jira-config.json
