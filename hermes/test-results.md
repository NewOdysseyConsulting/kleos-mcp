# Hermes Agent: local test of the Kleos package

For the PR that adds `plugin-catalog/kleos.yaml` to NousResearch/hermes-agent (reviewers look for a test report). Run on
macOS (arm64), 11 November 2026, Hermes Agent v0.23.1 (stable, local 3b4215785882) installed with the official
installer (`install.sh --non-interactive`), package snapshot = the contents of this repository at the commit being
pinned (the commands below ran against a local git snapshot of it, `168684fc…`; rerun them against the pinned commit
and paste the output into the PR).

## 1. `hermes plugins validate` (what catalog CI runs)

```
$ hermes plugins validate ./hermes      # the package lives in the hermes/ subdirectory (catalog entry: subdir: hermes)
✓ portable manifest — plugin.json parses (Agent Plugins v1)
✓ manifest fields — name present
✓ config schema — shape valid
✓ server trust: kleos — untrusted (Hermes asks before every write-capable tool call)
✓ security scan — safe
◆ Isolation — portable plugin (no Python): runs anywhere

Validation passed.
```

## 2. Install, list, enable, show

```
$ hermes plugins install <git url of this repository> --no-enable --yes-deps
Cloning …
╭──── ✓ Installed ────╮
│ Plugin installed: … │
│ Location: ~/.hermes/plugins/kleos │
╰─────────────────────╯
Plugin installed but not enabled. Run `hermes plugins enable kleos` to activate.

$ hermes plugins list
│ kleos │ not enabled │ 1.0.0 │ Plan, write, approve, schedule and report on a small business's marketing in Kleos, through Kleos's hosted MCP server (OAuth sign-in with your Kleos login). │ git │

$ hermes plugins enable kleos
✓ Plugin kleos enabled.

$ hermes plugins show kleos
kleos v1.0.0
Status: enabled
Source: git
Key: kleos
Runs in: Hermes process — portable plugin (no Python): runs anywhere
Emits: (none)
Listens: (none)

$ hermes plugins doctor kleos
Plugin Doctor: ~/.hermes/plugins/kleos
  manifest: kleos 1.0.0 (standalone)
  OK: runtime discovery, manifest parsing, import, and registration passed
  registrations: 0 tool(s), 0 hook(s)
```

`registrations: 0 tool(s)` is expected for a portable package: the tools (31 on 12 November 2026; the README lists
them) come from the remote server at session start, not from Python registrations, which is why
`capabilities.provides_tools` in the catalog entry is empty.

## 3. The server through Hermes's native MCP client

`hermes mcp add kleos --url https://app.getkleos.ai/mcp` (no key given) reached the server and got Kleos's own
unauthenticated answer, which is the 401 challenge that starts OAuth:

```
  Connecting to https://app.getkleos.ai/mcp
  Connecting to 'kleos'...
  ✗ Failed to connect: Connect Kleos first: sign in to authorize this assistant, or send an API key.
```

So the endpoint, transport (Streamable HTTP, JSON-RPC over POST) and the discovery documents behind it
(`/.well-known/oauth-authorization-server`, `/.well-known/oauth-protected-resource`, dynamic client registration and
client ID metadata documents) are what Hermes expects; the remaining step is the person's sign-in.

## 4. The prompt ("list my businesses"): not completed here, and why

`hermes chat -q "Use the Kleos MCP server's list_businesses tool to list my businesses" --oneshot` stopped at
"Hermes is not connected to any AI provider yet". This test machine's session had no model-provider key to give
Hermes, and the sign-in to Kleos is an OAuth browser flow (the package carries no key on purpose), which a
non-interactive session cannot complete. Nothing in the package is the cause.

To finish the report before opening the catalog PR, on a machine with a provider configured (`hermes model`):

```
hermes plugins install kleos --no-enable      # or, before the catalog entry exists: hermes plugins install https://github.com/NewOdysseyConsulting/kleos-mcp --no-enable (installs the hermes/ package once the catalog subdir is known; from git the repo root must hold the package, so for a pre-catalog test clone the repo and install file://<clone>/hermes)
hermes plugins enable kleos
hermes            # first Kleos tool call opens the sign-in in the browser: sign in with the test account, pick "Kleos review", Allow
> Use Kleos to list my businesses.
```

Expected: Hermes asks to approve nothing (list_businesses is annotated read-only), calls `mcp__kleos__list_businesses`
and answers with the one business in the review workspace, Maple & Vine Bookkeeping (LinkedIn and a blog). Then:

```
> Draft a LinkedIn post about why a shoebox of receipts stops working at month-end.
```

Expected: because the server is marked `untrusted`, Hermes asks before `draft_post` runs (Once / Session / Always);
after approval the draft comes back and waits for review in Kleos. Paste both exchanges into the PR under "Test
results", with the `hermes plugins validate` output above.

## Disclosure lines for the PR description

- Hosted-only connector; no local code, no Python dependencies, no shell commands, no background processes.
- Network: every tool call goes to `https://app.getkleos.ai/mcp` (Kleos, made by New Odyssey). Kleos applies the
  signed-in person's role, plan allowance and each business's approval settings, and writes assistant actions to the
  workspace's audit log.
- Credentials: none stored by the package. OAuth tokens are handled by Hermes (`~/.hermes/mcp-tokens/`).
- Spend: drafting, planning and AI-visibility checks use the workspace's plan allowance; approving and scheduling
  publish to the business's own connected channels. The server is declared `untrusted`, so each write-capable call
  asks first and is refused in unattended runs.
- The tool set is served by the remote server at runtime and is not covered by the pinned sha.
