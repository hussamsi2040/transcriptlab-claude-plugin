# TranscriptLab — Claude plugin

This package connects Claude to the TranscriptLab MCP server at
`https://api.transcriptlab.dev/mcp`.

## Layout

- `.claude-plugin/plugin.json` — plugin manifest.
- `.mcp.json` — declares the remote MCP server (streamable HTTP).
- `skills/transcriptlab/SKILL.md` — routing instructions: which tool answers which request.
- `assets/icon.png` — plugin icon.

## Authentication

The server implements the MCP authorization spec end to end: protected-resource metadata at
`/.well-known/oauth-protected-resource`, authorization-server metadata at
`/.well-known/oauth-authorization-server`, and RFC 7591 dynamic client registration at
`/oauth/register`. A conformant client connects with nothing but the URL — no pre-registered
client id.

On first use the client opens `https://api.transcriptlab.dev/oauth/authorize`, where the user
either signs in with their TranscriptLab account (Clerk) or pastes an API key. Tokens are minted
per session as `oauth:<client>` rows in the account's key list, scoped `transcriptlab:read` /
`transcriptlab:write`, valid one hour with rotating refresh tokens. Sessions that sign in with a
key — and bare API-key bearer credentials — cannot reach upload/export tools (session-only routes);
Clerk-signed connections and API-key connections both see the core tools.

## Local check

```bash
claude mcp add --transport http transcriptlab https://api.transcriptlab.dev/mcp
# then: /mcp in Claude Code, or connect in claude.ai → MCP connectors
```

## What the plugin does

- Declares one remote MCP server, `https://api.transcriptlab.dev/mcp`. Every request goes to that
  URL; nothing else is fetched or contacted.
- Ships one skill (`skills/transcriptlab`) that maps user requests onto the server's
  model-facing tools. The skill runs no scripts and touches no files.
- Authenticates via OAuth. On the consent page the user signs in or pastes a TranscriptLab API
  key; the token stays between Claude and the TranscriptLab gateway.

## Marketplace submission

Submit this repository (plugin path: repository root) as a **Plugin bundle** from the developer
portal at `claude.ai/directory/manage`. Also submit the MCP server as an **MCP connector** with
URL `https://api.transcriptlab.dev/mcp`; the two are separate listings that reference each other.
