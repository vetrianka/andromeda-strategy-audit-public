# Audit Scope

This repository was reorganized on 2026-06-18 to support a strategy-neutral public audit ledger.

## Active scope

The active public scope is the ledger-based publication process defined by:

`multi_strategy_audit_publication_v1`

New active artifacts are proven by rows in:

`ledger.csv`

Each active ledger row should point to a private path under:

`artifacts/{strategy_id}/{period}/...`

## Strategy ids

Current active strategy id at this migration boundary:

- `orion` — Orion

The publication protocol is not named after this strategy. Additional strategies can be added later.

## Legacy material

Deprecated pre-migration material is stored under:

`legacy/old/`

That material is retained for transparency only and is excluded from active causal publication claims.

## Repository role

Repository role: public hash ledger and public audit metadata.
