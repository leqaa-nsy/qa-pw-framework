Any signed-in customer can read any other customer's order by its number via GET /api/orders/:orderId (OWASP A01 Broken Access Control)

Severity: Critical
Priority: P1
Found by: explore/order-privacy.spec.ts ("a customer cannot open another customer's order") · Environment: Local shop app, Chromium, Windows 11 Pro

Steps to reproduce
1. Run `npm run explore -- order-privacy` (or authenticate as Customer A via `shopApi`, add `PRODUCTS.hammer.id` to cart, and place an order to get `order.orderId`).
2. Authenticate as a separate user (`otherCustomer`) and send `GET /api/orders/:orderId` requesting Customer A's order ID.

Expected
The server rejects the unauthorized request and returns HTTP 404 Not Found (or 403 Forbidden), preventing Customer B from viewing Customer A's order.

Actual
HTTP 200 OK is returned with Customer A's order details instead of the expected 404 status, causing `expect(response.status()).toBe(404)` to fail (`1 failed`).

Evidence
- Test command output: `1 failed [chromium] > explore\order-privacy.spec.ts:26:5 > a customer cannot open another customer's order`
- Trace and error context generated under `test-results/order-privacy-a-customer-c-61390-en-another-customer-s-order-chromium/` (`trace.zip`, `error-context.md`)

Impact
Any signed-in customer can access another customer's order details (including personal data and purchase history), and because order IDs increment sequentially, an attacker can enumerate orders across customers at scale.
```[cite: 4, 5, 6, 7]

### ملحوظات سريعة وأنت بتحطه في الملف:
* **Severity / Priority:** حطينا `Critical` لأن تسريب بيانات طلبات العملاء واختراق صلاحيات الوصول (OWASP A01) أثره خطير جدًا، و`P1` كأولوية مقترحة للإصلاح الفوري[cite: 6, 7].
* **Environment:** عدّل رقم البورت أو الـ URL لو كان ظاهر في ملف الـ env أو الـ trace عندك (زي `localhost:4175` لو ده البورت اللي شغال عليه المتجر)[cite: 6].
* بعد ما تحفظ `docs/BUG-REPORT-TEMPLATE.md`، افتح `docs/STATUS-UPDATE-TEMPLATE.md` وحط فيه الـ 5 سطور بتوع **Exercise 7**[cite: 6, 7]. تحب نراجع شكل `STATUS-UPDATE-TEMPLATE.md` مع بعض دلوقتي ولا شغال فيه؟
