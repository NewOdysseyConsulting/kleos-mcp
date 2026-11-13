---
title: "Kleos + Hermes Agent: a marketing team your agent can ask, with you approving every change"
description: "Connect Hermes Agent to Kleos's hosted MCP server. Hermes reads your calendar, results and buyers freely, and asks you before anything in your workspace changes."
status: draft
before_publishing:
  - If Kleos is in the Hermes plugin catalog by then, lead with `hermes plugins install kleos` and keep the config.yaml
    route as the alternative. Today the catalog entry isn't merged, so this draft uses the config.yaml route.
  - The OAuth sign-in (`hermes mcp login kleos`) was not driven in our own Hermes test, which used an API key; run it
    once before publishing.
  - Re-run each example against a real workspace.
---

# Kleos + Hermes Agent: a marketing team your agent can ask, with you approving every change

Hermes Agent is good at long-running work across your tools. Kleos is an AI marketing team for small businesses: it
plans a month of posts, articles and newsletters in your voice, writes them through its own checks, publishes what you
approve and measures the results. Put together, Hermes can look after your marketing the way it looks after the rest of
your work, and Kleos does the writing.

## Connect it

Add Kleos to your Hermes profile's `config.yaml`:

```yaml
mcp_servers:
  kleos:
    url: https://app.getkleos.ai/mcp
    auth: oauth
    trust: untrusted
```

Then run `hermes mcp login kleos`. Kleos opens in the browser: sign in with your usual login, pick your workspace and
approve. Hermes keeps and refreshes the sign-in; it acts with your own role in Kleos.

On the Growth and Pro plans you can use a Kleos API key instead (made under **Settings → API**), set as
`KLEOS_API_KEY` in your Hermes profile's environment. A read key can only read; a write key can draft and approve but
not publish.

You need a Kleos workspace: [start one free at getkleos.ai/start](https://getkleos.ai/start).

## Why `trust: untrusted` is the point

With the server marked untrusted, Hermes runs Kleos's read-only tools (your calendar, review queue, results, buyers,
AI visibility) without interrupting you, and asks before every tool that changes something: planning, drafting,
approving, scheduling. You answer once, for the session, or always. In cron jobs and other runs where nobody can
answer, Hermes refuses those tools. In our own test, Hermes listed all 31 tools, ran the reads, and refused a write
in an unattended run, with nothing changed in Kleos.

## Things to ask

- "What's waiting for my approval in Kleos?"
- "Plan this week's LinkedIn from my client problems." (Hermes reads your buyers' problems, shows you a plan, and
  plans each open LinkedIn entry once you agree.)
- "Answer the Reddit questions in my niche." (Hermes lists the Reddit threads Kleos found where your buyers ask for
  help, and drafts replies for you to read and post yourself.)
- "How did last month's posts do? Did they bring any leads?"
- "Does ChatGPT recommend my business? What would change that?"
- "We shipped invoice reminders yesterday. Record it in Kleos." (Kleos opens your next newsletter with it and plans
  one announcement post.)

## What Kleos keeps for itself

Every call goes to Kleos, which applies your role in the workspace, your plan's allowances and each business's approval
settings, and writes every change to the workspace's audit log. Drafts wait for review; nothing is published unless
someone with publishing permission schedules it, or the business has auto-publish on. Disconnect at any time under
**Settings → Integrations → Connected assistants**.

The package, with both sign-in routes, is in the
[kleos-mcp repository](https://github.com/NewOdysseyConsulting/kleos-mcp/tree/main/hermes).

---

Kleos: an AI marketing team for small businesses. [Start free](https://getkleos.ai/start) ·
[Docs for agents](https://getkleos.ai/agents)
