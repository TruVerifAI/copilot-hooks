# Changelog — TruVerifAI gates for GitHub Copilot

## 0.19.30

- **Gate-self WRITE-gate deadlock fully fixed.** The write gate's deny message
  now prescribes the required `@@ -0,0 +1,N @@` hunk header in the diff an agent
  submits to `audit_coding`; without it (0.19.28–0.19.29) the reviewed diff
  parsed to zero content and a PASS could never release the write. Plus a
  server-side guard that refuses releasing coverage for a degenerate gate-self
  diff (content headers but no reviewable lines) while still releasing a
  legitimate rename/mode-only change. Fail-safe unchanged.


## 0.19.29

- **Fixed a false-positive "gate integrity" tamper warning on Windows (no actual tampering occurred).** `run_gate.js` was hashed raw (not line-ending-normalized) in the tamper-evidence manifest, so a clean install whose `run_gate.js` landed with CRLF line endings falsely reported `modified:run_gate.js` on every invocation. It is now normalized like every other gate file, so a clean install verifies clean. Informational only (the gate always enforced and failed open). **Upgrading resolves it automatically** — the new manifest ships with the update and the self-check re-runs on the next gate invocation; no reinstall step needed.


## 0.19.28

Catch-up republish: `copilot-hooks` had not been updated since 0.18.0, so this
release brings the vendored gate code current with the shared cross-platform
gate core (the primary Copilot delivery path, `npx @truverifai/init`, already
shipped these via the npm package; this syncs the manual-install template repo).
Includes everything between 0.18.0 and 0.19.28, notably:

- **Gate-self WRITE-gate deadlock fix** — the write gate now relativizes the
  path against the git root before the self-coverage hash, so a gate-self write
  can be released by a real `audit_coding` PASS (previously unmatchable).
- **Tamper-evidence self-check** — new `integrity.py` + `gate_manifest.json`:
  the gate verifies its own files on each invocation and discloses tampering
  (warn-loud, fail-open).
- **Cross-platform host adapters** refreshed (`host/*.py`), classifier
  evolution (`risk_classifier.py` / `risk_signals.json` v2.x), post-commit
  backstop with the integrity flag, and launcher/hook updates.

## 0.18.0 (first release)
- Write + commit gates for Copilot CLI / VS Code / Cloud Agent via
  `.github/hooks` config, generated from the shared cross-platform gate core.
- Total-containment lifecycle on the CLI surface (fail-closed host): any gate
  error emits an explicit allow — never blocks your edit.
- VS Code agent hooks are Preview upstream; a format change fails open.
