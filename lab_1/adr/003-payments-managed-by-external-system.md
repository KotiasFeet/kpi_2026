# ADR 003: Payments managed by external PaymentProcessing System

## **Context**

The platform requires financial transactions to enable players to buy and refund **`Game`**.

## **Alternatives**

1. In-House Ledger and Payment Gateway.
2. Payments managed by external PaymentProcessing System.

## **Decision**

Implement external PaymentProcessing System.

## **Justification**

Handling financial data natively introduces massive security risks and complex state management for transaction approvals. By offloading the payment logic to a dedicated external system the platform avoid legal and financial liabilities from prossesing raw payment details. This also allows the backend of our platform to be lightweight and focused on the core functionality: distributing and managing game access.

## **Consequences**

Core user flow(purchasing/refunding) become completle dependant on the external PaymentProcessing System. Backend of the platform must implement failure handling and asynchronous listeners to ensure that the Game ownership is only created when the transaction is successful.
