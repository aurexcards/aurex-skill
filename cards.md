# Cards

All card endpoints are prefixed with `/users/{userId}/cards`

---

## Create Card

```
POST /users/{userId}/cards
Content-Type: application/json

{
  "cardName": "Marketing Card",
  "initialBalance": 100    // min: 25 USD, max: 100,000 USD
}
```

Response:
```json
{
  "success": true,
  "data": {
    "cardId": "card_abc123",
    "cardName": "Marketing Card",
    "balance": 100,
    "cardNumber": "4111 1111 1111 1234",
    "expiryDate": "12/28",
    "cvv": "123",
    "status": "active",
    "fees": {
      "depositAmount": 124,
      "ourFee": 5,
      "ourFeeRate": 0.05,
      "cardIssueFee": 19,
      "totalFees": 24,
      "cardBalance": 100
    }
  }
}
```

> Total deducted from wallet = initialBalance + service fee + issuance fee

---

## List Cards

```
GET /users/{userId}/cards
```

Response:
```json
{
  "success": true,
  "data": [
    {
      "cardId": "card_abc123",
      "cardName": "Marketing Card",
      "balance": 100,
      "status": "active"
    }
  ]
}
```

---

## Get Card Details

```
GET /users/{userId}/cards/{cardId}
```

Response:
```json
{
  "success": true,
  "data": {
    "cardId": "card_abc123",
    "cardName": "Marketing Card",
    "cardNumber": "4111 1111 1111 1234",
    "expiryDate": "12/28",
    "cvv": "123",
    "balance": 100,
    "status": "active"
  }
}
```

> **Security:** Never return card number or CVV to client-side code.

---

## Top Up Card

```
POST /users/{userId}/cards/{cardId}/topup
Content-Type: application/json

{
  "amount": 100    // min: 10 USD, max: 10,000 USD
}
```

Fee: **3% service fee** deducted from wallet.

Response:
```json
{
  "success": true,
  "data": {
    "cardId": "card_abc123",
    "newBalance": 200,
    "rechargeAmount": 100,
    "fees": 3
  }
}
```

---

## Card Transactions

```
GET /users/{userId}/cards/{cardId}/transactions
```

Response:
```json
{
  "success": true,
  "data": [
    {
      "transactionId": "txn_123",
      "amount": 25.50,
      "currency": "USD",
      "status": "completed",
      "createdAt": "2026-01-31T12:00:00Z"
    }
  ]
}
```

Transaction statuses: `pending`, `completed`, `declined`, `failed`

---

## Get OTP Code

```
GET /users/{userId}/otp?cardId={cardId}
```

Response:
```json
{
  "success": true,
  "data": {
    "otp": "123456",
    "expiresIn": 300,
    "cardId": "card_abc123"
  }
}
```

> OTP expires in 300 seconds. Use immediately after retrieval.

---

## Card Status Values

| Status | Meaning |
|--------|---------|
| active | Card is operational |
| frozen | Temporarily suspended |
| blocked | Permanently blocked |
| expired | Past expiry date |
