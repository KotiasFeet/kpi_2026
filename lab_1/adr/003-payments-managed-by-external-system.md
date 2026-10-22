# ADR 003: Payments managed by external PaymentProcessing System

## **Context**

The platform requires financial transactions to enable players to buy and refund **Game**.

## **Alternatives**

### **1. In-House Ledger and Payment Gateway**

- *Pros:* Full control over the user experience; no third-party transaction fees.
- *Cons:* Enormous PCI-DSS compliance overhead; severe legal and financial liability; complex state management required for handling raw card data.

### **2. Payments managed by external PaymentProcessing System**

- *Pros:* Offloads legal/financial liability; tokenized security out-of-the-box; faster development time.
- *Cons:* Dependency on a third-party service uptime; integration complexity for asynchronous webhooks.

## **Decision**

Implement external PaymentProcessing System.

## **Justification**

Handling financial data natively introduces massive security risks. By offloading the payment logic, we avoid liabilities from processing raw payment details. 

## **Consequences**

Core user flow becomes dependent on the external PaymentProcessing System. The backend must implement failure handling and asynchronous listeners to ensure Game ownership is only created when the transaction is successful.