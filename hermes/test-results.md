# Hermes Agent: local test of the Kleos package

For the PR that adds `plugin-catalog/kleos.yaml` to NousResearch/hermes-agent. Run on macOS (arm64) on 12 November 2026
with Hermes Agent v0.23.1 (stable, local 3b4215785882) from the official installer, against the package in this
repository's `hermes/` directory at the commit pinned by the catalog entry, and against the live server at
https://app.getkleos.ai/mcp with a Kleos review workspace (one sample business, Maple & Vine Bookkeeping).

Sign-in for the test used the package's optional `api_key` setting (a Kleos API key in `KLEOS_API_KEY`, which
`mcp.json` sends as `Authorization: Bearer`). Without a key the server answers 401 with the OAuth challenge and Hermes
runs the browser sign-in; that path was not driven here because the test ran unattended.

## 1. `hermes plugins validate` (what catalog CI runs)

```
$ hermes plugins validate ./hermes
✓ portable manifest — plugin.json parses (Agent Plugins v1)
✓ manifest fields — name present
✓ config schema — shape valid
✓ server trust: kleos — untrusted (Hermes asks before every write-capable tool call)
✓ security scan — safe
◆ Isolation — portable plugin (no Python): runs anywhere

Validation passed.
```

## 2. Install, enable, show, doctor

Installed from a git snapshot of this directory (the catalog entry points at the same files through `subdir: hermes`):

```
$ hermes plugins install <git snapshot of hermes/> --no-enable --yes-deps
╭──── ✓ Installed ────╮
│ Location: ~/.hermes/plugins/kleos │
╰─────────────────────╯
Plugin installed but not enabled. Run `hermes plugins enable kleos` to activate.

$ hermes plugins enable kleos
✓ Plugin kleos enabled.

$ hermes plugins show kleos
kleos v1.0.0
Connects Hermes to Kleos, an AI marketing team for small businesses: …
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

`registrations: 0 tool(s)` is expected for a portable package: the 31 tools come from the remote server at session
start, which is why `capabilities.provides_tools` in the catalog entry is empty.

## 3. The server through Hermes's native MCP client

With `KLEOS_API_KEY` set in the profile's `.env`, the package's `mcp.json` entry connects and discovers every tool
(the same check `hermes mcp test` prints for a user-added server):

```
  Transport: HTTP → https://app.getkleos.ai/mcp
    Authorization: ***
  ✓ Connected (1994ms)
  ✓ Tools discovered: 31
```

Without a key, the first connection is Kleos's own 401 challenge, which starts OAuth:

```
  ✗ Failed to connect: Connect Kleos first: sign in to authorize this assistant, or send an API key.
```

## 4. Reads through the package: list businesses, read the month's plan

```
$ hermes chat -q "Use the Kleos MCP tools: call list_businesses and tell me the business names." --oneshot
Query: Use the Kleos MCP tools: call list_businesses and tell me the business 
names.
🔌 Kleos · list businesses  0.4s

────────────────────────────────────────
☤ Hermes
The workspace "Kleos review" has one business: Maple & Vine Bookkeeping.
────────────────────────────────────────


Duration:       4s
Messages:       4 (1 user, 2 tool calls)
```

```
$ hermes chat -q "Use the Kleos MCP tools: call list_businesses and tell me the business names, then call get_month_plan for the first business and summarise what is planned in three lines." --oneshot
Query: Use the Kleos MCP tools: call list_businesses and tell me the business 
names, then call get_month_plan for the first business and summarise what is 
planned in three lines.
📖 Reading tool details · 2 tools
🔌 Kleos · list businesses  0.5s
🔌 Kleos · get month plan  0.3s

────────────────────────────────────────
☤ Hermes
Business: Maple & Vine Bookkeeping (the only one in the "Kleos review" workspace).November 2026 plan, 26 items under the "No more April surprises" campaign, mostly LinkedIn with some blog posts and one newsletter.Status: 5 published, 2 approved, 12 planned, 2 with no draft yet, and 4 waiting on you (the newsletter, 2 LinkedIn posts and 1 blog article).Needs attention: the LinkedIn post "Ten seconds per receipt saves a weekend per month" was due today and missed because it wasn't approved. The blog post "Lumpy income makes your tax set-aside a guess" is also due today and has no draft.
────────────────────────────────────────


Duration:       9s
Messages:       8 (1 user, 6 tool calls)
```

Hermes's log for the second run shows the calls going through the package's server as native MCP tools:

```
agent.tool_executor: tool mcp__kleos__list_businesses completed (0.53s, 566 chars)
agent.tool_executor: tool mcp__kleos__get_month_plan completed (0.26s, 7325 chars)
```

## 5. The trust gate: a write-capable tool in an unattended run

The package marks the server `untrusted`, so a tool not annotated read-only asks the user first and fails closed where
nobody can answer:

```
$ hermes chat -q "Use the Kleos MCP tool record_note to leave the note 'Hermes catalog test, 12 Nov 2026' on Maple & Vine Bookkeeping." --oneshot
Query: Use the Kleos MCP tool record_note to leave the note 'Hermes catalog 
test, 12 Nov 2026' on Maple & Vine Bookkeeping.
📖 Reading tool details · 2 tools
✗ Kleos · record note [The user did not approve running write-capabl...]

────────────────────────────────────────
☤ Hermes
The note wasn't recorded. The call to record_note was blocked because it writes data and the kleos MCP server isn't approved to do that, so nothing ran. I haven't retried.If you want it posted, approve write access for the kleos server and tell me to go ahead. I'll then send "Hermes catalog test, 12 Nov 2026" to Maple & Vine Bookkeeping.
────────────────────────────────────────


Duration:       7s
Messages:       6 (1 user, 4 tool calls)
```

Log: `tools.mcp_tool: MCP trust gate: user denied 'record_note' on untrusted server 'kleos'`. Nothing was written in
Kleos.

## Notes for reviewers

- A user who has added the same server themselves (`hermes mcp add kleos --url https://app.getkleos.ai/mcp`) sees
  `Portable MCP server 'kleos' conflicts with native config; skipping` and their own entry wins, as the docs say.
- The package carries no credentials. `KLEOS_API_KEY` is optional; the README's Hermes section describes both ways in.
