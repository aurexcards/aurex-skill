# Aurex API Skill

Teach your AI agent how to integrate with the Aurex API — virtual crypto-funded cards, user management, deposits, and swaps.

## What is Aurex?

Aurex is a platform that lets users spend cryptocurrency through virtual reloadable cards. Via the API, you can programmatically create users, fund wallets, issue cards, top up balances, and retrieve transaction history.

---

## Base URL

```
https://aurex.cash/api/dashboard
```

---

## Authentication

All requests require a Bearer token:

```
Authorization: Bearer YOUR_API_KEY
```

- API keys are generated from the Aurex dashboard
- Never expose keys in client-side code
- Each key has an independent rate limit: **60 requests/minute**
- Multiple keys can be created per account (useful for separating environments)

---

## Balance Model

Aurex uses a **two-layer balance system**:

| Layer | Purpose |
|-------|---------|
| Wallet balance | Holds deposited crypto funds |
| Card balance | Used for actual transactions |

Deposits → Wallet. Card creation/top-up → deducts from Wallet → adds to Card.

---

## Quick Start Flow

The typical integration flow is:

```
1. Create a user          POST /users
2. Create a deposit       POST /users/{userId}/deposits
3. Check wallet balance   GET  /users/{userId}/wallet
4. Issue a card           POST /users/{userId}/cards
5. Top up the card        POST /users/{userId}/cards/{cardId}/topup
6. Get card details       GET  /users/{userId}/cards/{cardId}
7. View transactions      GET  /users/{userId}/cards/{cardId}/transactions
```

---

## Users

### Create User
```
POST /users
Content-Type: application/json

{
  "firstName": "John",
  "lastName": "Doe"
}
```

Response:
```json
{
  "success": true,
  "data": {
    "userId": "507f1f77bcf86cd799439011",
    "firstName": "John",
    "lastName": "Doe"
  }
}
```

### Get User
```
GET /users/{userId}
```

### Get Wallet Balance
```
GET /users/{userId}/wallet
```

Response includes: `balance`, `pendingBalance`, `inCardsBalance`, `currency` (USD)

---

## Deposits

### Create Deposit
```
POST /users/{userId}/deposits
Content-Type: application/json

{
  "amount": 100,
  "currency": "SOL"   // SOL | USDT | USDC
}
```

Response includes: `depositAddress`, `status`, `fees`, `expiresAt`

Deposit statuses: `waiting` → `confirmed` | `failed` | `expired`

### List Deposits
```
GET /users/{userId}/deposits?limit=10&offset=0
```

---

## Cards

### Create Card
```
POST /users/{userId}/cards
Content-Type: application/json

{
  "cardName": "My Card",
  "initialBalance": 100    // min: 25 USD, max: 100,000 USD
}
```

Fee breakdown in response:
- `cardIssueFee` — fixed card issuance fee
- `ourFeeRate` — service fee percentage on allocated amount
- `totalFees` — total deducted from wallet = initialBalance + fees

### List Cards
```
GET /users/{userId}/cards
```

### Get Card Details
```
GET /users/{userId}/cards/{cardId}
```

Returns: `cardNumber`, `expiryDate`, `cvv`, `balance`, `status`

> Handle card details server-side only. Never expose in client-side code.

### Top Up Card
```
POST /users/{userId}/cards/{cardId}/topup
Content-Type: application/json

{
  "amount": 100    // min: 10 USD, max: 10,000 USD, service fee: 3%
}
```

### Card Transactions
```
GET /users/{userId}/cards/{cardId}/transactions
```

Transaction statuses: `pending`, `completed`, `declined`, `failed`

### Get OTP Code
```
GET /users/{userId}/otp?cardId={cardId}
```

Returns time-limited OTP (`expiresIn: 300` seconds). Use immediately.

### Card Status Values
- `active` — card is operational
- `frozen` — temporarily suspended
- `blocked` — permanently blocked
- `expired` — past expiry date

---

## Commission & Markup

Aurex supports a **partner markup system** for resellers and white-label integrators.

- Configure a markup % per API key from the dashboard
- Markup is added on top of the platform fee at deposit time
- Commission accumulates automatically and can be withdrawn on-chain

Fee structure in deposit response:
```json
{
  "fees": {
    "platformFeePercent": 5,
    "platformFeeUsd": 5,
    "merchantCommissionPercent": 2,
    "merchantCommissionUsd": 2,
    "totalCollected": 7
  }
}
```

Commission lifecycle: `accrued` → `available` → `paid`

---

## Error Handling

All errors follow this format:
```json
{
  "success": false,
  "error": "Error description"
}
```

| Status | Meaning |
|--------|---------|
| 400 | Invalid request parameters |
| 401 | Invalid or missing API key |
| 404 | Resource not found |
| 409 | Conflict or insufficient balance |
| 429 | Rate limit exceeded (60 req/min) |
| 500 | Internal server error |

---

## Important Notes

- No sandbox environment — test carefully with small amounts
- All timestamps in ISO 8601 UTC (`2026-01-31T12:00:00Z`)
- Pagination via `?limit=10&offset=0` on list endpoints
- API key provides full account access — treat like a password
- Rotate keys immediately if compromised
