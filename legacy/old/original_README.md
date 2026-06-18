# Andromeda Strategy Audit Public Ledger

This is the public audit repository for the Andromeda strategy.

This repository does not contain private trading signals, positions, scores, weights, returns, raw market data, or strategy internals.

It contains only public cryptographic commitments, audit metadata, ledger records, and audit snapshots for private files stored separately.

The corresponding private repository is:

`andromeda-strategy-audit-private`

## Purpose

The purpose of this repository is to provide a public, timestamped audit trail for private strategy files.

For each private audit file, the SHA256 hash is published in `ledger.csv`.

An auditor can later receive the private file, recompute its SHA256 hash, and compare it with the hash previously published in this repository.

If the hash matches, the private file is exactly the same byte-level file that was previously committed to the public ledger.

## Core audit principle

The private repository stores confidential files.

This public repository stores only SHA256 commitments and audit metadata.

The audit chain is:

private file  
→ SHA256  
→ public ledger  
→ public audit snapshot / release  
→ independent verification by auditor

This repository is intended to make later file substitution detectable.

## What is public

This repository may contain:

- `README.md`;
- `AUDIT_PUBLIC_POLICY.md`;
- `ledger.csv`;
- public audit snapshots;
- public checksums for public snapshot files;
- public release metadata.

This repository must not contain:

- private signals;
- tickers and weights from private daily target files;
- model scores;
- private portfolio returns;
- raw Andromeda data;
- adjusted price files;
- internal strategy research files;
- model training artifacts;
- candidate grids;
- debug logs.

## Public repository structure

Expected structure:

`README.md`

`AUDIT_PUBLIC_POLICY.md`

`ledger.csv`

`snapshots/`

Monthly or package-level public snapshots may be stored under:

`snapshots/YYYY-MM/`

or:

`snapshots/audit_package_v1/`

## Public ledger

The main public ledger file is:

`ledger.csv`

The recommended ledger schema is:

`published_date,strategy_id,package_id,artifact_type,file_date,period,private_file_name,sha256,row_count,file_size_bytes,created_utc,publication_cutoff_utc,timeliness_status,notes`

Column meanings:

- `published_date` — calendar date when the ledger row is published.
- `strategy_id` — strategy identifier.
- `package_id` — operational or audit package identifier.
- `artifact_type` — type of private artifact being committed.
- `file_date` — date associated with the private file.
- `period` — daily, monthly, static, or audit package period.
- `private_file_name` — path to the private file inside the private repository.
- `sha256` — SHA256 hash of the private file bytes.
- `row_count` — number of data rows in the private CSV file, excluding header when applicable.
- `file_size_bytes` — exact private file size in bytes.
- `created_utc` — UTC timestamp when the ledger row was created or published.
- `publication_cutoff_utc` — UTC cutoff time for daily target files when applicable.
- `timeliness_status` — publication status.
- `notes` — human-readable notes.

Recommended `timeliness_status` values:

- `on_time` — valid file published before the cutoff.
- `late_ignored` — file exists but was published too late to be valid for the next open rebalance.
- `missing_no_change` — no valid file was published before the cutoff, so the previous target weights are carried forward.
- `not_applicable` — cutoff logic is not applicable, for example for static universe files or historical audit package files.

## Timestamp convention

GitHub API timestamps are UTC ISO 8601 timestamps.

The public ledger uses UTC timestamps in the `created_utc` column.

Example UTC timestamp:

`2026-04-24T12:05:00Z`

GitHub UI may display relative or local browser-rendered times. The audit system does not rely on GitHub UI display time as the primary timestamp.

The primary audit timestamp is the explicit UTC timestamp recorded in `ledger.csv`, supported by the public commit and release history.

## US market open and cutoff rule

Regular US equity market open is 9:30 a.m. America/New_York time.

This is normally:

- 14:30 UTC during US Eastern Standard Time;
- 13:30 UTC during US Eastern Daylight Time.

For a daily target file to be valid for the next US market open, its SHA256 must be published in the public ledger at least 1 hour before the regular US equity market open.

Therefore, the usual publication cutoff is normally:

- 13:30 UTC during US Eastern Standard Time;
- 12:30 UTC during US Eastern Daylight Time.

The exact cutoff for each daily target file should be recorded in:

`publication_cutoff_utc`

## Missing or late file rule

If a daily target file is missing, or if its public ledger entry is published less than 1 hour before the next regular US equity market open, the next-day portfolio is not changed.

In that case, the previously effective target weights are carried forward.

A late file may be recorded in the ledger with:

`timeliness_status = late_ignored`

but it is not valid for the next open rebalance.

If no valid file exists, a `daily_no_change_marker` row may be recorded with:

`timeliness_status = missing_no_change`

## Private daily target file format

Daily private target files are stored in the private repository under:

`signals/YYYY/YYYY-MM-DD.targets.csv`

The `YYYY-MM-DD` in the file name is the signal calculation date.

The private daily target file schema is:

`date,ticker,candles_w,optimizer_w,swing_long_w,short_w,long_w,short_abs_w,net_w`

Column meanings:

- `date` — signal calculation date. Signals are computed after the close of this date.
- `ticker` — ticker symbol.
- `candles_w` — target weight contribution from the candles sleeve.
- `optimizer_w` — target weight contribution from the optimizer sleeve.
- `swing_long_w` — target weight contribution from the swing_long sleeve.
- `short_w` — target weight contribution from the swing_long_short short sleeve.
- `long_w` — `candles_w + optimizer_w + swing_long_w`.
- `short_abs_w` — `abs(short_w)`.
- `net_w` — `long_w + short_w`.

The daily target file stores target portfolio state, not merely weight changes.

The daily target file does not contain an explicit planned rebalance date.

The daily target file does not contain a `row_type` column.

## Daily signal and execution timing

Signals are computed after the close of `date` using only information available up to and including that close.

The resulting target portfolio is implemented at the open of the next US trading day after `date`.

The target weights then apply over:

`open(next US trading day after date) -> open(following US trading day)`

Example:

If the private file is:

`signals/2026/2026-04-24.targets.csv`

then `2026-04-24` is the signal calculation date.

The target portfolio is intended for implementation at the open of the next US trading day after 2026-04-24, provided that the public ledger entry was published before the cutoff.

## Daily zero-row policy

Private daily target files may contain fully zero rows.

A fully zero row is valid when a ticker had exposure in the previously effective portfolio but must be fully closed at the next rebalance.

Because the daily target file has no `row_type` column, close rows are inferred as follows:

- all sleeve weights and `net_w` are zero in the current file;
- the ticker had non-zero exposure in the previously effective target portfolio.

Rows should be included when:

`current exposure is non-zero OR previous exposure was non-zero`

Rows should not be included when both current exposure and previous exposure are zero.

This rule is required so that turnover and trade costs can be independently reconstructed from daily changes in target weights.

## Universe file

The strategy universe is stored privately as:

`universe/andromeda_universe_351.csv`

The private universe file schema is:

`ticker`

Rules:

- one ticker per row;
- sorted ascending by ticker;
- UTF-8 encoding;
- LF newlines;
- no duplicate tickers.

Every ticker appearing in a private daily target file should be present in the private universe file.

This public repository stores only the SHA256 hash of the universe file.

Example public ledger row type:

`artifact_type = universe`

The public ledger should reference the private path:

`universe/andromeda_universe_351.csv`

## Price adjustment policy

Portfolio returns are calculated using corporate-action-adjusted prices.

Open-to-open returns should be calculated from adjusted open prices derived consistently with the adjusted close series.

The intended basis is an adjusted-close-consistent price basis, so that splits and dividends are handled consistently.

The public repository does not store private adjusted price files.

If private adjusted return or price-derived audit files are used, their SHA256 hashes may be published in the public ledger.

## Monthly private result files

Monthly private result files are stored in the private repository under:

`monthly/YYYY/YYYY-MM.monthly_results.csv`

The SHA256 of each monthly private result file should be published in `ledger.csv`.

Recommended artifact type:

`monthly_results`

Monthly public audit snapshots may be created after month-end.

## Public snapshots

Public snapshots are intended to provide stable month-end or package-level views of the public audit ledger.

A public snapshot may contain:

- `ledger_snapshot.csv`;
- `audit_manifest.json`;
- `checksums_public.txt`.

Example path:

`snapshots/2026-04/`

or:

`snapshots/audit_package_v1/`

Public snapshots should not contain private files.

They should contain only public metadata, hashes, and audit instructions.

## GitHub releases

Monthly or package-level public snapshots may be published as GitHub Releases.

Recommended release naming:

`audit-YYYY-MM`

or:

`audit-package-v1`

If immutable releases are enabled for this repository, published release assets and associated tags are intended to provide a stronger public snapshot trail.

## Verification method

For each private file:

1. The auditor receives the private file.
2. The auditor computes SHA256 of the private file bytes.
3. The auditor opens this public repository.
4. The auditor finds the corresponding row in `ledger.csv` or a public audit snapshot.
5. The auditor compares the computed SHA256 with the published SHA256.
6. If the hashes match, the private file is exactly the same file that was previously committed to the public audit ledger.

## Example daily ledger row

Header:

`published_date,strategy_id,package_id,artifact_type,file_date,period,private_file_name,sha256,row_count,file_size_bytes,created_utc,publication_cutoff_utc,timeliness_status,notes`

Example:

`2026-04-24,synthetic_candles_optimizer_swinglong_plus_swingls_short_borrow300,operational_daily_v1,daily_target_portfolio,2026-04-24,daily,signals/2026/2026-04-24.targets.csv,<sha256>,20,1366,2026-04-24T12:05:00Z,2026-04-27T12:30:00Z,on_time,target portfolio generated after close of file_date and effective at next US trading day open`

## Example universe ledger row

`2026-04-24,synthetic_candles_optimizer_swinglong_plus_swingls_short_borrow300,operational_universe_v1,universe,2026-04-24,static,universe/andromeda_universe_351.csv,<sha256>,351,<file_size_bytes>,2026-04-24T12:00:00Z,,not_applicable,private universe file hash`

## Example late file ledger row

`2026-04-24,synthetic_candles_optimizer_swinglong_plus_swingls_short_borrow300,operational_daily_v1,daily_target_portfolio,2026-04-24,daily,signals/2026/2026-04-24.targets.csv,<sha256>,20,1366,2026-04-27T13:10:00Z,2026-04-27T12:30:00Z,late_ignored,late publication; target file is stored but not valid for next open rebalance`

## Example no-change marker row

`2026-04-24,synthetic_candles_optimizer_swinglong_plus_swingls_short_borrow300,operational_daily_v1,daily_no_change_marker,2026-04-24,daily,,NA,0,0,2026-04-27T12:30:00Z,2026-04-27T12:30:00Z,missing_no_change,no valid daily target file was published before cutoff; previous target weights carried forward`

## Correction policy

Past public ledger entries should not be silently edited.

If a private file is later found to be wrong, the original private file should remain in place.

A correction file should be created in the private repository.

The correction should be recorded in the public ledger with a clear note explaining:

- the original file being corrected;
- the correction file path;
- the correction issue date;
- the reason for correction;
- whether the correction affects audit performance calculations.

A correction file must not pretend to have been available at the original publication time.

## Public ledger discipline

The public ledger is append-oriented.

Operationally, new rows should be appended.

If a previously published row is wrong, the preferred approach is to add a correction row rather than silently editing historical records.

Monthly or package public snapshots should preserve the public ledger state at the snapshot time.

## What this repository proves

This repository does not prove that the strategy is profitable.

This repository does not prove that all research steps were optimal or unbiased.

This repository proves that specific private files later shown to an auditor match SHA256 hashes that were publicly committed earlier.

The core audit question is:

Did the private files shown later match the files whose hashes were published earlier?

## What this repository does not contain

This repository does not contain:

- private trading signals;
- private daily target portfolio files;
- positions;
- weights;
- returns;
- raw market data;
- adjusted price files;
- model scores;
- training features;
- model artifacts;
- candidate grids;
- debug outputs.

Only hashes and public audit metadata are stored here.
