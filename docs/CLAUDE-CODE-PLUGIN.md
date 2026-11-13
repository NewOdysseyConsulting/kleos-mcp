# Kleos in Claude Code: the plugin and its marketplace

This repository is a Claude Code plugin marketplace with one plugin in it. Claude Code reads
[`.claude-plugin/marketplace.json`](../.claude-plugin/marketplace.json) from the repository, finds the `kleos` plugin at
the repository root (`"source": "./"`), and installs:

- the Kleos MCP server, `https://app.getkleos.ai/mcp` ([`.mcp.json`](../.mcp.json)), signed in with OAuth;
- three skills from [`skills/`](../skills/):
  - `kleos:kleos`: which Kleos tool fits which job, and the publishing rules;
  - `kleos:linkedin-from-client-problems`: plan this week's LinkedIn entries from the business's client problems;
  - `kleos:answer-reddit-questions`: reply drafts for the Reddit threads Kleos found, for the person to post.

The marketplace is called `new-odyssey`, so the plugin's install id is `kleos@new-odyssey`. The format follows Claude
Code's marketplace reference (code.claude.com/docs/en/plugins/marketplace-reference, read 13 November 2026), and
`claude plugin validate .` passes on Claude Code 2.1.323.

## Install

You need a Kleos workspace ([getkleos.ai/start](https://getkleos.ai/start)). From your shell:

```bash
claude plugin marketplace add NewOdysseyConsulting/kleos-mcp
claude plugin install kleos@new-odyssey
```

Or from inside a session: `/plugin marketplace add NewOdysseyConsulting/kleos-mcp`, then `/plugin install kleos@new-odyssey`.

Then start a session, run `/mcp`, choose **kleos** and sign in: Kleos opens in the browser, you sign in with your usual
login, pick the workspace and choose what Claude Code may do (everything, or reading only). There is no key to paste.

Check it loaded:

```bash
claude plugin list            # kleos@new-odyssey, enabled
claude plugin details kleos   # the MCP server and the three skills
```

Try: "What's waiting for my approval in Kleos?", "Plan this week's LinkedIn from my client problems", or "Answer the
Reddit questions in my niche".

### Without the plugin

Only the server, no skills:

```bash
claude mcp add --transport http kleos https://app.getkleos.ai/mcp
```

### For a team

Put the marketplace in the project's `.claude/settings.json` so everyone who opens the project is offered it:

```json
{
  "extraKnownMarketplaces": {
    "new-odyssey": {
      "source": { "source": "github", "repo": "NewOdysseyConsulting/kleos-mcp" }
    }
  },
  "enabledPlugins": {
    "kleos@new-odyssey": true
  }
}
```

Each person still signs in to Kleos themselves with `/mcp`; the connection acts with their own role in the workspace.

## Updates

The plugin sets `version` in [`.claude-plugin/plugin.json`](../.claude-plugin/plugin.json), so people stay on the
version they installed until it changes. To get a new one:

```bash
claude plugin update kleos@new-odyssey
```

Auto-update is off by default for a marketplace like this one; each person can turn it on for `new-odyssey` (see
"Turn on auto-update" in Claude Code's plugin docs, code.claude.com/docs/en/plugins/host-marketplace).

**For maintainers:** bump `version` in `.claude-plugin/plugin.json` with every change to the skills or the server
config, or `claude plugin update` reports "already at the latest version" and nobody gets it. Run
`claude plugin validate --strict .` before pushing. Never rename the plugin (`kleos`) or the marketplace
(`new-odyssey`): existing installs are recorded under those names.

## Uninstall

```bash
claude plugin uninstall kleos@new-odyssey
claude plugin marketplace remove new-odyssey
```

Disconnect Claude Code from Kleos too, under **Settings → Integrations → Connected assistants** in Kleos.

## Anthropic's directory (owner steps, not done)

Our own marketplace works as soon as this repository is public; nothing needs submitting for that. A listing in
Anthropic's directory (claude.ai/directory) would also reach people on claude.ai and in Cowork. That is a submission
for the owner to make:

1. Sign in to claude.ai on a paid plan (Pro, Max or Ultra from a personal account; on Team or Enterprise, an Owner).
2. Run `claude plugin validate . --strict` in this repository and fix anything it reports. The directory runs more
   checks than the CLI (a safety scan and a policy review), so a clean local run isn't a guarantee.
3. Check claude.com/docs/plugins/pre-submission-checklist and the component support table
   (claude.com/docs/plugins/platform-support): MCP servers and skills load on claude.ai; check nothing else is needed.
4. Open the developer portal, claude.ai/directory/manage, and submit the plugin from
   `https://github.com/NewOdysseyConsulting/kleos-mcp`. The listing reads `icon`, `documentationUrl`, `supportUrl`,
   `privacyPolicyUrl` and `termsOfServiceUrl` from `plugin.json`; they are set.
5. The portal also offers submitting the MCP server as a connector; Kleos's connector listing is tracked separately
   (see the directory listings work in the Kleos repository).
