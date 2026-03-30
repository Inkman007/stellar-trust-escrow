against main # Redis Caching for Escrow Query Endpoints - Implementation Plan

## Completed Steps
- [x] Create TODO.md with implementation plan

## Pending Steps

### 1. Project Setup (Branch & Dependencies)
- [x] Create branch `feature/backend-escrow-redis-caching`
- [ ] Verify/install redis package (should exist via cacheService)
  ```bash
  cd backend && npm ls redis
  ```

### 2. Add Redis Service to docker-compose.yml
- [x] Create `docker-compose.override.yml`: Added Redis service + backend REDIS_URL/depends_on
- [ ] `docker compose up -d redis`
- [ ] Verify connection: Check backend logs for '[Cache] Redis connected'

### 3. Define TTL Constants
- [x] Edit `backend/middleware/cache.js`: Added `export const TTL = { LIST: 300 /*5min*/, DETAIL: 900 /*15min*/ };`

### 4. Enhance Cache Invalidation for Status Changes
- [x] Dispute autoResolve already invalidates 'escrows' tag
- [x] Admin resolveDispute: Added `cache.invalidateTags(['escrows', \`escrow:\${escrowId}\`])`
- [x] Payment endpoints (webhook/refund) - no direct escrow status changes

### 5. Add Cache Metrics Endpoint
- [ ] Create `/api/health/cache` route using `cache.analytics()`
- [ ] Log Redis INFO periodically (via cron or health check)

### 6. Update Tests
- [ ] Edit `backend/tests/escrowController.test.js`: Add tests for TTL, tags, invalidation, Redis fallback
- [ ] Run `cd backend && npm test`

### 7. Performance Verification
- [ ] Baseline load test: `node load-tests/run.js`
- [ ] Deploy changes, re-run load test (target 70% latency reduction)
- [ ] Check Redis: `redis-cli INFO keyspace_stats`

### 8. Documentation & Commit
- [ ] Update README/docs/api-reference.md: Cache keys (`escrow:list:{page}`, `escrow:{id}`), TTLs
- [ ] Commit changes with feat/test/chore messages
- [ ] Create PR

**Next Action:** Start with Step 1 after approval.

