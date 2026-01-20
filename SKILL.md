---
name: smart-followups
description: Generate contextual follow-up suggestions after AI responses. Adds a /followups command that shows 3 clickable buttons (Quick, Deep Dive, Related).
commands:
  - name: followups
    aliases: [fu, next, suggest]
    description: Generate 3 smart follow-up suggestions based on recent conversation
channels:
  - telegram
  - discord
  - slack
  - signal
  - whatsapp
  - imessage
  - sms
  - matrix
  - email
---

# Smart Follow-ups Skill

Generate contextual follow-up suggestions for Clawdbot conversations.

## Commands

| Command | Description |
|---------|-------------|
| `/followups` | Generate 3 follow-up suggestions |
| `/fu` | Shortcut for /followups |
| `/next` | Alias for /followups |
| `/suggest` | Alias for /followups |

## Usage

Type `/followups` (or `/fu`) in any conversation:

```
You: What is Docker?
Bot: Docker is a containerization platform...

You: /followups

Bot: 💡 What would you like to explore next?
[⚡ How do I install Docker?]
[🧠 Explain container architecture]
[🔗 Docker vs Kubernetes?]
```

**On button channels (Telegram/Discord/Slack):** Tap a button to ask that question.

**On text channels (Signal/WhatsApp/iMessage/SMS):** Reply with 1, 2, or 3.

## Categories

Each generation produces 3 suggestions:

| Category | Emoji | Purpose |
|----------|-------|---------|
| **Quick** | ⚡ | Clarifications, definitions, immediate next steps |
| **Deep Dive** | 🧠 | Technical depth, advanced concepts, thorough exploration |
| **Related** | 🔗 | Connected topics, broader context, alternatives |

## Authentication

**Default:** Uses Clawdbot's existing auth — same login and model as your current chat.

**Optional providers:**
- `openrouter` — Requires `OPENROUTER_API_KEY`
- `anthropic` — Requires `ANTHROPIC_API_KEY`

## Configuration

```json
{
  "skills": {
    "smart-followups": {
      "enabled": true,
      "provider": "clawdbot",
      "model": null
    }
  }
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `provider` | `"clawdbot"` | Auth provider: `clawdbot`, `openrouter`, `anthropic` |
| `model` | `null` | Model override (null = inherit from session) |
| `apiKey` | — | API key for non-clawdbot providers |

## Channel Support

| Channel | Mode | Interaction |
|---------|------|-------------|
| Telegram | Buttons | Tap to ask |
| Discord | Buttons | Click to ask |
| Slack | Buttons | Click to ask |
| Signal | Text | Reply 1-3 |
| WhatsApp | Text | Reply 1-3 |
| iMessage | Text | Reply 1-3 |
| SMS | Text | Reply 1-3 |
| Matrix | Text | Reply 1-3 |
| Email | Text | Reply with number |

See [CHANNELS.md](CHANNELS.md) for detailed channel documentation.

## How It Works

1. User types `/followups`
2. Handler captures recent conversation context
3. Clawdbot generates 3 contextual questions (using current model/auth)
4. Formatted as buttons or text based on channel
5. User clicks button or replies with number
6. Clawdbot answers that question

## Files

| File | Purpose |
|------|---------|
| `handler.js` | Command handler and channel formatting |
| `cli/followups-cli.js` | Standalone CLI for testing/scripting |
| `README.md` | Full documentation |
| `CHANNELS.md` | Channel-specific guide |
| `FAQ.md` | Common questions |

## Credits

Inspired by [Chameleon AI Chat](https://github.com/robbyczgw-cla/Chameleon-AI-Chat)'s smart follow-up feature.
