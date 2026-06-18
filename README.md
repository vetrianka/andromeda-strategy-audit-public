# Public Audit Ledger Repository

This repository stores public integrity records for one or more private strategy publication streams.

It does not contain private target files, model scores, strategy internals, private returns, or raw market data. It contains public hashes and metadata that allow later verification of private artifacts.

The publication protocol is strategy-neutral:

`multi_strategy_audit_publication_v1`

The current active strategy id is:

- `orion` — Orion

## Main public files

- `ledger.csv` — public hash ledger for active private artifacts.
- `strategy_registry.csv` — registry of active and deprecated strategy ids.
- `scope.md` — audit boundary and active-scope rules.
- `AUDIT_PUBLIC_POLICY.md` — public ledger and correction rules.
- `PUBLICATION_PROTOCOL.md` — producer/publisher workflow.
- `DIRECTORY_INDEX.md` — human-readable directory and file map.
- `FILE_CATALOG.csv` — machine-readable catalog of current repository paths.
- `LEDGER_COMMENT_GUIDE.md` — rules and examples for meaningful artifact comments.

## Active ledger coverage

The active ledger is intended to cover publishable files under the private repository path:

`artifacts/{strategy_id}/{period}/...`

The active ledger is not intended to cover repository documentation, Git placeholder files, Git configuration files, or `legacy/old/`.

## Legacy material

Deprecated pre-migration material is kept under:

`legacy/old/`

It is retained for transparency only and is outside the active audit scope.
