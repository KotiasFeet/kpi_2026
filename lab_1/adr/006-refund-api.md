# ADR 006: Dedicated Refund API

## **Context**

Pika-8 Hub must process game refund requests.

## **Alternatives**

### **1. Refund logic inside the monolithic web backend**

- *Pros:* Fewer HTTP boundaries and simpler local deployment.
- *Cons:* Refund orchestration, authentication checks, payment callbacks, retries, and library updates become tightly coupled with unrelated application features.

### **2. Dedicated Refund API**

- *Pros:* Isolates refund policy and workflow; easy control over refund policy
- *Cons:* Adds a network boundary, deployment component, and monitoring.

### **3. Let Refund API be fully managed by PaymentProcessing**

- *Pros:* Least custom payment integration code.
- *Cons:* The external provider would also need purchase history and playtime data and would be responsible for application-specific policy and library access.

## **Decision**

Implement a dedicated internal **`Refund API`** service for refund.

## **Justification**

Refund processing has a distinct stateful workflow and cross-system coordination. A dedicated API keeps policy validation in one component while preserving **`Auth Service`** and **`PaymentProcessing`** as external dependencies.

## **Consequences**

**`Refund API`** must validate the token with **`Auth Service`** before processing a request. It reads purchase and playtime data from **`Transaction`** and **`LibraryRecord`** through library/application services. The service requires retry limits, idempotency, timeout handling, and monitoring.
