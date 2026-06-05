## v2.0.123 - Native bridge gate hardening and upstream deadline diagnostics

- Native bridge default production canary scope is now limited to
  `Bash` / `shell_command` / `run_command`. `Read`, `Grep`, and `Glob` remain
  available for protocol matrix work, but require an explicit
  `WINDSURFAPI_NATIVE_TOOL_BRIDGE_TOOLS=...` allowlist before they route through
  native bridge.
- Documented the distinction between protobuf encode/decode coverage and
  production readiness in `docs/native-bridge-protocol-notes.md`.
- `context deadline exceeded` / `Client.Timeout or context cancellation while
  reading body` is now classified as `upstream_deadline_exceeded` with code
  `windsurf_provider_deadline`, instead of being folded into generic transient
  upstream errors.
- Stream and non-stream paths both keep invalidating half-finished cascade reuse
  entries after provider deadline failures.

Verification:

- `node --test test\*.test.js` passes: 1014/1014.
