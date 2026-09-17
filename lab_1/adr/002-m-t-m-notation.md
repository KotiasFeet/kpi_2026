# ADR 002: Pure M:N Notation

## **Context**

The platform requires many Many-to-Many relationships (F.E.: between `User` and `Game`(the user's game library)).

## **Alternatives**

1. Model explicel junction tables (`Library`)
2. Pure M:N relashenship using direct declarative links (`}|..|{`) without junction tables.

## **Decision**

Use Pure M:N relashenship.

## **Justification**

There is no need for the Junction tables as it is a workaround for DataBases due to lack of native M:N support. We have no need for it as we do not forsee using attributes such as `purchase_date` or `time_played` that would be carried by the `Library`. Junction tables clutter the diagram and lower its readability.

## **Consequences**

When translating this model into an actual database, we'll need to implement the necessary junction tables.
