# Audit Scope

This repository was reorganized on 2026-06-18 to support auditable strategy publications.

## Active scope start

The active audited publication scope starts with the Git commit that adds this `scope.md` file and the strategy-neutral multi-strategy layout.

This wording is intentional: the commit containing this file is the boundary. No pre-migration file is silently promoted into the active causal audit scope.

## Deprecated legacy material

Files related to the previous non-causal single-strategy layout were moved to:

`legacy/old/`

Deprecated legacy material is retained only for historical transparency. It must not be used for active causal performance claims, production audit evidence, or new strategy publications.

## Active publication rule

Only artifacts referenced by new active ledger rows created after this migration boundary are part of the current audit publication process.

## Strategy registry

Active and deprecated strategy identifiers are listed in `strategy_registry.csv`.

A strategy identifier is not the name of the repository, not the name of the publication protocol, and not the name of the ledger schema.

## Deprecated strategy id

`synthetic_candles_optimizer_swinglong_plus_swingls_short_borrow300`

## Repository role

Repository role: public
