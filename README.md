# Damdit: a website audit CLI and MCP server for coding agents

Damdit audits a website in a real browser on your machine: accessibility, broken links, SEO and indexing, metadata, security headers, forms and more. It records each issue with the page and the element and says what it could not check. Your coding agent runs it over MCP or as a command and fixes what it found. With a licence, Damdit compares each audit with the one before, and the next audit shows what is Resolved.

- **Local.** Pages are rendered in Chromium where Damdit runs. No address, page or finding of the audited website is sent to Damdit.
- **Read-only.** Damdit never changes the website. The fixing stays with you and your agent.
- **For any agent, and for pipelines.** An MCP server (`damdit-cli mcp`), a command line with JSON output and exit codes, skills, and a block for `AGENTS.md`. If your agent speaks MCP or can run a command, it can use Damdit, whichever model is behind it.

Website: [damdit.com/cli](https://damdit.com/cli/) · Docs: [docs.damdit.com/cli](https://docs.damdit.com/cli/overview/) · Setup per agent: [damdit.com/for](https://damdit.com/for/) · For AI systems: [damdit.com/llms.txt](https://damdit.com/llms.txt) · 90-second video: [youtube.com/watch?v=rRrmYHdr_as](https://www.youtube.com/watch?v=rRrmYHdr_as)

Current version: Damdit CLI 27.1.1, released 28 November 2026.

## Quick start

With Node.js 22.13 or later, on macOS, Linux or Windows x64. No Docker, no account and no key:

```sh
npx damdit-cli audit https://example.com/
```

The first audit of an address creates its project, named after the host (`example.com`), and the first audit on a computer downloads Chromium, about 180 MB. Without a key this is the free version.

### Free version and licence

| | Free version, no account | With a licence or the 7-day trial |
|---|---|---|
| Pages in one audit | Up to 20 | Up to 500 in a browser, and up to 5,000 addresses checked |
| Issues with evidence, Coverage, Needs review | Yes | Yes |
| Command line and MCP server | Yes | Yes |
| Comparing audits: Resolved, New, Reappeared | No | Yes |
| Reading older audits of a project | No, only the newest | Yes |
| Reports, scheduled audits, the `ci` pipeline commands | No | Yes |
| Webflow, PageSpeed Insights, Search Console | No | Yes |

The free version tells you what is wrong on a site. It cannot verify a fix: that needs the comparison of the audit before and the audit after, and comparing audits needs a licence or the trial. A feature the free version doesn't have answers `licence_feature` (exit code 6), so an agent can tell you why instead of guessing.

For the licence or the trial, sign in at [damdit.com/account](https://damdit.com/account) with your e-mail, start the 7-day trial (no card) and create a key there. Put it in `DAMDIT_CI_KEY`. The audits you saved in the free version stay, and with the key they can be compared. After the trial Damdit is €149 once, excluding VAT, with a year of updates; the version you have keeps working after that.

### With Docker, for CI and servers

The Docker image gives every run the same environment, and it is the recommended way for pipelines and servers. It runs on a Mac, on Linux and on Windows inside WSL2:

```sh
curl -fsSL https://download.damdit.com/cli/install.sh | sh
export DAMDIT_CI_KEY=…   # a key from damdit.com/account; without it, the free version
damdit-cli audit https://example.com/
```

### What you get

What an audit answers, in a terminal (this one with a licence, so 31 pages):

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

And after a fix, with a licence, `damdit-cli compare`:

```text
Changes: New 0, Resolved 1, Reappeared 0, Inconclusive 1  Still detected: 4
  Inconclusive: This URL returns an HTTP error
  Resolved: An image does not have an accessible alternative
```

Through a pipe, into a file or to an agent the same commands print JSON.

## The MCP server

`damdit-cli mcp` is an MCP server on standard input and output (stdio). The agent starts it on the same machine. Any MCP client that starts a server with a command can use it ([`examples/mcp-npx.json`](examples/mcp-npx.json)):

```json
{
  "mcpServers": {
    "damdit-cli": { "command": "npx", "args": ["-y", "damdit-cli", "mcp"] }
  }
}
```

Some MCP clients on Windows need `cmd /c` in front ([`examples/mcp-npx-windows.json`](examples/mcp-npx-windows.json)):

```json
{
  "mcpServers": {
    "damdit-cli": { "command": "cmd", "args": ["/c", "npx", "-y", "damdit-cli", "mcp"] }
  }
}
```

With Damdit CLI installed for Docker, the command is `damdit-cli` itself ([`examples/mcp-docker.json`](examples/mcp-docker.json)); Docker must be running. If the agent can't find `damdit-cli`, give the full path, for example `~/.local/bin/damdit-cli`.

Without a key the server runs as the free version, and it tells the agent so. To use the licensed features, the agent needs `DAMDIT_CI_KEY` in its environment.

Where each agent keeps this. The rows with npx were run with the npm package on 27 November 2026; Cursor's steps were run with the Docker launcher on 25 November 2026 (with npm, put `npx` and `-y damdit-cli mcp` in place of `damdit-cli` and `mcp`).

| Agent | Add the server | Steps |
|---|---|---|
| Claude Code | `claude mcp add --transport stdio --scope user damdit-cli -- npx -y damdit-cli mcp` | [damdit.com/for/claude-code](https://damdit.com/for/claude-code/) |
| Codex | `codex mcp add damdit-cli -- npx -y damdit-cli mcp` | [damdit.com/for/codex](https://damdit.com/for/codex/) |
| Cursor | the Docker block above in `~/.cursor/mcp.json` | [damdit.com/for/cursor](https://damdit.com/for/cursor/) |
| GitHub Copilot | the block above in `.mcp.json` | [damdit.com/for/copilot](https://damdit.com/for/copilot/) |
| Hermes | `mcp_servers` in `~/.hermes/config.yaml` | [damdit.com/for/hermes](https://damdit.com/for/hermes/) |
| Mistral Vibe | `vibe mcp add damdit-cli --transport stdio --command npx --arg=-y --arg=damdit-cli --arg=mcp` | [damdit.com/for/mistral-vibe](https://damdit.com/for/mistral-vibe/) |
| OpenClaw | `openclaw mcp set damdit-cli …` | [damdit.com/for/openclaw](https://damdit.com/for/openclaw/) |
| OpenCode | `opencode mcp add damdit-cli --global -- npx -y damdit-cli mcp` | [damdit.com/for/opencode](https://damdit.com/for/opencode/) |

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

`get_status` says which edition runs: `free` or `licensed`. In the free version, `compare_issue_history`, `get_issue_history`, `create_report` and `set_schedule` answer `licence_feature`, and only a project's newest audit can be read.

`DAMDIT_CLI_ACCESS` sets the most an agent may do: `read`, `audit` or `full`. Text copied from the audited website is marked as untrusted in every answer, so an agent treats it as data, not as instructions.

## What an audit checks

One Full Audit, the same every time: 285 rules, up to 500 pages rendered in a browser (20 in the free version) and up to 5,000 addresses checked for links.

- **Accessibility**, with axe-core in the rendered page
- **Links and addresses**: broken links, redirects, HTTP errors
- **Indexing and SEO**: titles, descriptions, canonical addresses, noindex, the sitemap, robots.txt, structured data, social previews
- **Security**: HTTPS, security headers, what the site's own Content Security Policy blocks
- **Assets and forms**: images, scripts, stylesheets and fonts that fail to load; form fields without names
- **Runtime and AI readiness**, as signals that don't touch the score
- Optional, with a licence and your own keys: **PageSpeed Insights**, **Google Search Console** and the **Webflow API**

Three things it does with every result:

- **Evidence.** Each issue names the page and the element, what was measured and what would pass.
- **Coverage.** What the audit couldn't check is listed as not checked. It never counts as passed.
- **Comparison**, with a licence. The next audit says what is New, Resolved, Reappeared, Inconclusive or Still detected.

No automated audit finds every problem. Judgement calls go to Needs review and wait for a person.

## Skills and AGENTS.md

A skill tells the agent how to use Damdit and what it may claim. Three ways to give it to your agent:

- **Agents that read skills** (Claude Code, Codex, Cursor, OpenCode and more): `npx skills add damdit-soft/damdit-public`
- **Agents that read `AGENTS.md`**, `CLAUDE.md` or their own rules file: paste [`examples/AGENTS.md`](examples/AGENTS.md).
- **As a Claude Code plugin:** `/plugin install damdit --marketplace damdit-soft/damdit-public`

| Skill | What it gives the agent |
|---|---|
| [`damdit`](skills/damdit/SKILL.md) | Damdit's commands, what the results mean, and the rule to say only what the evidence shows |
| [`verified-website-change`](skills/verified-website-change/SKILL.md) | Four steps for every change to a website: read the audit before, change, audit the deployed site, report by the comparison. Fixed only when the audit says Resolved. In the free version the agent reports what the newest audit shows and calls the change unverified |

### A workflow that hands off with evidence

Two more skills live in [`workflow/`](workflow/) as a plugin of their own, `damdit-workflow`. They are public versions of the skills Damdit is built with, and they work without Damdit:

- [`implementation-shaping`](workflow/skills/implementation-shaping/SKILL.md) turns an idea or a brief into a bounded assignment another agent can implement.
- [`engineering-orchestrator`](workflow/skills/engineering-orchestrator/SKILL.md) implements it and hands it off with evidence for every acceptance criterion: `complete`, `unverified`, `needs decision` or `blocked`.

They are plain skill folders: copy them into your agent's skills directory, or in Claude Code install the plugin `damdit-workflow` from this marketplace. They are kept apart from the Damdit skills on purpose: they take part in every implementation task, not only in website audits.

## In a pipeline

`damdit-cli ci audit` compares with the previous run and fails the build only when an issue is new or came back. The `ci` commands need a licence (`DAMDIT_CI_KEY`):

```sh
damdit-cli ci audit https://staging.example.com \
  --baseline previous.dpk --out current.dpk \
  --fail-on new,reappeared --report client.pdf
```

For GitHub Actions, copy [`examples/github-actions/damdit.yml`](examples/github-actions/damdit.yml) and [`examples/damdit.yml`](examples/damdit.yml). Any CI that can start a Docker container works the same way; Docker is the recommended way in a pipeline. [Pipelines in the docs](https://docs.damdit.com/cli/pipelines/).

## On a server

`damdit-cli serve` runs scheduled audits in the background and is the host your agents talk to: `damdit-cli schedule example.com weekly`. Scheduled audits need a licence. On Windows, `serve` and `schedule` are not available yet; use Docker there. [Serve in the docs](https://docs.damdit.com/cli/serve/).

## Requirements, limits and price

| | |
|---|---|
| System, npm | macOS, Linux or Windows x64, with Node.js 22.13 or later. On Windows, `serve` and `schedule` are not available yet |
| System, Docker | Anywhere Docker runs: a Mac, Linux, Windows inside WSL2, a server, a CI runner |
| Processor, Docker | 64-bit Intel or AMD (x86-64, also called amd64) or ARM (arm64: Apple silicon, Graviton, Ampere and similar). 32-bit processors are not supported |
| Download | npm: Chromium, about 180 MB, on the first audit. Docker: the image is about 1.1 GB; Docker downloads it once and again for each update |
| Chromium's sandbox | On Linux systems that refuse it (Ubuntu 23.10 and later, most containers), the npm package runs Chromium without the sandbox and says so. Audit websites you trust there, or use the Docker image |
| Network | Access to the website you audit. With a key, access to damdit.com to check it on each run |
| Needs | Nothing more for the free version. A key from your Damdit account in `DAMDIT_CI_KEY` for the licence or the trial |
| One audit | Up to 20 pages in the free version; with a licence up to 500 pages rendered in a browser. Up to 5,000 addresses checked for links |
| Trial | 7 days, every feature, no card. Start it at [damdit.com/account](https://damdit.com/account) |
| Price | €149 once, excluding VAT, with every update for 12 months. After that the version you have keeps working; another year of updates is €70, only if you want it |
| One licence | Damdit for Mac and Damdit CLI on two places at a time, in any combination |

## Damdit for Mac

The same audit as an app, for a Mac with Apple silicon (macOS 14 or later), with the projects, issues, history and reports in a window: [download](https://damdit.com/download/). The app has no free version. One licence covers the app and Damdit CLI on two places at a time.

## What leaves your machine

Pages are rendered on the computer that runs Damdit, and audits are saved there. The skills send nothing themselves: they run the `damdit` or `damdit-cli` command. Damdit talks to:

- **the website you audit**, to load its pages;
- **damdit.com** (`api.damdit.com`), to check your licence when you have one: the key or your sign-in, never the audited website or its results;
- in Damdit CLI's free version, one anonymous event per audit: the audit's score, how many issues it found per severity and area, how many modules ran, the hour it ended, the Damdit version, the operating system and its major version, whether it runs from npm or Docker, whether it is the first audit of this data folder and, against the audit before, the earlier score, the difference and a coarse interval. No address, no project, no identifier of you or your computer. `DAMDIT_TELEMETRY=0` or `DO_NOT_TRACK=1` turns it off. The licensed version sends none;
- only when you turn them on, with a licence and your own credentials: **Google PageSpeed Insights** (the page addresses it measures), **Google Search Console** and the **Webflow API** (your site's data).

No address, page or finding of the audited website is sent to Damdit. Details: [privacy policy](https://damdit.com/privacy/).

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
