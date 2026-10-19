# ADR 001: The use of UUIDs for primary keys

## **Context**

The need to choose an identification type for entities in Pika-8 Hub.

## **Alternatives**

1. Auto-incremental integer
2. UUID

## **Decision**

Use UUIDs for all entities.

## **Justification**

UUIDs allow client-side or third-party key generation without database locks. They also complicate enumeration attacks compared to auto-incremental integers.

## **Consequences**

Increased memory usage for IDs (128 bits for a UUID instead of 32 bits).
