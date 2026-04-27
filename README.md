# Andromeda Strategy Audit Public Ledger

This repository does not contain private trading signals, positions, scores, weights, returns, or strategy data.

It contains only public cryptographic hashes of private audit files.

## Purpose

The purpose of this repository is to provide a public, timestamped audit trail for private strategy files.

For each private file, the SHA256 hash is published in `ledger.csv`.

An auditor can later receive the private file, recompute its SHA256 hash, and compare it with the hash previously published in this repository.

If the hash matches, the private file is exactly the same file that was committed to this public ledger.

## Public files

- `ledger.csv` — append-only public ledger of private file hashes.
- `snapshots/` — monthly audit snapshots.

## Private files

Private files are not stored in this repository.

Private files include:

- daily signal CSV files;
- monthly result CSV files;
- any supporting private audit files.

## Verification method

For each private file:

1. Receive the private file from the strategy owner.
2. Compute SHA256 of the file bytes.
3. Find the corresponding row in `ledger.csv`.
4. Compare the computed SHA256 with the published SHA256.
5. If they match, the file is unchanged.

## Ledger columns

`date` — business date or report date.  
`artifact_type` — `daily_signals` or `monthly_results`.  
`private_file_name` — expected private file path.  
`sha256` — SHA256 hash of the private file.  
`row_count` — number of rows in the private CSV file.  
`file_size_bytes` — file size in bytes.  
`created_utc` — UTC time when the ledger row was created.  
`notes` — optional notes.

## Policy

Past ledger entries should not be silently edited.

If an error is found, a new correction entry should be added with an explanation in the `notes` column.


## Public ledger timing

This public repository stores SHA256 hashes of private audit files.

GitHub API timestamps are UTC ISO 8601 timestamps. The public ledger uses UTC timestamps in the `created_utc` column.

Regular NYSE core trading opens at 9:30 a.m. Eastern Time.

This is normally:

- 14:30 UTC during US Eastern Standard Time;
- 13:30 UTC during US Eastern Daylight Time.

For a daily target file to be valid for the next US market open, the public ledger entry containing its SHA256 must be published at least 1 hour before the regular US equity market open.

Therefore, the usual cutoff is normally:

- 13:30 UTC during US Eastern Standard Time;
- 12:30 UTC during US Eastern Daylight Time.

If a daily target file is missing, or if its public ledger entry is published less than 1 hour before the next regular US equity market open, the next-day portfolio is not changed. The previously effective target weights are carried forward.

## Daily private target file format

Daily private target files are stored in the private repository under:

`signals/YYYY/YYYY-MM-DD.targets.csv`

The daily target file schema is:

`date,ticker,candles_w,optimizer_w,swing_long_w,short_w,long_w,short_abs_w,net_w`

The `date` column is the signal calculation date.

The daily file does not contain an explicit planned rebalance date.

The target portfolio is implemented at the open of the next US trading day after `date`.

## Universe file

The private repository should contain:

`universe/andromeda_universe_351.csv`

This file contains the strategy universe, one ticker per row.

The public repository stores only the SHA256 hash of that private universe file.

## Price adjustment policy

Portfolio returns are calculated using corporate-action-adjusted prices.

Open-to-open returns should be calculated from adjusted open prices derived consistently with the adjusted close series.

The public ledger may contain hashes of private return or price-derived audit files, but not the private data itself.
