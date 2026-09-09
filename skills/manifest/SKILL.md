---
name: manifest
description: Driver manifest for agent-device-driver. Data, not instructions — the blocks spine-toolkit reads to learn which tools drive an app, which surfaces they reach, and what each surface supports.
---

# agent-device-driver Manifest

> This skill is **data**, not instructions. spine-toolkit reads the blocks below by invoking this
> skill; there is no procedure here to follow.

Adapter for `agent-device` (callstack), which runs as an MCP server via `agent-device mcp`. The
adapter does not install it — install it separately and register it with your MCP client. Written
against v0.21.0.

## Driver

The server's own MCP configuration snippet prints `agent-device` as the key, so it leads; the two
punctuation variants follow for anyone who registered it under those instead.

namespace = agent-device, agentdevice, agent_device

## Targets

ios-simulator
ios-device
android-emulator
android-device
macos
linux
browser

## Capabilities: ios-simulator

launch stop install reset_state
ui_tree find assert screenshot video logs
tap type swipe gesture key
deeplink permissions alerts push biometrics location
performance
record_replay

## Capabilities: ios-device

launch stop install
ui_tree find assert screenshot video logs
tap type swipe key
deeplink alerts
performance

## Capabilities: android-emulator

launch stop install reset_state
ui_tree find assert screenshot video logs
tap type swipe gesture key
deeplink permissions alerts push biometrics location network_conditions
performance
record_replay

## Capabilities: android-device

launch stop install reset_state
ui_tree find assert screenshot video logs
tap type swipe gesture key
deeplink permissions alerts push network_conditions
performance
record_replay

## Capabilities: macos

launch stop
ui_tree find assert screenshot video logs
tap type
deeplink alerts
performance

## Capabilities: linux

launch stop
ui_tree screenshot
tap type

## Capabilities: browser

launch stop
ui_tree find assert screenshot
tap type
viewport
record_replay

## Procedure

**Ask the server before planning around a surface.** `agent-device capabilities --platform
<platform>` — or `--device`/`--udid`/`--serial` for one target — answers `{ device,
availableCommands }` for that exact device. The answer narrows this table and never widens it: a
capability absent from the block above stays unsupported even when `availableCommands` lists its
command.

Two narrowings are worth expecting. A physical iPhone discovered only through `xctrace` runs the
XCTest backend, which drives open, close, interactions, snapshots and screenshots but not install,
logs, recording, deep links or performance sampling; a CoreDevice-backed device has all of them.
And the modular platform packages are per-backend, so a surface this table declares may simply be
absent from a given machine.

**`diff screenshot` is not declared.** It exists in the CLI and in the command reference, but the
MCP `diff` tool locks its `kind` field to `snapshot`, so pixel-baseline comparison is unreachable
from this transport.

Prefer the accessibility snapshot to a screenshot for reading state. Screenshots and video are
evidence, not inspection, and the tree is an order of magnitude cheaper.

Do not pin a device. Choose the target from what is attached, and pass an explicit
`--device`/`--udid`/`--serial` when more than one candidate is equally preferred — the server
refuses to guess and fails with `AMBIGUOUS_MATCH` rather than answering about a device nobody
selected.

The server also drives tvOS, Android TV, Amazon Vega OS and HarmonyOS. Core's surface vocabulary has
no names for those, so they are not declared here; they arrive when a platform that produces them
does.

Leave the app stopped when the run ends, and restore anything `settings` changed.
