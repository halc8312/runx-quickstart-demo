# runx quickstart, run for real

A complete walkthrough of the [runx](https://github.com/runxhq/runx) local
quickstart on Ubuntu 24.04, Node 20, `@runxhq/cli` 0.9.1. Everything below was
executed on 2026-09-19; the sealed receipt produced by the run is committed to
this repository as [sealed-receipt.json](./sealed-receipt.json).

[runx](https://runx.ai) is a governed runtime for agent skills: a skill is a
portable `SKILL.md`, every run is admitted under explicit authority, and the
runtime seals each result into a verifiable receipt. The local CLI executes
skills on your machine with no account and no hosted service required.

## Install

```bash
npm i -g @runxhq/cli
runx --version
# runx-cli 0.9.1
```

## Run the bundled example

```bash
git clone --depth 1 https://github.com/runxhq/runx.git
cd runx
runx skill ./examples/hello-world -i message="hello, runx" --json
```

The CLI prints a preparation summary, then a `runx.skill_run.v1` result:

```json
{
  "status": "sealed",
  "receipt_id": "sha256:e6b05ab4eebc97ad5ba36abe64ccca8c2141e3170f7099847fd2a89ba9435b47",
  "result": { "message": "hello, runx" },
  "schema": "runx.skill_run.v1",
  "skill_name": "hello-world",
  "outcome": "completed"
}
```

The whole run takes well under a second on a modest VM.

## Keep receipts somewhere known

`runx` stores receipts in `.runx/receipts` under the working directory unless
you point it elsewhere. `RUNX_RECEIPT_DIR` is honored consistently by
`runx skill`, `runx history`, and `runx verify`:

```bash
export RUNX_RECEIPT_DIR="$(mktemp -d)"
runx skill ./examples/hello-world -i message="dir test" --json
ls "$RUNX_RECEIPT_DIR"
# index.json  sha256-e6b05ab4....json
```

## Inspect and verify the receipt

```bash
runx history <receipt-id> --detail --json
```

returns a `runx.receipt_inspection.v1` projection with the receipt's authority,
decisions, acts, and seal summary. A receipt sealed without a configured
production signer reports `verification.status: "unverified"` — it is sealed but
self-attested.

Cryptographic verification needs either the signing identity or the public
verification key. For local fixture receipts, `runx verify` accepts an explicit
override:

```bash
RUNX_RECEIPT_DIR=<dir> runx verify <receipt-id> --allow-local-development-signatures
# tree sha256:e6b05ab4... (1 receipt): ok
# verification: ok
```

Without that flag (or without `RUNX_RECEIPT_VERIFY_KID` +
`RUNX_RECEIPT_VERIFY_ED25519_PUBLIC_KEY_BASE64`), verify refuses rather than
silently trusting local signatures — which is the correct security default.

## What this demonstrates

- Skills run locally with zero hosted dependencies; the whole flow is offline.
- Every act is recorded with authority provenance and criteria, not just output.
- Receipts are content-addressed (`sha256:`), canonicalized
  (`runx.receipt.c14n.v1`), and sealed with Ed25519.

## Links

- Runtime: https://github.com/runxhq/runx
- Catalog and spec: https://runx.ai
- CLI package: https://www.npmjs.com/package/@runxhq/cli
