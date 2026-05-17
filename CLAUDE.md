# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

LangChain Academy's "Introduction to LangChain" course — a Python-based repository of Jupyter notebooks organized into three progressive modules covering LangGraph agents, tools, memory, MCP, and multi-agent systems.

## Setup & Environment

**Package manager:** `uv` is required (also needed for `uvx` in Module 2).

```bash
uv sync                  # install deps into .venv (Python >=3.12, <3.14)
cp example.env .env      # then fill in API keys
uv run python env_utils.py  # verify setup
```

Required `.env` keys: `OPENAI_API_KEY`, `TAVILY_API_KEY`  
Optional: `ANTHROPIC_API_KEY`, `GOOGLE_API_KEY`, `LANGSMITH_API_KEY`

## Common Commands

```bash
# Run Jupyter
uv run jupyter lab

# Run LangGraph Studio (from notebooks/module-1 or notebooks/module-3)
uv run langgraph dev

# Verify environment
uv run python env_utils.py

# Agent Chat UI (Module 3 bonus — Next.js app using pnpm)
cd notebooks/module-3/agent-chat-ui
pnpm install && pnpm dev
```

## Architecture

```
notebooks/
  module-1/       # Foundational models, tools, memory, multimodal, Personal Chef project
  module-2/       # MCP, runtime context/state, multi-agent, Wedding Planner project
  module-3/       # Middleware, message management, HITL, dynamic agents, Email Agent project
    agent-chat-ui/  # Next.js 16 + Tailwind chat UI that connects to the LangGraph server
```

Each module is self-contained. Notebooks reference a shared `../../.env` via `langgraph.json`.

**LangGraph integration:** Modules 1 and 3 each contain a `langgraph.json` that wires a Python graph entrypoint (`1.5_personal_chef.py:agent` and `3.5_email_agent.py:agent`) to the local LangGraph dev server. `uv run langgraph dev` reads this file from the current directory.

**Agent Chat UI:** A standalone Next.js app (`pnpm` workspace) in `notebooks/module-3/agent-chat-ui/` that connects to the LangGraph API server running locally. Uses Radix UI, Tailwind v4, and the `@langchain/langgraph-sdk`.

**`env_utils.py`:** A standard-library-first diagnostic script that checks Python version, venv activation, manual tool installs (declared in `example.env` comments), `.env` vs system env conflicts, and package versions against `pyproject.toml`. Run it when environment issues arise.

## Key Dependencies

- `langgraph` + `langgraph-cli` — graph orchestration and local dev server
- `langchain`, `langchain-openai`, `langchain-anthropic`, `langchain-google-genai` — LLM integrations
- `langchain-tavily` — web search tool
- `langchain-mcp-adapters`, `mcp` — Model Context Protocol (Module 2)
- `pypdf`, `langchain-text-splitters` — RAG/document processing (bonus notebooks)
- `jupyterlab` — notebook runtime
