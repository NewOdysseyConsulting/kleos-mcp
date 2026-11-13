# Kleos in ChatGPT: what OpenAI asks for

What OpenAI's directory needs before Kleos's MCP server can be listed in ChatGPT, read on 13 November 2026 from
OpenAI's developer docs ("Upload and submit your plugin" and "Plugin guidelines" under developers.openai.com/apps-sdk,
which now call these listings *plugins*). Against each item: where Kleos already has it, and what is still open.

The upload package itself (manifest, MCP config, icons, test cases) is in the Kleos repository at
`apps/clio/listing/chatgpt/` with its own step-by-step README; `pnpm -F clio listing:chatgpt` builds the ZIP. This page
doesn't repeat that package; it checks it against today's requirements.

Anyone can already use Kleos in ChatGPT without a listing, through developer mode (see the README's ChatGPT section).
The listing is what makes Kleos findable in ChatGPT's directory.

## What OpenAI asks for

### Who can submit

- "Organization owners can submit; other members need Apps Management Write."
- Individual or business verification in the OpenAI organization's settings: the listing shows the verified name.
- Submissions come from platform.openai.com/plugins → "Upload new or existing plugin"; a remote MCP server goes in
  "With MCP".

**Kleos:** owner step. Verify as the business (New Odyssey) so the listing doesn't show a personal name.

### Listing fields

| Field | OpenAI's limit | Kleos's package today |
| --- | --- | --- |
| Display name | required, at most 30 characters | `Kleos` |
| Short description | required, **at most 30 characters** ("the limits here are for public submission; a package upload can accept longer draft text") | `Plan, write, approve and publish a small business's marketing`: **61 characters, too long.** Suggested: `Your AI marketing team` (22) or `Plan, write and publish posts` (29) |
| Long description | required, at most 4,000 characters: tasks, intended users, limitations | 1,264 characters; no prices or offers |
| Developer name, category | category from the dashboard's list ("such as Productivity or Developer Tools") | New Odyssey; `Productivity` (take Marketing if offered) |
| Capabilities | at most 20 labels, 120 characters each | four labels |
| Starter prompts | up to three, 128 characters each, no @mentions | three, 38 to 53 characters |
| Icons | square, at least 48×48, PNG/JPEG/WebP/SVG, 5 MiB max | 256×256 and 512×512 PNG |
| Website, support, privacy, terms | all four required, HTTPS | getkleos.ai, getkleos.ai/contact, /privacy, /terms |
| Release notes | what the submitted version changes | **says "twelve tools"; the server has 31.** Update before uploading |

### The MCP server

- One MCP server per listing, at a stable public HTTPS address: `https://app.getkleos.ai/mcp`.
- **Domain verification:** the exact token, as plain text, at `/.well-known/openai-apps-challenge`. Kleos serves
  `OPENAI_APPS_CHALLENGE` there once the owner sets it on the web service (Render); until then it answers 404.
- **Auth:** none, OAuth, or mixed. "API-key and bearer-token flows aren't supported in the portal." Client
  registration may be a registered client, dynamic client registration or a client ID metadata document (CIMD).
  Kleos supports both DCR and CIMD; the package uses `cimd`. Any client secret goes into the portal, never the package.
- Public submission rejects non-empty server `headers` (the package has none) and a protected server set to auth
  `none`.
- After publication OpenAI rescans the server daily; tool changes that pass go live without a new upload. Changing the
  server address needs OpenAI's support.

### Tools

- "Tool names must be unique", plain verbs, and every tool needs a description that "explicitly and accurately explains
  its purpose and behavior". Kleos's 31 tool names are verbs (`draft_post`, `approve_draft`…), each with a description.
- "Set readOnlyHint, destructiveHint, and openWorldHint to explicit boolean values." All 31 tools on the server card do
  (checked 13 November 2026). `readOnlyHint: false` for write actions "such as posting messages": `create_post` and
  `schedule_post` are false with `openWorldHint: true`.
- **One to look at:** `approve_draft` has `openWorldHint: false`, but when a business has auto-publish on, an approval
  schedules the post to its connected channel. A reviewer may expect `true`. Decide before submitting.
- Irreversible actions need human confirmation. Nothing in Kleos's tools deletes anything. Publishing needs an admin
  (`schedule_post`, or `create_post` with `schedule` or `now`), and the write tools carry `readOnlyHint: false` so
  ChatGPT can tell them from reads.
- Model-readable fields "must not" steer the model into choosing the plugin, and listings must not compare with or
  disparage other products. The descriptions describe Kleos's own tools only.

### Sign-in and the reviewer's account

- "Provide a login and password for a fully featured demo account that includes sample data." It must work without
  MFA, email or SMS codes, magic links or a private network; credentials go in the dashboard's review details, never in
  the package. "Plugins that require additional login steps, such as a new account sign-up or 2FA" are rejected.
- **Kleos:** the production reviewer workspace ("Kleos review", sample business Maple & Vine Bookkeeping) with a
  fixed-code sign-in for reviewer addresses (`REVIEWER_EMAILS`, `REVIEWER_CODE` on the accounts service). The code is
  typed into the dashboard by the owner; it is never written into a repository.

### Test cases and a video

- Five positive cases (scenario, prompt, expected tools, expected result), each run with the test account before
  submitting, and three negative cases (where the plugin shouldn't act, and the expected refusal or clarification).
  The package has five and three. They cover the first twelve tools; that's enough for review, but re-run each one
  against today's server before submitting.
- A demo video of the positive cases that a reviewer can open (Loom, unlisted YouTube, Drive). **Not made yet.**

### Policy points that touch Kleos

- **Commerce:** "Plugins must not display subscription plans, initiate new subscriptions, or promote upgrades", and
  "Do not advertise pricing, subscriptions, free trials, discounts, or promotions." Over OAuth, the tools don't sell
  anything; two places to keep an eye on:
  - the API-key refusal ("…or upgrade under Billing.") only reaches clients that send an API key, which the portal
    doesn't allow, so ChatGPT never sees it;
  - `get_allowances` reports the plan's name and whether the workspace is on a trial, as information. Keep tool
    results from suggesting an upgrade.
  The commerce declaration in the package says no commerce.
- **Privacy:** a published privacy policy that names the categories of personal data collected; collect the minimum;
  never request chat history. Kleos's tools take only the fields each job needs.
- **"Trial or demo plugins will not be accepted"**: this is about the listing itself being a demo, not about Kleos
  offering a free start. Kleos is a production service.
- **General audiences**, including 13 to 17 year olds; nothing in Kleos targets children.
- **Third parties:** no scraping without authorization and no unofficial connectors. Kleos's server is Kleos's own;
  publishing goes through each business's own connected accounts.

## Owner's steps, in order

1. Fix the two package fields in `apps/clio/listing/chatgpt/plugin.json` (Kleos repository): `shortDescription` to 30
   characters or fewer, and `publication.release_notes` to the 31-tool server. Or change them in the dashboard after
   uploading. Decide on `approve_draft`'s `openWorldHint`.
2. Verify the OpenAI organization as New Odyssey.
3. Check the reviewer sign-in works on production (accounts service has `REVIEWER_EMAILS` and `REVIEWER_CODE`; the
   "Kleos review" workspace exists).
4. Record the demo video of the five positive cases with the reviewer account.
5. Follow `apps/clio/listing/chatgpt/README.md` steps 1 to 7: domain challenge (`OPENAI_APPS_CHALLENGE` on Render and
   redeploy), upload the ZIP, connect and scan the server, review details, submit with the attestations, publish after
   approval.

## Not verified

- The exact dashboard category list (the docs name only examples).
- Whether OpenAI's reviewers accept a fixed sign-in code as the "password" for an account whose normal sign-in is an
  email link. The docs rule out magic links and codes sent by email or SMS; Kleos's reviewer code is fixed and no email
  is sent, but say so plainly in the review instructions.
