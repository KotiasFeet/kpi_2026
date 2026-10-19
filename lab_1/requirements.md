# Requirements & Scenarios

## EARS Requirements

- **[REQ-001]** WHEN a **Developer** submits a new **Game** with a valid `title`, `description`, `base_price`, and `release_date`, the system SHALL create a new **Game** record associated with that **Developer**.
- **[REQ-002]** WHEN a **Player** requests to buy a **Game** they do not own, the system SHALL send a payment request to **PaymentGateway** before granting access.
- **[REQ-003]** WHEN **PaymentGateway** confirms a successful transaction for a **Game** purchase, the system SHALL create an ownership relation between the **User** and the **Game**.
- **[REQ-004]** WHILE a **Player** views a **Game** page, the system SHALL calculate and display the average **Review** rating across all reviews for that **Game**.
- **[REQ-005]** WHEN a **Player** submits a **Review** for a **Game** not present in their library, the system SHALL reject the submission and SHALL NOT create a **Review** record.
- **[REQ-006]** WHEN a **Player** requests a refund within 31 days of purchase and the total playtime is less than 120 minutes, the system SHALL revoke access to the **Game** and SHALL initiate a refund request through **PaymentGateway**.
- **[REQ-007]** WHEN a **Developer** changes the price of a published **Game**, the system SHALL retain the previous price in an audit record for history tracking.

## User Stories

- **[US-001]** As a **Developer**, I want to publish a **Game** so that it becomes available for purchase in the catalog.
- **[US-002]** As a **Developer**, I want to receive payment for each successful sale so that my revenue is tracked.
- **[US-003]** As a **Player**, I want to buy a **Game** so that it is added to my library.
- **[US-004]** As a **Player**, I want to read reviews for a **Game** so that I can evaluate the title before purchase.
- **[US-005]** As a **Player**, I want to leave a review for a **Game** I own so that my opinion is visible to other players.
- **[US-006]** As a **Player**, I want to request a refund under the refund policy so that I can recover money if the game does not meet my expectations.

## Gherkin Scenarios

- **[GC-001]** **Game Publication**  
  GIVEN a **User** is registered as a **Developer**  
  WHEN they submit a valid `title`, `description`, `base_price`, and `release_date`  
  THEN the system creates a new **Game** record linked to that **Developer**.

- **[GC-002]** **Game Purchase**  
  GIVEN a **Player** is viewing an unowned **Game** page  
  AND the selected **Game** is available for purchase  
  WHEN the player confirms the purchase and **PaymentGateway** approves the payment  
  THEN the system creates an ownership relation between the **User** and the **Game**.

- **[GC-003]** **Review Submission**  
  GIVEN a **Player** owns a **Game**  
  WHEN they submit a rating and comment for that **Game**  
  THEN the system creates a **Review** linked to the **User** and the **Game**.

- **[GC-004]** **Blocked Review for Unowned Game**  
  GIVEN a **Player** does not own a **Game**  
  WHEN they attempt to submit a review for that **Game**  
  THEN the system rejects the review and does not create a **Review** record.

- **[GC-005]** **Refund Request Within Policy**  
  GIVEN a **Player** has purchased a **Game** less than 31 days ago  
  AND the total playtime is less than 120 minutes  
  WHEN the player requests a refund  
  THEN the system revokes access to the **Game** and submits a refund request to **PaymentGateway**.

## Traceability Matrix

| Requirement ID | Related User Stories | Related Gherkin Scenarios | Related Use Cases | Coverage Status |
| :--- | :--- | :--- | :--- | :--- |
| **REQ-001** | US-001 | GC-001 | UC-003 | Covered |
| **REQ-002** | US-003 | GC-002 | UC-001, UC-002 | Covered |
| **REQ-003** | US-003 | GC-002 | UC-001, UC-002 | Covered |
| **REQ-004** | US-004 | GC-003, GC-005 | UC-005 | Covered |
| **REQ-005** | US-005 | GC-004 | UC-004 | Covered |
| **REQ-006** | US-006 | GC-005 | UC-006 | Covered |
| **REQ-007** | - | - | - | **UNCOVERED** – out of current sprint scope and not represented by a use case or scenario |

> This matrix deliberately represents many-to-many coverage: one requirement may map to multiple user stories and use cases, and one use case may support multiple requirements.

## Audit Notes

### EARS ambiguity audit

- **REQ-002** was rewritten to specify a clear trigger: “Player requests to buy an unowned Game,” and the output is explicit: “send payment request to PaymentGateway before granting access.”
- **REQ-005** was tightened to define the rejection condition using ownership state; the system must not create a review when the Game is not in the library.
- **REQ-006** was clarified with measurable conditions: “within 31 days” and “playtime less than 120 minutes.”

### Use-case / include-extend audit

- **UC-001 Purchase Game** includes **UC-002 Process Payment**.
- **UC-004 Leave Review** extends **UC-005 View Game Page** because the review action is only valid while the player is already viewing the game details.
- **UC-006 Request Refund** is modeled as a separate service path that depends on the purchase and ownership state.

### Actor and entity consistency audit

- **Player** and **Developer** are human actors; **PaymentGateway** is an external service actor.
- The ER model uses **User**, **Developer**, **Game**, and **Review** as domain entities. This matches the use-case and requirement vocabulary.
- The key alignment issue was the use of inconsistent names such as “tittle” and “Reweiv”; these were normalized to `title`, `Review`, and `base_price`.

### Top-3 divergences resolved

1. Requirement IDs were inconsistent with scenario IDs; now all artifacts align with `REQ-*`, `US-*`, and `GC-*` conventions.
2. The original traceability matrix was effectively 1:1 and did not reflect many-to-many relationships; the updated matrix maps multiple stories and use cases to the same requirement.
3. The original artifacts mixed actor roles with entity names; the updated version clearly distinguishes human actors from domain entities and external services.

## Use-Case Coverage Summary

- **Covered use cases:** UC-001, UC-002, UC-003, UC-004, UC-005, UC-006
- **Uncovered requirement:** REQ-007
- **Uncovered use case:** None in the current scope

