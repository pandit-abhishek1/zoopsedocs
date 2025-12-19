# Subscription Service and API Routes

This document describes the **Subscription** module in the CRM backend, including:
- the main concepts and data model,
- all subscription-related routes,
- and the responsibility of each route and internal component.

---

## 1. Concepts and Data Model

### 1.1. Subscription

A **Subscription** represents a customer’s ongoing access to a service or product under a specific pricing plan.

Typical fields (adjust to match your schema):

- `id` (string | number) – Unique identifier of the subscription.
- `customerId` (string | number) – Reference to the customer.
- `planId` (string | number) – Reference to the subscription plan.
- `status` (`active` | `pending` | `cancelled` | `expired` | `trial` | `paused`) – Lifecycle state of the subscription.
- `startDate` (ISO string) – Date from which the subscription became active.
- `endDate` (ISO string | null) – Date on which the subscription ended or will end, if applicable.
- `nextBillingDate` (ISO string | null) – The next scheduled billing date.
- `billingCycle` (`monthly` | `yearly` | `custom`) – Frequency of billing.
- `amount` (number) – Recurring amount to be billed.
- `currency` (string) – Currency code (e.g., `"INR"`).
- `trialEndDate` (ISO string | null) – Trial expiry date, if applicable.
- `metadata` (object) – Any additional attributes.

### 1.2. Subscription Plan

A **Subscription Plan** defines what the subscription offers (features, price, billing period).

Typical fields:

- `id`
- `name`
- `description`
- `price`
- `currency`
- `billingCycle` (e.g. `monthly`, `yearly`)
- `isActive` (boolean)

---

## 2. Route Overview

> Base path (example): `/api/subscriptions`  
> Adjust base paths and HTTP methods to match your actual implementation.

### 2.1. Plans

- `GET /api/subscription-plans`
- `POST /api/subscription-plans`
- `GET /api/subscription-plans/:planId`
- `PATCH /api/subscription-plans/:planId`
- `DELETE /api/subscription-plans/:planId`

### 2.2. Subscriptions

- `POST /api/subscriptions` – Create a new subscription for a customer.
- `GET /api/subscriptions` – List/filter subscriptions.
- `GET /api/subscriptions/:subscriptionId` – Get details of a subscription.
- `PATCH /api/subscriptions/:subscriptionId` – Update properties such as plan or metadata.
- `POST /api/subscriptions/:subscriptionId/activate` – Activate a pending subscription.
- `POST /api/subscriptions/:subscriptionId/cancel` – Cancel a subscription.
- `POST /api/subscriptions/:subscriptionId/pause` – Pause a subscription (if supported).
- `POST /api/subscriptions/:subscriptionId/resume` – Resume a paused subscription.
- `POST /api/subscriptions/:subscriptionId/renew` – Manually trigger renewal.
- `GET /api/customers/:customerId/subscriptions` – List all subscriptions for a customer.

### 2.3. Billing / Webhooks (if applicable)

- `POST /api/subscriptions/webhook/payment` – Handle payment provider callbacks.
- `POST /api/subscriptions/:subscriptionId/payment/retry` – Retry failed payment.

---

## 3. Route Responsibilities

### 3.1. `GET /api/subscription-plans`

**Responsibility**

- Return a list of all active subscription plans that can be assigned to customers.
- Support filters such as:
  - `isActive`
  - `billingCycle` (e.g. `monthly`, `yearly`)

**Typical behavior**

- Query the `Plan` table.
- Only return plans that are `isActive = true` by default.
- Optionally support pagination (`page`, `limit`) and sorting.

---

### 3.2. `POST /api/subscription-plans`

**Responsibility**

- Create a new subscription plan that can be used for future subscriptions.

**Expected input (example)**

```json
{
  "name": "Standard Monthly",
  "description": "Standard subscription plan billed monthly",
  "price": 499,
  "currency": "INR",
  "billingCycle": "monthly",
  "isActive": true
}
```

**Notes**

- Validate that the plan name is unique (depending on business rules).
- Only allow authorized roles (e.g. Admin) to create plans.

---

### 3.3. `GET /api/subscription-plans/:planId`

**Responsibility**

- Fetch details of a specific subscription plan by `planId`.

---

### 3.4. `PATCH /api/subscription-plans/:planId`

**Responsibility**

- Update attributes of a plan (e.g. name, description, active status).
- Typically, changing price or billing cycle should be applied **prospectively** to new subscriptions only, depending on business rules.

---

### 3.5. `DELETE /api/subscription-plans/:planId`

**Responsibility**

- Soft-delete or deactivate a plan so it cannot be assigned to new subscriptions.
- Existing subscriptions may continue using the old plan, or be migrated, depending on business logic.

---

### 3.6. `POST /api/subscriptions`

**Responsibility**

- Create a **new subscription** for a customer based on a selected plan.

**Typical workflow**

1. Validate customer exists (`customerId`).
2. Validate plan exists and is active (`planId`).
3. Calculate initial `startDate`, `nextBillingDate`, and `endDate` (if fixed term).
4. If integrated with payment:
   - Create a payment mandate or first charge.
   - Set status to `pending` until payment is confirmed.
5. Save subscription in database.
6. Return the created subscription.

**Example request**

```json
{
  "customerId": 123,
  "planId": 5,
  "startDate": "2025-01-01T00:00:00.000Z",
  "metadata": {
    "source": "sales-portal"
  }
}
```

---

### 3.7. `GET /api/subscriptions`

**Responsibility**

- Return a paginated list of subscriptions.

**Possible filters**

- `customerId`
- `status` (`active`, `cancelled`, etc.)
- `planId`
- Date range (`startDate`, `endDate`)

**Typical behavior**

- Useful for admin dashboards and reporting.
- Must enforce appropriate access control (e.g. admins see all, customers see only their own).

---

### 3.8. `GET /api/subscriptions/:subscriptionId`

**Responsibility**

- Return detailed information about a single subscription:
  - Plan details
  - Current status
  - Billing history (if joined or requested)
  - Next billing date, amount, currency

---

### 3.9. `PATCH /api/subscriptions/:subscriptionId`

**Responsibility**

- Update fields that are allowed to change mid-life:
  - `metadata`
  - Optional: `planId` (plan upgrade/downgrade)
  - Optional: `nextBillingDate` (admin corrections)
- Must enforce business rules:
  - If changing plan, may need prorated billing or next-cycle change.

---

### 3.10. `POST /api/subscriptions/:subscriptionId/activate`

**Responsibility**

- Move a subscription from `pending` (e.g. waiting for payment or admin approval) to `active`.
- Typically:
  - Validate that initial payment was successful (or skip for free/trial).
  - Update `status` to `active`.
  - Set `startDate` if not already set.
  - Calculate `nextBillingDate`.

---

### 3.11. `POST /api/subscriptions/:subscriptionId/cancel`

**Responsibility**

- Cancel an existing subscription.

**Common behaviors**

- Option A: Cancel at end of current billing cycle
  - Set `status` to `cancelled`.
  - Set `endDate` to current period end.
  - Access remains valid until `endDate`.

- Option B: Immediate cancellation
  - Set `status` to `cancelled`.
  - Set `endDate` to `now`.
  - Disable access immediately.

**Notes**

- When integrated with a payment gateway, also:
  - Cancel recurring payments/mandates.
  - Record cancellation reason (user requested, payment failure, admin action, etc.).

---

### 3.12. `POST /api/subscriptions/:subscriptionId/pause`

**Responsibility**

- Temporarily pause a subscription without fully cancelling it.

**Behavior (if supported)**

- Allowed only if subscription is `active`.
- Update `status` to `paused`.
- Optionally store `pausedAt` and preserve `nextBillingDate`.
- Billing may be skipped or adjusted based on business rules.

---

### 3.13. `POST /api/subscriptions/:subscriptionId/resume`

**Responsibility**

- Resume a previously paused subscription.

**Behavior**

- Change `status` from `paused` to `active`.
- Recalculate `nextBillingDate` if necessary.

---

### 3.14. `POST /api/subscriptions/:subscriptionId/renew`

**Responsibility**

- Manually trigger **renewal** of a subscription (usually used by admins or for recovery).

**Behavior**

1. Validate that subscription is eligible for renewal (e.g. `active` and near `nextBillingDate`).
2. Call payment service to charge the billing amount.
3. On success:
   - Extend access into the next period.
   - Update `nextBillingDate`.
4. On failure:
   - Record failure reason.
   - Optionally set status to `payment_failed` or similar.

---

### 3.15. `GET /api/customers/:customerId/subscriptions`

**Responsibility**

- List all subscriptions for a given customer.

**Behavior**

- Used in:
  - Customer profile views
  - Customer self-service portal
- Can support filters like `status` and pagination.

---

### 3.16. `POST /api/subscriptions/webhook/payment` (if applicable)

**Responsibility**

- Receive asynchronous payment events from the payment gateway (e.g. Razorpay, Stripe).

**Typical events handled**

- Payment success → activate/renew subscription.
- Payment failure → mark as `payment_failed` and maybe send notification.
- Refunds and chargebacks → adjust subscription status.

**Notes**

- Must verify webhook signatures to ensure the request is from your payment provider.
- Should be idempotent (safe to call multiple times with same event).

---

## 4. Internal Responsibilities (Service Layer)

To keep the codebase maintainable, subscription logic is usually split into layers:

### 4.1. Controller Layer

- Located in routes/controllers (e.g. `routes/subscriptionRoutes.js` or `controllers/subscriptionController.js`).
- Responsibilities:
  - Parse and validate HTTP request.
  - Call service layer methods.
  - Format HTTP responses.
  - Handle authentication/authorization checks.

### 4.2. Service Layer (e.g. `SubscriptionService`)

Core business logic is centralized here:

- Create subscription:
  - Validate customer & plan.
  - Initialize dates and amounts.
  - Integrate with payment if needed.
- Manage status:
  - `activate`, `cancel`, `pause`, `resume`, `renew`.
- Handle side effects:
  - Trigger email/SMS notifications.
  - Update related CRM entities (e.g. account status, entitlements).

### 4.3. Repository / Model Layer

- Directly interacts with the database (ORM models or raw queries).
- Responsibilities:
  - Create, read, update, delete subscription records.
  - Maintain referential integrity with plans, customers, and invoices.

---

## 5. Error Handling and Common Status Codes

- `400 Bad Request` – Invalid input (missing fields, invalid plan ID, etc.).
- `401 Unauthorized` – Not authenticated.
- `403 Forbidden` – Authenticated, but not allowed to perform the action.
- `404 Not Found` – Subscription or plan not found.
- `409 Conflict` – Conflicting state (e.g. trying to activate an already active subscription).
- `500 Internal Server Error` – Unexpected server-side errors.

---

## 6. Notes and Future Enhancements

- Add support for:
  - Trials and introductory pricing.
  - Prorated upgrades/downgrades.
  - Invoices and detailed billing history endpoints.
- Consider rate-limiting sensitive routes (like manual renewals) to avoid accidental multiple charges.

---

_Last updated: 2025-12-19_