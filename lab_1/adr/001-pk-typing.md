# ADR 001: The use of UUID's for primary keys

## **Context**

The need to choose identification type for entities in Pika-8 Hub.

## **Alternatives**

1. Autoincremental Integer
2. UUID

## **Decision**

Use of UUID for all entities.

## **Justification**

UUID allows clientside or 3rd-party key generation without Data Base locks. It also complicates enum attack compared to Autoincremental Integer.

## **Consequences**

Increased memory usage for IDs (128 bits for UUID instead of 32 bit).
