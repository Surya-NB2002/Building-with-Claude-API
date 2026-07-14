# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository is a collection of self-contained Jupyter notebooks demonstrating and exercising features of the Claude API (Anthropic Python SDK). There is no application code, package, or build system — each `.ipynb` file is an independent learning/reference notebook for one API capability.

## Setup & running

- Dependencies are installed inline in each notebook's first cell (`%pip install anthropic python-dotenv` plus whatever else that notebook needs, e.g. `dataset` helpers). There is no `requirements.txt` or `pyproject.toml`.
- An Anthropic API key must be available via a `.env` file (loaded with `dotenv.load_dotenv()`) as `ANTHROPIC_API_KEY`. `.env` is gitignored — never commit it.
- Notebooks are run interactively cell-by-cell (Jupyter/VS Code notebook UI). There are no test suites, lint configs, or CLI entry points in this repo.

## Structure and conventions

Every notebook follows the same skeleton:
1. Install deps, `load_dotenv()`, construct `client = Anthropic()`, and pick a `model` string (e.g. `"claude-sonnet-4-5"`, `"claude-haiku-4-5"`).
2. Define local helpers `add_user_message(messages, text)` / `add_assistant_message(messages, text)` that append `{"role": ..., "content": ...}` dicts to a shared `messages` list, and a `chat(messages, ...)` wrapper around `client.messages.create(...)`.
3. Exercise one specific API feature by building up `messages` and calling `chat`/`client.messages.create` directly.

These helpers are redefined per-notebook (copy-pasted, not imported from a shared module) — when editing one notebook's helpers, check whether the same pattern should be updated in others, but don't try to factor them into a shared package unless asked.

### What each notebook covers
- `requests.ipynb` — basic request/response loop, minimal chatbot loop using `input()`.
- `prompting.ipynb` — prompt engineering techniques; also contains `PromptEvaluator`, a harness that generates synthetic test-case datasets via the model, runs a prompt under test against them, grades outputs with an LLM-as-judge rubric (mandatory + secondary criteria, 1–10 score), and emits `output.json`/`output.html` reports (HTML report built by `generate_prompt_evaluation_report`).
- `prompt_eval_and_grading.ipynb` — companion/variant of the evaluation-and-grading workflow above.
- `system_prompt.ipynb` — system prompt usage.
- `temperature.ipynb` — `temperature` parameter effects.
- `output_control.ipynb` — controlling output format/length (e.g. `stop_sequences`, prefilling assistant turns).
- `streaming.ipynb` — streaming responses.
- `tool_use.ipynb` — tool/function calling: defining tool schemas (e.g. `set_reminder`, `get_current_datetime`), dispatch via `run_tool`/`run_tools`, and a `run_conversation` loop that keeps calling the model until it stops requesting tools.
- `tool_streaming.ipynb` — combining tool use with streaming responses.
- `text_editor_tool.ipynb` — Anthropic's built-in text-editor tool; implements the local filesystem-backed tool handlers and a text-edit schema that varies by model version, run through a similar tool-use loop.
- `web_search_tool.ipynb` — Anthropic's built-in web search tool.

### Generated/data artifacts
- `dataset.json`, `dataset1.json` — synthetic test-case datasets produced by `PromptEvaluator.generate_dataset`.
- `output.json`, `output.html` — evaluation results/report produced by `PromptEvaluator.run_evaluation`.

These are notebook outputs, not hand-authored fixtures — regenerating them by re-running a notebook is expected to change their contents.

## Working in this repo

- When modifying a notebook's tool-use loop or message-helper pattern, check `tool_use.ipynb`, `tool_streaming.ipynb`, and `text_editor_tool.ipynb` since they share the same `run_tool`/`run_conversation` shape.
- Prefer editing notebooks in place cell-by-cell rather than regenerating the whole file, to preserve existing outputs/history where relevant.
