## Building with Claude API

A collection of self-contained Jupyter notebooks for learning and practicing the Claude API (Anthropic Python SDK). Each notebook focuses on one API capability, with runnable examples you can tweak and re-run.

### Setup

1. Install [Jupyter](https://jupyter.org/) (or open the notebooks in VS Code / another notebook-capable editor).
2. Create a `.env` file in the repo root with your Anthropic API key:
   ```
   ANTHROPIC_API_KEY=your-key-here
   ```
   `.env` is gitignored — never commit your key.
3. Open any notebook and run its first cell, which installs dependencies (`anthropic`, `python-dotenv`, and anything else that notebook needs) and loads your API key.
4. Run the remaining cells in order. Each notebook builds up a `messages` list and calls the model via a small `chat(...)` helper defined at the top of the notebook.

There's no shared package or build step — notebooks are meant to be run and edited interactively, cell by cell.

### Notebooks

| Notebook | Topic |
|---|---|
| `requests.ipynb` | Basic request/response loop; a minimal terminal chatbot |
| `prompting.ipynb` | Prompt engineering techniques, plus a `PromptEvaluator` harness that generates synthetic test cases, runs a prompt against them, and grades outputs with an LLM-as-judge rubric |
| `prompt_eval_and_grading.ipynb` | Companion notebook for the prompt evaluation/grading workflow |
| `system_prompt.ipynb` | Using system prompts to steer model behavior |
| `temperature.ipynb` | Effects of the `temperature` parameter on output |
| `output_control.ipynb` | Controlling output format/length (e.g. stop sequences, prefilled assistant turns) |
| `streaming.ipynb` | Streaming responses |
| `tool_use.ipynb` | Tool/function calling — defining tool schemas, dispatching tool calls, and looping until the model stops requesting tools |
| `tool_streaming.ipynb` | Combining tool use with streaming |
| `text_editor_tool.ipynb` | Anthropic's built-in text-editor tool, backed by local filesystem handlers |
| `web_search_tool.ipynb` | Anthropic's built-in web search tool |

### Generated files

`dataset.json`, `dataset1.json`, `output.json`, and `output.html` are artifacts produced by running `prompting.ipynb` (synthetic test datasets and evaluation reports). They're regenerated each time you re-run that notebook, so don't worry about their exact contents changing.

### Practicing with this repo

- Work through the notebooks roughly in the order above — later ones (tool use, streaming, built-in tools) build on the request/response and message-handling basics from the earlier ones.
- Duplicate a notebook before making large edits if you want to keep the original example intact for reference.
- See `CLAUDE.md` for more detail on the shared code patterns (message helpers, tool-use loop) if you're using Claude Code to work in this repo.

### Subprojects

This repo also contains standalone subprojects, each with its own `README.md` and `CLAUDE.md`:

| Directory | What it is |
|---|---|
| [`Claude features/`](./Claude%20features/README.md) | More notebooks, covering advanced API features: prompt caching, citations, code execution, extended thinking, and image/PDF support |
| [`cli_project/`](./cli_project/README.md) | A standalone Python CLI chat app ("MCP Chat") built on the Anthropic API and the Model Context Protocol, with document tools/resources/prompts |
