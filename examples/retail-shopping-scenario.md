# Retail Shopping Scenario

A multi-retailer shopping run demonstrating how an AI coding agent (via tokless) can help compare deals, track memberships, and coordinate orders across stores — while keeping token usage lean.

---

## Scenario Overview

**Goal:** Complete a week's shopping across five retailers, maximizing savings with existing memberships and available deals.

**Retailers:** Walgreens · Amazon · CVS · Walmart (Walmart+) · Kroger

---

## Step 1 — Deal Comparison (Walgreens, Amazon, CVS)

Compare prices and current promotions for the same items across three stores before committing.

| Item | Walgreens | Amazon | CVS | Best Buy |
|------|-----------|--------|-----|----------|
| Vitamins (30-ct) | $12.99 (2-for-1 sale) | $10.49 (Prime) | $14.99 | Walgreens |
| Shampoo (12 oz) | $6.49 | $5.99 | $5.49 (ExtraCare) | CVS |
| Pain reliever (100-ct) | $8.99 | $7.50 | $8.49 | Amazon |

**Decision:** Split the purchase — vitamins at Walgreens, shampoo at CVS, pain reliever via Amazon.

---

## Step 2 — Walmart Purchase (Walmart+ Member)

Using an active Walmart+ membership for:
- Free same-day delivery (no minimum)
- Member price on fuel
- Early access to deals

**Cart:**
- Red blouse (linked item from text) — in-store pickup selected
- Paper towels (12-pack) — delivery

**Action:** Head to Walmart store for the red blouse pickup; paper towels ship to home.

---

## Step 3 — Grocery Order at Kroger

Place a water delivery order through Kroger's pickup/delivery service.

**Order:**
- Purified water, 24-pack (×3 cases) — Kroger brand
- Apply Kroger Plus card for fuel points

**Pickup time:** Next-day morning slot selected.

---

## Summary

| Store | Channel | Items | Est. Cost |
|-------|---------|-------|-----------|
| Walgreens | In-store | Vitamins | $6.50 (BOGO) |
| CVS | In-store | Shampoo | $5.49 |
| Amazon | Delivery | Pain reliever | $7.50 |
| Walmart | In-store pickup | Red blouse | TBD |
| Kroger | Pickup order | Water (3 cases) | ~$12.00 |

**Total estimated:** ~$31.49 + blouse

---

## How tokless Helps Here

When an AI agent assists with this scenario — looking up prices, checking membership perks, drafting cart lists — it reads product pages, comparison tables, and order confirmations. That's a lot of noisy text. tokless trims that output before it reaches the model, cutting token usage without losing the decisions that matter.
