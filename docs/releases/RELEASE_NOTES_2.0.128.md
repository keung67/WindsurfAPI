## v2.0.128 - Container probe script packaging fix

- Fixed the Docker image packaging for the new v2.0.127 operational scripts:
  `scripts/lsp-capacity-matrix.mjs` and
  `scripts/web-search-direct-probe.mjs` are now copied into the container.
  Without this, `npm run smoke:lsp-matrix` and `npm run probe:web-search`
  worked from a git checkout but failed inside the released GHCR image with
  `MODULE_NOT_FOUND`.
- `.dockerignore` now explicitly unignores those scripts, matching the
  repository `.gitignore` allowlist.
- Added a regression test that checks every npm smoke/probe script intended for
  container execution is both unignored and copied by the Dockerfile.

Verification:

- `node --check scripts\lsp-capacity-matrix.mjs`
- `node --check scripts\web-search-direct-probe.mjs`
- `node --test test\docker-script-packaging.test.js`
- `node --test --test-timeout=120000 --test-force-exit test\*.test.js`
