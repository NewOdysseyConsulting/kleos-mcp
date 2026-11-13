# Kleos MCP server

[Kleos](https://getkleos.ai) is an AI marketing team for small businesses. It plans a month of posts, articles and
newsletters in the business's voice, writes them through its own checks, publishes what's approved to the channels the
business has connected (LinkedIn, X, Facebook, Instagram, Google Business Profile, a blog, newsletters), and measures the
results: what was published, what it did, and whether AI assistants recommend the business when buyers ask them.

This repository is the public home of Kleos's MCP server, `https://app.getkleos.ai/mcp`: how to connect it to Claude,
ChatGPT, Cursor, Claude Code, Hermes Agent, LobeHub or any other MCP client, and what an assistant can do with it. The
server itself runs at Kleos; nothing here needs to be installed. It is also, from one tree, a Claude Code plugin and
marketplace (`.claude-plugin/`, `.mcp.json`), a Cursor plugin (`.cursor-plugin/`, `rules/`), a Hermes Agent package
(`hermes/`), a LobeHub manifest (`lhm.plugin.json`) and an agent skill (`skills/kleos/`).

**What's new (12 November 2026):** the server now has 31 tools. The 19 added since the first twelve are the planning
tools the Kleos director works with (the Results overview, tracked AI questions, searches, competitor pages, open
calendar entries, business facts, customer results, allowances and planning notes, plus five writes: plan an open
entry, set its brief, add an AI question, add a competitor, leave a note), product updates (read and record what the
business shipped) and buyer topics (what each buyer deals with day to day: read, confirm, add).

- Website: [getkleos.ai](https://getkleos.ai) · Start free, see your content plan first: [getkleos.ai/start](https://getkleos.ai/start)
  · Sign up: [app.getkleos.ai/signup](https://app.getkleos.ai/signup) · Pricing: [getkleos.ai/pricing](https://getkleos.ai/pricing)
- Documentation for assistants and developers, with curl examples and troubleshooting: [getkleos.ai/agents](https://getkleos.ai/agents)
  · [Kleos in Claude](https://getkleos.ai/claude) · [Kleos in ChatGPT](https://getkleos.ai/chatgpt) · [Integrations](https://getkleos.ai/integrations)
- Setup page for every assistant: [app.getkleos.ai/mcp](https://app.getkleos.ai/mcp)
- Server card (JSON): [app.getkleos.ai/.well-known/mcp.json](https://app.getkleos.ai/.well-known/mcp.json)

## What an assistant can do

31 tools, each working with the connected person's own role in the workspace, the plan's allowance and each business's
approval settings. Nothing is published unless someone with publishing permission schedules it or the business has
auto-publish on. Every tool is annotated the MCP way (`readOnlyHint`, `destructiveHint`, `idempotentHint`,
`openWorldHint`), so a client can tell a read from a change before calling; nothing in the catalog deletes anything.

In the tables, **Read** means the tool changes nothing in the workspace; **Writes** means it does (a draft, a plan, an
approval, a schedule, a note), and the lowest role that may use it is given. Reads need the viewer role.

### Reading the workspace

| Tool | Read or writes | What it does |
| --- | --- | --- |
| `list_businesses` | Read | See the businesses in your workspace |
| `get_month_plan` | Read | Read the calendar for a month or a week |
| `list_review_queue` | Read | See what's waiting for your approval |
| `get_buyers_and_problems` | Read | Read your buyers and the problems they have |
| `get_business_facts` | Read | Read the business's facts, claims and words |
| `get_customer_results` | Read | Read the customer results you can use as proof |
| `get_allowances` | Read | See what's left of the plan this period |

### Planning

| Tool | Read or writes | What it does |
| --- | --- | --- |
| `plan_my_month` | Writes (editor; admin to change which channels are on) | Plan the next 30 days on the channels you choose |
| `get_open_slots` | Read | See the calendar entries still open to plan |
| `plan_calendar_entry` | Writes (editor) | Plan an open calendar entry with its reason and brief |
| `set_slot_brief` | Writes (editor) | Rewrite the reader brief of a planned entry |
| `get_director_notes` | Read | Read the notes from past weeks' planning |
| `record_note` | Writes (editor) | Leave a note on the planning record |

### Drafting

| Tool | Read or writes | What it does |
| --- | --- | --- |
| `draft_post` | Writes (editor) | Write a post on a topic or a buyer problem |
| `create_post` | Writes (editor; admin to schedule or post now) | Post your exact words, no AI |
| `draft_article` | Writes (editor) | Research and write a blog article |

### Approving and publishing

| Tool | Read or writes | What it does |
| --- | --- | --- |
| `approve_draft` | Writes (reviewer) | Approve a draft that's ready |
| `schedule_post` | Writes (admin) | Publish an approved post now or at its time |

### Results and AI visibility

| Tool | Read or writes | What it does |
| --- | --- | --- |
| `get_results` | Read | See posts published, impressions, clicks and leads |
| `check_ai_visibility` | Read; with `run_now`, starts a new check that uses the plan's allowance | See if ChatGPT, Google AI Mode and Perplexity recommend you |
| `get_results_overview` | Read | Read the Results page in one call |
| `get_ai_questions` | Read | See which buyer questions AI assistants answer without you |
| `add_ai_question` | Writes (editor) | Track a new buyer question in the weekly AI check |
| `get_searches` | Read | See the buyer searches with no article yet |
| `get_competitor_pages` | Read | See what competitors publish and where you have no answer |
| `add_competitor` | Writes (editor) | Add a brand the assistants named to your competitors |

### Product updates

| Tool | Read or writes | What it does |
| --- | --- | --- |
| `get_product_updates` | Read | Read what changed in your product |
| `record_product_update` | Writes (editor) | Tell Kleos what you shipped |

### Buyer topics

| Tool | Read or writes | What it does |
| --- | --- | --- |
| `get_buyer_topics` | Read | Read what each buyer deals with day to day |
| `confirm_buyer_topic` | Writes (editor) | Confirm or hide a day-to-day topic |
| `add_buyer_topic` | Writes (editor) | Add something a buyer deals with day to day |

The planning, results and AI-visibility reads above, with the five planning writes, are the same tools the Kleos
director (the agent inside Kleos that plans each business's week) works with: one definition, served to the director
in-process and to outside assistants over MCP. Each tool's full description, input and output schema comes from the
server's `tools/list` after sign-in; the names, descriptions and annotations are also on the
[server card](https://app.getkleos.ai/.well-known/mcp.json).

Try:

- "What's on my Kleos calendar this week?"
- "Which drafts are waiting for my approval in Kleos?"
- "How did my content do in Kleos over the last 30 days?"
- "Plan my LinkedIn and X month around my buyers' problems."
- "Does ChatGPT recommend my business? What would change that?"
- "Which buyer questions are the AI assistants answering without us? Plan the open slots this week to close them."
- "We shipped invoice reminders yesterday. Record it in Kleos."

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

Guide: [getkleos.ai/claude](https://getkleos.ai/claude).

### ChatGPT

1. In ChatGPT, open **Settings → Apps & Connectors → Advanced settings** and switch on **Developer mode**.
2. In **Apps & Connectors**, choose **Create**. Name it **Kleos**, paste `https://app.getkleos.ai/mcp` and choose **OAuth**.
3. Sign in to Kleos, pick the workspace and choose **Allow**. In a chat, add Kleos from the tools menu.

Guide: [getkleos.ai/chatgpt](https://getkleos.ai/chatgpt).

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

Then run `/mcp` once and sign in to Kleos. The plugin bundles the server and three skills: `kleos`, plus two outcome
skills, `linkedin-from-client-problems` and `answer-reddit-questions` (see below). Install, team setup and updates:
[`docs/CLAUDE-CODE-PLUGIN.md`](docs/CLAUDE-CODE-PLUGIN.md). Without the plugin:

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

Add Kleos from its [LobeHub page](https://lobehub.com/mcp/newodysseyconsulting-kleos-mcp), or add a remote MCP server
by hand with the address `https://app.getkleos.ai/mcp` and OAuth.

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
Settings → API in Kleos). A read key acts as a viewer, a write key as an editor:

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

## Skills

| Skill | What it does |
| --- | --- |
| [`kleos`](skills/kleos/SKILL.md) | Which Kleos tool fits which job, and the rules (say what will be published first; never publish unless asked) |
| [`linkedin-from-client-problems`](skills/linkedin-from-client-problems/SKILL.md) | Plan this week's LinkedIn entries from the business's client problems (or the person's own client notes), each with a reason and a brief; drafts wait for review |
| [`answer-reddit-questions`](skills/answer-reddit-questions/SKILL.md) | Reply drafts for the Reddit threads Kleos found where the business's buyers ask for help. Threads come only from Kleos's tools (found through Google results); the person opens each thread and posts the reply themselves |
| [`getkleos`](clawhub/getkleos/SKILL.md) (ClawHub) | The popular jobs through Kleos's public API and the `kleos` CLI, for OpenClaw; the API key stays in the environment or the CLI's config, never in the conversation. See [`docs/CLAWHUB.md`](docs/CLAWHUB.md) |

The first three use the MCP server; Claude Code, Cursor and Hermes load them from this repository.

## For programs and directories

- Server card: [`/.well-known/mcp.json`](https://app.getkleos.ai/.well-known/mcp.json), also at
  [`/.well-known/mcp/server-card.json`](https://app.getkleos.ai/.well-known/mcp/server-card.json) and
  [`/mcp/server-card`](https://app.getkleos.ai/mcp/server-card). It lists every tool with its title, description and
  annotations.
- OAuth: [`/.well-known/oauth-authorization-server`](https://app.getkleos.ai/.well-known/oauth-authorization-server) (RFC 8414)
  and [`/.well-known/oauth-protected-resource`](https://app.getkleos.ai/.well-known/oauth-protected-resource) (RFC 9728).
  Authorization code with PKCE (S256), refresh tokens, dynamic client registration and client ID metadata documents.
  Scopes: `kleos` (read and change) and `kleos:read` (read only).
- Protocol: JSON-RPC 2.0 over POST to `https://app.getkleos.ai/mcp` (Streamable HTTP, JSON responses; protocol
  versions 2025-03-26, 2025-06-18 and 2025-11-25). A request without a token gets 401 with a `WWW-Authenticate` header
  that points at the metadata above.
- Developers: the REST API (`https://app.getkleos.ai/v1/…`, OpenAPI at `/v1/openapi.json`) and
  [webhooks](https://getkleos.ai/integrations/webhooks), both documented at [getkleos.ai/agents](https://getkleos.ai/agents).
- Disclosure: this repository is configuration only, no local code; every tool call goes to app.getkleos.ai under the
  signed-in person's own role, plan allowance and approval settings, and is recorded in the workspace's audit log.

## Distribution kit

For the people listing Kleos in directories and writing about it:

- [`docs/CLAUDE-CODE-PLUGIN.md`](docs/CLAUDE-CODE-PLUGIN.md): the plugin marketplace, installing, updating, and
  Anthropic's directory
- [`docs/CLAWHUB.md`](docs/CLAWHUB.md): the ClawHub skill, how the key stays out of the model's context, publishing
- [`docs/AWESOME-LISTS.md`](docs/AWESOME-LISTS.md): the awesome lists that fit, each one's rules and a ready entry
- [`docs/CHATGPT-CONNECTOR.md`](docs/CHATGPT-CONNECTOR.md): what OpenAI asks for to list Kleos in ChatGPT
- [`docs/articles/`](docs/articles/): drafts of "Kleos + Claude Code", "Kleos + ChatGPT", "Kleos + Hermes" and
  "Kleos + OpenClaw"

## Privacy, terms and support

Kleos is made by [New Odyssey](https://new-odyssey.com). [Privacy](https://getkleos.ai/privacy) ·
[Terms](https://getkleos.ai/terms) · [Security](https://getkleos.ai/security) · [Support](https://getkleos.ai/contact) ·
support@getkleos.ai · [Partner programme](https://getkleos.ai/partners)

The files in this repository are MIT-licensed (see [LICENSE](LICENSE)); the Kleos service is governed by its Terms.
