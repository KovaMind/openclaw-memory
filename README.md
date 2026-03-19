# @kovamind/openclaw-memory

OpenClaw memory plugin — use [Kova Mind](https://kovamind.ai) as your agent's memory backend.

Replaces OpenClaw's local file-based memory with Kova Mind's cloud API. Your agent gets persistent, learning memory with pattern extraction, surprise scoring, and reinforcement — no local embeddings or SQLite needed.

## Install

```bash
openclaw plugins install @kovamind/openclaw-memory
```

## Configure

Set the memory slot to use Kova Mind:

```bash
openclaw config set plugins.slots.memory memory-kovamind
```

Configure your API key and user ID:

```bash
openclaw config set plugins.entries.memory-kovamind.apiKey "km_live_xxx"
openclaw config set plugins.entries.memory-kovamind.userId "axiom"
```

Or add to your `openclaw.json`:

```json
{
  "plugins": {
    "slots": {
      "memory": "memory-kovamind"
    },
    "entries": {
      "memory-kovamind": {
        "apiKey": "km_live_xxx",
        "userId": "axiom",
        "autoRecall": true,
        "autoCapture": true
      }
    }
  }
}
```

Restart the gateway:

```bash
openclaw gateway restart
```

## What It Does

| Feature | How |
|---------|-----|
| **Auto-Recall** | Before each agent turn, searches Kova Mind for relevant memories and injects them into context |
| **Auto-Capture** | After each turn, extracts patterns from user messages and stores them in Kova Mind |
| **memory_recall** | Agent tool — search memories by natural language query |
| **memory_store** | Agent tool — extract and store patterns from text |
| **memory_forget** | Agent tool — deny a stored pattern (GDPR-friendly) |
| **memory_surprise** | Agent tool — score how novel content is vs existing memory |
| **memory_reinforce** | Agent tool — confirm or strengthen a validated pattern |

## Config Options

| Option | Default | Description |
|--------|---------|-------------|
| `apiKey` | required | Your `km_live_xxx` API key |
| `userId` | required | Default user ID for memory operations |
| `apiUrl` | `https://api.kovamind.ai` | API base URL |
| `autoRecall` | `true` | Inject memories before each turn |
| `autoCapture` | `true` | Extract patterns after each turn |
| `maxRecallPatterns` | `5` | Max memories injected per turn |

## CLI

```bash
openclaw kovamind status              # Check API health
openclaw kovamind search "dark mode"  # Search memories
openclaw kovamind surprise "new info" # Score novelty
```

## How It Compares

| Feature | memory-core | memory-lancedb | memory-kovamind |
|---------|-------------|----------------|-----------------|
| Storage | Local SQLite + files | Local LanceDB | Cloud API |
| Embeddings | Local/OpenAI/Gemini | OpenAI | Server-side (zero local) |
| Pattern extraction | None (raw text) | Rule-based triggers | LLM-powered |
| Surprise scoring | No | No | Yes |
| Reinforcement | No | No | Yes (confirm/deny/strengthen/weaken) |
| Cross-device | No | No | Yes |
| Dependencies | None | LanceDB + OpenAI | None (just HTTP) |

## Get an API key

Sign up at [kovamind.ai](https://kovamind.ai).

## License

MIT
