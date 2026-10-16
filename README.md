# spine-driver-agent-device

Driver adapter for spine-toolkit: declares what the `agent-device` MCP server can drive, per surface.

## What This Is

spine-driver-agent-device is a driver plugin for [spine-toolkit](https://github.com/iruirc/spine-toolkit) that declares the capabilities of [agent-device](https://github.com/callstack/agent-device), Callstack's device-automation MCP server. The driver ships the server: installing the plugin registers it.

## Installing the MCP Server

Nothing to install by hand: the plugin's `.mcp.json` runs `agent-device@0.21.23 mcp` through `npx`
(Node.js required), and Claude Code and Codex CLI both pick it up when the plugin is installed. The
first start downloads the package; `npx -y agent-device@0.21.23 doctor` checks the machine. In Codex
the server's tools are approved without a question per call; to be asked again, set
`default_tools_approval_mode = "prompt"` under
`[plugins."spine-driver-agent-device@<marketplace>".mcp_servers.agent-device]` in `~/.codex/config.toml`.

Running a build of the server of your own: in Codex, register it under the key `agent-device` in your
configuration — it replaces the plugin's. In Claude Code a server you register yourself runs next to
the plugin's rather than instead of it, unless it has the same command and arguments; point a copy of
the plugin's `.mcp.json` at your build instead.

## Using the Driver

In your project config (`SPINE.md`), declare this driver in the `## Task defaults` block:

```markdown
## Task defaults

[DRIVER] = [spine-driver-agent-device]
```

The manifest in `skills/manifest/SKILL.md` declares which surfaces and capabilities are supported, and names the runtime check that narrows them for a given machine. See [spine-toolkit: docs/building-a-driver.md](https://github.com/iruirc/spine-toolkit/blob/main/docs/building-a-driver.md) for the driver contract and architecture.
