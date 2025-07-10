# Letta Study Notes

This document summarizes key components of the [Letta](https://github.com/letta-ai/letta) codebase.

## Overview
- Letta is an open source framework for building **stateful agents** with transparent long-term memory. It is model agnostic and designed for advanced reasoning【F:README.md†L16-L18】.
- Agents run inside a Letta server which exposes REST APIs and can be managed via the Agent Development Environment (ADE). The recommended deployment uses Docker:
  ```sh
  docker run \
    -v ~/.letta/.persist/pgdata:/var/lib/postgresql/data \
    -p 8283:8283 \
    -e OPENAI_API_KEY="your_openai_api_key" \
    letta/letta:latest
  ```
  【F:README.md†L46-L58】

## Core Modules
- `letta/agent.py` defines the `Agent` class which maintains agent state and tools. It initializes components like managers and telemetry, and handles memory updates【F:letta/agent.py†L80-L177】.
- `letta/server/server.py` implements the main FastAPI based server and provides abstract methods for managing agents, messages, and memory【F:letta/server/server.py†L102-L151】.
- `letta/streaming_utils.py` includes utilities such as `JSONInnerThoughtsExtractor` for parsing streamed JSON and extracting internal thoughts separately from main output【F:letta/streaming_utils.py†L1-L29】.
- Configuration constants like `LETTA_DIR` and tool module names are defined in `letta/constants.py`【F:letta/constants.py†L1-L18】【F:letta/constants.py†L20-L33】.

## Memory Handling
Agents store their long-term memory in `Memory` blocks that can be summarized when context grows too large. Memory functions and summarization logic live in `letta/memory.py`【F:letta/memory.py†L1-L15】.

## Directory Layout
The project root contains several directories beyond the main `letta` package:

- `alembic/` – Alembic migration environment for managing database schema changes.
- `assets/` – Image assets referenced in the README and docs.
- `certs/` – Self-signed certificates for local HTTPS development.
- `db/` – Postgres container helper scripts.
- `examples/` – Notebooks and sample code demonstrating agent usage.
- `paper_experiments/` – Research experiments and replication scripts.
- `performance_tests/` – Load‑testing utilities.
- `prompts/` – Development guidelines and base prompts.
- `scripts/` – Helper scripts for Docker and development setup.
- `tests/` – Pytest suite covering the agent logic and API.
- `study/` – This folder with notes about the repository.

## Tests
A sample unit test (`tests/test_stream_buffer_readers.py`) validates streaming JSON parsing. Running it locally with `pytest` succeeds:
```
5 passed, 12 warnings in 0.16s
```
【cc3dd0†L1-L32】

