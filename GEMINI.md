# Micromart operator tools

You have access to the Micromart MCP server (read-only). It answers questions
about the signed-in operator's own cabinets, stores, products, and sales. All
data is automatically scoped to their operator account.

Rules:

- Monetary amounts come back in cents; convert to dollars when presenting.
- Resolve product names with `search_products` before calling product tools.
- Cabinet tools need `site_id` (cabinet UUID); store tools take `store_id`.
- Dates accept `YYYY-MM-DD` or relative forms (`today`, `30d`, `4w`, `3mo`).
- Lead with the direct answer, then a markdown table for rankings.

Pick the right tool:

- Snapshot: `get_operator_overview`
- Sales trends / peak hours: `get_sales_summary` (group_by day/week/month/hour)
- Period compare: `get_period_comparison`
- Store or cabinet rankings: `get_store_ranking` (metric revenue/orders/aov)
- Product rankings / deep dive: `get_product_ranking` / `get_product_performance`
- Bought together: `get_product_affinity` (30-day cap)
- Planogram layout / pending changes: `get_planogram_layout` /
  `get_pending_planogram` (need `site_id`)
- Placement advice: `get_product_placement_recommendations`
- Stockouts: `get_stockouts`
- Restock planning: `get_restock_status`, `get_restock_priorities`,
  `get_store_visit_plan`
- Inventory: `get_inventory_levels`, `get_cabinet_slot_inventory` (needs
  `site_id`)
- Ops digest: `get_ops_digest`
