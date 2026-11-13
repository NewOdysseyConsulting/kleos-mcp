---
title: "Kleos + Claude Code: run your marketing from the terminal"
description: "Install the Kleos plugin in Claude Code, sign in once, and ask Claude to plan your week, draft posts in your voice and clear your review queue. Nothing is published until you say so."
status: draft
before_publishing:
  - The agent-kit branch of NewOdysseyConsulting/kleos-mcp is merged, so the plugin has the three skills.
  - Re-run each example below against a real workspace and check the answers still match.
---

# Kleos + Claude Code: run your marketing from the terminal

If you already spend your day in Claude Code, your marketing doesn't need another tab. Kleos is an AI marketing team
for small businesses: it plans a month of posts, articles and newsletters in your voice, writes them through its own
checks, publishes what you approve to the channels you connected, and tells you what worked. The Kleos plugin puts all
of that inside Claude Code.

## Set it up in two commands

```bash
claude plugin marketplace add NewOdysseyConsulting/kleos-mcp
claude plugin install kleos@new-odyssey
```

Start a session, type `/mcp`, choose **kleos** and sign in. Kleos opens in your browser: sign in with your usual login,
pick your workspace, and choose whether Claude Code may make changes or only read. There's no API key to copy anywhere.

You need a Kleos workspace. [Start one free at getkleos.ai/start](https://getkleos.ai/start) and see your content plan
first.

## What you get

- **The Kleos MCP server**, with 31 tools: your calendar, your buyers and their problems, drafting, your review queue,
  approving, scheduling, results and whether AI assistants recommend you.
- **Three skills** that tell Claude how to use them well:
  - `kleos`: which tool fits which job, and the rule that nothing is published unless you ask;
  - `linkedin-from-client-problems`: plans this week's LinkedIn posts around the problems your clients really have;
  - `answer-reddit-questions`: drafts replies to the Reddit threads Kleos found where your buyers ask for help, for
    you to read and post yourself.

## Try these

**"What's waiting for my approval in Kleos?"**
Claude lists the drafts in your review queue, and says which are ready to approve and which need a fix first.

**"Plan this week's LinkedIn from my client problems."**
Claude reads your buyers' problems, your business facts and your customer results, finds this week's open LinkedIn
entries, and shows you a short plan: one problem per post, with the reason. When you say yes, it plans each entry with
a brief Kleos's writer follows. Paste notes from a client call and it matches them to the problems Kleos already knows,
and asks before adding anything new.

**"Answer the Reddit questions in my niche."**
Kleos finds Reddit threads where your buyers describe their problems (through Google's results, titles and short
snippets). Claude lists the ones it found, drafts a helpful reply for each one you pick, and leaves the posting to you:
open the thread, read it, adjust the reply and post it from your own account.

**"How did my content do over the last 30 days?"**
Posts published, impressions, clicks and leads, straight from Kleos.

**"Does ChatGPT recommend us? Plan the open slots this week to close the gaps."**
Claude reads which buyer questions AI assistants answer without you, then plans this week's open calendar entries
around them, each with the question it comes from.

## What stays in your hands

- Drafts go to your review queue. "Write a post" means a draft, not a published post.
- Before anything is published, Claude tells you the text, the channel and the time, and waits for your yes.
- Every action uses your own role in the workspace and your plan's allowances. Each draft uses one post from your
  plan, so Claude asks before drafting in bulk.
- You can connect read-only, and disconnect at any time under **Settings → Integrations → Connected assistants**.

## For teams

Add the marketplace to your project's `.claude/settings.json` and everyone who opens the project is offered the
plugin; each person still signs in with their own Kleos login. The full guide, with updating and uninstalling, is in
[docs/CLAUDE-CODE-PLUGIN.md](https://github.com/NewOdysseyConsulting/kleos-mcp/blob/main/docs/CLAUDE-CODE-PLUGIN.md).

---

Kleos: an AI marketing team for small businesses. [Start free](https://getkleos.ai/start) ·
[Docs for agents](https://getkleos.ai/agents)
