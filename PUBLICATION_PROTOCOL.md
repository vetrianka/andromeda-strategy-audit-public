# Audit Publication Protocol

Protocol id: `multi_strategy_audit_publication_v1`

This protocol is strategy-neutral.

A strategy-specific producer creates finalized artifacts and an owner-supplied manifest. The publisher then computes technical evidence, copies the private file into the private repository, and records a public ledger row.

## Separation of responsibilities

Producer-owned fields:

- `strategy_id`
- `package_id`
- `artifact_type`
- `period`
- `file_date` / `period_start` / `period_end`
- `private_file_name`
- `source_script`
- `methodology_version`
- `comment`
- `notes`
- `correction_of` when applicable

Publisher-computed fields:

- `sha256`
- `row_count`
- `file_size_bytes`
- `published_utc`
- `timeliness_status`
- `private_commit`
- `public_commit`
- `publisher_run_id`
- `publisher_note` when applicable

## Private artifact layout

`artifacts/{strategy_id}/{period}/...`

Recommended periods are `daily`, `monthly`, and `adhoc`.

## Legacy boundary

Deprecated legacy material is retained under `legacy/old/` for transparency and is outside the active audit scope.
