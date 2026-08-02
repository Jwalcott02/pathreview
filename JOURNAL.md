## Week 7 — Issue selection

**Issue link:** [https://github.com/ascherj/pathreview/issues/155]

**Issue title:** [Health check references settings.redis_host, which does not exist on Settings]

**Tier:** [X] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
[The core issue for this bug is that the `redis_host` field is missing from `Settings` in config.py, which raises an AttributeError when it's referenced within api/routes/health.py. While investigating, I also found that `settings.redis_port` is referenced in the same block and is also missing from `Settings`  meaning fixing only `redis_host` wouldn't fully resolve the crash. Additionally, the broad `except Exception` in the health check silently catches this error, so instead of visibly failing, the endpoint just reports `redis: unhealthy`  a false negative regardless of Redis's actual status. A good fix could look like using `redis.Redis.from_url(settings.redis_url)` instead of relying on separate host/port fields.]




**Branch name:** [fix/155-redis-host-attributeerror]

**Setup confirmation:** [X] App runs locally at localhost:5173

**Cohort ledger:** [X] Issue added to cohort ledger





## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [https://github.com/Jwalcott02/pathreview/commit/ba91c39]

**Reproduction summary:**
[Reproduced by temporarily reverting to the pre-fix code and calling curl http://localhost:8000/health, which triggered redis_health_check_failed error=\"'Settings' object has no attribute 'redis_host'\" in the server logs confirming the AttributeError occurs exactly as described in the issue.]

**PLAN.md link:** [link to PLAN.md in your fork]

**Walkthrough video (recommended):** [link to your Loom video, ≤2 min — recommended, not graded]

**Blockers or open questions:**
[Anything you're still uncertain about going into Week 9, or leave blank]




## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:** Implemented the fix (redis.Redis.from_url() replacing the broken host/port kwargs), added the first test coverage for /health (2 passing tests), and established a documented baseline of pre-existing lint/type/test failures unrelated to this issue.

**Next steps:** Open the PR, write the full PR description, and complete Check-in 2.

**Blockers:** None — ready to open the PR.
---

#### Check-in 2 (end of week)

**PR link:** [https://github.com/ascherj/pathreview/pull/590]
**Branch:** fix/155-redis-host-attributeerror
**What you built:** Fixed the /health Redis check by using redis.Redis.from_url(settings.redis_url) instead of nonexistent redis_host/redis_port fields; added first-ever test coverage for the endpoint (2 passing tests covering healthy and unhealthy Redis states).
**Tests added or updated:** tests/unit/test_health.py — new file, 2 tests.
**Self-review confirmation:** [x] make check passes (no new failures introduced) [x] make test-unit passes (no new failures introduced)
**Draft PR feedback received from:** none (submitted directly due to time constraints)