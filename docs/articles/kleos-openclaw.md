---
title: "Kleos + OpenClaw: let your agent run your marketing, with Kleos doing the writing"
description: "Two ways to connect OpenClaw to Kleos: sign in to Kleos's MCP server with OAuth, or install the getkleos skill and the kleos CLI. Your key never enters the conversation."
status: draft
before_publishing:
  - The kleos CLI is on npm (`npm view kleos` answers) and the getkleos skill is published on ClawHub; until then, drop
    the "Or: the getkleos skill" section.
  - Neither route has been run end to end in OpenClaw by us. The commands come from OpenClaw's docs (docs.openclaw.ai,
    MCP and skills pages, read 13 November 2026). Run both once and correct anything that differs.
  - Re-run each example against a real workspace.
---

# Kleos + OpenClaw: let your agent run your marketing, with Kleos doing the writing

OpenClaw is the agent you already trust with your inbox and your files. Kleos is an AI marketing team for small
businesses: it plans a month of posts, articles and newsletters in your voice, writes them through its own checks,
publishes what you approve and tells you what worked. Connect the two and your agent can keep your marketing moving
without you opening another app.

One thing first: there's more than one product called Kleos. This one is [getkleos.ai](https://getkleos.ai).

## Connect with your Kleos login (any plan)

Kleos runs a hosted MCP server at `https://app.getkleos.ai/mcp`. Add it with OAuth and sign in:

```bash
openclaw mcp set kleos '{"url":"https://app.getkleos.ai/mcp","transport":"streamable-http","auth":"oauth"}'
openclaw mcp login kleos
```

Kleos opens in the browser: sign in with your usual login, pick your workspace and approve. OpenClaw keeps the
sign-in. Check it with `openclaw mcp doctor kleos --probe`. Your agent now has Kleos's 31 tools, with your own role in
the workspace.

## Or: the getkleos skill and the kleos CLI (Growth and Pro)

If you'd rather your agent use Kleos's public API from the command line, install the CLI and the skill:

```bash
npm i -g kleos
clawhub install getkleos
```

Then give OpenClaw your Kleos API key (made under **Settings → API** in Kleos) as the skill's key in
`~/.openclaw/openclaw.json`, under `skills.entries.getkleos.apiKey`, or run `kleos login` yourself in a terminal. The
CLI reads the key on its own. The skill tells your agent never to ask for it, print it or read where it's kept, so the
key never appears in the conversation.

## Things to ask

- "What's waiting for my approval in Kleos?"
- "Write a post about why clients pay late, and put it in my review queue."
- "Post this exactly as I wrote it, on LinkedIn tomorrow at 9." (Your words go in unchanged, with no AI rewriting.)
- "How did last month's posts do?"
- "Does ChatGPT recommend us when people ask our buyers' questions?"

## You approve, Kleos publishes

- Drafts wait in your review queue. "Write a post" never means "publish a post".
- Before anything is published, the agent says the text, the channel and the time, and waits for your yes.
- Every request uses your role and your plan's allowances, the same as clicking in Kleos, and lands in the workspace's
  audit log.
- Disconnect at any time under **Settings → Integrations → Connected assistants** (or revoke the key under
  **Settings → API**).

Setup for every client, with troubleshooting: [getkleos.ai/agents](https://getkleos.ai/agents).

---

Kleos: an AI marketing team for small businesses. [Start free](https://getkleos.ai/start) ·
[Docs for agents](https://getkleos.ai/agents)
