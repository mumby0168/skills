---
name: render-contract
description: Render an API, event or DB contract readably in the terminal, as annotated JSONC or a schema tree, for the contract as it is or for a change to it. Use when asked to show a contract, a request or response model, an event, or a table, or when a review stop changes one.
---

Render one contract so a reviewer reads its shape in seconds. Contracts are API request and response models, events (stored domain events and integration or message events), and DB tables.

Inputs: the contract (a type, route, event or table name, or a file path) and, for a change, the diff ref.

Values come from tests, fixtures or seed data. When none exist, use a placeholder like `"…"` and say so in one line.

## API and event contracts: annotated JSONC

The default. A heading comment with the route or event name, the type name and `(new)` or `(changed)`, then a realistic payload. Add a short `//` comment on each field that needs one: its constraint, its meaning, or a risk. Leave self-explanatory fields bare.

```jsonc
// POST consolidations   ConsolidateRequest   (new)
{
  "scheduleIds": ["s-mon", "s-wed"],   // 2+, any order; server sorts
  "requestedBy": "coordinator-1"       // from the body, not the caller
}
// 200 ConsolidationStarted
{ "runId": "r-1", "alreadyRequested": false }  // true = same request already in hand
```

## DB tables: shape tree

Mark the table and each column `+` new, `-` removed, `~` changed. Each column carries its type, nullability and a one-line note. Fold unchanged columns into one line. Show indexes and constraints when they change.

```text
schedule_summaries                  ~ changed
├─ id                    TEXT pk
├─ …                                (4 unchanged columns)
├─ + consolidated_into   TEXT null  new Schedule that took the care
└─ + ix_consolidated_into           index on consolidated_into
```

## Extra parts, only where they help

- **Change:** a `diff` block, `-` lines before and `+` lines after, comments only on changed lines.
  ```diff
    "endDate": "2026-10-20",
  - "source": "Manual"
  + "source": "Consolidation"   // new value; finance reads it
  ```
- **Wide model** (more than about 8 fields): a table of field, type, example, note.
- **Reach:** each producer, consumer and stored copy the change touches, with `CHECK` on anything unverified.

Done when every new or changed field carries a value and, where needed, a note, and every part shown earns its place.
