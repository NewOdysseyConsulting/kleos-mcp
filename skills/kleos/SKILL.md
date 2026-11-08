---
name: kleos
description: Plan, draft, approve, schedule and report on a small business's marketing in Kleos (getkleos.ai) through its MCP server. Use when the user asks to plan their content month, write a social post or blog article, see or clear their review queue, publish an approved post, see marketing results, check whether ChatGPT or other AI assistants recommend their business, or read their buyers and buyer problems.
---

# Kleos

Kleos is an AI marketing team for small businesses. It plans a rolling month of posts, articles and newsletters in the
business's voice, writes them through its own checks, publishes what's approved on connected channels, and measures
Google, AI-assistant visibility and leads. This skill drives Kleos through its remote MCP server; Kleos does the work.

## When to use

- "Plan my month", "what's on the calendar", "move/publish that post"
- "Write a post about …", "draft an article on …"
- "What's waiting for my approval?", "approve the ones that are ready"
- "How did we do last month?", "did our posts bring leads?"
- "Does ChatGPT recommend us?", "who are our buyers and what problems do they have?"

Don't use it for businesses or accounts outside the user's Kleos workspace, or to write content Kleos isn't asked for.

## Setup

MCP server (Streamable HTTP): `https://app.getkleos.ai/mcp`

Either:

1. **API key** (scripts and headless agents). In Kleos, an admin or owner creates a key under **Settings → API**
   (Growth and Pro plans). Store it as `KLEOS_API_KEY` and send `Authorization: Bearer $KLEOS_API_KEY`. A read key acts
   as a viewer; a write key acts as an editor (plans, drafts, approves; can't publish).
2. **OAuth** (any plan). Add the server URL with no headers; the client discovers Kleos's OAuth metadata, opens the
   Kleos sign-in, and the user picks a workspace and approves. The agent then acts with the user's own role.

Example MCP server entry:

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

Leave out `headers` to use OAuth. Full setup and troubleshooting:
https://getkleos.ai/agents.

## Tools

| Tool | What it does |
|---|---|
| `list_businesses` | The workspace's businesses with id, website, Autopilot level and channels. Call first when the workspace may have more than one. |
| `get_month_plan` | The calendar for a month or a week, each item with channel, date, time and status. |
| `plan_my_month` | Plan the next 30 days on the named channels (`add` keeps what's planned, `rebuild` replans the rest). Runs in the background. |
| `draft_post` | Write a post on a topic, or about a buyer problem (`problem_id` from `get_buyers_and_problems`). |
| `draft_article` | Research and write a blog article on a topic or search phrase. Takes a few minutes. |
| `list_review_queue` | Drafts waiting for review, each ready to approve or needing a fix. |
| `approve_draft` | Approve a post, article or newsletter that's ready. |
| `schedule_post` | Publish an approved post on its connected channel now or at its planned time (needs an admin). |
| `get_results` | Posts published, impressions, clicks, engagements and leads over 7 to 365 days. |
| `check_ai_visibility` | Whether ChatGPT, Google AI Mode, Perplexity and the others on the plan name the business; `run_now` starts a new check. |
| `get_buyers_and_problems` | Buyers and their problems in priority order, with evidence. |

Results are short JSON with a one-line summary; pass ids from one tool's result into the next.

## Rules

- **Drafts wait for review.** A drafted post or article goes to the review queue. Only approve when the user asked you
  to, and only drafts `list_review_queue` marks ready; a flagged draft can't be approved until it's fixed in Kleos.
- **Publishing follows the business's settings.** After approval, Kleos schedules the post only if the business has
  auto-publish on; otherwise use `schedule_post` when the user asks. Never publish something the user didn't ask for.
- **Allowances are real.** Each drafted post, article and new AI-visibility check uses the plan's allowance. Don't
  draft speculatively or in bulk without the user's say-so.
- **Refusals are Kleos's own words.** If a tool refuses (role, billing, allowance, a paused channel, an article that
  already exists for that search), tell the user what it said; don't retry around it.
- **One workspace.** The connection covers one Kleos workspace. If a business isn't listed, it's in another workspace.

## Example requests

- "Plan my month for Acme on LinkedIn and the blog." → `list_businesses`, `plan_my_month`
- "Write a LinkedIn post about our buyers' biggest problem." → `get_buyers_and_problems`, `draft_post`
- "What's waiting for me? Approve what's ready." → `list_review_queue`, `approve_draft` (each ready draft)
- "Publish the approved pricing post now." → `get_month_plan`, `schedule_post`
- "How did last month go?" → `get_results` with `days: 30`
- "Is ChatGPT recommending us?" → `check_ai_visibility`
