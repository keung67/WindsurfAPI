## v2.0.112 - LSP admission telemetry and native proto trace

This release tightens the low-memory LSP pool path and adds the protocol-level trace data needed to continue the native bridge work without guessing.

### LSP scheduling

- Cold proxy starts now reserve capacity before their placeholder enters the pool, so concurrent first-use proxy bursts cannot over-admit past `LS_MAX_INSTANCES`.
- Pool and account telemetry now reports effective occupancy, pending reservations, evictable idle instances, and memory-guard estimate source.
- The memory guard uses observed live LS RSS plus margin when `LS_SPAWN_MIN_AVAILABLE_BYTES` is not explicitly set, instead of always assuming 700MB after a real sample exists.
- `LS_PREWARM_DEFAULT=0` is now honored by both startup and auth warmup paths.

### Native bridge reverse engineering

- Proto trace now adds semantic summaries for `SendUserCascadeMessage` and `GetCascadeTrajectorySteps`: planner mode, native tool config fields, allowlist strings, additional step count, and native step oneof fields.
- `native-bridge-smoke.mjs` defaults to the same hashed workspace path the LS registers during Cascade warmup, reducing false negatives for Grep/Glob.
- Added `grep_v2` as a native bridge alias for matrix experiments.

### Reliability

- Dashboard logger disk writes are now best-effort and recoverable if a daily JSONL stream hits a transient file error, while in-memory logs and console output continue.

### Verification

- `node --test test/*.test.js` -> 993/993 passing.
