## Claude features

A collection of self-contained Jupyter notebooks exercising specific Claude API features beyond the basics covered in the repo root — prompt caching, citations, code execution, extended thinking, and image/PDF support.

### Setup

1. Create a `.env` file in this directory with your Anthropic API key:
   ```
   ANTHROPIC_API_KEY=your-key-here
   ```
   `.env` is gitignored — never commit your key.
2. Open any notebook and run its first cell, which loads the API key and builds the shared `client`/`chat(...)` helpers.
3. Run the remaining cells in order.

### Notebooks

| Notebook | Topic |
|---|---|
| `caching.ipynb` | Prompt caching via `cache_control` on system prompts and tools |
| `citations_feature.ipynb` | Document citations — grounding responses in source text with `citations.enabled` |
| `code_execution.ipynb` | Anthropic's built-in code execution tool, combined with the Files API |
| `thinking.ipynb` | Extended/redacted thinking blocks |
| `image&pdf_support.ipynb` | Sending images and PDFs (base64 and document blocks) as message content |

### Supporting files

- `earth.pdf`, `images/`, `streaming.csv` — sample inputs used by the notebooks above.
- `churn_analysis_detailed.png` — output artifact produced by `code_execution.ipynb`'s churn analysis.

See `CLAUDE.md` in this directory for shared code patterns, and the root `README.md`/`CLAUDE.md` for how this fits into the wider repo.
