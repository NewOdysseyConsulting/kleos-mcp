# Kleos MCP server

[Kleos](https://getkleos.ai) is an AI marketing team for small businesses. It plans a month of posts, articles and
newsletters in the business's voice, writes them through its own checks, publishes what's approved to the channels the
business has connected (LinkedIn, X, Facebook, Instagram, Google Business Profile, a blog, newsletters), and measures the
results: what was published, what it did, and whether AI assistants recommend the business when buyers ask them.

This repository is the public home of Kleos's MCP server, `https://app.getkleos.ai/mcp`: how to connect it to Claude,
ChatGPT, Cursor, Claude Code, Hermes Agent, LobeHub or any other MCP client, and what an assistant can do with it. The
server itself runs at Kleos; nothing here needs to be installed. It is also, from one tree, a Claude Code plugin and
marketplace (`.claude-plugin/`, `.mcp.json`), a Cursor plugin (`.cursor-plugin/`, `rules/`), a Hermes Agent package
(`hermes/`), a LobeHub listing (`lhm.plugin.json`) and an agent skill (`skills/kleos/`).

- Website: [getkleos.ai](https://getkleos.ai) · Start free, see your content plan first: [getkleos.ai/start](https://getkleos.ai/start)
  · Sign up: [app.getkleos.ai/signup](https://app.getkleos.ai/signup) · Pricing: [getkleos.ai/pricing](https://getkleos.ai/pricing)
- Documentation for assistants and developers, with curl examples and troubleshooting: [getkleos.ai/agents](https://getkleos.ai/agents)
  · [Kleos in Claude](https://getkleos.ai/claude) · [Kleos in ChatGPT](https://getkleos.ai/chatgpt) · [Integrations](https://getkleos.ai/integrations)
- Setup page for every assistant: [app.getkleos.ai/mcp](https://app.getkleos.ai/mcp)
- Server card (JSON): [app.getkleos.ai/.well-known/mcp.json](https://app.getkleos.ai/.well-known/mcp.json)

## What an assistant can do

Twelve tools, each working with the connected person's own role in the workspace, the plan's allowance and each
business's approval settings. Nothing is published unless someone with publishing permission schedules it or the
business has auto-publish on.

| Tool | What it does |
| --- | --- |
| `list_businesses` | See the businesses in your workspace |
| `get_month_plan` | Read the calendar for a month or a week |
| `plan_my_month` | Plan the next 30 days on the channels you choose |
| `draft_post` | Write a post on a topic or a buyer problem |
| `create_post` | Post your exact words, no AI |
| `draft_article` | Research and write a blog article |
| `list_review_queue` | See what's waiting for your approval |
| `approve_draft` | Approve a draft that's ready |
| `schedule_post` | Publish an approved post now or at its time |
| `get_results` | See posts published, impressions, clicks and leads |
| `check_ai_visibility` | See if ChatGPT, Google AI Mode and Perplexity recommend you |
| `get_buyers_and_problems` | Read your buyers and the problems they have |

Try:

- "What's on my Kleos calendar this week?"
- "Which drafts are waiting for my approval in Kleos?"
- "How did my content do in Kleos over the last 30 days?"
- "Plan my LinkedIn and X month around my buyers' problems."
- "Does ChatGPT recommend my business? What would change that?"

Details, and the rules an assistant follows (say what will be published first; never publish unless asked), are in
[`skills/kleos/SKILL.md`](skills/kleos/SKILL.md).

## Connect

Sign-in is OAuth 2.1 with PKCE: when you connect, the assistant sends you to Kleos to sign in with your usual login,
pick a workspace and approve what the assistant may do (everything, or reading only). There is no key to paste. Kleos
supports dynamic client registration and client ID metadata documents, so any client that speaks MCP's authorization
spec connects with the address alone. You can disconnect at any time under Settings → Integrations → Connected
assistants in Kleos.

You need a Kleos workspace: [start one at getkleos.ai/start](https://getkleos.ai/start).

### Claude

1. In Claude, open **Settings → Connectors** and choose **Add custom connector**.
2. Name it **Kleos** and paste `https://app.getkleos.ai/mcp`.
3. Choose **Connect**, sign in to Kleos, pick the workspace and choose **Allow**.

Or add Kleos from [Anthropic's directory](https://claude.ai/directory) once it is listed there. Guide: [getkleos.ai/claude](https://getkleos.ai/claude).

### ChatGPT

1. In ChatGPT, open **Settings → Apps & Connectors → Advanced settings** and switch on **Developer mode**.
2. In **Apps & Connectors**, choose **Create**. Name it **Kleos**, paste `https://app.getkleos.ai/mcp` and choose **OAuth**.
3. Sign in to Kleos, pick the workspace and choose **Allow**. In a chat, add Kleos from the tools menu.

Or install the Kleos plugin from the ChatGPT plugin directory once it is listed there. Guide: [getkleos.ai/chatgpt](https://getkleos.ai/chatgpt).

### Cursor

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](cursor://anysphere.cursor-deeplink/mcp/install?name=kleos&config=eyJ1cmwiOiJodHRwczovL2FwcC5nZXRrbGVvcy5haS9tY3AifQ==)

Or copy [`.cursor/mcp.json`](.cursor/mcp.json) into your project (or `~/.cursor/mcp.json` for every project), then
choose **Connect** next to Kleos under Settings → MCP. This repository is also a Cursor plugin
([`.cursor-plugin/plugin.json`](.cursor-plugin/plugin.json): the server, the skill and a rule).

### Claude Code

```bash
claude plugin marketplace add NewOdysseyConsulting/kleos-mcp
claude plugin install kleos@new-odyssey
```

Then run `/mcp` once and sign in to Kleos. The plugin bundles the server and the `kleos` skill. Without the plugin:

```bash
claude mcp add --transport http kleos https://app.getkleos.ai/mcp
```

### Hermes Agent

```bash
hermes plugins install kleos          # once it is in the Hermes plugin catalog
hermes plugins enable kleos
```

The [`hermes/`](hermes/) directory is the Agent Plugins v1 package Hermes installs; [`hermes/README.md`](hermes/README.md)
covers the OAuth sign-in (`hermes mcp login kleos`) and the API-key alternative. The package marks the server
**untrusted**, so Hermes asks before any tool that isn't read-only runs.

### Any agent, as a skill (skills.sh)

```bash
npx skills add NewOdysseyConsulting/kleos-mcp
```

Installs the `kleos` skill into the agents on your machine (Claude Code, Cursor, Codex, Gemini CLI and the others the
[skills CLI](https://skills.sh) supports). Add the server itself with your agent's own MCP settings.

### LobeHub

Install Kleos from the [LobeHub MCP marketplace](https://lobehub.com/mcp/newodysseyconsulting-kleos-mcp), or add a
remote MCP server by hand with the address `https://app.getkleos.ai/mcp` and OAuth.

### Any other MCP client

Add a remote (Streamable HTTP) MCP server with the address `https://app.getkleos.ai/mcp`. With OAuth, give only the
address; the client finds the authorization server on its own. The exact file and key names differ by client (`type`
may be `http`, `streamable-http` or `url`); [`mcp.json`](mcp.json) is the generic entry:

```json
{
  "mcpServers": {
    "kleos": {
      "type": "http",
      "url": "https://app.getkleos.ai/mcp"
    }
  }
}
```

Scripts that can't open a browser can send a Kleos API key as a bearer token instead (Growth and Pro plans, made under
Settings → API in Kleos):

```json
{
  "mcpServers": {
    "kleos": {
      "type": "http",
      "url": "https://app.getkleos.ai/mcp",
      "headers": { "Authorization": "Bearer ${KLEOS_API_KEY}" }
    }
  }
}
```

## For programs and directories

- Server card: [`/.well-known/mcp.json`](https://app.getkleos.ai/.well-known/mcp.json), also at
  [`/.well-known/mcp/server-card.json`](https://app.getkleos.ai/.well-known/mcp/server-card.json) and
  [`/mcp/server-card`](https://app.getkleos.ai/mcp/server-card).
- OAuth: [`/.well-known/oauth-authorization-server`](https://app.getkleos.ai/.well-known/oauth-authorization-server) (RFC 8414)
  and [`/.well-known/oauth-protected-resource`](https://app.getkleos.ai/.well-known/oauth-protected-resource) (RFC 9728).
  Scopes: `kleos` (read and change) and `kleos:read` (read only).
- Protocol: JSON-RPC 2.0 over POST to `https://app.getkleos.ai/mcp` (Streamable HTTP, JSON responses). A request
  without a token gets 401 with a `WWW-Authenticate` header that points at the metadata above.
- Developers: the REST API (`https://app.getkleos.ai/v1/…`, OpenAPI at `/v1/openapi.json`) and
  [webhooks](https://getkleos.ai/integrations/webhooks), both documented at [getkleos.ai/agents](https://getkleos.ai/agents).
- Disclosure: this repository is configuration only, no local code; every tool call goes to app.getkleos.ai under the
  signed-in person's own role, plan allowance and approval settings, and is recorded in the workspace's audit log.

## Privacy, terms and support

Kleos is made by [New Odyssey](https://new-odyssey.com). [Privacy](https://getkleos.ai/privacy) ·
[Terms](https://getkleos.ai/terms) · [Security](https://getkleos.ai/security) · [Support](https://getkleos.ai/contact) ·
support@getkleos.ai · [Partner programme](https://getkleos.ai/partners)

The files in this repository are MIT-licensed (see [LICENSE](LICENSE)); the Kleos service is governed by its Terms.
