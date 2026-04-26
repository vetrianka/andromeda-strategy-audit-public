# Audit Policy

This repository is used as a public hash ledger for private strategy audit files.

The repository is configured with:

- protected `main` branch;
- pull requests required before merging;
- linear history required;
- force pushes disabled;
- branch deletion disabled;
- bypass disabled for administrators;
- signed commits required where available;
- monthly immutable releases.

Private strategy files are not stored in this repository.

For each private file, SHA256 is published in `ledger.csv`.

Monthly releases named `audit-YYYY-MM` contain immutable snapshots of the public ledger.
