# Public Audit Policy

This repository stores public SHA256 commitments and audit metadata for active private artifacts.

The protocol is strategy-neutral:

`multi_strategy_audit_publication_v1`

`orion` is a strategy id. It is not the name of the publication protocol.

## What belongs in the public ledger

`ledger.csv` should contain one row for each active publishable private artifact under:

`artifacts/{strategy_id}/{period}/...`

The ledger should not describe repository maintenance commits. It should describe the artifact itself.

## What does not belong in the active ledger

The active ledger should not cover:

- `README.md`
- `scope.md`
- policy files
- `strategy_registry.csv`
- `migration_report.json`
- `.gitkeep`
- `.gitignore`
- `.gitattributes`
- files under `legacy/old/`

## Meaningful comments

The `comment` field must explain what the artifact contains and why it was published.

Bad comment:

`Improve public audit repository semantic documentation`

Good comment:

`Monthly Orion report for June 2026. Contains the monthly performance summary, drawdown review, exposure statistics, and reconciliation tables for the active publication stream.`

## Publisher-computed fields

The publisher computes and records technical proof fields, including:

- `sha256`
- `file_size_bytes`
- `row_count`
- `created_utc`
- `published_utc`
- `publication_cutoff_utc`
- `timeliness_status`
- `private_commit`
- `public_commit`
- `publisher_run_id`

## Correction rule

Do not edit an old ledger row to make a changed file appear original.

Publish a new row and link it with:

- `correction_of`
- `supersedes_sha256`
