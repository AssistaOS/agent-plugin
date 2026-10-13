# Assista for your writing tool

Connect your writing tool to Assista. It reads your voice brief and this week's
interview, drafts posts for LinkedIn, X, Threads, Facebook or Instagram, and
sends them to your Assista approvals.

**What this can do:** read your voice brief, your interview answers and your
recent feedback, and send up to three drafts at a time to your approvals.
**What it cannot do:** publish, schedule or spend anything. Every post waits in
Assista until you approve it.

You sign in to Assista in your browser the first time you use it. There is no
key to copy and nothing secret in this repository.

## Install

Pick your tool. Each one is a single step, then a sign-in.

### Claude Code

```sh
claude plugin install assista --marketplace getassista/agent-plugin
```

On Claude Code older than 2.1.292, use two steps:

```sh
claude plugin marketplace add getassista/agent-plugin
claude plugin install assista@assista
```

Then run `/mcp`, choose `assista` and sign in. Ask for a post, or run
`/assista:draft linkedin what I learned shipping the new pricing page`.

### Claude on the web, desktop and phone

Settings → Connectors → Add custom connector, and paste
`https://mcp.getassista.com/mcp`. Claude asks you to sign in to Assista.

### Codex

```sh
codex plugin marketplace add getassista/agent-plugin
codex plugin add assista@assista
codex mcp login assista
```

Start a new Codex session afterwards.

### Cursor

Install **Assista** from the Cursor Marketplace (Customize → Plugins), choose
a project or user scope, then sign in when Cursor asks.

Before the listing is live, clone this repository into
`~/.cursor/plugins/local/assista` and reload the window.

### Gemini CLI

```sh
gemini extensions install https://github.com/getassista/agent-plugin
```

Then run `/mcp auth assista` and sign in. `/draft` runs the same steps as
`/assista:draft` in Claude Code.

### VS Code with GitHub Copilot

Turn on `chat.plugins.enabled`, run **Chat: Install Plugin From Source** and
enter `https://github.com/getassista/agent-plugin`.

### Any other tool

Add the writing guide:

```sh
npx skills add getassista/agent-plugin
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

The connection has four tools: `get_voice_brief`, `get_interview`,
`get_feedback` and `submit_drafts`. There is no publish tool, and there will not
be one.

## Support

https://getassista.com/contact · security@getassista.com

MIT licensed. See [LICENSE](LICENSE).
