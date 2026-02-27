# Aurex API Skill

The official AI agent skill for integrating with the [Aurex](https://aurex.cash) API.

Teaches Claude Code, Cursor, Windsurf, Codex, and Gemini CLI how to build on top of Aurex — virtual crypto-funded cards, user management, deposits, and wallet infrastructure.

---

## What it does

One install. Your AI agent knows how to:

- **Create and manage users** via the Aurex API
- **Handle crypto deposits** in SOL, USDT, and USDC
- **Issue virtual cards** with custom names and balances
- **Top up cards** and retrieve full transaction history
- **Retrieve card details** including card number, CVV, and OTP codes
- **Apply partner markup** and track commission earnings

---

## Install

### Claude Code (Plugin Marketplace)
```
/plugin marketplace add aurex-cash/aurex-skill
```

### Claude Code (Manual)
```bash
git clone https://github.com/aurex-cash/aurex-skill.git
cp -r aurex-skill/aurex-api ~/.claude/skills/
```

### Codex CLI
```bash
git clone https://github.com/aurex-cash/aurex-skill.git
cp -r aurex-skill/aurex-api ~/.codex/skills/
```

### Cursor / Windsurf
```bash
cp aurex-skill/.cursorrules ./
```

### Per-project (shared via git)
```bash
cp -r aurex-skill/aurex-api .claude/skills/
```

---

## What's inside

| File | Purpose |
|------|---------|
| `SKILL.md` | Main skill — full API reference, quick start flow, examples |
| `references/users.md` | Users, wallets, and deposits |
| `references/cards.md` | Card creation, top-up, transactions, OTP |
| `references/commission.md` | Partner markup and commission system |
| `.cursorrules` | Drop-in rules file for Cursor/Windsurf |

---

## Example prompts after installing

Once installed, you can say to your AI agent:

> "Create a new Aurex user and fund their wallet with 100 USDC"

> "Issue a virtual card for user {userId} with a $50 balance"

> "Get the transaction history for card {cardId}"

> "Top up card {cardId} with $25"

> "Show me the wallet balance for all users"

---

## Aurex API

- **Base URL:** `https://aurex.cash/api/dashboard`
- **Auth:** Bearer token (`Authorization: Bearer YOUR_API_KEY`)
- **Rate limit:** 60 requests/minute per API key
- **Supported deposit currencies:** SOL, USDT, USDC
- **Docs:** [docs.aurex.cash](https://docs.aurex.cash)

Get your API key at [aurex.cash](https://aurex.cash) → Dashboard → API Keys.

---

## Compatibility

| Agent | Install method |
|-------|---------------|
| Claude Code | `/plugin marketplace add` or `~/.claude/skills/` |
| Codex CLI | `~/.codex/skills/` |
| Cursor | `.cursorrules` in project root |
| Windsurf | `.cursorrules` in project root |
| Gemini CLI | `.claude/skills/` (same format) |

---

## Built by

[Aurex](https://aurex.cash) — Virtual crypto-funded cards for everyday payments.

## License

MIT
