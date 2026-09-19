# Frantic #49 — runx support action report

## What was posted

A public demo repository: https://github.com/halc8312/runx-quickstart-demo

It contains a full, genuinely executed walkthrough of the runx quickstart plus
the real sealed receipt (`sealed-receipt.json`) produced by the run.

## Why it is authentic support

- Every command in the README was actually run on Ubuntu 24.04 with
  `@runxhq/cli` 0.9.1; outputs are copied verbatim, including the real
  `receipt_id` and the committed `runx.receipt.v1` JSON.
- It documents behavior a newcomer would otherwise have to discover:
  `RUNX_RECEIPT_DIR` consistency across skill/history/verify, the
  `unverified` status of unsigned local receipts, and the exact flag
  combination `runx verify` needs for local receipts
  (`--allow-local-development-signatures`).
- It links runx.ai, github.com/runxhq/runx, and the npm package page.

## What it is not

Not link spam: no automated posting, no reciprocal promotion, no venue where
sharing is disallowed. It is a self-hosted demo artifact — an allowed action
type — whose value is reproducible instructions and a real receipt artifact.
