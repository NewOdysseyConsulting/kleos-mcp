# The ClawHub skill (`clawhub/getkleos/`)

[`clawhub/getkleos/SKILL.md`](../clawhub/getkleos/SKILL.md) is one skill file for ClawHub, the public registry of
OpenClaw skills. It drives Kleos's public API through the `kleos` CLI and covers the popular jobs only: the review
queue, the calendar, drafting a post, posting the person's own words, approving, scheduling, recording a post that went
out elsewhere, asking for an article, results, AI visibility and plan usage. Everything else goes through
`kleos api <method> <path>`.

## How the key stays out of the model's context

- The CLI reads the key itself: `KLEOS_API_KEY` from the environment first, then the key `kleos login` saved in the
  person's config folder (mode 0600). The skill never needs to see it and tells the agent never to ask for it, print it,
  read the config file, or run `kleos login --key`.
- In OpenClaw, the skill declares `primaryEnv: KLEOS_API_KEY`, so the person sets the key once as
  `skills.entries.getkleos.apiKey` in `~/.openclaw/openclaw.json`. OpenClaw applies it to the host process for the agent
  run only, then restores the environment (docs.openclaw.ai/tools/skills).
- `KLEOS_API_KEY` is declared as optional (`envVars`, `required: false`), not in `requires.env`, because a person who
  ran `kleos login` doesn't need it. ClawHub checks that what a skill declares matches what it uses.
- With no key (or on the Starter plan, where API keys aren't offered), the skill points at OAuth through Kleos's MCP
  server: `openclaw mcp set kleos '{"url":"https://app.getkleos.ai/mcp","transport":"streamable-http","auth":"oauth"}'`
  and `openclaw mcp login kleos`.

## The format, and what was checked

Read on 13 November 2026 from docs.openclaw.ai (ClawHub, "Skill format", "Skills"):

- A skill is a folder with `SKILL.md` (YAML frontmatter, then Markdown). `name` matches the folder, 1 to 64 lowercase
  letters, digits or hyphens; `description` is the summary in ClawHub's UI and search.
- `metadata.openclaw` holds `requires.bins`, `requires.env`, `primaryEnv`, `envVars`, `homepage`, `install` and the
  rest. The nested JSON-style block is the form OpenClaw's own examples use.
- Publishing: `clawhub skill publish <folder> --slug <slug> --name <name> --version <semver>`; every publish is a new
  version. ClawHub needs a GitHub account old enough to pass its upload gate, scans each release, and may hold one.
- Skills on ClawHub are licensed MIT-0; there are no paid skills.

Not verified: the `install` entry's field names for the `node` kind. OpenClaw's docs show a `brew` example
(`id`, `kind`, `formula`, `bins`, `label`) and name `node` as a kind without an example; the skill uses `package` for
the npm package name. If ClawHub's scan flags it, check the current docs and adjust.

## Why the slug is `getkleos`

Another product also called Kleos (zurc.app, short UGC videos) already publishes ClawHub skills named `kleos` and
`kleos-cli` (and the npm package `kleos-cli`). The skill is named after the domain to keep them apart, and its first
lines say which Kleos it is.

## Owner's steps, in order

1. **Publish the CLI to npm first.** The skill needs the `kleos` command, and `npm view kleos` still returns 404 on 13
   November 2026. The CLI's own `SUBMISSION.md` (Kleos repository, `integrations/cli/`) has the steps: the public repo
   `NewOdysseyConsulting/kleos-cli`, an npm account and token, the first release.
2. Install it and try the skill once by hand: `npm i -g kleos`, `kleos login`, `kleos api GET /v1/me`, `kleos review`.
3. Install the ClawHub CLI and sign in with the GitHub account that will own the listing (`clawhub login`; check
   `clawhub --help`, the docs list the command set).
4. From this repository: `clawhub skill publish ./clawhub/getkleos --slug getkleos --name "Kleos (getkleos.ai)" --version 1.0.0 --changelog "First release: the popular Kleos jobs through the kleos CLI"`.
5. Wait for ClawHub's scan; fix anything it flags (most likely the `install` entry above) and publish again with a new
   version.
6. Only then add the awesome-openclaw-skills entry (see [AWESOME-LISTS.md](AWESOME-LISTS.md)): that list takes only
   skills already on ClawHub with passing checks.
