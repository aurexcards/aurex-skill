# Commission & Markup

For resellers, integrators, and white-label partners.

---

## How It Works

1. Configure a markup % on your API key from the Aurex dashboard
2. When a deposit is created, your markup is added on top of the platform fee
3. Commission accumulates automatically in your account
4. Withdraw earnings to any supported blockchain wallet

---

## Fee Structure

Example deposit response with markup enabled:

```json
{
  "success": true,
  "data": {
    "depositId": "dep_abc123",
    "amount": 100,
    "currency": "SOL",
    "fees": {
      "platformFeePercent": 5,
      "platformFeeUsd": 5,
      "merchantCommissionPercent": 2,
      "merchantCommissionUsd": 2,
      "totalCollected": 7
    }
  }
}
```

| Field | Meaning |
|-------|---------|
| platformFeePercent | Base Aurex fee |
| platformFeeUsd | Aurex fee in USD |
| merchantCommissionPercent | Your configured markup |
| merchantCommissionUsd | Your earnings on this deposit |
| totalCollected | Total charged to user |

---

## Commission Lifecycle

```
accrued → available → paid
```

Commission becomes `available` after deposit confirmation.

---

## Earnings Tracking

Track in the Aurex dashboard:

- **Total Earned** — lifetime accumulated commission
- **Paid Out** — already withdrawn
- **Available** — ready to withdraw

Commission values update in real-time.

---

## Withdrawals

- Withdrawals processed on-chain
- May require network confirmation time
- Supported chains depend on account configuration

---

## Configuration

- Markup % is set per API key
- Different keys can have different commission rates
- Configure in: aurex.cash → Dashboard → API Keys

---

## Restrictions

- Commission applies to eligible deposit transactions only
- Card fees and swap fees may not generate commission unless explicitly enabled
- Commission rates may be limited by account tier
