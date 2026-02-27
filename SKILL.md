# Aurex API Skill

Build on the Aurex API — virtual crypto-funded cards, user management, wallet deposits, and commission infrastructure.

## Decision Tree

What are you trying to do?

- **Create a user and fund their wallet** → See [Users & Deposits](references/users.md)
- **Issue a virtual card** → See [Cards](references/cards.md)
- **Top up a card or view transactions** → See [Cards](references/cards.md)
- **Set up partner markup / earn commission** → See [Commission](references/commission.md)
- **Handle errors or rate limits** → See Error Handling below

---

## Base URL

```
https://aurex.cash/api/dashboard
```

---

## Authentication

```
Authorization: Bearer YOUR_API_KEY
```

- Get your API key at aurex.cash → Dashboard → API Keys
- Never expose in client-side code
- Rate limit: **60 requests/minute** per key
- Multiple keys supported per account

---

## Balance Model

Aurex uses a **two-layer system**:

| Layer | Purpose |
|-------|---------|
| Wallet balance | Holds deposited crypto (SOL, USDT, USDC) |
| Card balance | Used for card transactions |

Flow: Deposit → Wallet → Card creation/top-up → Card balance

---

## Quick Start (3 steps to a working card)

**Step 1 — Create a user**
```
POST /users
{ "firstName": "John", "lastName": "Doe" }
→ save userId
```

**Step 2 — Fund their wallet**
```
POST /users/{userId}/deposits
{ "amount": 100, "currency": "USDC" }
→ send crypto to depositAddress, wait for status: confirmed
```

**Step 3 — Issue a card**
```
POST /users/{userId}/cards
{ "cardName": "My Card", "initialBalance": 50 }
→ card is ready
```

---

## Error Handling

All errors:
```json
{ "success": false, "error": "description" }
```

| Code | Meaning |
|------|---------|
| 400 | Invalid parameters |
| 401 | Bad or missing API key |
| 404 | Resource not found |
| 409 | Insufficient balance |
| 429 | Rate limit exceeded |
| 500 | Server error |

---

## Important Rules

- No sandbox environment — always test with small amounts first
- Card details (number, CVV) must be handled **server-side only**
- All timestamps in ISO 8601 UTC
- Pagination via `?limit=10&offset=0` on list endpoints
- Amounts always in USD unless a `currency` field is present
