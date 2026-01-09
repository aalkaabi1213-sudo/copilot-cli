# Copilot agent instructions — copilot-cli 🔧

Purpose: Help AI coding agents be immediately productive in this repository. Focus on the repo's intent, developer workflows, environment variables, and observable conventions (do not assume anything not present in the files).

## Short summary
- This repo contains the GitHub Copilot CLI packaging and docs (install script and release workflows). The user-facing CLI is installed via `npm`, `brew`, or platform installers; run `copilot` in a working code folder to start the agent UI.
- Key files to read: `README.md`, `install.sh`, `changelog.md`, `.github/workflows/winget.yml`.

## Big picture & why
- Purpose: provide a terminal-native Copilot coding agent. The CLI integrates with GitHub (issues, PRs, work flows) and exposes slash commands (`/login`, `/model`, `/feedback`) to authenticate and control models.
- Distribution: Releases are published as GitHub releases and submitted to package managers (WinGet, Homebrew, npm). See `.github/workflows/winget.yml` and `install.sh` for installer details and asset naming conventions.

## How to run & reproduce locally
- Install: `npm install -g @github/copilot` or use `install.sh` to fetch platform-specific tarball. `copilot` launches the CLI UI.
- Common flags: `copilot --banner` (replay the startup banner). Start `copilot` in a repository folder to enable code navigation and repo-aware features.
- Authentication: preferred interactive flow is `/login` inside the CLI. Alternatively set a PAT with ``GH_TOKEN`` or ``GITHUB_TOKEN`` (in that precedence) or `COPILOT_GITHUB_TOKEN` (documented to take precedence in changelog).

## Important env vars & local switches
- COPILOT_GITHUB_TOKEN — preferred token (over `GH_TOKEN`).
- GH_TOKEN / GITHUB_TOKEN — fallback PAT env vars.
- USE_BUILTIN_RIPGREP — opt-in to builtin ripgrep implementation (refer changelog for context).

## Debugging and logs
- The project has enhancements for logging and debug messages in the changelog; to locate latest guidance, search `changelog.md` for "debug" or "log" entries.
- If investigating auth or model issues, trace: `README.md` (auth/user-facing), `changelog.md` (recent debug additions), and the workflows (release/install behavior).

## Project-specific conventions & patterns
- Releases: assets are named by platform (e.g. `*-win32-x64.zip`, `*-win32-arm64.zip`) — the WinGet workflow extracts asset URLs by pattern matching; preserve/extend that naming in release artifacts.
- CLI is model-aware; the default model currently referenced is Claude Sonnet 4.5 (see `README.md`). Use `/model` to change.
- Tools exposed to the coding agent include repo navigation, issue/PR access, and workflow reads (see `changelog.md` for an inventory of default tools).

## Integration points to be careful with
- Publishing: `release` events trigger downstream packaging (winget PR submissions). Don't modify asset naming or release tag formats without updating `.github/workflows/winget.yml`.
- Auth flow: both interactive `/login` and PAT env vars used in CI or local dev—tests or automation should prefer `COPILOT_GITHUB_TOKEN` for explicitness.

## How agents should operate when editing code
- Read `README.md`, `install.sh`, and `changelog.md` first for UX and packaging constraints.
- For any change affecting release artifacts, update `.github/workflows/winget.yml` (or add a note in the README) and include tests or a changelog entry.
- Include specific examples in PR descriptions: e.g., "Updated installer asset to include `win32-arm64` and `win32-x64` binaries to comply with winget workflow".

---

If anything here is unclear or if you'd like me to expand sections (examples of release naming, auth telemetry, or a short troubleshooting checklist for Windows installs), tell me what to add and I will iterate. ✅