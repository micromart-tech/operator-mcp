---
name: micromart-analysis
description: Analyze Micromart smart-store data (sales trends, product and store rankings, planograms, stockouts, and restock planning) using the Micromart MCP tools. Use when the user asks about their Micromart cabinets, stores, products, or sales performance.
---

# Micromart operator analysis

Guidance for answering operator questions with the Micromart MCP tools. All
tools are read-only, and all data is automatically scoped to the signed-in
operator's own account. Never ask for an operator id.

## Core rules

- Monetary amounts come back in **cents**; convert to dollars when presenting.
- Resolve product names with `search_products` before calling product tools.
- Cabinet-level tools need `site_id` (the cabinet UUID); store-level tools take
  `store_id`. Prefer `store_id` when the question is about a whole store; it
  covers all cabinets there.
- Dates accept `YYYY-MM-DD` or relative forms (`today`, `30d`, `4w`, `3mo`).
  Bare dates are whole local days, inclusive at both ends.
- Lead with the direct answer, then a markdown table. Rankings belong in tables.
- If a tool errors because an id doesn't exist, the resource isn't in the
  operator's account. Say so instead of guessing.

## Which tool for which question

| Question | Tool |
| --- | --- |
| "How are we doing?" (snapshot) | `get_operator_overview` |
| Revenue, orders, trends, peak hours | `get_sales_summary` (`group_by`: day/week/month/hour) |
| Compare two periods | `get_period_comparison` |
| Best / worst stores, or cabinets in a store | `get_store_ranking` (`metric`: revenue/orders/aov, `direction`: top/bottom) |
| Best / worst products | `get_product_ranking` (`metric`: revenue/units) |
| Single-product deep dive | `get_product_performance` |
| "What sells together" | `get_product_affinity` (30-day cap) |
| Shelf-row or per-slot sales | `get_row_performance` / `get_cell_performance` (needs `site_id`) |
| Cabinet layout | `get_planogram_layout` (slot labels like A1, B2; layout digits: 1 = single-width, 2 = double) |
| Draft / scheduled planogram changes | `get_pending_planogram` (needs `site_id`) |
| Where should a product go | `get_product_placement_recommendations` |
| Compare cabinets within a store | `get_store_cabinet_comparison` |
| Promotion performance | `get_promotion_performance` |
| Out of stock | `get_stockouts` |
| Which cabinets need a visit, and route order | `get_restock_status` / `get_restock_priorities` / `get_store_visit_plan` |
| On-hand inventory | `get_inventory_levels` / `get_cabinet_slot_inventory` (needs `site_id`) |
| Weekly / ops digest | `get_ops_digest` |

## Interpretation notes

- `orders` and `revenue` count paid (> $0) orders with a reviewed fridge open;
  revenue excludes tax. `units` = items sold.
- On-hand quantities and stockout data are vision-estimated; present them as
  estimates, not audits.
- AOV = average order value. Lift in `get_product_affinity` > 1 means the
  products sell together more than chance.
