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
