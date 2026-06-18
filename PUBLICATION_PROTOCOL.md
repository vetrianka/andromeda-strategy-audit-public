# Publication Protocol

Protocol id:

`multi_strategy_audit_publication_v1`

This protocol is independent of any single strategy. A strategy is identified by `strategy_id`, for example `orion`.

## Roles

### Producer

A producer is the strategy-specific script or workflow that creates a file.

The producer knows:

- what the file contains;
- which strategy it belongs to;
- whether it is daily, monthly, or adhoc;
- the correct human comment for the file;
- the source script and methodology label.

The producer should write a publication manifest into an outbox. The manifest should contain owner-supplied metadata such as `strategy_id`, `period`, `artifact_type`, `private_repo_path`, `comment`, `notes`, `source_script`, and `methodology_version`.

### Publisher

The publisher is strategy-neutral. It does not decide what a file means.

The publisher:

1. reads the manifest;
2. validates the file and destination path;
3. computes SHA256, size, and row count when applicable;
4. copies the artifact into the private repository;
5. commits the private artifact;
6. appends the public ledger row;
7. publishes the public ledger change.

## Standard periods

- `daily`: scheduled daily outputs tied to a date or trading session.
- `monthly`: scheduled monthly reports and reconciliation files.
- `adhoc`: important non-scheduled material, such as major change history, unusual reports, incident reviews, methodology notes, and one-off reconciliation packages.

## Active artifact path convention

`artifacts/{strategy_id}/{period}/...`

Examples:

`artifacts/orion/daily/2026/2026-06-18.targets.csv`

`artifacts/orion/monthly/2026/2026-06/monthly_report.pdf`

`artifacts/orion/adhoc/2026/2026-06-18_publication_change_note.md`

## Comment rule

Commit messages describe repository changes. Ledger comments describe file contents.

Never use a repository maintenance phrase as a ledger artifact comment.

## File role

This file defines the strategy-neutral publication process: producer manifest, publisher validation, private artifact storage, and public hash-ledger publication. It is not named after any single strategy.
