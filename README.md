# Reflex

**A faster, parallel browser for your AI agent.**

Reflex is a local MCP server that gives your AI assistant a real browser. It drives your own Chrome for Claude Desktop, Cursor, Claude Code, and any MCP client, and it is built for the way an agent actually pays for a browser: in round trips and in tokens.

[Site and benchmarks](https://reflexmcp.com/?utm_source=github&utm_medium=repo&utm_campaign=launch) · [npm: reflex-browser](https://www.npmjs.com/package/reflex-browser)

## Why it is faster

When a person uses a browser, the cost is their attention. When an agent uses one, the cost is two things: how many times the model has to stop and read the page, and how much it has to read each time. Reflex sends one compact view of the page, then only what changed, and it batches a whole task with its checks into a single call.

In a benchmark where a real agent (Claude Opus 4.8) drove five live site tasks on each tool, Reflex finished them about **3.5x faster end to end** than Playwright MCP (about 23.6 seconds versus 82.5), on **2.3x fewer round trips** and **7.7x less context**, with the same task success on every flow. Full numbers, including the places Reflex loses, are at [reflexmcp.com/benchmarks](https://reflexmcp.com/benchmarks).

## Run browsers in parallel

Pass a session name on any call and Reflex spins up a separate, isolated browser for it, on demand and free. One agent can research ten sites at the same time, and a swarm of subagents can each drive their own. Playwright MCP shares a single browser, so parallel agents fight over the same tab. With Reflex you never choose between speed and isolation.

## Private by design

The server, the browser, your logins, and every page you visit stay on your machine. The only thing that crosses the network is your API key and a count of tool calls, for credit metering. Metering never blocks a call.

## Install

Get a free key at [reflexmcp.com](https://reflexmcp.com/?utm_source=github&utm_medium=repo&utm_campaign=launch) (30 credits, no card), then add Reflex to your client.

### Claude Desktop

Add this to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "reflex": {
      "command": "npx",
      "args": ["-y", "reflex-browser@latest", "mcp"]
    }
  }
}
```

Then ask your agent to run the `reflex_auth` tool with `action: "login"` to sign in once in the browser. If you prefer, drop your key into the config instead by adding `"env": { "REFLEX_API_KEY": "your_key_here" }` to the block above.

### Cursor and VS Code

Use the one click install buttons at [reflexmcp.com](https://reflexmcp.com/?utm_source=github&utm_medium=repo&utm_campaign=launch), or add the same JSON to your client's MCP config.

### Claude Code

```bash
claude mcp add reflex -- npx -y reflex-browser@latest mcp
```

## Quick start

1. Install for your client (above).
2. Sign in: run `reflex_auth` with `action: "login"`, or set `REFLEX_API_KEY`.
3. Ask your agent to do a browser task in plain English.

## The tools

Reflex exposes 10 tools: `reflex_open`, `reflex_act`, `reflex_read`, `reflex_extract`, `reflex_check`, `reflex_shot`, `reflex_tab`, `reflex_flow`, `reflex_session` (manage parallel browsers), and `reflex_auth`. The full tool definitions total under 1,000 tokens of context.

## When not to use Reflex

If your agent has a shell, the Playwright CLI and Vercel's agent browser are great and free, and on a flow you have already planned out they send even fewer tokens. Use one of them. Reflex is for clients that only speak MCP, like Claude Desktop, where command line tools are not an option and your real alternative is Playwright MCP.

## Notes

This repository is documentation and install help. Reflex ships as the published npm package `reflex-browser`; install it with the commands above. macOS is supported today; Windows is in beta.
