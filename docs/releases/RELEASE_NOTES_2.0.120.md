## v2.0.120 - native smoke requires bridge-enabled preflight

Follow-up to v2.0.119 after VPS smoke showed a remaining gap: when native
bridge was default-off, the smoke still sent a normal chat request and grew the
default LS RSS before failing.

- Verbose `/health` now includes `nativeBridgeConfig`, a redacted summary of
  native bridge mode, tool/model/provider/route/caller gates, and whether API
  key/account/raw-config gates are present.
- `scripts/native-bridge-smoke.mjs` now refuses to run by default when
  `nativeBridgeConfig` says the bridge is off or not enabled. Set
  `NATIVE_BRIDGE_SMOKE_REQUIRE_BRIDGE_ENABLED=0` only for explicit diagnostics.
- Added tests proving the smoke skips chat when bridge mode is not enabled, and
  that health config status does not expose API key/account gate values.

Validation:

- `node --test test/*.test.js` passes locally.

