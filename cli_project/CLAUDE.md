# CLAUDE.md

This file provides guidance to Claude Code when working in this subdirectory.

## Overview

`cli_project/` is a standalone Python application (not a notebook), distinct from the rest of the repo. It's an interactive command-line chat client ("MCP Chat") built on the Anthropic API and the Model Context Protocol (MCP) — it connects to an MCP server that exposes document tools/resources/prompts, and lets a Claude model call those tools to answer questions and edit documents. It's a subproject managed independently with its own `pyproject.toml`, `uv.lock`, and virtualenv.

## Setup & running

- Dependencies are managed via `pyproject.toml` / `uv.lock` (uv-based) — see this directory's `README.md` for full setup steps (`uv venv`, `uv pip install -e .`, or the plain-pip fallback).
- Requires a `.env` file in this directory with `ANTHROPIC_API_KEY` and `CLAUDE_MODEL` set — `main.py` asserts both are non-empty at startup.
- Run with `uv run main.py` (or `python main.py` without uv). On Windows, `main.py` explicitly sets `WindowsProactorEventLoopPolicy` since MCP's stdio transport needs it.
- Optionally pass additional MCP server scripts as CLI args (`main.py` launches each via `uv run <script>` in addition to the default `mcp_server.py`).

## Architecture

- `mcp_server.py` — a `FastMCP` server (`docs` MCP) exposing an in-memory document store via: `read_doc_contents`/`edit_document` tools, `docs://documents` and `docs://documents/{doc_id}` resources, and a `format` prompt. Has a documented TODO for a "summarize" prompt.
- `mcp_client.py` — `MCPClient`: thin async wrapper around an MCP `ClientSession` over stdio, used as an async context manager (`connect`/`cleanup` via `AsyncExitStack`).
- `core/claude.py` — `Claude`: wraps `anthropic.Anthropic().messages.create`, with the same `add_user_message`/`add_assistant_message`/`chat`/`text_from_message` helper shape used throughout the root notebooks, plus `thinking`/`tools` support.
- `core/tools.py` — `ToolManager`: converts MCP `Tool` definitions into Claude tool schemas, finds which connected client owns a given tool name, and executes tool-use requests from a Claude `Message` into `tool_result` blocks.
- `core/chat.py` / `core/cli_chat.py` — `Chat` base class plus `CliChat`, which adds `@doc_id` mention resolution (`_extract_resources`), `/command` dispatch to MCP prompts (`_process_command`), and prompt-message → Claude-message conversion.
- `core/cli.py` — `CliApp`: the `prompt_toolkit`-based REPL, with `/`-command and `@`-resource autocomplete plus a key-binding-driven completion UX.
- `main.py` — wires a `Claude` service, an MCP client for the doc server plus any extra server scripts, a `CliChat`, and a `CliApp`, then runs the REPL loop.

## Working in this repo

- This is real application code, not a notebook — normal Python editing conventions apply (no need to preserve cell outputs).
- If you change the `run_tool`-style tool dispatch shape here, note it's independent from the notebook `run_tool`/`run_conversation` pattern documented in the root `CLAUDE.md` (`tool_use.ipynb`, `tool_streaming.ipynb`, `text_editor_tool.ipynb`) — don't assume the two need to stay in sync.
- `mcp_server.py` has an explicit TODO to add a document-summarization prompt; check there before assuming prompt coverage is complete.
