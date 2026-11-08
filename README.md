# Kleos MCP server

[Kleos](https://getkleos.ai) is an AI marketing team for small businesses. It plans a month of posts, articles and
newsletters in your voice, writes them through its own checks, publishes what you approve, and shows what's working on
Google and in AI assistants.

This repo holds the configuration, skill and rules for using Kleos from AI assistants and editors (Cursor, Claude,
ChatGPT, OpenClaw and any MCP client). The server itself is hosted by Kleos.

**Server (Streamable HTTP):** `https://app.getkleos.ai/mcp`

## Connect

- **Cursor:** add `.cursor/mcp.json` from this repo (or copy the entry below) to your project or global MCP settings.
- **Claude / ChatGPT:** add `https://app.getkleos.ai/mcp` as a custom connector.
- **Any MCP client:** use the entry in [`mcp.json`](mcp.json).

```json
{
  "mcpServers": {
    "kleos": { "type": "http", "url": "https://app.getkleos.ai/mcp" }
  }
}
```

On first use your client opens the Kleos sign-in: sign in, pick a workspace and approve. The assistant then acts with
your own role in Kleos. Scripts can use an API key instead (Settings → API in Kleos, Growth and Pro plans) sent as
`Authorization: Bearer <key>`.

## What you can ask

- "Plan my LinkedIn and X month around my buyers' problems."
- "Write a post about why invoices get paid late, and put it in my review queue."
- "What's waiting for my approval? Approve the ones that are ready."
- "How did last month's posts do? Did they bring any leads?"
- "Does ChatGPT recommend my business? What would change that?"

## Tools

`list_businesses` · `get_month_plan` · `plan_my_month` · `draft_post` · `draft_article` · `list_review_queue` ·
`approve_draft` · `schedule_post` · `get_results` · `check_ai_visibility` · `get_buyers_and_problems`

Details are in [`skills/kleos/SKILL.md`](skills/kleos/SKILL.md). Kleos applies your workspace's own roles, plan limits
and approval rules to everything an assistant does.

## Links

- Website: https://getkleos.ai
- Setup guide: https://getkleos.ai/agents
- Privacy: https://getkleos.ai/privacy
- Support: support@getkleos.ai
