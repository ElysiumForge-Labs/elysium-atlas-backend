# elysium-atlas-backend

Backend for **Elysium Atlas**. Serves team and visitor APIs for chat agents, and keeps conversation state, knowledge, and tool results in MongoDB, Redis, and Qdrant.

Part of [ElysiumForge Labs](https://github.com/ElysiumForge-Labs). Companion frontend: [elysium-atlas-app](https://github.com/ElysiumForge-Labs/elysium-atlas-app).

> **Independent research repository.** Not affiliated with, endorsed by, or sponsored by Kinetiq or Hyperliquid. Protect JWT secrets, application passkeys, and model API keys. Never commit `.env`.

## Features

- Agent configuration, team membership, and dashboard summaries
- Knowledge-base ingestion from files and URLs, with vector search in Qdrant
- Live visitor chat, session history, monitoring, and human takeover
- Custom HTTP tools and sandboxed Python plugins invoked during a chat
- Lead collection, human handover, and support tickets
- Model calls through OpenAI, Groq, Anthropic, xAI, and DeepSeek

HTTP API is mounted at `/elysium-agents`. Live traffic uses Socket.IO at `/socket.io`.

## Stack

| Layer | Technology |
| --- | --- |
| Runtime | Python ≥ 3.11 |
| Framework | FastAPI, Uvicorn, python-socketio |
| Data | MongoDB (Motor), Redis, Qdrant |
| Jobs | ARQ |
| Packaging | uv (`pyproject.toml`) |
| Optional | LibreOffice (legacy `.doc`), Playwright Chromium (page fetch) |

Version: `0.3.14` (see `pyproject.toml`).

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
| `postman/` | Collection for manual API checks |

## Getting started

### Prerequisites

- Python 3.11+
- [uv](https://github.com/astral-sh/uv)
- MongoDB, Redis, and Qdrant reachable from the host
- Optional: LibreOffice; Playwright Chromium

### Install and run

```bash
uv sync
cp .env.example .env
uv run playwright install chromium   # if knowledge-base page fetch is needed
uv run python main.py
```

Fill `.env` before starting. See `.env.example` for process, MongoDB, Redis, Qdrant, auth, object storage, model, and widget variables.

## Security

- Rotate JWT secrets and application passkeys per environment.
- Sandboxed plugins still require careful allowlisting of network and filesystem access.
- Do not expose Socket.IO or admin routes without authentication in front.
