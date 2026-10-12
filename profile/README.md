# TugQL

**An open, portable query language and structured query tree (TugQT) for relational, document, API, and graph-connected data.**

[![Website](https://img.shields.io/badge/website-tugql.org-0969da?style=flat-square)](https://tugql.org)
[![Specification](https://img.shields.io/badge/spec-tugql%2Ftugql-blue?style=flat-square)](https://github.com/tugql/tugql)
[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg?style=flat-square)](https://opensource.org/licenses/Apache-2.0)

---

## What is TugQL?

**TugQL** (formerly known as **DTQL**) is a vendor-neutral query definition language designed to query disparate SQL, NoSQL, REST/GraphQL APIs, and graph-connected data sources without proprietary query lock-in.

It provides a dual representation with **1:1 lossless round-trip fidelity**:

1. **TugQL (Text Syntax):** A clean, human-friendly SQL-like query language designed for writing, editing, and interactive exploration.
2. **TugQT — Tug Query Tree (Structured Syntax):** A declarative JSON/YAML representation of the parsed/canonical query tree. Perfect for version control, AST manipulation, automated policy evaluation, and execution across heterogeneous storage backends without requiring a bespoke DSL parser.

---

## Core Repositories

| Repository | Purpose |
| :--- | :--- |
| [**`tugql/tugql`**](https://github.com/tugql/tugql) | The canonical specification, JSON Schema (draft 2020-12), test vectors, and language documentation. |
| [**`tugql/.github`**](https://github.com/tugql/.github) | Community health files and organization profile. |

---

## Implementations & Ecosystem

TugQL is actively integrated across the open data stack:

- **Go Reference Implementation:**
  [`github.com/dal-go/dalgo/dtql`](https://github.com/dal-go/dalgo/tree/main/dtql) — Full parser, validator, query planner, and executor for Go backends.
- **TypeScript / JavaScript:**
  [`@dal-go/dalgo`](https://github.com/dal-go/dalgo-js) — AST parser, validator, and browser/Node executors.
- **DataTug:**
  [`datatug/datatug-core`](https://github.com/datatug/datatug-core), [`datatug-cli`](https://github.com/datatug/datatug-cli), [`datatug-apps`](https://github.com/datatug/datatug-apps) — Native support for saved queries, visual builders, and Git-native `.datatug` projects.
- **OpenVaultDB:**
  [`openvaultdb/openvaultdb-go`](https://github.com/openvaultdb/openvaultdb-go) — Query routes and execution adapters.

---

## Example: TugQL & TugQT

### Human-friendly TugQL Syntax
```sql
FROM orders o
JOIN customers c ON o.customer_id = c.id
WHERE o.status == 'completed' AND o.total > 100
GROUP BY c.country
HAVING revenue > 1000
ORDER BY revenue DESC
SELECT
  c.country,
  COUNT(*) AS total_orders,
  SUM(o.total) AS revenue
```

### Lossless TugQT (Tug Query Tree) in YAML
```yaml
from:
  name: orders
  alias: o
join:
  - name: customers
    alias: c
    on:
      op: ==
      left: { field: o.customer_id }
      right: { field: c.id }
where:
  op: AND
  args:
    - { op: ==, left: { field: o.status }, right: { value: completed } }
    - { op: '>', left: { field: o.total }, right: { value: 100 } }
groupBy:
  - field: c.country
having:
  op: '>'
  left: { field: revenue }
  right: { value: 1000 }
orderBy:
  - field: revenue
    desc: true
columns:
  - field: c.country
  - aggregate: { function: count, args: [{ star: true }] }
    as: total_orders
  - aggregate: { function: sum, args: [{ field: o.total }] }
    as: revenue
```

---

## Rebrand Notice

> **DTQL has been renamed to TugQL.**
>
> - Canonical website: [https://tugql.org](https://tugql.org)
> - Former domain: `dtql.org` (permanently redirects to `tugql.org`)
> - Former repository: `datatug/dtql` (permanently redirects to `tugql/tugql`)
> - Query Language: **TugQL**
> - Query Tree Structure: **TugQT**

---

## Resources & Links

- 🌐 **Website:** [https://tugql.org](https://tugql.org)
- 📖 **Specification & Documentation:** [github.com/tugql/tugql](https://github.com/tugql/tugql)
- ⚖️ **License:** [Apache 2.0](https://github.com/tugql/tugql/blob/main/LICENSE)
