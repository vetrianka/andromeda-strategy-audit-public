# Public Audit Policy

This repository contains public SHA256 commitments for private strategy audit files.

It does not contain private strategy signals, positions, weights, returns, scores, or raw market data.

## Timestamp convention

GitHub/API publication timestamps and the public ledger `created_utc` field use UTC ISO 8601 format.

## US market open and cutoff rule

Regular US equity core trading opens at 9:30 a.m. Eastern Time.

This is normally:

- 14:30 UTC during US Eastern Standard Time;
- 13:30 UTC during US Eastern Daylight Time.

For a daily target file to be valid for the next US market open, its SHA256 must be published in the public ledger at least 1 hour before the regular US equity market open.

The usual cutoff is normally:

- 13:30 UTC during US Eastern Standard Time;
- 12:30 UTC during US Eastern Daylight Time.

## Missing or late file rule

If a daily target file is missing, or if its public ledger entry is published less than 1 hour before the next regular US equity market open, the next-day portfolio is not changed.

The previously effective target weights are carried forward.

A late file may be recorded in the ledger with `timeliness_status = late_ignored`, but it is not valid for the next open rebalance.

If no valid file exists, a `daily_no_change_marker` row may be recorded with `timeliness_status = missing_no_change`.

## Private daily target format

The private daily target file schema is:

`date,ticker,candles_w,optimizer_w,swing_long_w,short_w,long_w,short_abs_w,net_w`

The `date` column is the signal calculation date.

The file does not contain an explicit planned rebalance date.

The target portfolio is implemented at the open of the next US trading day after `date`.

## Universe file

The strategy universe is stored privately as:

`universe/andromeda_universe_351.csv`

The public ledger stores only the SHA256 of this file.

## Price adjustment policy

Portfolio returns are calculated using corporate-action-adjusted prices.

Open-to-open returns should be calculated from adjusted open prices derived consistently with the adjusted close series.
