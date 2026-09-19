# spine-driver-agent-device

Driver adapter for spine-toolkit: declares what the `agent-device` MCP server can drive, per surface.

## What This Is

spine-driver-agent-device is a driver plugin for [spine-toolkit](https://github.com/iruirc/spine-toolkit) that declares the capabilities of [agent-device](https://github.com/callstack/agent-device), Callstack's device-automation MCP server. The driver does not install the server — you register it separately with your MCP client.

## Installing the MCP Server

```bash
npm install -g agent-device@latest
agent-device doctor
```

Then register the stdio MCP server with your client under the key `agent-device`:

```json
{
  "mcpServers": {
    "agent-device": {
      "command": "agent-device",
      "args": ["mcp"]
    }
  }
}
```

## Using the Driver

In your project's `CLAUDE-spine-toolkit.md`, declare this driver in the `## Task defaults` block:

```markdown
## Task defaults

[DRIVER] = [spine-driver-agent-device]
```

The manifest in `skills/manifest/SKILL.md` declares which surfaces and capabilities are supported, and names the runtime check that narrows them for a given machine. See [spine-toolkit: docs/building-a-driver.md](https://github.com/iruirc/spine-toolkit/blob/main/docs/building-a-driver.md) for the driver contract and architecture.
