# Full Examples

One of the difficult questions to answer for people new to a system like this is _how it will fit in my system?_

In this document, we aim to give full examples of how LNX can play a role in realistic systems.

## E-Wallet / Bank

This diagram assumes that the e-wallet operator also runs its own rates service.

## Deposit

```
sequenceDiagram
    Participant Customer
    Participant WalletCore
    Participant LNX
    Participant RatesService
    Customer->>WalletCore: Requests to deposit ₱180
    WalletCore->>LNX: How much BTC needs to be sold to get ₱180
    LNX->>RatesService: How much BTC needs to be sold to get ₱180?
    RatesService-->>LNX: 8,700 sats
    LNX-->>WalletCore: Returns invoice for 8,700 sats
    WalletCore-->>Customer: Show Lightning Invoice
    opt Invoice is paid
        Customer->>LNX: Pays invoice
        LNX-->>WalletCore: Invoice paid
        WalletCore-->>WalletCore: Add ₱180 to Customer account
        WalletCore-->>Customer: ₱180 deposit successful
    end
```

## Withdraw

```
sequenceDiagram
    Participant Customer
    Participant WalletCore
    Participant LNX
    Participant RatesService
    Customer->>WalletCore: Requests to pay invoice of 8,600 sats
    WalletCore->>LNX: Create an invoice ₱ -> 8,600 sats
    LNX->>RatesService: 8,600 sats is how much ₱?
    RatesService-->>LNX: 8,600 sats will cost you ₱180
    LNX-->>WalletCore: Returns quote
    alt Customer may withdraw ₱180
        WalletCore-->>Customer: Deduct ₱180?
        opt Customer approves
            Customer->>WalletCore: Yes
            WalletCore->>LNX: Create order from quote
            Note right of LNX: LNX sends sats out
            LNX-->>WalletCore: Order complete
            WalletCore-->>Customer: Withdrawal complete
        end
    else
        WalletCore-->>Customer: Not enough balance
    end
```

## Notes

The rates service will likely fetch order book information from the exchanges that the operator of LNX uses. Apart from Bitcoin rates, fiat rates must also be taken from other sources if the fiat currency used (such as in the example) is not available on the exchange.

There are other simplifications here:
1. LNX talks to a Lightning Node and instructs it to generate an invoice
2. The payment isn't made to LNX, thus LNX does not need to be accessed by the customer. The payment is actually made on the Lightning Network and the Lightning Node notifies LNX.

If the WalletCore wants to make an income, they may make those changes in the RatesService or in the fiat amount passed on to LNX or the customer.
