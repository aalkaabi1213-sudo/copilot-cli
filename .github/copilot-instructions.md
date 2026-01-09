# Copilot CLI — Instructions for AI coding agents

Purpose: Short, actionable guidance so an AI agent can be immediately productive in this repo.

## Big picture (what matters) 🔧
- This repo powers a terminal-native Copilot coding agent (the CLI). The agent runs locally and uses an MCP server for tool integrations. See `README.md` for the high-level overview.
- Important runtime pieces: the CLI binary (`copilot`), local/remote MCP servers, model selection (`/model`), and custom agents loaded from local or repo-provided files.

## Key locations & config files 📁
- User-level config & runtime: `~/.copilot/config` (log levels, etc.), `~/.copilot/mcp-config.json` (MCP servers), `~/.copilot/session-state` (new session format) and legacy `~/.copilot/history-session-state`.
- Custom agents lookup order: `~/.copilot/agents`, `.github/agents` in the repo, and the organization's `.github` repo. Agent files commonly have `.agent.md` or `.agent.md`-style suffixes.

## How to run & authenticate ▶️
- Install: `winget`, `brew`, `npm` (or install script). Launch with `copilot`.
- First run: use `/login` or provide a PAT with the **Copilot Requests** permission via `GH_TOKEN` / `GITHUB_TOKEN` (or `COPILOT_GITHUB_TOKEN` — this repo expects these env vars for automation).
- For GHE: `GH_HOST` is respected for non-interactive logins.

## Developer workflows & flags (examples) ⚙️
- Resume remote session: `copilot --resume`
- Pass additional MCP config: `copilot --additional-mcp-config @/path/to/config.json` or inline JSON: `copilot --additional-mcp-config '{"mcpServers":{...}}'`
- Toggle streaming: `--stream off`; quiet scripting: `--silent`.
- Control tools the model can access: `--available-tools` and `--excluded-tools`.

## Common slash commands & behaviors (important to use correctly) 🧭
- `/model` — see and select available models (default: Claude Sonnet 4.5 in this codebase). `/model` output includes premium multipliers.
- `/terminal-setup` — configure multi-line input / terminal integration.
- `/delegate` — commit unstaged changes to a branch and open a PR to delegate work asynchronously.
- `/share`, `/share-gist` — export a session as markdown or gist.
- `/context` — visualize token usage.

## Project-specific patterns & constraints ✅
- Custom agents are provided as tools and can be invoked interactively (`/agent`) or non-interactively (`--agent <agent>`).
- Tool calls usually require explicit user confirmation; parallel tool calls are supported but can be disabled via `--disable-parallel-tools-execution`.
- Large tool outputs are written to disk; prefer incremental reading patterns (e.g., `view_range`) when processing big files.
- `gh` CLI is used as a fallback for some missing MCP tools when available locally.

## Debugging & telemetry 🐞
- Set `log_level` in `~/.copilot/config` (values: `none,error,warning,info,debug,all,default`) to collect richer diagnostics.
- Errors include Copilot API request IDs to help trace backend issues in logs.

## Notes for agents modifying code 🤖
- Follow existing repo patterns documented in `README.md` and `changelog.md` — e.g., configuration locations, agent discovery, and MCP interaction conventions.
- Prefer making minimal, well-tested changes and always surface expected side effects (new config keys, new agent files, or changes to MCP server config).

> Quick tip: check `changelog.md` for recent behavioural changes (session storage, MCP flags, model availability). Use exact example commands above when instructing the user to reproduce behavior.

---

If you want, I can (1) merge this into an existing `.github/copilot-instructions.md` if one exists elsewhere, (2) add short examples or tests that validate agent-loading behavior, or (3) expand on any of the sections above — tell me which you'd prefer.