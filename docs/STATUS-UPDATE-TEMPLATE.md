# Test status — Toolshop, 26 Nov 2026

Five lines a busy person can read on a phone. The verdict first; detail only if they ask.

**Verdict:** Not ready — any authenticated customer can read another customer's order via GET /api/orders/:orderId (Broken Access Control / IDOR).

**What ran:** Playwright suite across chromium, firefox, webkit, and mobile (Pixel 7) in the nightly run, perf/load.mjs search load tests (1, 5, 10, 25, and 50 users), and order-privacy exploration test on chromium (1 failed, 0 flaky).

**New problems:**
- Broken Access Control on GET /api/orders/:orderId returns 200 instead of 404 with sequential order IDs, exposing other customers' order details (Critical)
- Search p95 crosses the 200 ms budget around 5 users (p95 = 223 ms) while throughput levels off around 34–35 req/s, alongside mobile layout and accessibility issues on the 412px screen (High)

**Not tested:** Real physical mobile devices and deeper security/payment edge cases, leaving real-device behavior and unverified edge flows unconfirmed.

**Decision needed:** Approve holding the release until the order access-control bug is fixed (and align on mobile/performance fixes), from the Tech Lead / Product Owner before release sign-off.
