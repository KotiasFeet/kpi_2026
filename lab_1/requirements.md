# **Requirements & Scenarios**

## **EARS Requirements**

- **[REQ-001]** *WHEN* **`Developer`** publishes a new **`Game`** system *SHALL* create new **`Game`** record assosiated with his `developer_id`
- **[REQ-002]** *WHEN* a **`Player`** purchases a **`Game`** the system *SHALL* send a transaction request to **`PaymentProcessing`**.
- **[REQ-003]** *WHEN* a transaction in **`PaymentProcessing`** is sucesssful system *SHALL* create a Many-to-Many link betwen the **`User`** and the **`Game`**
- **[REQ-004]** *WHILE* a **`Player`** views a **`Game`** page, the system *SHALL* calculate an average **`Reweiv`** score from all associated **`Reviews`**
- **[REQ-005]** *WHEN* **`Player`** atamptes to write a **`Reweiv`** for a **`Game`**  not in their library, the system *SHALL* reject the submission
- **[REQ-006]** *WHEN* a **`Player`** requests a refund within 31 days of perchasing the **`Game`** and with playtime of less then 120 minutes, the system *SHALL* revoke the **`Game`** access and initiate a refund via **`PaymentProcessing`**.

## **User Stories**

- **[US-001]** As a **`Developer`** i want to publish my **`Games`** and resieve money for their purcheses.
- **[US-002]** As a **`Developer`** i want to resieve money for **`Game`** purcheses.
- **[US-003]** As a **`Player`** i want to see reviews for **`Game`**s.
- **[US-004]** As a **`Player`** i want to give give all my money to **`Developer`** and recieve the **`Game`**.
- **[US-005]** As a **`Player`**, I want to leave a review with a rating and comment on a **`Game`** I own to share my opinion.

## **Gherkin Scenarios**

- **[GC-001]** **Game Publication** *GIVEN* a **`User`** is a **`Developer`**, *WHEN* they submit a `game`, `tittle`, `price` and `release_date` *THEN* the system creates a new **`Game`** entity linked to their `developer_id`
- **[GC-002]** **Game Purchase** *GIVEN* a **`Player`** is on an unowened **`Game`**'s page, *WHEN* they click "Buy" and **`PaymentProcessing`** approves the transaction, *THEN* the system links the **`Game`** to the **`User`**.
- **[GC-003]** **Writing a Review** *GIVEN* a **`Player`** own's a **`Game`**, *WHEN* they submit a `rating` and `comment`, *THEN* the system creates the **`Reweiv`** entity linked to **`Game`** and **`User`**	

## **Traceability Matrix**

| Requirement ID | Associated EARS Trigger | User Story | Gherkin | Use Case | Coverage Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **REQ-001** | send request to PaymentGateway | US-001 | SC-001 | UC-001, UC-002 | Covered |
| **REQ-002** | create ownership link | US-001 | SC-001 | UC-001 | Covered |
| **REQ-003** | display average rating | - | - | UC-005 | Partially Covered (No US/SC) |
| **REQ-004** | reject unowned game review | US-003 | SC-003 | UC-004 | Covered |
| **REQ-005** | create Game record | US-002 | SC-002 | UC-003 | Covered |
| **REQ-006** | refund within 14 days | - | - | - | **UNCOVERED** |
