# 💡 Smart Follow-ups

> Generate contextual follow-up suggestions for your AI conversations

[![ClawdHub](https://img.shields.io/badge/ClawdHub-smart--followups-22c55e?style=flat-square)](https://clawdhub.com/skills/smart-followups)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-orange?style=flat-square)](CHANGELOG.md)

After every AI response, get **3 smart suggestions** for what to ask next:

- ⚡ **Quick** — Clarifications and immediate questions
- 🧠 **Deep Dive** — Technical depth and detailed exploration
- 🔗 **Related** — Connected topics and broader context

**Telegram/Discord/Slack:** Clickable inline buttons  
**Signal/iMessage/SMS:** Numbered text list

---

## ✨ Features

- **🎯 Context-Aware** — Analyzes your last 1-3 exchanges
- **🔘 Interactive Buttons** — One tap to ask (Telegram, Discord, Slack)
- **📝 Text Fallback** — Numbered lists for channels without buttons
- **⚡ Fast** — ~2 second generation time
- **🔐 Privacy-First** — Uses your existing Clawdbot auth by default
- **🔧 Flexible** — Multiple provider options (see below)

---

## 🚀 Quick Start

### Installation

```bash
# Via ClawdHub (recommended)
clawdhub install smart-followups

# Or manually
cd /path/to/clawdbot/skills
git clone https://github.com/robbyczgw-cla/smart-followups
cd smart-followups
npm install
```

### Usage

Just type `/followups` in any Clawdbot conversation:

```
You: What is Docker?
Bot: Docker is a containerization platform that...

You: /followups

Bot: 💡 What would you like to explore next?
[⚡ How do I install Docker?]
[🧠 Explain container architecture]
[🔗 Docker vs Kubernetes?]
```

Click any button → sends that question automatically!

---

## 🔐 Authentication Options

### Option 1: Clawdbot Native (Default) ⭐

**Uses your existing Clawdbot authentication** — same model and login as your current chat.

- ✅ No additional API keys needed
- ✅ Uses your current session's model (Haiku/Sonnet/Opus)
- ✅ Works out of the box

```json
{
  "skills": {
    "smart-followups": {
      "provider": "clawdbot"
    }
  }
}
```

### Option 2: OpenRouter

Use OpenRouter for model access. Requires API key.

```json
{
  "skills": {
    "smart-followups": {
      "provider": "openrouter",
      "apiKey": "${OPENROUTER_API_KEY}",
      "model": "anthropic/claude-sonnet-4.5"
    }
  }
}
```

**Get an OpenRouter API key:** [openrouter.ai/keys](https://openrouter.ai/keys)

### Option 3: Direct Anthropic

Use Anthropic's API directly. Requires API key.

```json
{
  "skills": {
    "smart-followups": {
      "provider": "anthropic",
      "apiKey": "${ANTHROPIC_API_KEY}",
      "model": "claude-sonnet-4-5"
    }
  }
}
```

**Get an Anthropic API key:** [console.anthropic.com](https://console.anthropic.com/)

---

## ⚙️ Configuration

Add to your `clawdbot.json`:

```json
{
  "skills": {
    "smart-followups": {
      "enabled": true,
      "provider": "clawdbot",
      "model": null,
      "autoTrigger": false
    }
  }
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `enabled` | `true` | Enable/disable the skill |
| `provider` | `"clawdbot"` | Auth provider: `clawdbot`, `openrouter`, `anthropic` |
| `model` | `null` | Model override (null = inherit from session) |
| `apiKey` | — | API key for openrouter/anthropic providers |
| `autoTrigger` | `false` | Auto-show follow-ups after every response |

---

## 📱 Channel Support

Works on **every Clawdbot channel** with adaptive formatting:

| Channel | Mode | Interaction |
|---------|------|-------------|
| **Telegram** | Inline buttons | Tap to ask |
| **Discord** | Inline buttons | Click to ask |
| **Slack** | Inline buttons | Click to ask |
| **Signal** | Text list | Reply 1, 2, or 3 |
| **WhatsApp** | Text list | Reply 1, 2, or 3 |
| **iMessage** | Text list | Reply 1, 2, or 3 |
| **SMS** | Text list | Reply 1, 2, or 3 |
| **Matrix** | Text list | Reply 1, 2, or 3 |
| **Email** | Text list | Reply with number |

📖 See [CHANNELS.md](CHANNELS.md) for detailed channel-specific documentation.

---

## 🛠️ CLI Tool (Optional)

A standalone CLI is included for testing and scripting:

```bash
# Set API key (OpenRouter or Anthropic)
export OPENROUTER_API_KEY="sk-or-..."

# Generate follow-ups from JSON context
echo '[{"user":"What is Docker?","assistant":"Docker is..."}]' | \
  followups-cli --mode text

# Output modes: json, telegram, text, compact
followups-cli --mode telegram < context.json
```

See `followups-cli --help` for all options.

---

## 📖 Examples

### Telegram Buttons

```
💡 What would you like to explore next?

[⚡ How do I install Docker?        ]
[🧠 Explain Docker's architecture   ]
[🔗 Compare Docker to Kubernetes    ]
```

### Signal Text Mode

```
💡 Smart Follow-up Suggestions

⚡ Quick
1. How do I install Docker?

🧠 Deep Dive
2. Explain Docker's architecture

🔗 Related
3. Compare Docker to Kubernetes

Reply with 1, 2, or 3 to ask that question.
```

---

## ❓ FAQ

### Why 3 suggestions instead of 6?

Cleaner UX, especially on mobile. Each category (Quick, Deep, Related) gets one focused suggestion instead of overwhelming you with options.

### Can I use this without Clawdbot?

Yes! The CLI tool works standalone with OpenRouter or Anthropic API keys. But the best experience is integrated with Clawdbot.

### How does it know what to suggest?

The skill analyzes your last 1-3 message exchanges and generates contextually relevant questions across three categories: quick clarifications, deep technical dives, and related topics.

### Will it work with my custom model?

Yes! With `provider: "clawdbot"` (default), it uses whatever model your current chat is using. With other providers, specify the model in config.

### Is my conversation data sent anywhere?

**With Clawdbot native:** Same privacy as your normal chat — processed by your configured AI provider.

**With OpenRouter/Anthropic:** Your recent exchanges are sent to generate suggestions. See their respective privacy policies.

### How much does it cost?

- **Clawdbot native:** Uses your existing chat's API usage
- **OpenRouter/Anthropic:** ~$0.001-0.01 per generation depending on model

---

## 🏗️ Project Structure

```
smart-followups/
├── cli/
│   └── followups-cli.js    # Standalone CLI tool
├── handler.js              # Clawdbot command handler
├── package.json
├── README.md               # This file
├── SKILL.md                # Clawdbot skill manifest
├── FAQ.md                  # Frequently asked questions
├── INTERNAL.md             # Development notes
├── CHANGELOG.md            # Version history
└── LICENSE                 # MIT License
```

---

## 🤝 Contributing

Contributions welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) first.

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test across multiple channels
5. Submit a pull request

---

## 📄 License

MIT © [Robby](https://github.com/robbyczgw-cla)

---

## 🙏 Credits

- Inspired by [Chameleon AI Chat](https://github.com/robbyczgw-cla/Chameleon-AI-Chat)'s smart follow-up feature
- Built for the [Clawdbot](https://clawdbot.com) ecosystem
- Powered by Claude

---

**Made with 🦎 by the Clawdbot community**
