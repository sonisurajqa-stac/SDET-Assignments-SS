# PR: Add API automation for Subscription & Billing Service

## Summary
Adds a JavaScript and Playwright API validation suite plus a minimal local service fixture for subscription lifecycle and billing behavior.

## Test strategy
- API contracts: creation, retrieval, cancellation, validation errors and webhook request handling.
- State machine: trialing/active/past_due/canceled transitions, invalid reactivation, retries exhausted and stale out-of-order failure.
- Persistence: subscriptions, invoices, webhook event records and audit history.
- Provider integration seam: deterministic mock for success, decline and timeout; verifies exact arguments and call count.
- End-to-end: API/webhook request through processing to persisted state assertions.

## OOP and design patterns
- State transition table in `src/domain.js` prevents arbitrary lifecycle mutation.
- Builders in `src/builders.js` reduce scenario payload duplication.
- Payment-provider adapter seam is injected into `createApp()` and replaced by `MockPaymentProvider`.
- `InMemoryRepository` isolates persistence operations.
- `BillingApiClient` and `PersistenceVerifier` separate HTTP transport and persistence assertions from scenario intent.

## API validation
Playwright's API request fixture calls the Express service. Tests validate status codes, response shapes, invalid input, HMAC signatures over raw request bodies, malformed payloads and duplicate event IDs.

## Database validation
The current fixture uses an in-memory repository. Tests query subscriptions, invoices, webhook events and audit history through test-only inspection endpoints and assert consistency with public API results. State resets before each test.

## Mock provider / webhook validation
The mock provider records customer, payment method, amount, currency and idempotency key. Tests configure success, decline and timeout. Webhook tests create signed payloads and verify duplicate and stale event behavior.

## Tradeoffs and limitations
The fixture uses in-memory persistence and does not prove SQL durability or concurrent race safety. It has no real provider, retry scheduler, auth, proration, taxes, or production monitoring. Test-only endpoints must be disabled in production. See `docs/TEST-STRATEGY.md`.

## Responsible AI usage
AI assistance was used to draft and review the fixture, test architecture, and documentation. I reviewed the lifecycle and webhook implementation and corrected invoice ownership/retry handling so a retry success updates the existing invoice instead of leaving a failed invoice beside an active subscription. I also checked JavaScript syntax with `node --check src/app.js`. The Playwright suite has not been run as part of this review; run `npm test` and report its actual result before submitting.
