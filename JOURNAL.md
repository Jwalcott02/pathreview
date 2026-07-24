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

**Reproduction commit link:** [link to commit documenting the reproduced issue]

**Reproduction summary:**
[Reproduced by temporarily reverting to the pre-fix code and calling curl http://localhost:8000/health, which triggered redis_health_check_failed error=\"'Settings' object has no attribute 'redis_host'\" in the server logs confirming the AttributeError occurs exactly as described in the issue.]

**PLAN.md link:** [link to PLAN.md in your fork]

**Walkthrough video (recommended):** [link to your Loom video, ≤2 min — recommended, not graded]

**Blockers or open questions:**
[Anything you're still uncertain about going into Week 9, or leave blank]