# AGENTS.md — spine-driver-agent-device

> This repo is a dual-runtime Claude Code and Codex plugin: `spine-driver-agent-device`, the driver that tells
> `spine-toolkit` what `callstack/agent-device` drives, per surface.
> This file configures the agent working on the plugin itself: Claude Code reads it through
> `CLAUDE.md`, Codex reads it directly.

## What this repository is

- `skills/manifest/SKILL.md` — the driver manifest. Its format is `conventions/driver-contract.md`
  of `spine-toolkit`; `docs/building-a-driver.md` there is the guide.
- `.mcp.json` — the MCP server the driver binds.
- `CLAUDE.md` — imports `AGENTS.md` for Claude Code.

## When working on this repo

- Check a change with `../spine-toolkit/scripts/lint-driver-manifest.sh .`. What to run and when:
  `spine-ops/WORKSPACE.md`.
- This repository is public, and `spine-ops: leak-guard` checks every commit, commit message, tag
  and push for absolute local paths and the owner's real project names. Never pass `--no-verify`
  and never change `core.hooksPath`. When the check stops you, replace the data with a neutral
  placeholder (`/Users/foo`, `com.example.app`, `MyApp`) instead of working around it.

## Codex compatibility

- Keep `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json` aligned on `name`, `version`,
  `author.name`, and `repository`. Never move `version` by hand: a release goes through
  `spine-ops: scripts/release.sh`, which moves it in both. Never commit the `+codex.<cachebuster>`
  suffix Codex's local reinstall flow writes into `version`.
- `skills/manifest` carries `policy.allow_implicit_invocation: false` in its `agents/openai.yaml`:
  only `spine-toolkit` reads it.
