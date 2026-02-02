# Copilot Instructions and Agent Rules

This file configures the Tenx MCP server for VS Code and provides agent rules to guide Copilot / Copilot Chat behavior. Use this as the primary rules file for AI-assisted work in this repository.

## MCP Server Configuration

- **Server Name**: tenxfeedbackanalytics
- **URL**: https://mcppulse.10academy.org/proxy
- **Headers**:
  - X-Device: linux
  - X-Coding-Tool: vscode

## Quick Setup Steps

1. Update VS Code to the latest version.
2. Install both extensions: GitHub Copilot and GitHub Copilot Chat.
3. Open the MCP UI (bottom panel), add the server if needed, then Start the `tenxfeedbackanalytics` server and complete GitHub OAuth in your browser.

## Agent Rules (best-practice, inspired by community workflows)

Purpose: make the coding agent predictable, safe, and efficient.

- **Persona & Tone**: Be concise, factual, and helpful. Prefer short actionable steps over long essays. When asked for options, provide 2–3 ranked choices.
- **Clarifying Questions**: If the user's request is ambiguous or missing necessary context (files, intent, environment), ask at most 2 targeted clarifying questions before making large changes.
- **Edit Policy**: Always summarize proposed file edits before applying them. When making edits, use minimal diffs and preserve code style. If a change affects multiple files, list them first and ask for confirmation unless the user requested an autonomous patch.
- **Preamble for Tool Calls**: Before using tools (apply_patch, run terminal commands, or call external MCP tools), provide a 1-line preamble describing what will be done and why.
- **Testing & Verification**: After code changes, run the smallest, fastest test possible (lint, unit test, or run a single file) and report results. If tests are not available, run formatting and a static-check where possible.
- **Commit Messages**: When committing changes, use concise messages that state intent (e.g., "mcp: add tenxfeedbackanalytics config" or "rules: tighten agent edit policy").
- **Security & Secrets**: Never create, store, or commit secrets (API keys, tokens) into the repository. Ask the user to add secrets to env or use the platform secret store.
- **Scope & Safety**: Do not run unknown scripts or programs without telling the user what will run and why. Use a dry-run first when possible.
- **Formatting & Style**: Prefer existing repo style. If none exists, use sensible defaults (Black for Python, Prettier for JS/TS). Offer to create or update config files if requested.
- **MCP Logging**: Include a short log/comment in edits when relevant to signal that the action should be logged to the Tenx MCP (e.g., a short JSON snippet in the tool call payload or comment indicating "log:tenxfeedbackanalytics").
- **Response Structure**: For multi-step tasks, present a short plan (3–6 steps), then execute step-by-step, marking steps completed in the plan.

## Examples (how to ask the agent)

- Good: "Refactor `src/utils.py` to extract a helper for X and add unit tests. Show plan, then apply."
- Bad: "Fix the project." (too broad)

## Troubleshooting

- If authentication fails during MCP start, re-open the MCP panel, stop the server, click Start again, and ensure the browser completes OAuth. Check `mcp.json` headers for correct `X-Device` value.
- If tools don't appear in Copilot Chat after successful start, restart VS Code and re-open Copilot Chat → Agent mode.

---

_Last updated: automated rules update for MCP challenge._
