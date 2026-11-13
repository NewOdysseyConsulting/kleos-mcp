# Awesome lists for Kleos

Which GitHub "awesome" lists could carry Kleos, each list's own contribution rules, and a ready entry in that list's
format. Checked 13 November 2026 with `gh api` (stars, last push, CONTRIBUTING, PR and issue templates, CI) and each
list's submission page. Ordered by fit. Nothing has been submitted, starred, forked or opened.

Every entry says only what is true today. Facts the lists ask for:

- **Endpoint:** `https://app.getkleos.ai/mcp`, Streamable HTTP. **Auth:** OAuth 2.1 with dynamic client registration,
  or an API key. Without credentials `initialize` gets `401` with `WWW-Authenticate: Bearer … resource_metadata=…`.
- **Repo:** `NewOdysseyConsulting/kleos-mcp`: MIT, configuration only, created 8 November 2026, no stars yet.
- **Glama connector:** `ai.getkleos/kleos` exists and its badge loads; on 13 November its page showed "Unhealthy",
  0% uptime, unclaimed (most likely no test credentials).
- **Not on npm yet:** the `kleos` CLI and `n8n-nodes-kleos`.

> **Name clash.** Another product also called Kleos (zurc.app, UGC videos) has the ClawHub skills `kleos` and
> `kleos-cli`, the npm package `kleos-cli` and the Glama connector `app.zurc/kleos`. Always link getkleos.ai or this
> repository, and never use the bare slug `kleos` on ClawHub (ours is `getkleos`, see [CLAWHUB.md](CLAWHUB.md)).

## 1. punkpeye/awesome-remote-mcp-servers (best fit)

- https://github.com/punkpeye/awesome-remote-mcp-servers · 3,128 stars · last push 6 Nov 2026.
- **Hosted commercial servers:** yes; it exists for them. "Public sign-up is fine. Private, invite-only, and
  single-tenant endpoints are not."
- **How:** a PR that edits `README.md`, with the PR template (Server, Endpoint, four checkboxes).
- **Rules:**
  - "The endpoint has to answer an MCP `initialize` request. CI checks this on every PR." A `401` with
    `WWW-Authenticate` counts, and CI marks it 🔐.
  - "Every entry carries the connector badge… PRs that add an entry without one are not merged." CI checks the Glama
    connector exists.
  - Three lines per entry; "Link the name to the product's homepage or docs — not a GitHub repo"; endpoint in
    backticks; after the auth marker, ` - ` and "a one-sentence description ending in a period, up to 120 characters".
  - "Alphabetical [order] within each category, case-insensitive."
  - "Star the repository. This is required — PRs are not merged unless the account opening them has starred the repo."
- **Section:** Marketing, between Jepto and Layrcake.
- **Entry** (description 112 characters):

```markdown
- [Kleos](https://getkleos.ai) `https://app.getkleos.ai/mcp`
  [![Kleos MCP connector](https://glama.ai/mcp/connectors/ai.getkleos/kleos/badges/score.svg)](https://glama.ai/mcp/connectors/ai.getkleos/kleos)
  🔐 - Plan a month of small-business content, draft and approve posts, schedule them, track results and AI visibility.
```

- **First:** claim the Glama connector and give Glama working test credentials so the badge shows a score.

## 2. jaw9c/awesome-remote-mcp-servers

- https://github.com/jaw9c/awesome-remote-mcp-servers · 1,186 stars · last commit 9 Nov 2026.
- **Hosted commercial servers:** yes. "Servers that are maintained by their underlying companies are preferred."
- **How:** a PR adding a row to the README table.
- **Rules:** the stated bar is "Official Support", "Production Ready", "Active Maintenance", "Proven track record of
  uptime and performance" and "Active user community and support channels". The PR description must say "Why the
  server meets our quality criteria" and "How it's being used in production", with security measures and support.
  Auth notation: `OAuth2.1` means dynamic client registration is supported; `API Key`, `Open` are the others.
- **Section:** the single table; `Social Media` is the closest existing category (VibeMarketing uses it). Between
  `| Jam |` and `| Kollektiv |` (roughly alphabetical).
- **Entry:**

```markdown
| Kleos | Social Media | `https://app.getkleos.ai/mcp` | OAuth2.1 & API Key | [Kleos](https://getkleos.ai) |
```

## 3. wong2/awesome-mcp-servers (through mcpservers.org)

- https://github.com/wong2/awesome-mcp-servers · 4,409 stars · last push 13 Jul 2026.
- **How:** the website form only: "We do not accept PRs. Please submit your MCP on the website:
  https://mcpservers.org/submit". PRs and issues are off on the repo.
- **Form:** name, category (includes Marketing), short description, link, optional MCP Registry name, "supports remote
  connections", contact email; plan Free ("Review within 2 weeks") or Premium ($39 one-time, 24-hour review, badge and
  dofollow link).
- **Values:** Name `Kleos`; Category `Marketing`; Link `https://getkleos.ai/agents`; Remote ticked; Description:
  `Hosted MCP server for Kleos, an AI marketing team for small businesses: plan a month of content, draft posts and articles, review, approve and schedule them, and read results and AI-assistant visibility.`
- **Not verified:** whether a free submission reaches the GitHub README or only the website.

## 4. hashgraph-online/awesome-ai-plugins

- https://github.com/hashgraph-online/awesome-ai-plugins · 1,040 stars · last push 13 Nov 2026.
- **Hosted commercial servers:** yes by precedent ("Karma.Domains MCP – MCP server: hosted Streamable HTTP server
  (OAuth or Pro API key)…").
- **How:** a PR with the template (repository, category and description, how you checked it works).
- **Rules:** `- [Extension Name](https://github.com/owner/repo) - Description (max 1 sentence).`; "Keep native MCP
  servers in the relevant topical category and begin their description with `MCP server: `"; their scanner reads the
  source repo and it "must establish a normalized score of at least 75 before merge"; alphabetical, case-insensitive;
  a first-time contributor's workflow waits for a maintainer.
- **Section:** Tools & Integrations, between Kindle Highlights and Kreuzberg.
- **Entry:**

```markdown
- [Kleos](https://github.com/NewOdysseyConsulting/kleos-mcp) - MCP server: hosted Streamable HTTP server (OAuth 2.1 or API key) for Kleos, an AI marketing team for small businesses, to plan, draft, approve, schedule and measure posts and articles.
```

## 5. 0xNyk/awesome-hermes-agent

- https://github.com/0xNyk/awesome-hermes-agent · 6,011 stars · last push 27 Oct 2026.
- **Hosted commercial:** allowed if disclosed (the form has "This resource depends on a paid service or API key").
  "open source is required for listing": the MIT repository is what's listed.
- **How:** the issue form only, https://github.com/0xNyk/awesome-hermes-agent/issues/new?template=resource-submission.yml
  ("Please do not open a PR directly to add a resource").
- **Rules:** a "Why now" sentence; "Description stays neutral — no pricing or promotional copy"; maintainers set the
  maturity label.
- **Values:** Category Plugins; name `kleos-mcp (Hermes plugin)`; URL the repository; author `NewOdysseyConsulting`;
  licence MIT; maturity `experimental`; tick both disclosures. Description:
  `Hermes Agent Plugins v1 package (hermes/) that connects Hermes to Kleos's hosted MCP server, an AI marketing team for small businesses: plan a month of content, draft, review, approve and schedule posts and articles, and read results. OAuth sign-in with hermes mcp login kleos, or an API key; the server is marked untrusted so Hermes asks before any tool that is not read-only.`
- **Caution:** `hermes plugins install kleos` works only once Kleos is in the Hermes plugin catalog, and the catalog PR
  to NousResearch/hermes-agent was closed. Write "Why now" around the route that works today (the `config.yaml` entry
  in [`hermes/README.md`](../hermes/README.md)).

## 6. VoltAgent/awesome-agent-skills (later)

- https://github.com/VoltAgent/awesome-agent-skills · 37,146 stars · last push 5 Nov 2026.
- **How:** a PR titled `Add skill: author/skill-name`.
- **Rules:** "Add your skill to the end of the relevant category"; "Description must be short, 10 words or fewer";
  author prefix in the name; "Brand new skills that were just created are not accepted. Give your skill time to mature
  and gain users before submitting."
- **Section:** Community Skills → Marketing, at the end.
- **Entry** (9 words):

```markdown
- **[NewOdysseyConsulting/kleos](https://github.com/NewOdysseyConsulting/kleos-mcp/tree/main/skills/kleos)** - Plan, draft, approve and schedule small-business marketing through Kleos
```

- **When:** once the skill shows some use (stars, installs).

## 7. ComposioHQ/awesome-claude-skills (low odds)

- https://github.com/ComposioHQ/awesome-claude-skills · 79,409 stars · last commit 24 Jul 2026.
- **How:** a PR titled `Add [Skill Name] skill`.
- **Rules:** "Add your skill in alphabetical order within the category"; "Follow the existing format exactly - no
  emojis, consistent punctuation". Its bot labels a PR ready only when README.md is the only file changed, the line
  sits between `## Skills` and `## Getting Started`, the link is external and the order is alphabetical; it blocks some
  words (for example `token`, `wallet`).
- **Section:** Business & Marketing, between Internal Comms and Lead Research Assistant.
- **Entry:**

```markdown
- [Kleos](https://github.com/NewOdysseyConsulting/kleos-mcp/tree/main/skills/kleos) - Plans, drafts, approves and schedules a small business's posts, articles and newsletters through the Kleos MCP server. *By [@NewOdysseyConsulting](https://github.com/NewOdysseyConsulting)*
```

## 8. VoltAgent/awesome-openclaw-skills (after ClawHub)

- https://github.com/VoltAgent/awesome-openclaw-skills · 53,742 stars · last push 30 Oct 2026.
- **How:** a PR titled `Add skill: author/skill-name`, with the ClawHub link in the description.
- **Rules:** "Every skill listed here must already be published on ClawHub"; "The skill's tests on ClawHub must be
  passing" and not "flagged as suspicious"; "10 words or fewer"; "Brand new skills are not accepted"; no links to
  clawskills.sh.
- **Section:** Marketing & Sales, at the end.
- **Entry** (after `getkleos` is on ClawHub):

```markdown
- [getkleos](https://clawhub.ai/<your-handle>/getkleos) - Plan, draft, approve and schedule small-business marketing content.
```

## 9. hesreallyhim/awesome-claude-code (not before 22 November, low odds)

- https://github.com/hesreallyhim/awesome-claude-code · 57,928 stars · last push 13 Nov 2026.
- **How:** the web issue form only,
  https://github.com/hesreallyhim/awesome-claude-code/issues/new?template=recommend-resource.yml, titled
  `[Resource]: <name>`. "Do not open a PR." "It is not possible to submit a resource recommendation using the `gh`
  CLI." "Resource recommendations must be created by human beings."
- **Rules:** at least 14 days old "AND show signs of active development", or 100 stars (so from 22 November 2026);
  one resource at a time; "If your project requires any form of signup or payment, this is a blocker for reviewing it
  — I can only recommend such a resource on the basis of community adoption signals"; specific to Claude Code; "not a
  sales pitch", no emojis.
- **Category:** Multi-Purpose. **Description:** `Claude Code plugin and marketplace that connects Claude Code to Kleos's hosted MCP server (OAuth 2.1), with skills for planning, drafting, approving and scheduling a small business's posts and articles.`
- **Verdict:** Kleos needs a sign-up; expect nothing until there are adoption signals.

## 10. Accept commercial tools but rarely merge

- **composio-community/awesome-claude-plugins** (1,992 stars; 3 of the last 40 closed PRs merged). A PR adding a
  plugin folder in their standard structure, under Integrations:
  `- [kleos](https://github.com/NewOdysseyConsulting/kleos-mcp) - Claude Code plugin that connects Claude to Kleos's hosted MCP server to plan, draft, approve and schedule a small business's posts and articles.`
- **alternbits/awesome-ai-marketing** (148 stars; last commit 27 Jul 2026; about six PRs open unmerged). Content
  Creation:

```markdown
- **[Kleos](https://getkleos.ai)**  
  AI marketing team for small businesses that plans a month of posts, articles and newsletters, drafts them for approval, schedules what is approved and reports results.
```

## Skip

| List | Stars / last push | Why not |
| --- | --- | --- |
| punkpeye/awesome-mcp-servers | 97,681 / 6 Nov 2026 | "This list is for servers with a public GitHub repository — something you install and run yourself. If your server is remote-only… it belongs in awesome-remote-mcp-servers instead." See §1. |
| appcypher/awesome-mcp-servers | 5,838 | Archived. |
| travisvn/awesome-claude-skills | 15,803 / 28 Apr 2026 | "If the skill you're submitting is based around a SaaS, it's unlikely to get reviewed and will be closed." Needs 10 stars; no AI-assisted PRs. |
| heilcheng/awesome-agent-skills | 6,386 / 9 Nov 2026 | "hosted MCP endpoints and SaaS sign-up pages are not accepted"; 20 stars and 14 days needed. |
| jqueryscript/awesome-claude-code | 551 / 10 Nov 2026 | Guidelines "Under Construction"; curated by stars (lowest plugin about 413). |
| quemsah/awesome-claude-plugins | 1,604 | Generated "Top 100" by stars; no submissions. |
| ccplugins/awesome-claude-code-plugins | 1,007 / 12 Aug 2026 | Issues and PRs switched off. |
| e2b-dev/awesome-ai-agents | 30,927 / 21 Aug 2026 | Links a Google form, but nothing added since Feb 2025 and recent PRs closed unmerged. |
| restyler/awesome-n8n | 3,003 / 20 Jan 2026 | Generated from npm download rankings; `n8n-nodes-kleos` isn't published. |
| amplifying-ai/awesome-generative-engine-optimization, best-of-ai/awesome-ai-seo | 541, 327 | Recent PRs unmerged; GEO/SEO tool lists rather than marketing teams. |
| mergisi/awesome-openclaw-agents | 4,067 | Lists agent templates (SOUL.md), not services or skills. |

## Owner's steps

Before any of them:

- [ ] Claim `ai.getkleos/kleos` on Glama and give it working test credentials (the reviewer workspace) until the page
      shows healthy and scored.
- [ ] Merge this repository's `agent-kit` branch, so the links to `skills/` resolve on `main`.

Now:

1. **punkpeye/awesome-remote-mcp-servers:** star the repo (required), fork, branch `add-kleos`, paste the §1 entry in
   Marketing between Jepto and Layrcake, open the PR `Add Kleos` with the template filled (Server: Kleos, Endpoint:
   `https://app.getkleos.ai/mcp`, four boxes ticked), check the CI labels (endpoint, 🔐, badge).
2. **jaw9c/awesome-remote-mcp-servers:** fork, add the §2 row between Jam and Kollektiv, PR `Add Kleos` saying: made
   by New Odyssey, in production, OAuth 2.1 with dynamic registration plus API keys, docs at getkleos.ai/agents,
   support at support@getkleos.ai.
3. **mcpservers.org:** fill https://mcpservers.org/submit with the §3 values; Free, or Premium for a 24-hour review.
4. **hashgraph-online/awesome-ai-plugins:** optionally run their scanner on this repo first
   (`pipx run plugin-scanner verify .`), fork, add the §4 line in Tools & Integrations, PR
   `Add Kleos to Tools & Integrations`, wait for workflow approval and a scan score of 75 or more.
5. **0xNyk/awesome-hermes-agent:** settle the Hermes install route first, then the issue form with the §5 values.
6. **ComposioHQ/awesome-claude-skills (optional):** README.md only, the §7 line, PR `Add Kleos skill` with the use
   case and an example prompt.

Later:

7. **VoltAgent/awesome-agent-skills** once the skill has stars and installs: §6 line, PR
   `Add skill: NewOdysseyConsulting/kleos`.
8. **VoltAgent/awesome-openclaw-skills** after `getkleos` is on ClawHub with passing checks: §8 line, PR
   `Add skill: <handle>/getkleos`, ClawHub link in the description.
9. **hesreallyhim/awesome-claude-code** from 22 November at the earliest: the web form (§9), filed by you, one
   resource.
10. Once `kleos` and `n8n-nodes-kleos` are on npm, look again for lists that take CLIs and n8n nodes.
