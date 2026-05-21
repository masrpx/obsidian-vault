  ---
  Lark Base field types — what's actually stored:

  ┌───────────────────────────────────┬────────────────────────────────────┬───────────────────────────────────────┐
  │               Field               │             Stored as              │             Filter syntax             │
  ├───────────────────────────────────┼────────────────────────────────────┼───────────────────────────────────────┤
  │ Date (e.g. visit_date, date)      │ Integer serial — days since        │ CurrentValue.[field]=46162            │
  │                                   │ 1899-12-30                         │                                       │
  ├───────────────────────────────────┼────────────────────────────────────┼───────────────────────────────────────┤
  │ DateTime (e.g. start_dt)          │ Unix milliseconds                  │ >= timestamp comparison               │
  ├───────────────────────────────────┼────────────────────────────────────┼───────────────────────────────────────┤
  │ Formula (e.g. if_new_case)        │ [{text:"...", type:"text"}] array  │ Can't filter at API — do it           │
  │                                   │                                    │ client-side                           │
  ├───────────────────────────────────┼────────────────────────────────────┼───────────────────────────────────────┤
  │ SingleSelect record value         │ Plain display text (e.g. "ฟิล์ม")    │ String equality works                 │
  ├───────────────────────────────────┼────────────────────────────────────┼───────────────────────────────────────┤
  │ SingleSelect option IDs (via      │ ["optJduPYQH"] array               │ Resolve via /fields metadata endpoint │
  │ formula)                          │                                    │                                       │
  └───────────────────────────────────┴────────────────────────────────────┴───────────────────────────────────────┘

  The big one: DATE("2026/05/20") and DATE("2026-05-20") both return 0 results on Date serial and DateTime fields. Use
  numeric comparison instead.

  Debug tip: never use getRecords() (paginates everything) for debugging — use a direct axios.get with page_size: 5 and
  timeout: 8000.