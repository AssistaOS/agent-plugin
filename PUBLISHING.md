# Publishing `AssistaAI/agent-plugin`

This directory (`plugins/assista/` in AssistaOS) is the complete root of the
public repository `AssistaAI/agent-plugin`. AssistaOS stays private; only this
folder is pushed.

Items marked **(unverified)** could not be checked against live documentation
or a live run on 2026-10-13. Check them at publish time.

## 0. Before every release

From the AssistaOS root:

```sh
pnpm plugin:sync          # copy packages/mcp/skills into plugins/assista/skills
pnpm plugin:check         # fails if the copy differs (also run by the test suite)
pnpm vitest run --project scripts
claude plugin validate --strict plugins/assista
claude plugin validate --strict plugins/assista/.claude-plugin/plugin.json
```

Bump `version` in all of these together (the test enforces it):
`.claude-plugin/plugin.json`, `.codex-plugin/plugin.json`,
`.cursor-plugin/plugin.json`, `plugin.json`, `gemini-extension.json`,
`server.json`. Claude Code keeps users on the old copy until `version` changes.

Check that `https://mcp.getassista.com/mcp` answers a bare POST with `401` and a
`WWW-Authenticate: Bearer resource_metadata="…"` header. Every client's sign-in
starts there.

## 1. Create the repository and push this folder

1. On GitHub, create the organisation `getassista` if it does not exist, then a
   **public**, empty repository `AssistaAI/agent-plugin` (no README, no
   licence: both are in this folder).
2. From the AssistaOS root, split this folder's history into its own branch and
   push it as `main`:

   ```sh
   git subtree split --prefix plugins/assista -b agent-plugin-export
   git push git@github.com:AssistaAI/agent-plugin.git agent-plugin-export:main
   git branch -D agent-plugin-export
   ```

   For later releases, run the same three commands. The split is
   deterministic, so the push is a fast-forward as long as nobody commits to
   the public repository directly. Make every change here, never there.
3. Tag the release in the public repository: `v0.1.0`.
4. Smoke test from a clean machine or a throwaway home directory:
   - `claude plugin install assista --marketplace AssistaAI/agent-plugin`,
     then `/mcp` → `assista` → sign in, then `/assista:draft linkedin test`.
   - `codex plugin marketplace add AssistaAI/agent-plugin`,
     `codex plugin add assista@assista`, `codex mcp list` shows `assista`.
     (Verified against a local copy of this folder with Codex CLI 0.162.1.)
   - `gemini extensions install https://github.com/AssistaAI/agent-plugin`,
     then `/mcp auth assista`. **(unverified: no Gemini CLI was available.)**
   - `npx skills add AssistaAI/agent-plugin` lists `assista-drafts`.
     **(unverified)**

## 2. Anthropic's directory (claude.ai, Cowork and Claude Code)

One listing reaches all three; Claude Code users get it as `assista@synced`.

1. You need a paid claude.ai plan (Pro or Max from your own account; on Team or
   Enterprise, an Owner submits).
2. Run the validation in step 0, then the pre-submission checklist at
   https://claude.com/docs/plugins/pre-submission-checklist.
3. Open https://claude.ai/directory/manage and submit:
   - the **plugin**: the repository `https://github.com/AssistaAI/agent-plugin`;
   - the **connector**: `https://mcp.getassista.com/mcp`, OAuth. Ask for
     directory OAuth credentials at `mcp-review@anthropic.com` if the portal
     asks for a client id. **(unverified: the exact portal fields.)**
4. The directory reads `icon`, `documentationUrl`, `supportUrl`,
   `privacyPolicyUrl` and `termsOfServiceUrl` from `.claude-plugin/plugin.json`.
   They are set. Replace `assets/logo.svg` with a PNG if the portal refuses SVG.
   **(unverified)**
5. Name rules: the plugin is `assista`, which passes. Never rename it; a rename
   is a new plugin to every existing install.

Once listed, change the "Claude" card on Settings → Connect your writing tool
to the directory link.

## 3. Official MCP Registry (`server.json`)

The registry is in preview and may reset its data. `server.json` in this folder
is ready: name `com.getassista/assista`, one `streamable-http` remote at
`https://mcp.getassista.com/mcp`, no headers (OAuth).

1. Install the publisher: `brew install mcp-publisher`.
2. Prove you own `getassista.com` with a DNS TXT record:

   ```sh
   openssl genpkey -algorithm Ed25519 -out key.pem
   PUBLIC_KEY="$(openssl pkey -in key.pem -pubout -outform DER | tail -c 32 | base64)"
   echo "getassista.com. IN TXT \"v=MCPv1; k=ed25519; p=${PUBLIC_KEY}\""
   ```

   Add that TXT record in Cloudflare DNS. Keep `key.pem` in the password
   manager, never in a repository.
3. Log in and publish from this folder:

   ```sh
   PRIVATE_KEY="$(openssl pkey -in key.pem -noout -text | grep -A3 "priv:" | tail -n +2 | tr -d ' :\n')"
   mcp-publisher login dns --domain getassista.com --private-key "${PRIVATE_KEY}"
   mcp-publisher publish
   ```

4. Check: `curl "https://registry.modelcontextprotocol.io/v0.1/servers?search=com.getassista/assista"`.
5. Claim the listings that read from the registry: Smithery, Glama, PulseMCP.

The `$schema` date (`2025-12-11`) is what the registry docs showed on
2026-10-13; `mcp-publisher publish` tells you if it has moved on.

## 4. Cursor Marketplace

1. The repository must be public and open source (MIT, done).
2. Submit at https://cursor.com/marketplace/publish. Cursor reviews every
   plugin and every update by hand.
3. Cursor reads `.cursor-plugin/plugin.json` (or the root `plugin.json`),
   `skills/` and `mcp.json`. **(unverified: whether Cursor's review wants
   `rules/` or a different logo format.)**

## 5. Codex / ChatGPT plugin directory (later)

Codex installs straight from the repository through
`.agents/plugins/marketplace.json`; no listing is needed for that. A listing in
the shared ChatGPT and Codex directory is a separate submission
(https://developers.openai.com/plugins/deploy/submission). Hold it: ChatGPT
limits write tools such as `submit_drafts` to Business, Enterprise and Edu.

## What lives where

- Source of truth for the skill: `packages/mcp/skills/` in AssistaOS. The copy
  in `skills/` is generated; `pnpm plugin:check` fails when it drifts.
- The connect address: `web/src/lib/mcp/endpoint.ts` and `deploy/Caddyfile`.
  If it ever changes, update `.mcp.json`, `mcp.json`, `gemini-extension.json`,
  `server.json` and the test in `scripts/plugin-manifests.test.mjs`.
