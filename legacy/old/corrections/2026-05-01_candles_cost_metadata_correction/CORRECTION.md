# Public correction notice — candles sleeve metadata

Date: 2026-05-01

Created UTC: 2026-05-01T09:45:17Z

Strategy id:

`synthetic_candles_optimizer_swinglong_plus_swingls_short_borrow300`

Audit package:

`audit_package_v1`

Private metadata path:

`strategies/synthetic_candles_optimizer_swinglong_plus_swingls_short_borrow300/audit_package_v1/strategy_metadata.json`

## Correction

A metadata error was identified in the previously published `strategy_metadata.json`.

The previous metadata incorrectly stated that the transaction cost assumption for the `candles` sleeve was 0 bps one-way.

The correct transaction cost assumption is 10 bps one-way.

The corrected private metadata file also updates the corresponding OOS summary parameters.

## Corrected base OOS summary

- CAGR: 42.25%
- Max drawdown: -18.40%
- Sharpe: 1.964
- Calmar: 2.297
- Profit factor: 1.420
- Win rate: 55.53%
- Mean return: 14.70 bps/day
- Final NAV: 137.22

## Verification

The corrected private metadata file SHA256 is:

`617e81b65f7627d5bf81e5028c7ec7e8268b4c0f0481d76d8e2224b0863db9e3`

The public `sha256_manifest.csv` in this directory contains hashes for the private correction package files.

This correction does not alter any previously published daily target position files. It only corrects descriptive strategy metadata and performance-summary metadata.

The original private metadata file remains preserved. The corrected private metadata file, diff, correction payload, and correction notice have public SHA256 hashes so that an auditor can verify the correction package.
