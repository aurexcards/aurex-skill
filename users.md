# Users & Deposits

## Create User

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

---

## Get User

```
GET /users/{userId}
```

Response:
```json
{
  "success": true,
  "data": {
    "userId": "507f1f77bcf86cd799439011",
    "firstName": "John",
    "lastName": "Doe",
    "createdAt": "2026-01-31T12:00:00Z"
  }
}
```

---

## Get Wallet Balance

```
GET /users/{userId}/wallet
```

Response:
```json
{
  "success": true,
  "data": {
    "userId": "507f1f77bcf86cd799439011",
    "balance": 150.50,
    "pendingBalance": 25.00,
    "inCardsBalance": 100.00,
    "currency": "USD"
  }
}
```

| Field | Meaning |
|-------|---------|
| balance | Available for card creation/top-up |
| pendingBalance | Awaiting blockchain confirmation |
| inCardsBalance | Already allocated to cards |

---

## Create Deposit

```
POST /users/{userId}/deposits
Content-Type: application/json

{
  "amount": 100,
  "currency": "USDC"   // SOL | USDT | USDC
}
```

Response:
```json
{
  "success": true,
  "data": {
    "depositId": "dep_abc123",
    "amount": 100,
    "netAmount": 95,
    "currency": "USDC",
    "depositAddress": "WALLET_ADDRESS_HERE",
    "status": "waiting",
    "fees": {
      "platformFeePercent": 5,
      "platformFeeUsd": 5,
      "merchantCommissionPercent": 0,
      "merchantCommissionUsd": 0,
      "totalCollected": 5
    },
    "expiresAt": "2026-01-31T13:00:00Z"
  }
}
```

Deposit statuses: `waiting` → `confirmed` | `failed` | `expired`

> Send crypto to `depositAddress`. Poll status until `confirmed` before proceeding.

---

## List Deposits

```
GET /users/{userId}/deposits?limit=10&offset=0
```

Response:
```json
{
  "success": true,
  "data": [
    {
      "depositId": "dep_abc123",
      "amount": 100,
      "currency": "USDC",
      "status": "confirmed",
      "createdAt": "2026-01-31T12:00:00Z"
    }
  ]
}
```
