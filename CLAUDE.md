# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository status

The repo is currently empty (no commits, no files). The constraints below are the project's non-negotiables and must hold as scaffolding and features land.

## Stack (fixed)

- Next.js 15 (App Router)
- TypeScript in `strict` mode
- Tailwind CSS + shadcn/ui
- React Flow (graph UI)
- `ts-morph` (static TypeScript analysis)
- `better-sqlite3` (local persistence/index)

## Non-negotiable architecture rules

- **Static analysis never calls an LLM.** Parsing, dependency extraction, and the graph are produced by `ts-morph` + deterministic code. The LLM is only used to *explain* snippets that static analysis has already retrieved.
- **AI Q&A caps are hard limits**, not defaults:
  - at most **5 snippet files**, each truncated to **60 lines**
  - `max_output_tokens: 800`
  - model id read from the `OPENAI_MODEL` env var (never hard-coded)
- **Always exclude** from scanning/indexing: `node_modules`, `.env*`, `dist`, `build`, `.next`, binary files, and any file larger than **1 MB**.
- **Impact view labeling:** the impact/blast-radius UI must always say *"static dependency impact — not runtime"*. Do not weaken, shorten, or imply runtime coverage.

## Testing

- **Vitest** unit tests for the parser (the `ts-morph`-based static analysis layer).
- **One Playwright smoke test** for the app end-to-end — keep it to one, don't grow a Playwright suite in place of unit coverage.
