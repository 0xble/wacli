# Maintenance

## Background

Maintained fork: `0xble/wacli` of `openclaw/wacli`; maintained and upstream-default
branch `main`. Canonical checkout: `/Users/brianle/Repos/tools/checkouts/wacli`.
Accepted upstream baseline: `27dae09461a59c60e98c0f4447e0d5f0195c6410` (fetched
2026-09-09). Publish only to `origin`; never push to `upstream`.

## Preserve

- Revoked WhatsApp sessions fail honestly instead of presenting stale authenticated
  state; read-only status and doctor output agree with that state.
- Source sync, publication, installation, and runtime activation are distinct;
  only the first two belong to routine maintenance.

## Active patches

### WACLI-001: `fix(sync): fail honestly when session is revoked`

- **Status:** Active; source difference confirmed against `upstream/main` on 2026-09-09.
- **Provenance:** `dd8b4b16a1a9d9e52bd77618fe21e5dce4f43286` (fork merge
  `0e64908ab50568425bde0d819572e0d2b098c696`).
- **Surfaces/invariant:** `internal/app/{session_state.go,sync*.go}`, `cmd/wacli/
  {auth.go,auth_status_readonly.go,doctor.go}`, and their tests keep revocation
  detection, sync failure, doctor, and read-only status consistent.
- **Proof:** `pnpm test -- --run TestSessionState` is insufficient; run the
  repository gate `pnpm format:check && pnpm lint && pnpm test && pnpm build`.
- **Rollback:** revert `dd8b4b16` and its regression tests together after proving
  upstream-equivalent revoked-session behavior.
- **Upstream PR:** direct `https://github.com/openclaw/wacli/pull/389`, OPEN,
  `UNSTABLE`, head includes `dd8b4b16`, checked 2026-09-09; live-check reviews,
  threads, CI, merge/revert, and release before every change. **Issue:** none recorded.
- **Retire when:** released upstream implements the complete invariant and the
  repository gate passes with no fork delta.

## Update

Every run fetches `origin` and `upstream`, reconciles `main` onto latest
`upstream/main`, preserves only active recorded patches, live-checks the typed PR
record, and runs the repository gate before authorized publication. Update this
contract with any patch addition/change/retirement; missing or stale coverage
blocks publication. Immediately before `Updated` or `Already current`, fetch
upstream again and require zero upstream-only commits; otherwise report `Blocked`
with failed stage, exact refs, and evidence. Publish to `origin` or report that
concrete blocker.

## Verify

```text
git diff --check
git rev-list --left-right --count upstream/main...main
```

Require a fresh final fetch with zero upstream-only commits and, after authorized
publication, local `main` SHA equal to `origin/main`. Installed/runtime SHA proof
is required only for separately authorized later stages.
