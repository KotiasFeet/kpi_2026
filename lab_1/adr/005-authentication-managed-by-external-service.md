# ADR 005: Authentication managed by an external Auth Service

## **Context**

Pika-8 Hub requires authentication and authorization for protected actions such as purchasing, refunding, publishing games, and leaving reviews.

## **Alternatives**

### **1. In-House Authentication**

- *Pros:* Full control over login flows and stored identity data.
- *Cons:* All fault is on us, when the database is breached.

### **2. Authentication managed by an external Auth Service**

- *Pros:* Centralizes credential verification and identity data; provides established token and authorization mechanisms; reduces password-handling risk; can consistently supply roles or authorization claims to the application.
- *Cons:* Adds runtime dependency on a third-party service; requires secure token validation, failure handling, and availability strategies; introduces integration and contract-management complexity.

## **Decision**

Use an external **`Auth Service`** for user authentication and authorization.

## **Justification**

Deicated **`Auth Service`** provides stronger protection against fraud and reduces the security burden alongside implementing credit storage and login protocols.

## **Consequences**

Depandance on **`Auth Service`** availability, credibility and response correctness.
