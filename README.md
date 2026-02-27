# Aurex API Skill

The official AI agent skill for the [Aurex](https://aurex.cash) API.

Teaches Claude Code, Cursor, Windsurf, Codex, and Gemini CLI how to build on Aurex — virtual crypto-funded cards, user management, deposits, and white-label infrastructure.

## What it does

One install. Your AI agent knows how to:

- **Create and manage users** via the Aurex API
- **Handle crypto deposits** in SOL, USDT, and USDC
- **Issue virtual cards** with custom names and balances
- **Top up cards** and retrieve full transaction history
- **Get card details** including card number, CVV, and OTP codes
- **Apply partner markup** and track commission earnings

## Install

### Claude Code (Plugin Marketplace)
```
/plugin marketplace add aurexcards/aurex-skill
```

### Claude Code (Manual)
```bash
git clone https://github.com/aurexcards/aurex-skill.git
cp -r aurex-skill/aurex-api ~/.claude/skills/
```

### Codex CLI
```bash
git clone https://github.com/aurexcards/aurex-skill.git
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

## What's inside

| File | Purpose |
|------|---------|
| `aurex-api/SKILL.md` | Main skill — decision tree, quick start, error handling |
| `aurex-api/references/users.md` | Users, wallets, and deposits |
| `aurex-api/references/cards.md` | Card creation, top-up, transactions, OTP |
| `aurex-api/references/commission.md` | Partner markup and commission system |
| `.cursorrules` | Drop-in rules for Cursor/Windsurf |

## Example prompts after installing

```
"Create a new Aurex user and fund their wallet with 100 USDC"
"Issue a virtual card for user {userId} with a $50 balance"
"Top up card {cardId} with $25"
"Get the full transaction history for card {cardId}"
"Show me the wallet balance for user {userId}"
"Set up partner markup on my API key"
```

## Aurex API

- **Base URL:** `https://aurex.cash/api/dashboard`
- **Auth:** `Authorization: Bearer YOUR_API_KEY`
- **Rate limit:** 60 requests/minute per API key
- **Deposit currencies:** SOL, USDT, USDC
- **Docs:** [docs.aurex.cash](https://docs.aurex.cash)

Get your API key at [aurex.cash](https://aurex.cash) → Dashboard → API Keys.

## Compatibility

| Agent | Install method |
|-------|---------------|
| Claude Code | `/plugin marketplace add` or `~/.claude/skills/` |
| Codex CLI | `~/.codex/skills/` |
| Cursor | `.cursorrules` in project root |
| Windsurf | `.cursorrules` in project root |
| Gemini CLI | `.claude/skills/` (same format) |

## Built by

[Aurex](https://aurex.cash) — Virtual crypto-funded cards for everyday payments.

## License

MIT
