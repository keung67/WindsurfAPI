## v2.0.116 - Native bridge smoke diagnostics

This release does not widen native bridge by default. It improves the real smoke loop so failed Read/Grep/Glob runs explain why they failed instead of only reporting "no tool_calls".

### Native bridge smoke

- `scripts/native-bridge-smoke.mjs` now includes authenticated `GET /health?verbose=1` snapshots before and after a run by default.
- Failed stream and non-stream scenarios now include compact diagnostics: finish reason, response text preview, usage frame, parsed tool-call names, and raw response preview.
- Health snapshots summarize native bridge counters and the LSP pool/memory guard, so matrix runs can show whether a failure was model behavior, bridge mapping, account gate, pool pressure, or memory admission.
- `NATIVE_BRIDGE_SMOKE_HEALTH=0` disables health snapshots for minimal external smoke runs.

### Verification

- `node --test test/native-bridge-smoke.test.js test/native-bridge-stats.test.js test/native-tool-routing.test.js` -> 18/18 passing.
- `node --test test/*.test.js` -> 999/999 passing.
