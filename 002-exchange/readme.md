This document describes a business'/exchange's interactions with the LNX server. The two main functions are cash-in and cash-out.

## LNX's responsibilities

The LNX server is in charge of:

- knowing exchange rates
- setting timeouts for generated invoices to minimize volatility risk
- generating invoices given a currency and amount
- making Bitcoin/LN payments given a currency and amount
- detecting invoices paid (in and out) and notifying given webhook url

Below are diagrams for cashing in and out from an institution's point of view. LNX itself is composed of multiple software modules. We will dive into that detail in a later section.

## Cash-in

```mermaid
sequenceDiagram
    Participant Customer
    Participant Core
    Participant LNX
    Customer->>Core: Provides LN invoice
    Core->>LNX: Gets rate
    LNX-->>Core: Returns rate
    alt Enough funds
        Core->>LNX: Pay invoice
        alt Success
            LNX-->>Core: Webhook
            Core-->>Customer: Money sent
        else Failed
            LNX-->>Core: Webhook
            Core-->>Customer: Failed
        end
    else Lacks funds
        Core-->>Customer: Not enough funds
    end
```

## Cash-out

```mermaid
sequenceDiagram
    Participant Customer
    Participant Core
    Participant LNX
    Customer->>Core: Provides LN invoice
    Core->>LNX: Gets rate
    LNX-->>Core: Returns rate
    alt Enough funds
        Core->>LNX: Pay invoice
        alt Success
            LNX-->>Core: Webhook
            Core-->>Customer: Money sent
        else Failed
            LNX-->>Core: Webhook
            Core-->Customer: Failed
        end
    else Lacks funds
        Core-->>Customer: Not enough funds
    end
```

# LNX Internals

As mentioned above, LNX is just a bunch of modules / services that allow it to buy and sell some asset against Bitcoin. 

Here you'll find an example of the Customer (not shown), through Core, requesting for a Lightning Invoice in $ terms. LNX needs rates to make the right quote, because fundamentally LNX is an exchange, even if it uses other exchanges downstream to fulfill orders.

```mermaid
sequenceDiagram
    Participant Core
    Participant LNX
    Participant Rates
    Participant Rate sources
    Core->>LNX: Requests for LN invoice for 10$
    LNX->>Rates: Get quote: amunt bitcoin to sell for $10
    Rates->Rate sources: Fetch new rate data
    Rates-->>LNX: Return quote
    LNX-->>Core: Return invoice
```

These "Rate sources" could be configured to talk to Core again if the Core app already has that information.
