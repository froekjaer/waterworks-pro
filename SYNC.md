# SYNC.md — Repository relationships and base-sync procedure

## Language policy

**All new content in this repository — code, comments, documentation, commit
messages, issues and PRs — is written in English.**

Content inherited from `timelapse-pro` is largely Danish. It is translated
incrementally as part of the pivot work (when a file is touched for pivot
reasons, its Danish text is translated in the same change). Do not run
bulk-translation PRs; they would create needless upstream-merge conflicts.

## Repository topology

| Remote | Repository | Role |
|---|---|---|
| `origin` | `froekjaer/waterworks-pro` | This repo — the waterworks product (pivot) |
| `upstream` | `froekjaer/timelapse-pro` | The base — timelapse platform, receives ongoing improvements |
| — | `froekjaer/water-treatment-interface` | Public emulator sandbox (separate repo, Apache 2.0) |

`waterworks-pro` was seeded as a full copy of `timelapse-pro` (main + tags).
GitHub does not allow forking your own repository within one account, so the
fork-relationship is maintained manually via the `upstream` remote — the
effect is the same.

## How to pull base improvements (the sync)

```bash
git fetch upstream
git checkout main
git merge upstream/main --no-edit
# resolve conflicts if any, then:
git push origin main
```

### Sync rules

1. **Sync regularly** — small frequent merges beat rare big-bang merges.
   Suggested cadence: after every notable timelapse-pro release/PR batch.
2. **Keep reused subsystems on identical paths** — auth, GRC, CMDB,
   notifications, update flow, backend layout. Renaming/moving files that
   upstream still evolves guarantees merge conflicts.
3. **Pivot changes go in feature branches** and are squash-merged, exactly
   like the timelapse-pro workflow (PR + CI + signed commits).
4. **Conflicts are resolved in favour of the pivot** for files we have
   deliberately changed (UI pages, ingest endpoints); in favour of upstream
   for shared infrastructure we have not touched.
5. **Never push internal documents to public repos.** This repo's history
   contains internal operational docs (handover log, GRC register, security
   reviews). Visibility changes are a deliberate, separate decision.
6. **Tags follow upstream** (`git push origin --tags` after a sync) so the
   update flow can reference released versions.

## Pivot boundary (what diverges from the base)

Tracked here as the pivot proceeds — update this list when a subsystem is
deliberately changed away from the base:

- *(pending)* Captures/images → telemetry time series
- *(pending)* Customer→Site→Camera hierarchy → Utility→Plant→Measurement point
- *(pending)* Timelapse UI pages → plant overview / trend / alarm pages
- *(pending)* Danish UI text → English UI text (per language policy)

Everything not listed here should be treated as shared with the base and
kept merge-friendly.
