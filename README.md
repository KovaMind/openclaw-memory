# @kovamind/openclaw-memory

OpenClaw memory plugin — use [Kova Mind](https://kovamind.io) as your agent's memory backend.

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
| `apiUrl` | `https://api.kovamind.io` | API base URL |
| `autoRecall` | `true` | Inject memories before each turn |
| `autoCapture` | `true` | Extract patterns after each turn |
| `maxRecallPatterns` | `5` | Max memories injected per turn |

## CLI

```bash
openclaw kovamind status              # Check API health
openclaw kovamind search "dark mode"  # Search memories
openclaw kovamind surprise "new info" # Score novelty
```

## Errors

| Status | Meaning | What to do |
|--------|---------|------------|
| `401` | Missing or invalid API key | Check your `km_live_xxx` key. The key is sent as `Authorization: Bearer <key>` — `X-API-Key` is not accepted. |
| `403` `{"detail":"API key is bound to a different agent identity"}` | The API key is **bound** server-side to a single `user_id`, and your request's `userId` does not match it. | Use the `userId` the key was bound to, or use an unbound key. See below. |

### Bound API keys (`403`)

An API key can be **bound** server-side to a single agent identity (`user_id`).
This is enforced by the Kova Mind API, not by this plugin:

- **Bound key** — every request must use the key's bound `user_id`. If the
  configured `userId` differs, the API rejects the request with
  `403 {"detail":"API key is bound to a different agent identity"}`. The plugin
  surfaces this as `Kova Mind API 403: ...`; auto-recall/auto-capture log a
  warning and skip silently, and the agent tools return the error text.
- **Unbound key** — the client-supplied `userId` is passed through unchanged, so
  a single key can serve multiple identities.

If you hit this `403`, set `userId` to the identity the key was minted for (one
agent = one bound key is the recommended pattern), or request an unbound key.

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

Sign up at [kovamind.io](https://kovamind.io).

## License

MIT
