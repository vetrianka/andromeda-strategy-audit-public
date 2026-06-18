# Multi-Strategy Audit Public Ledger

This repository is the public audit ledger for multiple strategy publication streams.

It does not contain private trading signals, positions, model scores, strategy internals, private returns, or raw market data. It contains public cryptographic commitments and audit metadata.

## Active scope

Active audited publication starts with the Git commit that adds `scope.md` and this Orion audit layout.

- scope_start_date: 2026-06-18
- active strategy ids at migration: orion
- deprecated legacy strategy id: synthetic_candles_optimizer_swinglong_plus_swingls_short_borrow300

Legacy non-causal material has been moved to:

`legacy/old/`

That directory is retained only for historical transparency. It must not be used for active causal performance claims or production audit evidence.

## Public audit chain

private file  
→ SHA256  
→ public ledger row  
→ public Git commit / release  
→ later independent verification by recomputing the SHA256 of the private file

## Main files

- `ledger.csv` — active public hash ledger for new multi-strategy publications.
- `strategy_registry.csv` — active and deprecated strategy registry.
- `scope.md` — explicit audit scope and migration boundary.
- `AUDIT_PUBLIC_POLICY.md` — public publication policy.

## Active artifact paths

The private repository is expected to store active private artifacts under:

`artifacts/{strategy_id}/{period}/...`

The public ledger records the private path and SHA256 for each published private artifact.

## Correction policy

Published private paths must not be silently overwritten. If a correction is required, publish a new artifact path and link it through `correction_of` and `supersedes_sha256` in the public ledger.
