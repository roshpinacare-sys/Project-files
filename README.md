# Project-files — the canonical archive vault

This repository is a deliberate **archive vault**: canonical zip snapshots of
installers, patches and runtime substrates, stored in git so they are
versioned, hash-manifested and never lost.

## What is here

- ~69 MB of canonical zip archives at the repo root (10 archives as of
  2026-09-30) — installers, patches, runtime substrates and a full snapshot
  of the `steem` repository as of 2026-09-07.
- `docs/reports/` — historical session reports (dated broadcast snapshots).
- `broadcasts/` — dated broadcast archive.
- `agent/` — the archive-keeper bot's own truth report (`status.json`) and
  SHA-256 manifest (`archives.sha256`).

## What runs

One scheduled GitHub Action: **archive-keeper** (weekly, Mondays 11:31 UTC,
see `.github/workflows/archive-keeper.yml`). It opens every zip end-to-end
(`unzip -tq`) to prove it is not corrupt, writes a SHA-256 manifest, and
commits `agent/status.json` with the verdict. A corrupt archive is a red mark;
deletion of history files fails the run.

## Integrity

`agent/archives.sha256` holds the current SHA-256 of every archive; the
verdict and counts live in `agent/status.json` (measured, not hand-written).

## What this repo is not

No services, no secrets, no runtime state. Targeted audit (Task 11-e,
2026-09-30): the tracked zips were scanned for `.env` / key-shaped files —
the only `.env` files found contain a SQLite `DATABASE_URL` and a Vite app
id respectively; no key material detected inside the tracked archives.
