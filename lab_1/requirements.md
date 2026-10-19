# **Requirements & Scenarios**

## **EARS Requirements**

- **[REQ-001]** *WHEN* **`Developer`** publishes a new **`Game`** system *SHALL* create new **`Game`** record associated with his `developer_id`
- **[REQ-002]** *WHEN* a **`Player`** purchases a **`Game`** the system *SHALL* send a transaction request to **`PaymentProcessing`**.
- **[REQ-003]** *WHEN* a transaction in **`PaymentProcessing`** is successful system *SHALL* create a Many-to-Many link between the **`User`** and the **`Game`**
- **[REQ-004]** *WHILE* a **`Player`** views a **`Game`** page, the system *SHALL* calculate an average **`Review`** score from all associated **`Reviews`**
- **[REQ-005]** *WHEN* **`Player`** attempts to write a **`Review`** for a **`Game`**  not in their library, the system *SHALL* reject the submission
- **[REQ-006]** *WHEN* a **`Player`** requests a refund within 31 days of purchasing the **`Game`** and with playtime of less than 120 minutes, the system *SHALL* revoke the **`Game`** access and initiate a refund via **`PaymentProcessing`**.

## **User Stories**

- **[US-001]** As a **`Developer`** I want to publish my **`Games`** and receive money for their purchases.
- **[US-002]** As a **`Developer`** I want to receive money for **`Game`** purchases.
- **[US-003]** As a **`Player`** I want to see reviews for **`Game`**s.
- **[US-004]** As a **`Player`** I want to give all my money to **`Developer`** and receive the **`Game`**.
- **[US-005]** As a **`Player`**, I want to leave a review with a rating and comment on a **`Game`** I own to share my opinion.

## **Gherkin Scenarios**

- **[GS-001]** **Game Publication** *GIVEN* a **`User`** is a **`Developer`**, *WHEN* they submit a `game`, `title`, `price` and `release_date` *THEN* the system creates a new **`Game`** entity linked to their `developer_id`
- **[GS-002]** **Game Purchase** *GIVEN* a **`Player`** is on an unowned **`Game`**'s page, *WHEN* they click "Buy" and **`PaymentProcessing`** approves the transaction, *THEN* the system links the **`Game`** to the **`User`**.
- **[GS-003]** **Writing a Review** *GIVEN* a **`Player`** owns a **`Game`**, *WHEN* they submit a `rating` and `comment`, *THEN* the system creates the **`Review`** entity linked to **`Game`** and **`User`**

## **Traceability Matrix**

| Requirement ID | Associated EARS Trigger | User Story | Gherkin | Use Case | Coverage Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **REQ-001** | send request to PaymentProcessing| US-001 | GS-001 | UC-001, UC-002 | Covered |
| **REQ-002** | create ownership link            | US-001 | GS-001 | UC-001         | Covered |
| **REQ-003** | display average rating           |    -   |    -   | UC-005         | Partially Covered (No US/SC) |
| **REQ-004** | reject unowned game review       | US-003 | GS-003 | UC-004         | Covered |
| **REQ-005** | create Game record               | US-002 | GS-002 | UC-003         | Covered |
| **REQ-006** | refund within 31 days            |    -   |    -   |    -           | **UNCOVERED** |
