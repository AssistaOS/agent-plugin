# Assista: write LinkedIn, X and Threads posts in your voice from Claude Code, Codex and Cursor

```sh
claude mcp add --transport http assista https://mcp.getassista.com/mcp
```

Your Claude writes it. Assista makes sure it sounds like you and waits for your
tap. Free: your tool pays for the model, we never call one for you.

<!-- Badges: uncomment once each listing is live.
[![MCP Registry](https://img.shields.io/badge/MCP_Registry-com.getassista%2Fassista-black)](https://registry.modelcontextprotocol.io)
[![Smithery](https://smithery.ai/badge/assista)](https://smithery.ai)
[![Glama](https://glama.ai/mcp/servers/assista/badge)](https://glama.ai/mcp/servers)
-->

Connect your writing tool to Assista. It reads your voice brief, your brand
and this week's interview, drafts posts for LinkedIn, X, Threads, Facebook or
Instagram, and sends them to your Assista approvals. Assista checks every
draft with deterministic rules (zero tokens) and holds it until you approve.

You sign in to Assista in your browser the first time you use it. There is no
key to copy and nothing secret in this repository.

## The tools

| Tool | Kind | What it does |
|---|---|---|
| `get_voice_brief` | read | Your voice brief, banned words, lines to avoid, recent published posts |
| `get_interview` | read | This week's three questions and your answers |
| `get_feedback` | read | What you approved, edited or turned down, with your edits |
| `get_brand_kit` | read | Palette, type, templates and approved assets for graphics and video |
| `submit_drafts` | submit | Up to three text drafts, checked, then sent to your approvals |
| `submit_graphics` | submit | One to six graphics or carousels, drawn by Assista, sent to your approvals |
| `submit_video` | submit | One short brand video, where video is switched on for your account |

## What it cannot do

There is no publish, schedule, billing or key tool, and there will not be one.
Your tool can only read your material and send work to your approvals. Every
post waits in Assista until you approve it, and Assista publishes it at your
hour. A draft that breaks a rule (a link in the body, Markdown, a hashtag
stack) is refused with the rule id so your tool can fix it and resend.

## Install

Pick your tool. Each one is a single step, then a sign-in.

### Claude Code

```sh
claude plugin install assista --marketplace AssistaAI/agent-plugin
```

On Claude Code older than 2.1.292, use two steps:

```sh
claude plugin marketplace add AssistaAI/agent-plugin
claude plugin install assista@assista
```

Then run `/mcp`, choose `assista` and sign in. Ask for a post, or run
`/assista:draft linkedin what I learned shipping the new pricing page`.

### Claude on the web, desktop and phone

Settings → Connectors → Add custom connector, and paste
`https://mcp.getassista.com/mcp`. Claude asks you to sign in to Assista.

### Codex

```sh
codex plugin marketplace add AssistaAI/agent-plugin
codex plugin add assista@assista
codex mcp login assista
```

Start a new Codex session afterwards.

### Cursor

[Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=assista&config=eyJ1cmwiOiJodHRwczovL21jcC5nZXRhc3Npc3RhLmNvbS9tY3AifQ%3D%3D),
or put this in `.cursor/mcp.json` (project) or `~/.cursor/mcp.json` (global):

```json
{ "mcpServers": { "assista": { "url": "https://mcp.getassista.com/mcp" } } }
```

Once the listing is live, install **Assista** from the Cursor Marketplace
(Customize → Plugins) to get the writing guide too. Before then, clone this
repository into `~/.cursor/plugins/local/assista` and reload the window.

### Gemini CLI

```sh
gemini extensions install https://github.com/AssistaAI/agent-plugin
```

Then run `/mcp auth assista` and sign in. `/draft` runs the same steps as
`/assista:draft` in Claude Code.

### VS Code with GitHub Copilot

Turn on `chat.plugins.enabled`, run **Chat: Install Plugin From Source** and
enter `https://github.com/AssistaAI/agent-plugin`.

Or add only the server:

```sh
code --add-mcp '{"name":"assista","type":"http","url":"https://mcp.getassista.com/mcp"}'
```

### Windsurf and Zed (unverified)

Windsurf, `~/.codeium/windsurf/mcp_config.json`:
`{ "mcpServers": { "assista": { "serverUrl": "https://mcp.getassista.com/mcp" } } }`.
Zed, `settings.json`:
`{ "context_servers": { "assista": { "url": "https://mcp.getassista.com/mcp" } } }`.
If sign-in does not open, run the server through
`npx -y mcp-remote https://mcp.getassista.com/mcp` instead.

### OpenCode

```json
{ "mcp": { "assista": { "type": "remote", "url": "https://mcp.getassista.com/mcp" } } }
```

in `opencode.json`, then `opencode mcp auth assista`.

### Any other tool

Add the writing guide:

```sh
npx skills add AssistaAI/agent-plugin
```

and point your tool at `https://mcp.getassista.com/mcp` (streamable HTTP). It
signs in over OAuth on the first call.

## Use a key instead

For CI or a tool that cannot open a browser, create a key in Assista →
Settings → Connect your writing tool → New key. It starts with `asst_` and is
shown once. Keep it in an environment variable, never in a file you commit.

```sh
claude mcp add --transport http assista https://mcp.getassista.com/mcp \
  --header "Authorization: Bearer $ASSISTA_API_KEY"
```

This plugin does not ask for a key, because an empty `Authorization` header
would stop the browser sign-in for everyone else. If you maintain a private
fork for a team that only uses keys, Claude Code can ask for the key at install
time and keep it in the system keychain. Add this to
`.claude-plugin/plugin.json`:

```json
{
  "userConfig": {
    "api_key": {
      "type": "string",
      "title": "Assista key",
      "description": "Starts with asst_. Create one in Assista → Settings → Connect your writing tool.",
      "sensitive": true,
      "required": true
    }
  }
}
```

and add `"headers": { "Authorization": "Bearer ${user_config.api_key}" }` to
the `assista` entry in `.mcp.json`.

## What is in here

| Path | For |
|---|---|
| `skills/assista-drafts/` | The writing guide: order of work, rules, one file per channel |
| `commands/draft.md` | `/assista:draft` in Claude Code |
| `commands/draft.toml` | `/draft` in Gemini CLI |
| `.mcp.json` | The Assista connection for Claude Code and Codex |
| `mcp.json` | The same connection in the Agent Plugins format (Cursor, others) |
| `.claude-plugin/` | Claude Code plugin and marketplace |
| `.codex-plugin/`, `.agents/plugins/` | Codex plugin and marketplace |
| `.cursor-plugin/` | Cursor plugin and marketplace |
| `plugin.json` | Agent Plugins manifest (agent-plugins.org) |
| `gemini-extension.json`, `GEMINI.md` | Gemini CLI extension |
| `server.json` | Listing for the official server registry |

The connection has the seven tools listed above. There is no publish tool,
and there will not be one.

## Privacy

Assista stores the drafts, graphics and videos your tool submits, their check
results and your decisions; nothing else from your session. Drafts you never
decide on expire after seven days. Keys are stored only as a hash. Details,
retention and sub-processors: https://getassista.com/legal/privacy and
https://getassista.com/legal/subprocessors. Data protection: dpo@getassista.com.

## Want Assista to write for you?

The free plan is for writing from your own tool. Paid plans add a draft in your
inbox every morning, the weekly interview, campaigns, graphics and video, and
more channels: https://getassista.com/pricing

Docs: https://getassista.com/claude-code and https://getassista.com/developers

## Support

https://getassista.com/contact · security@getassista.com

MIT licensed. See [LICENSE](LICENSE).
