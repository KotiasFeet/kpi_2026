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

- **[REQ-010]** *WHEN* a **`User`** submits login credentials, the system *SHALL* delegate authentication to the external **`Auth Service`** *AND* establish an authenticated session when credentials are valid.

- **[REQ-011]** *WHEN* authentication fails or the external **`Auth Service`** is unavailable, the system *SHALL* reject the login or protected(such as Purchase, Refund, Publish, Review) action *AND* *SHALL NOT* grant a session or protected access.

- **[REQ-012]** *WHEN* a **`User`** is authenticated, the system *SHALL* retrieve the user's role or authorization claims from the **`Auth Service`** *AND* use them to authorize Developer-only actions.

**Cross-Cutting Requirements:**

- **[REQ-009]** *WHILE* a **`User`** attempts to access protected actions (such as Purchase, Refund, Publish, Review), the system *SHALL* validate the user's authentication token with the external **`Auth Service`** before allowing the action.

**NON-Functional Requirements:**

- **[NFR-001] Usability:** The platform shall be accessible via modern web browsers (Chrome, Safari, Firefox, etc.) on desktop *AND* mobile devices, supporting English interface languages.
- **[NFR-002] Performance:** The System shall respond to page load request within 200ms under a 1000 cuncurrent users load.
- **[NFR-003] Reliability & Load:** The system shall support at least 1000 concurrent checkout requests/minute. If the load exceeds this limit, the system shall queue subsequent requests *AND* display a "waiting in line" screen without dropping the connections.
- **[NFR-004] Security:** All payment data shall be encrypted via in transit. The system shall not store raw credit card numbers or CVV codes, using external PaymentProcessing system tokenization.
- **[NFR-005] Authentication Security:** Authentication data and access tokens shall be transmitted only over encrypted connections. The platform shall not store user passwords or credentials and shall use the external **`Auth Service`** for identity verification.

## **User Stories**

- **[US-001]** As a **`Developer`**, I want to publish my **`Games`** so players can purchase them.
- **[US-002]** As a **`Developer`**, I want to receive money for **`Game`** purchases to monetize my work.
- **[US-003]** As a **`Player`**, I want to see reviews for **`Games`** to make informed purchasing decisions.
- **[US-004]** As a **`Player`**, I want to pay for a **`Game`** to add it to my personal library.
- **[US-005]** As a **`Player`**, I want to leave a review with a rating *AND* comment on a **`Game`** I own to share my opinion.
- **[US-006]** As a **`Player`**, I want to refund a **`Game`** if I am unsatisfied, provided I meet the return policy criteria.
- **[US-007]** As a **`User`**, I want to authenticate through the **`Auth Service`** so I can access protected actions(such as Purchase, Refund, Publish, Review).

## **Gherkin Scenarios**

- **[GS-001] Game Publication:** *GIVEN* a **`User`** is a **`Developer`**, *WHEN* they submit a `game`, `title`, `price` *AND* `release_date` *THEN* the system creates a new **`Game`** entity linked to their `developer_id`

- **[GS-002] Game Purchase:** *GIVEN* a **`Player`** is on an unowned **`Game`**'s page, *WHEN* they click "Buy" *AND* **`PaymentProcessing`** approves the transaction, *THEN* the system links the **`Game`** to the **`User`**.

- **[GS-003] Writing a Review:** *GIVEN* a **`Player`** owns a **`Game`**, *WHEN* they submit a `rating` *AND* `comment`, *THEN* the system creates the **`Review`** entity linked to **`Game`** *AND* **`User`**

- **[GS-007] Successful Login:** *GIVEN* a **`User`** submits valid credentials, *WHEN* the system delegates authentication to the **`Auth Service`**, *THEN* the system establishes an authenticated session.

- **[GS-008] Failed Login or Unavailable Auth Service:** *GIVEN* a **`User`** submits faulty credentials *OR* the **`Auth Service`** is unavailable, *WHEN* authentication is attempted, *THEN* the system rejects login and does not create an authenticated session.

- **[GS-009] Developer Authorization:** *GIVEN* an authenticated **`User`** attempts to publish a **`Game`**, *WHEN* the system checks role with the **`Auth Service`**, *THEN* the action is allowed only when the **`Developer`** role is present.

### **Gherkin Scenarios: Deep dive into Game Refund (US-006)**

- **[GS-004] Valid Refund:** *GIVEN* a **`Player`** owns the **`Game`** *AND* the **`Game`** was purchased less than 31 days ago, *AND* the recorded playtime is less than 120 minutes, *WHEN* they submit a refund request *THEN* the system revokes the **`Game`** access *AND* initiates a refund transaction via **`PaymentProcessing`**. *IF* the refund was not initiated successfully on the first request, *THEN* the system retries the initiation up to 3 additional times.

- **[GS-005] NOT Valid Refund 1:** *GIVEN* a **`Player`** owns the **`Game`** *AND* the **`Game`** was purchesed MORE then 31 days ago, *AND* the recorded playtime is LESS then 120 minutes, *WHEN* they submit a refund request *THEN* the system rejects the refund request *AND* displays *"Time limit exceeded"* error.

- **[GS-006] NOT Valid Refund 2:** *GIVEN* a **`Player`** owns the **`Game`** *AND* the **`Game`** was purchesed LESS then 31 days ago, *AND* the recorded playtime is MORE then 120 minutes, *WHEN* they submit a refund request *THEN* the system rejects the refund request *AND* displays *"Playtime limit exceeded"* error.

## **Traceability Matrix**

| Requirement ID   | Associated EARS Trigger            | User Story      | Gherkin | Use Case       | Coverage Status                |
| :---             | :---                               | :---            | :---    | :---           | :---                           |
| **REQ-001**      | Create Game record                 | US-001          | GS-001  | UC-003         | Covered                        |
| **REQ-002**      | Send request to PaymentProcessing  | US-004          | GS-002  | UC-001         | Covered                        |
| **REQ-003**      | Create ownership link              | US-004          | GS-002  | UC-001         | Covered                        |
| **REQ-004**      | Display average rating             | US-003          | -       | UC-005         | Partially Covered              |
| **REQ-005**      | Reject unowned game review         | US-005          | GS-003  | UC-004         | Covered                        |
| **REQ-006**      | Refund valid request               | US-006          | GS-004  | UC-006         | Covered                        |
| **REQ-007**      | Reject refund > 31 days            | US-006          | GS-005  | UC-006         | Covered                        |
| **REQ-008**      | Reject refund >= 120 mins          | US-006          | GS-006  | UC-006         | Covered                        |
| **REQ-009**      | Verify auth token                  | All US          | All GS  | All UC         | Cross-cutting (Applies to all) |
| **REQ-010**      | Authenticate login credentials     | US-007          | GS-007  | UC-007         | Covered                        |
| **REQ-011**      | Reject failed or unavailable auth  | US-007          | GS-008  | UC-007         | Covered                        |
| **REQ-012**      | Authorize Developer role           | US-001, US-007  | GS-009  | UC-003, UC-007 | Covered                        |

## **Out of scope**

- *Social* (friends, chats, etc.)
- *Game age restriction* (user age verification and game tags PG-13, R-18, etc.)
- *Family sharing* (Create a family of players and share games within family)
