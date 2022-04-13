# Overview

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

# Fetching backend info

In order for the LN app to know what endpoints to hit, they go to `/.well-known/lnx.yml`. This file contains all necessary information to interact with the server, such as:

- authorization
- kyc, if needed
- where to create quotes
- get asset list and balances
