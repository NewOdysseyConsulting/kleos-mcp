# Kleos for Hermes Agent

[Kleos](https://getkleos.ai) is an AI marketing team for small businesses. It plans a month of posts, articles and
newsletters in your voice, writes them through its own checks, publishes what you approve, and shows what's working on
Google and in AI assistants.

This directory is a portable Agent Plugins v1 package for [Hermes Agent](https://hermes-agent.nousresearch.com)
(`plugin.json`, `mcp.json` and `skills/`). It is configuration only: no local code runs. It gives Hermes:

- **The Kleos MCP server** (`kleos`, Streamable HTTP at `https://app.getkleos.ai/mcp`), hosted by Kleos.
- **The `kleos:kleos` skill**, which tells the agent when and how to use the tools, and Kleos's review rules.

## Install

```bash
hermes plugins install kleos
hermes plugins enable kleos
```

## Connect your Kleos workspace

Pick one. Both go through Hermes, and the plugin never sees or stores your credentials.

**OAuth sign-in (any plan).** Add this to your profile's `config.yaml`:

```yaml
mcp_servers:
  kleos:
    url: https://app.getkleos.ai/mcp
    auth: oauth
    trust: untrusted
```

Then run `hermes mcp login kleos` in a terminal. Hermes opens the Kleos sign-in; sign in, pick a workspace and approve.
Hermes stores and refreshes the token. The assistant then acts with your own role in Kleos. This entry replaces the
package's own `kleos` server, so keep `trust: untrusted` to keep the approval prompts described below.

**API key (Growth and Pro plans).** Create a key in Kleos under **Settings → API** (admins and owners). Enter it as
**Kleos API key** on the plugin's page under **Settings → Plugins** in Hermes Desktop, or set `KLEOS_API_KEY` in your
Hermes profile's environment. Hermes sends it as `Authorization: Bearer <key>`. A read key acts as a viewer; a write
key acts as an editor (plans, drafts and approves, but can't publish).

## What you can ask

- "Plan my LinkedIn and X month around my buyers' problems."
- "Write a post about why invoices get paid late, and put it in my review queue."
- "What's waiting for my approval? Approve the ones that are ready."
- "How did last month's posts do? Did they bring any leads?"
- "Does ChatGPT recommend my business? What would change that?"

## Tools

| Tool | Changes anything? |
|---|---|
| `list_businesses`, `get_month_plan`, `list_review_queue`, `get_results`, `get_buyers_and_problems` | No (read-only) |
| `plan_my_month`, `draft_post`, `draft_article` | Yes: plans or drafts content in your workspace, using your plan's allowance |
| `approve_draft` | Yes: approves a draft that's ready; your business's publishing settings then apply |
| `schedule_post` | Yes: publishes an approved post on its connected channel (needs publishing permission) |
| `check_ai_visibility` | Reads the latest check; with `run_now` it starts a new one, using your plan's allowance |

The package marks the server `untrusted`, so Hermes asks you before every tool that changes something (Once, Session
or Always) and refuses those tools in cron jobs and other runs where nobody can answer. Read-only tools run without
asking.

## What to know before installing

- **Network calls.** Every tool call goes to Kleos at `app.getkleos.ai` (operated by New Odyssey). The OAuth sign-in
  also talks to `app.getkleos.ai` (metadata, client registration, authorize and token endpoints). Hermes contacts
  nothing else for this plugin. Kleos's privacy policy: https://getkleos.ai/privacy.
- **Credentials.** The plugin stores none. With OAuth, Hermes's MCP client keeps and refreshes the token in your
  Hermes profile. With an API key, Hermes keeps it in your profile's environment and adds the header. The plugin reads
  no files outside its own directory.
- **It acts on your Kleos workspace.** Drafts, plans, approvals, publishing and new AI-visibility checks happen in your
  Kleos workspace under Kleos's own rules: your role, your plan's allowances, the review queue and each business's
  publishing settings. Drafts the assistant writes wait in the review queue like any other; once approved, the
  business's own publishing settings apply. A flagged draft can't be approved until it's fixed in Kleos.
  Kleos records each change an assistant makes in the workspace's audit log.
- **No local code, no background processes, no telemetry** from the plugin. The tool set is served by Kleos's hosted
  server at run time, so it is not fixed by the pinned commit.

## Links

- Setup guide: https://getkleos.ai/agents
- Privacy: https://getkleos.ai/privacy
- Support: support@getkleos.ai
- Licence: MIT (see [LICENSE](../LICENSE))
