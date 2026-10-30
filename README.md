# elysium-atlas-backend

Backend for **Elysium Atlas**. Serves team and visitor APIs for chat agents, and keeps conversation state, knowledge, and tool results in MongoDB, Redis, and Qdrant.

Part of [ElysiumForge Labs](https://github.com/ElysiumForge-Labs). Companion frontend: [elysium-atlas-app](https://github.com/ElysiumForge-Labs/elysium-atlas-app).

> **Independent research repository.** Not affiliated with, endorsed by, or sponsored by Kinetiq or Hyperliquid. Protect JWT secrets, application passkeys, and model API keys; never commit `.env`.

## What it covers

- Agent configuration, team membership, and dashboard summaries
- Knowledge-base ingestion from files and URLs, with vector search in Qdrant
- Live visitor chat, session history, monitoring, and human takeover
- Custom HTTP tools and sandboxed Python plugins invoked during a chat
- Lead collection, human handover, and support tickets
- Model calls through OpenAI, Groq, Anthropic, xAI, and DeepSeek

HTTP API is mounted at `/elysium-agents`. Live traffic uses Socket.IO at `/socket.io`.

## Layout

| Path | Role |
| --- | --- |
| `main.py` | Process entry point, lifespan, health check |
| `routes/` | HTTP routers |
| `controllers/` | Request handling |
| `services/` | Persistence, retrieval, model calls, chat orchestration |
| `sockets.py` | Socket.IO handlers |
| `jobs/` | Background workers |
| `middlewares/` | JWT and application-passkey checks |
| `config/` | Settings and request models |
| `documentation/` | API and product guides |

## Requirements

- Python 3.11+
- [uv](https://github.com/astral-sh/uv)
- MongoDB, Redis, Qdrant
- LibreOffice (optional, for legacy `.doc` ingestion)
- Playwright Chromium (optional, for page-content fetch into a knowledge base)

## Local setup

```bash
uv sync
cp .env.example .env
uv run playwright install chromium   # if needed
uv run python main.py
```

Fill `.env` before starting. See `.env.example` for process, MongoDB, Redis, Qdrant, auth, object storage, model, and widget variables.
