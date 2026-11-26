# Damdit: a website audit CLI and MCP server for coding agents

Damdit audits a website in a real browser on your machine: accessibility, broken links, SEO and indexing, metadata, security headers, forms and more. It records each issue with the page and the element, says what it could not check, and compares each audit with the one before. Your coding agent runs it over MCP or as a command, fixes what it found, and the next audit shows what is Resolved.

- **Local.** Pages are rendered in Chromium where Damdit runs. Nothing about the audited website is sent to Damdit.
- **Read-only.** Damdit never changes the website. The fixing stays with you and your agent.
- **For any agent, and for pipelines.** An MCP server (`damdit-cli mcp`), a command line with JSON output and exit codes, skills, and a block for `AGENTS.md`. If your agent speaks MCP or can run a command, it can use Damdit, whichever model is behind it.

Website: [damdit.com/cli](https://damdit.com/cli/) · Docs: [docs.damdit.com/cli](https://docs.damdit.com/cli/overview/) · Setup per agent: [damdit.com/for](https://damdit.com/for/) · For AI systems: [damdit.com/llms.txt](https://damdit.com/llms.txt) · 90-second video: [youtube.com/watch?v=rRrmYHdr_as](https://www.youtube.com/watch?v=rRrmYHdr_as)

Current version: Damdit CLI 27.0.1, released 26 November 2026.

## Quick start

Damdit CLI needs Docker. It runs on a Mac, on Linux and on Windows inside WSL2.

```sh
curl -fsSL https://download.damdit.com/cli/install.sh | sh
export DAMDIT_CI_KEY=…   # a key from damdit.com/account
damdit-cli project create --name Example --production https://example.com/
damdit-cli audit Example
```

No account yet? Sign in at [damdit.com/account](https://damdit.com/account) with your e-mail, start the 7-day trial (no card) and create a key there. After the trial Damdit is €149 once, excluding VAT, with a year of updates; the version you have keeps working after that.

What an audit answers, in a terminal:

```text
staging — 2026-11-26T10:32:57Z
Score: 92
Issues: 6  occurrences: 11  critical 0, high 2, medium 3, low 1
Coverage: limited  (31 pages audited · 33 addresses found)
Not checked, or only in part:
  - 39 check results had incomplete evidence; they count neither as issues nor as passes.
  - PageSpeed Insights was off for this audit, so performance was not measured and is not part of the score.
  [high] An image does not have an accessible alternative — 2 elements on 31 pages
  [high] This URL returns an HTTP error — 1 occurrence on 1 page
  [medium] A submitted form field has no name — 1 element on 1 page
  [medium] A public page is marked noindex — 1 occurrence on 1 page
```

And after a fix, `damdit-cli compare`:

```text
Changes: New 0, Resolved 1, Reappeared 0, Inconclusive 1  Still detected: 4
  Inconclusive: This URL returns an HTTP error
  Resolved: An image does not have an accessible alternative
```

Through a pipe, into a file or to an agent the same commands print JSON.

## The MCP server

`damdit-cli mcp` is an MCP server on standard input and output (stdio). The agent starts it on the same machine. Any MCP client that starts a server with a command can use it ([`examples/mcp.json`](examples/mcp.json)):

```json
{
  "mcpServers": {
    "damdit-cli": { "command": "damdit-cli", "args": ["mcp"] }
  }
}
```

The agent needs `DAMDIT_CI_KEY` in its environment to run audits, and Docker running. If it can't find `damdit-cli`, give the full path, for example `~/.local/bin/damdit-cli`.

Where each agent keeps this, with the steps we ran on 25 November 2026:

| Agent | Add the server | Steps |
|---|---|---|
| Claude Code | `claude mcp add --transport stdio --scope user damdit-cli -- damdit-cli mcp` | [damdit.com/for/claude-code](https://damdit.com/for/claude-code/) |
| Codex | `codex mcp add damdit-cli -- damdit-cli mcp` | [damdit.com/for/codex](https://damdit.com/for/codex/) |
| Cursor | the block above in `~/.cursor/mcp.json` | [damdit.com/for/cursor](https://damdit.com/for/cursor/) |
| GitHub Copilot | the block above in `.mcp.json` | [damdit.com/for/copilot](https://damdit.com/for/copilot/) |
| Hermes | `mcp_servers` in `~/.hermes/config.yaml` | [damdit.com/for/hermes](https://damdit.com/for/hermes/) |
| Mistral Vibe | `vibe mcp add damdit-cli --transport stdio --command damdit-cli --arg mcp` | [damdit.com/for/mistral-vibe](https://damdit.com/for/mistral-vibe/) |
| OpenClaw | `openclaw mcp set damdit-cli …` | [damdit.com/for/openclaw](https://damdit.com/for/openclaw/) |
| OpenCode | `opencode mcp add damdit-cli --global -- damdit-cli mcp` | [damdit.com/for/opencode](https://damdit.com/for/opencode/) |

Another agent or a local model in a harness: use the block above, or skip MCP and let the agent run `damdit-cli` as a command.

| Tools | What they do |
|---|---|
| `list_projects`, `create_project`, `update_project` | Projects and their addresses |
| `run_audit`, `job_status`, `cancel_audit` | Start, follow and stop an audit |
| `check_url` | Check a website running on this computer once |
| `list_audits`, `get_audit_summary` | Audits; score, coverage, counts and the first issues |
| `list_issues`, `list_occurrences`, `get_evidence` | Issues, where they occur and the evidence |
| `compare_issue_history`, `get_issue_history` | What changed between audits; an issue's history |
| `list_review_checks`, `get_review_check` | Needs review: observations that wait for a person |
| `set_issue_status`, `set_schedule` | An issue's status; daily, weekly or monthly audits |
| `create_report`, `list_reports`, `export_data` | Reports for the client, the developer or AI; all data |

`DAMDIT_CLI_ACCESS` sets the most an agent may do: `read`, `audit` or `full`. Text copied from the audited website is marked as untrusted in every answer, so an agent treats it as data, not as instructions.

## What an audit checks

One Full Audit, the same every time: 285 rules, up to 500 pages rendered in a browser and up to 5,000 addresses checked for links.

- **Accessibility**, with axe-core in the rendered page
- **Links and addresses**: broken links, redirects, HTTP errors
- **Indexing and SEO**: titles, descriptions, canonical addresses, noindex, the sitemap, robots.txt, structured data, social previews
- **Security**: HTTPS, security headers, what the site's own Content Security Policy blocks
- **Assets and forms**: images, scripts, stylesheets and fonts that fail to load; form fields without names
- **Runtime and AI readiness**, as signals that don't touch the score
- Optional, with your own keys: **PageSpeed Insights**, **Google Search Console** and the **Webflow API**

Three things it does with every result:

- **Evidence.** Each issue names the page and the element, what was measured and what would pass.
- **Coverage.** What the audit couldn't check is listed as not checked. It never counts as passed.
- **Comparison.** The next audit says what is New, Resolved, Reappeared, Inconclusive or Still detected.

No automated audit finds every problem. Judgement calls go to Needs review and wait for a person.

## Skills and AGENTS.md

A skill tells the agent how to use Damdit and what it may claim. Three ways to give it to your agent:

- **Agents that read skills** (Claude Code, Codex, Cursor, OpenCode and more): `npx skills add damdit-soft/damdit-public`
- **Agents that read `AGENTS.md`**, `CLAUDE.md` or their own rules file: paste [`examples/AGENTS.md`](examples/AGENTS.md).
- **As a Claude Code plugin:** `/plugin install damdit --marketplace damdit-soft/damdit-public`

| Skill | What it gives the agent |
|---|---|
| [`damdit`](skills/damdit/SKILL.md) | Damdit's commands, what the results mean, and the rule to say only what the evidence shows |
| [`verified-website-change`](skills/verified-website-change/SKILL.md) | Four steps for every change to a website: read the audit before, change, audit the deployed site, report by the comparison. Fixed only when the audit says Resolved |

### A workflow that hands off with evidence

Two more skills live in [`workflow/`](workflow/) as a plugin of their own, `damdit-workflow`. They are public versions of the skills Damdit is built with, and they work without Damdit:

- [`implementation-shaping`](workflow/skills/implementation-shaping/SKILL.md) turns an idea or a brief into a bounded assignment another agent can implement.
- [`engineering-orchestrator`](workflow/skills/engineering-orchestrator/SKILL.md) implements it and hands it off with evidence for every acceptance criterion: `complete`, `unverified`, `needs decision` or `blocked`.

They are plain skill folders: copy them into your agent's skills directory, or in Claude Code install the plugin `damdit-workflow` from this marketplace. They are kept apart from the Damdit skills on purpose: they take part in every implementation task, not only in website audits.

## In a pipeline

`damdit-cli ci audit` compares with the previous run and fails the build only when an issue is new or came back:

```sh
damdit-cli ci audit https://staging.example.com \
  --baseline previous.dpk --out current.dpk \
  --fail-on new,reappeared --report client.pdf
```

For GitHub Actions, copy [`examples/github-actions/damdit.yml`](examples/github-actions/damdit.yml) and [`examples/damdit.yml`](examples/damdit.yml). Any CI that can start a Docker container works the same way. [Pipelines in the docs](https://docs.damdit.com/cli/pipelines/).

## On a server

`damdit-cli serve` runs scheduled audits in the background and is the host your agents talk to: `damdit-cli schedule Example weekly`. [Serve in the docs](https://docs.damdit.com/cli/serve/).

## Requirements, limits and price

| | |
|---|---|
| System | Anywhere Docker runs: a Mac, Linux, Windows inside WSL2, a server, a CI runner |
| Processor | 64-bit Intel or AMD (x86-64, also called amd64) or ARM (arm64: Apple silicon, Graviton, Ampere and similar). 32-bit processors are not supported |
| Download | The image is about 1.1 GB; Docker downloads it once and again for each update |
| Network | Access to the website you audit, and to damdit.com to check the key on each run |
| Needs | Docker, and a key from your Damdit account in `DAMDIT_CI_KEY` |
| One audit | Up to 500 pages rendered in a browser, up to 5,000 addresses checked for links |
| Trial | 7 days, every feature, no card. Start it at [damdit.com/account](https://damdit.com/account) |
| Price | €149 once, excluding VAT, with every update for 12 months. After that the version you have keeps working; another year of updates is €70, only if you want it |
| One licence | Damdit for Mac and Damdit CLI on two places at a time, in any combination |

## Damdit for Mac

The same audit as an app, for a Mac with Apple silicon (macOS 14 or later), with the projects, issues, history and reports in a window: [download](https://damdit.com/download/). One licence covers the app and Damdit CLI on two places at a time.

## What leaves your machine

Pages are rendered on the computer that runs Damdit, and audits are saved there. The skills send nothing themselves: they run the `damdit` or `damdit-cli` command. Damdit talks to:

- **the website you audit**, to load its pages;
- **damdit.com** (`api.damdit.com`), to check your licence: the key or your sign-in, never the audited website or its results;
- only when you turn them on, with your own credentials: **Google PageSpeed Insights** (the page addresses it measures), **Google Search Console** and the **Webflow API** (your site's data).

Nothing about the audited website is sent to Damdit. Details: [privacy policy](https://damdit.com/privacy/).

The skill is copied here from the app at each release; its version is the Damdit version it describes. The files in this repository, the skill among them, are under the [MIT licence](LICENSE). Damdit for Mac and Damdit CLI themselves are proprietary and are not in this repository.

## This repository

This repository is the public home for:

- bug reports and feature requests;
- the public roadmap;
- release notes;
- the skill for AI agents.

The application source code, internal planning, customer data, credentials and security-sensitive implementation details are kept private. Reports filed here are triaged by the maintainer and, when accepted, copied into the private tracker; the public issue stays open until the fix or decision is released and links to its release note.

## Feedback

- Use a [bug report](../../issues/new?template=bug.yml) for reproducible behaviour that looks wrong. In Damdit, **Settings → About & Support → Report a Bug…** opens it with your app version and system filled in.
- Use a [feature request](../../issues/new?template=feature.yml) for a problem or improvement you would like Damdit to address.
- Questions about your account, licence or payment: write to support@damdit.com instead of opening an issue.
- Do not post website credentials, access tokens, private audit reports, personal data or undisclosed security issues.

For responsible disclosure, see [SECURITY.md](SECURITY.md).
