# CLAUDE.md

This file provides guidance to Claude Code when working in this subdirectory.

## Overview

`Claude features/` is a sibling collection to the notebooks in the repo root, focused on specific, more advanced Claude API capabilities rather than core request/response basics. Like the root notebooks, each `.ipynb` file here is self-contained and independently runnable.

## Setup & running

- Same pattern as the repo root: each notebook installs its own deps in its first cell and loads `ANTHROPIC_API_KEY` from a local `.env` via `dotenv.load_dotenv()`. This directory has its own `.env` (gitignored) separate from the root one.
- No shared package, build system, or test suite — run notebooks interactively cell-by-cell.

## Structure and conventions

Each notebook redefines its own `add_user_message` / `add_assistant_message` / `chat(...)` helpers, matching the pattern used in the root notebooks. Some notebooks extend `chat(...)` with feature-specific parameters (e.g. `caching.ipynb` adds `cache_control` to the last tool/system block).

### What each notebook covers
- `caching.ipynb` — prompt caching: applying `cache_control: {"type": "ephemeral"}` to system prompts and the last tool definition to reduce token cost on repeated large prompts.
- `citations_feature.ipynb` — the citations feature: passing `citations: {"enabled": true}` on document content blocks so responses include `CitationCharLocation` references back to source text.
- `code_execution.ipynb` — the built-in `code_execution` tool combined with the Files API (`client.beta.files.upload/list/delete/download`) for sandboxed data analysis (e.g. churn analysis on `streaming.csv`).
- `thinking.ipynb` — extended thinking (`thinking={"type": "enabled", "budget_tokens": ...}`) and redacted thinking blocks.
- `image&pdf_support.ipynb` — multimodal input: base64-encoded image blocks and PDF/document blocks in message content.

## Working in this repo

- When adding a notebook for a new API feature, follow the existing skeleton (install cell → helpers cell → feature cells) rather than introducing a different structure.
- Prefer editing notebooks in place cell-by-cell to preserve existing outputs/history.
- This directory is part of a larger repo — see the root `CLAUDE.md` for conventions shared across all notebook collections.
