Any signed-in customer can read any other customer's order by its number via GET /api/orders/:orderId (OWASP A01 Broken Access Control)

Severity: Critical · Priority: P1 · Found by: explore/order-privacy.spec.ts ("a customer cannot open another customer's order") · Environment: Local shop app, Chromium, Windows 11 Pro

Steps to reproduce
1. Run `npm run explore -- order-privacy` (or sign in as Customer A via `shopApi`, add `PRODUCTS.hammer.id` to the cart, place an order, and note `order.orderId`).
2. Sign in as another customer (`otherCustomer`) and request Customer A's order via `GET /api/orders/:orderId` using `order.orderId` (or an adjacent sequential order number).

Expected
The server rejects the request and returns HTTP 404 Not Found (or 403 Forbidden) so Customer B cannot access Customer A's order.

Actual
HTTP 200 OK is returned along with Customer A's order details instead of 404, causing `expect(response.status()).toBe(404)` to fail (`1 failed`).

Evidence
- Command result: `1 failed [chromium] > explore\order-privacy.spec.ts:26:5 > a customer cannot open another customer's order`
- Artifacts generated in `test-results/order-privacy-a-customer-c-61390-en-another-customer-s-order-chromium/` (`trace.zip`, `error-context.md`)

Impact
Any signed-in customer can view another customer's order details (including customer name, email, and purchased items), and because order IDs are sequential, any authenticated user can enumerate orders at scale.
