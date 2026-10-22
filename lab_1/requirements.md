# **Requirements & Scenarios**

## **EARS Requirements**

**Functional Requirements:**

- **[REQ-001]** *WHEN* a **`Developer`** publishes a new **`Game`**, the system *SHALL* create a new **`Game`** record associated with their `developer_id`.
- **[REQ-002]** *WHEN* a **`Player`** purchases a **`Game`**, the system *SHALL* send a transaction request to the external **`PaymentProcessing`** system.
- **[REQ-003]** *WHEN* a transaction in **`PaymentProcessing`** is successful, the system *SHALL* create a **`Transaction`** record *AND* a **`LibraryRecord`** linking the **`User`** *AND* the **`Game`**.
- **[REQ-004]** *WHILE* a **`Player`** views a **`Game`** page, the system *SHALL* calculate *AND* display the average **`Review`** score from all associated reviews.
- **[REQ-005]** *WHEN* a **`Player`** attempts to write a **`Review`** for a **`Game`** not in their library, the system *SHALL* reject the submission.
- **[REQ-006]** *WHEN* a **`Player`** requests a refund for a **`Game`** purchased within the last 31 days *AND* with a recorded playtime of less than 120 minutes, the system *SHALL* revoke **`Game`** access *AND* initiate a refund via **`PaymentProcessing`**.
- **[REQ-007]** *WHEN* a **`Player`** requests a refund for a **`Game`** purchased more than 31 days ago, the system *SHALL* reject the request *AND* display a *"Time limit exceeded"* error.
- **[REQ-008]** *WHEN* a **`Player`** requests a refund for a **`Game`** with a playtime of 120 minutes or more, the system *SHALL* reject the request *AND* display a *"Playtime limit exceeded"* error.

**Cross-Cutting Requirements:**

- **[REQ-009]** *WHILE* a **`User`** attempts to access protected actions (such as Purchase, Refund, Publish, Review), the system *SHALL* verify their authentication status.

**NON-Functional Requirements:**

- **[NFR-001] Usability:** The platform shall be accessible via modern web browsers (Chrome, Safari, Firefox, etc.) on desktop *AND* mobile devices, supporting English interface languages.
- **[NFR-002] Performance:** The System shall respond to page load request within 200ms under a 1000 cuncurrent users load.
- **[NFR-003] Reliability & Load:** The system shall support at least 1000 concurrent checkout requests/minute. If the load exceeds this limit, the system shall queue subsequent requests *AND* display a "waiting in line" screen without dropping the connections.
- **[NFR-004] Security:** All payment data shall be encrypted via in transit. The system shall not store raw credit card numbers or CVV codes, using external PaymentProcessing system tokenization.

## **User Stories**

- **[US-001]** As a **`Developer`**, I want to publish my **`Games`** so players can purchase them.
- **[US-002]** As a **`Developer`**, I want to receive money for **`Game`** purchases to monetize my work.
- **[US-003]** As a **`Player`**, I want to see reviews for **`Games`** to make informed purchasing decisions.
- **[US-004]** As a **`Player`**, I want to pay for a **`Game`** to add it to my personal library.
- **[US-005]** As a **`Player`**, I want to leave a review with a rating *AND* comment on a **`Game`** I own to share my opinion.
- **[US-006]** As a **`Player`**, I want to refund a **`Game`** if I am unsatisfied, provided I meet the return policy criteria.

## **Gherkin Scenarios**

- **[GS-001] Game Publication:** *GIVEN* a **`User`** is a **`Developer`**, *WHEN* they submit a `game`, `title`, `price` *AND* `release_date` *THEN* the system creates a new **`Game`** entity linked to their `developer_id`

- **[GS-002] Game Purchase:** *GIVEN* a **`Player`** is on an unowned **`Game`**'s page, *WHEN* they click "Buy" *AND* **`PaymentProcessing`** approves the transaction, *THEN* the system links the **`Game`** to the **`User`**.

- **[GS-003] Writing a Review:** *GIVEN* a **`Player`** owns a **`Game`**, *WHEN* they submit a `rating` *AND* `comment`, *THEN* the system creates the **`Review`** entity linked to **`Game`** *AND* **`User`**

### **Gherkin Scenarios: Deep dive into Game Refund (US-006)**

- **[GS-004] Valid Refund:** *GIVEN* a **`Player`** owns the **`Game`** *AND* the **`Game`** was purchesed less then 31 days ago, *AND* the recorded playtime is less then 120 minutes, *WHEN* they submit a refund request *THEN* the system revoces the **`Game`** access *AND* initiates a refund transaction via **PaymentProcessing**.

- **[GS-005] NOT Valid Refund 1:** *GIVEN* a **`Player`** owns the **`Game`** *AND* the **`Game`** was purchesed more then 31 days ago, *AND* the recorded playtime is less then 120 minutes, *WHEN* they submit a refund request *THEN* the system rejects the refund request *AND* displays *"Time limit exceeded"* error.

- **[GS-005] NOT Valid Refund 2:** *GIVEN* a **`Player`** owns the **`Game`** *AND* the **`Game`** was purchesed less then 31 days ago, *AND* the recorded playtime is more then 120 minutes, *WHEN* they submit a refund request *THEN* the system rejects the refund request *AND* displays *"Playtime limit exceeded"* error.

## **Traceability Matrix**

| Requirement ID   | Associated EARS Trigger           | User Story   | Gherkin  | Use Case   | Coverage Status                |
| :---             | :---                              | :---         | :---     | :---       | :---                           |
| **REQ-001**      | Create Game record                | US-001       | GS-001   | UC-003     | Covered                        |
| **REQ-002**      | Send request to PaymentProcessing | US-004       | GS-002   | UC-001     | Covered                        |
| **REQ-003**      | Create ownership link             | US-004       | GS-002   | UC-001     | Covered                        |
| **REQ-004**      | Display average rating            | US-003       | -        | UC-005     | Partially Covered              |
| **REQ-005**      | Reject unowned game review        | US-005       | GS-003   | UC-004     | Covered                        |
| **REQ-006**      | Refund valid request              | US-006       | GS-004   | UC-006     | Covered                        |
| **REQ-007**      | Reject refund > 31 days           | US-006       | GS-005   | UC-006     | Covered                        |
| **REQ-008**      | Reject refund >= 120 mins         | US-006       | GS-006   | UC-006     | Covered                        |
| **REQ-009**      | Verify auth token                 | All US       | All GS   | All UC     | Cross-cutting (Applies to all) |

## **Out of scope**

- *Social* (friends, chats, etc.)
- *Game age restriction* (user age verification and game tags PG-13, R-18, etc.)
- *Family sharing* (Create a family of players and share games within family)
