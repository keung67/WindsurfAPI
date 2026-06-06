# v2.0.140 - truthful upstream cooldowns

## What changed

- IP-level rate-limit burst short-circuiting now carries the real upstream cooldown instead of always telling clients to wait 30 seconds.
- When Windsurf returns messages like `Resets in: 27m12s`, the 429 response now uses the same value in `Retry-After`, `error.retry_after_ms`, and the user-facing message.
- Non-stream chat handling now supports the same dependency injection hooks as the stream path, so rate-limit behavior can be covered by behavior tests without starting a real language server.
- Non-stream Cascade reuse invalidation now also recognizes structured `upstream_deadline_exceeded` / `windsurf_provider_deadline` responses, not only the raw upstream error text.
- README FAQ now separates local RPM limits, upstream free-tier throttling, IP cooldowns, and the upstream ~240s provider deadline.

## Context

Issues #176 and #189 showed real upstream cooldowns around 26-30 minutes, but the IP-burst guard surfaced a fixed 30-second retry hint. The guard was already doing the right thing by stopping account burn; this release makes the operator/client-facing cooldown truthful.

This does not bypass Windsurf upstream rate limits. It prevents misleading retry timing and reduces repeated hammering during an upstream IP cooldown.

## Validation

- `node --test test/rate-limit.test.js`
- `npm.cmd run test:release`
- `node --test test/stream-error.test.js test/cascade-timeout-invalidation.test.js test/stream-pool-exhausted-error.test.js`
- `npm.cmd run test:shard -- 0 4 --timeout-ms=90000`
- `npm.cmd run test:shard -- 1 4 --timeout-ms=90000`
- `npm.cmd run test:shard -- 2 4 --timeout-ms=90000`
- `npm.cmd run test:shard -- 3 4 --timeout-ms=90000`
