---
project: iam-service
last_updated: 2026-05-06T12:05
active_branch: feat/CALMSERVICES-58425-remove-replication-admin-rest-controller-clean
---

## Current state
Completed feature implementation for CALMSERVICES-58425 (removal of `ReplicationAdminRestController` and 8 exclusive admin methods per ADR). PR #3784 is open and ready for review. Parallel investigation underway on System Trust pipeline registration issue affecting scheduled runs.

## Active problems
- "System Trust Session Token Retrieval Failed" warning on all scheduled pipeline runs (nightly 23:40 UTC)
  - Root cause: Pipeline group `PIPELINE-GROUP-1183` (used by `sap-oss-ppms-workflow.yml`) not registered in System Trust database
  - Non-blocking — pipeline continues executing despite token retrieval failure
  - **Action pending**: Awaiting Piper/Compliance team response on Slack; verify resolution on next scheduled run
  - Tracked in pinned GitHub issue #3785

## Recent decisions
- 2026-05-06 11:44: Rebased `feat/CALMSERVICES-58425-remove-replication-admin-rest-controller-clean` onto `main` instead of merging to isolate changes from unrelated `fix/dependabot-security-overrides` commits (including `mtx/sidecar/package.json` drift)
- 2026-05-06 11:44: Closed old PR #3783; created clean PR #3784 with 7 expected files only
- 2026-05-06 12:05: Escalated System Trust registration issue to Piper/Compliance team; opened tracking issue #3785

## Jira tickets
- CALMSERVICES-58425: Remove `ReplicationAdminRestController` and exclusive admin methods (completed, PR #3784 open)
- GROUP-1183: System Trust pipeline group registration (infrastructure issue, awaiting platform team response)

## Don't forget
- **Deleted files** (PR #3784): `ReplicationAdminRestController.java`, `ReplicationAdminRestControllerTest.java` (unit + integration tests)
- **Removed from `ReplicationAdminService`**: `queuedSanitySanityCheckEvent`, `sanityCheckQueuedTasks`, `sanityCheckIssues`, `sanityCheckActions`, `sanityCheckIssue`, `sanityCheckAction`, `isConsistent`, `resetMessageProcessing`, `fetchMessage`
- **Removed from `PublishReplicationHelper`**: `fetchMessage` method (verified no other callers)
- System Trust warning is platform/infra issue, not code-related — do not attempt code fixes
- Hyperspace Portal on
