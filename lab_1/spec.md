# Specification of ER-model for Pika-8 Hub

## **Goal**

Describe the data structure to save *user* and *developer* profiles, *games*, *reviews*.

## **Entities & Attributes**

- **`User`:** `id` (UUID, PK), `username` (String), `e-mail` (String), `registeration_date` (DateTime)

- **`Developer`:** `id` (UUID, PK), `user_id` (UUID, FK), `company` (String), `company_contact` (String)

- **`Game`:** `id` (UUID), `developer_id` (UUID, FK), `tittle` (String), `description` (TEXT), `base_price` (DECIMAL), `discount` (Integer), `release_date` (DateTime)

- **`Review`:** `id` (UUID), `user_id` (UUID, FK), `game_id` (UUID, FK), `rating` (Integer), `comment` (TEXT), `creation_time` (DateTime)

## **Relationships**

- `User` can be or not be a `Developer` ( 1:(0..1) )
- `Developer`can have zero or many `Games` ( 1:N )
- `User` "owns" `Games` ( M:N )
- `User` writes a `Review` ( 1:N )
- `Game` gets a `Review` ( 1:N )

## **Acceptance Criteria**

1. **PK Type:** Thou shalt make all Primary Keys as `UUID`.

2. **Normalization:** Thy model shalt be Third Normal Form (3NF).

3. **Pure M:N Notation:** Thou shalt bring Many-to-Many entities directly unto one another. Thou shalt not forge no brigde betwixt them in thy ER-model.

4. **Naming Consistency:** Thou shalt not name thy field and entities differ betwixt thy specification and thy Mermaid diagram, but shalt they match character for character. Thou shalt keep to thine casing uniform and unto thine attribute, thou shalt decree only in `snake_case`.

5. **Declarative Syntax:** Thou shalt scribe thy diagram in Mermaid tongue. Thou shalt bear true to witness to thy cardinalities and thou shalt etch every Foreign Key reference upon thy path.
