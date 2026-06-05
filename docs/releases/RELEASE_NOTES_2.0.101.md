## v2.0.101 - LS pool admission hardening

This release tightens LSP scheduling after the v2.0.100 observability pass.

### LSP pool control

- Serialized LS start admission across different cold proxy keys so concurrent
  cold starts cannot all pass capacity checks before any instance is inserted
  into the pool.
- Starting instances now reserve pool capacity before they become ready.
- Evicted LS processes remain reserved until they exit or are killed after a
  bounded grace period, reducing transient RSS spikes from old+new overlap.
- Idle eviction no longer targets `ready=false` instances, and newly-ready
  instances get a short grace period before they are considered evictable.
- Memory guard calculations now subtract already-reserved starts, so parallel
  admission cannot reuse the same free-memory snapshot.

### Probe budget

- Scheduled probes, predictive prewarm, and dashboard probes default to
  resident-only behavior: they only run when the target LS is already running
  and idle.
- Dashboard `probe-all` and per-account `probe` accept `force:true` or
  `allowLsStart:true` when an operator explicitly wants a probe to start LS.

### Low-memory deployments

- Added `LS_PREWARM_DEFAULT=0` to skip startup default-LS prewarm and lazily
  start the first needed LS on demand. This is useful for low-memory or
  all-proxy account pools.
