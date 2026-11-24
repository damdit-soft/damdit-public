# Damdit

Damdit audits websites: it finds technical issues, shows the evidence behind each finding, and helps you decide what to fix first. It runs on your computer and never edits the website it audits. Website: [damdit.com](https://damdit.com) · Documentation: [docs.damdit.com](https://docs.damdit.com).

It comes in two forms with one licence:

- **Damdit for Mac**, the app, for a Mac with Apple silicon (macOS 14 or later): [download](https://damdit.com/download/).
- **Damdit CLI**, the same audit as a command, in Docker on a Mac, Linux or a server: `curl -fsSL https://download.damdit.com/cli/install.sh | sh` ([more](https://damdit.com/cli/)).

What we are working on is in the [roadmap](ROADMAP.md).

## Damdit in your AI agent

This repository also holds Damdit's skill for AI agents ([`skills/damdit`](skills/damdit/SKILL.md)). It tells your agent how to run a Damdit audit, read the result and say only what the evidence shows. It needs Damdit for Mac or Damdit CLI on the same machine.

- **Claude Code:**

  ```
  /plugin install damdit --marketplace damdit-soft/damdit-public
  ```

  On older versions of Claude Code: `/plugin marketplace add damdit-soft/damdit-public`, then `/plugin install damdit@damdit`.

- **Other agents** that read skills (Codex, Cursor, OpenCode and more): `npx skills add damdit-soft/damdit-public`.
- **The MCP server** comes with Damdit itself: `damdit-cli mcp` for Damdit CLI, `damdit mcp` for the app. For Claude Code: `claude mcp add damdit-cli -- damdit-cli mcp`. Setup for each agent: [damdit.com/for](https://damdit.com/for/).

The skill is copied here from the app at each release; its version is the Damdit version it describes. The skill is under the [MIT licence](skills/damdit/LICENSE); Damdit itself and the rest of this repository are not.

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
