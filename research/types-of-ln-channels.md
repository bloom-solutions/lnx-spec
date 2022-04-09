- Hosted
  - Someone manages channels for you or you share channels with others
  - Shared liquidity
  - Two types
    1. Fully custodial
      - Entry in their database
    2. Mostly custodial
      - 100% compatible with lightning channels
      - You build your own invoices
      - Cryptographically secure: you can't be lied to by the host
      - Only thing custodian can do is run away with your funds but they can't lie to you
- Self-custodial
  - Your own node, whether on phone or device at home
- Hedged
  - Can only be applied to hosted channels (https://devpost.com/software/standard-sats)
  - Host essentially buys the fiat currency when you receive bitcoin, sells the fiat currency when you send bitcoin
- Stabilized
  - Only makes sense to do so in self-custodial
  - Sats in the channel:
    - increases when the asset value it represents goes down in value vs bitcoin
    - decreases when the asset value it represents goes up in value vs bitcoin
  - https://hrf.org/strike-hrf-bounty

| Example           | Hosted | Self-custodial | Hedged (Native) | Hedged (Non-native) | Stabilized |
| -------           | ------ | -------------- | ------          | ------------        | ---------- |
| Your own node     |        | √              |                 |                     |            |
| Muun              | √      | √              |                 |                     |            |
| Bluewallet (LN)   | √      | √              |                 |                     |            |
| Phoenix           |        | √              |                 |                     |            |
| Wallet of Satoshi | √      |                |                 |                     |            |
| Strike            | √      |                |                 | √                   |            |
| CashApp           | √      |                |                 | √                   |            |
| Standard Sats     | √      |                | √               |                     |            |
| HRF+Strike grant  |        | √              |                 |                     | √          |
