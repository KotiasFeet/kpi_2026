# Specification of the ER Model for Pika-8 Hub

## Goal

Describe the data structure for user and developer profiles, game catalog entries, ownership, and review records for the digital storefront domain.

## Entities and Attributes

- **User**: `id` (UUID, PK), `username` (String), `email` (String), `registration_date` (DateTime)
- **Developer**: `id` (UUID, PK), `user_id` (UUID, FK), `company` (String), `company_contact` (String)
- **Game**: `id` (UUID, PK), `developer_id` (UUID, FK), `title` (String), `description` (TEXT), `base_price` (DECIMAL), `discount` (Integer), `release_date` (DateTime), `playtime` (Interval)
- **Review**: `id` (UUID, PK), `user_id` (UUID, FK), `game_id` (UUID, FK), `rating` (Integer), `comment` (TEXT), `creation_time` (DateTime)

## Relationships

- `User` may or may not be a `Developer` (1:0..1)
- `Developer` can publish zero or many `Game` records (1:N)
- `User` can own many `Game` records and each `Game` can be owned by many `User` records (M:N)
- `User` writes zero or many `Review` records (1:N)
- `Game` can have zero or many `Review` records (1:N)

## Actors

1. **Player** — a human user who browses catalog items, purchases games, stores them in a library, and writes or reads reviews.
2. **Developer** — a specialized human user who publishes games and manages the catalog items they created.
3. **PaymentGateway** — an external financial service used to validate transactions and process refunds.

## Main Goals

- Enable developers to publish games to the catalog.
- Allow players to purchase games and obtain access rights.
- Support review submission and rating aggregation.
- Manage refund flows through an external payment provider.

## Acceptance Criteria

1. **PK Type:** All primary keys SHALL be `UUID` values.
2. **Normalization:** The model SHALL be compliant with 3NF.
3. **M:N Representation:** Ownership SHALL be modeled as a many-to-many relationship between `User` and `Game` without hidden bridge-table ambiguity in the conceptual specification.
4. **Naming Consistency:** Entity and field names SHALL match the Mermaid diagram and the requirement vocabulary exactly.
5. **Field Style:** All attribute names SHALL use `snake_case` naming.
6. **Actors:** At least one human actor and one external service actor SHALL be present in the use-case model.

