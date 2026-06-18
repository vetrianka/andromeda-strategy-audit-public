# Public Audit Policy

This repository stores public SHA256 commitments for private strategy audit files.

It must not contain private strategy signals, target weights, model scores, private returns, raw market data, or internal research artifacts.

## Scope boundary

The active audit scope starts with the migration commit that creates `scope.md`.

Legacy non-causal material is archived under:

`legacy/old/`

Archived legacy material is retained for transparency but excluded from active causal audit claims.

## Ledger rule

The active ledger is `ledger.csv`.

The publisher computes and records:

- SHA256;
- file size;
- row count when applicable;
- publication timestamp;
- timeliness status;
- private commit id;
- public commit id when available;
- publisher run id.

Owner-supplied manifest fields such as `comment`, `notes`, `source_script`, and `methodology_version` are copied into the ledger. The publisher must not invent the business meaning of a file.

## Correction rule

Never edit a published ledger row to make a different file appear original. Publish a correction row with a new artifact path, `correction_of`, and `supersedes_sha256`.

## Daily target cutoff rule

When a strategy defines a next-open execution rule, its daily target hash must be published before the strategy-specific cutoff. Late files may be recorded with `timeliness_status = late_ignored`, but they are not valid for that rebalance.
