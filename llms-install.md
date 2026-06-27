# Installing Reflex

Reflex is a local MCP server (npm package `reflex-browser`) that drives the user's own Chrome for any MCP client. These steps let you install it automatically.

## Steps

1. Add this server to the user's MCP settings:

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

2. The user needs a free API key from reflexmcp.com (30 free credits, no card). There are two ways to authenticate:
   - Call the `reflex_auth` tool with `action: "login"`, which opens a browser sign in, or
   - Add `"env": { "REFLEX_API_KEY": "the_user_key" }` to the server block above.

3. Verify the install by calling `reflex_open` with a URL such as `https://example.com`. A successful call returns a compact view of the page.

## Requirements

- Node.js, so that `npx` is available.
- A Chrome or Chromium install (Reflex uses playwright-core to drive it).
- macOS is supported today; Windows is in beta.

## What it does

Reflex sends one compact view of a page, then only what changed, and lets the agent batch a whole task with its checks into a single call. The result is far fewer round trips and far less context than returning a full page representation on every look. It also supports parallel browsers: pass a session name on any call to get a separate, isolated browser on demand.
