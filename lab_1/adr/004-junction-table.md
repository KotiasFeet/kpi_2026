# ADR 004: Reintroduction of Junction Tables for State Management

## **Context**

[[ADR-002]](https://github.com/KotiasFeet/kpi_2026/blob/main/lab_1/adr/002-m-t-m-notation.md) mandated a pure Many-to-Many (M:N) declarative notation without explicit Junction tables, assuming that relationship attributes like `purchase_date` or `time_played` would not be needed. However, implementing the refund policy (REQ-006) requires tracking individual player playtime (less than 120 minutes) and purchase dates (within 31 days). Storing playtime directly on the Game entity incorrectly tracks global time instead of per-user time.

## **Alternatives**

- Maintain pure M:N notation and attempt to store user-specific game states in external logs, NoSQL documents or Json files.
- Superseed the [ADR-002] and implement explicit Junction tables such as, but not limited to, **`LibraryRecord`**, **`Transaction`**.

## **Decision**

Implement explicit Junction tables into the ER-model and supersede [ADR-002].

## **Justification**

Pure M:N notation is insufficient if the relationship itself needs attributes. The data model needs to relate these metrics to the specific relationship between a **`User`** and a **`Game`** to be able to enforce business rules correctly (e.g. the 120 minute `playtime` limit for refunds). Standardized relational structure for these attributes is given by explicit Junction tables.

## **Consequences**

- Supersedes [ADR-002]: The rule against forging bridges between M:N entities is revoked.
- The ER-model must be updated to include **`LibraryRecord`** (containing `playtime`) and Transaction (containing `purchase_date`) as Junction entities bridging User and Game.
- Increased diagram complexity
