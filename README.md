# Lumina for AI coding agents

[Lumina](https://luminalabs.sh) runs AI penetration tests of your web apps, APIs and code. This repository connects your coding agent to it:

- **MCP server** `https://app.luminalabs.sh/api/mcp`: your agent lists open issues, reads each one in full (proof of concept, code locations, proposed fix), fixes the code and marks it resolved for the next test to confirm. It can snooze or ignore issues and follow running tests. It never starts a test: it hands you a link to approve the test and its price.
- **Skill** [`plugins/lumina/skills/lumina/SKILL.md`](plugins/lumina/skills/lumina/SKILL.md): how the agent fixes, triages and follows tests safely.

## 1. Connect your agent

**Claude Code** (server and skill):

```
/plugin marketplace add lumina-security/lumina-agent
/plugin install lumina@lumina
```

Then run `/mcp`, choose **lumina** and sign in: Lumina opens in your browser and asks you to approve. There's no key to copy. Your agent then acts as you, reading and triaging only; disconnect it any time in Lumina, **Organization → API**.

Or the server alone: `claude mcp add --transport http lumina https://app.luminalabs.sh/api/mcp`

**Cursor** (`.cursor/mcp.json`), and most other clients, open the same sign-in:

```json
{
  "mcpServers": {
    "lumina": { "url": "https://app.luminalabs.sh/api/mcp" }
  }
}
```

**VS Code** (`.vscode/mcp.json`): the same under `"servers"`, with `"type": "http"`.

**claude.ai** and **ChatGPT**: add a custom connector with the URL `https://app.luminalabs.sh/api/mcp`.

**Codex** (`~/.codex/config.toml`), then `codex mcp login lumina`:

```toml
[mcp_servers.lumina]
url = "https://app.luminalabs.sh/api/mcp"
```

For agents that support skills, copy [`SKILL.md`](plugins/lumina/skills/lumina/SKILL.md) into their skills folder (for example `.claude/skills/lumina/SKILL.md` or `~/.codex/skills/lumina/SKILL.md`).

## 2. Or use an API key

For a client without MCP sign-in, or an agent that runs unattended (CI): an organization owner creates a key in Lumina under **Organization → API**, with **Can start and stop tests** off. Put it in your environment and send it as a header:

```sh
export LUMINA_API_KEY=lum_…
claude mcp add --transport http lumina https://app.luminalabs.sh/api/mcp --header "Authorization: Bearer $LUMINA_API_KEY"
```

Never commit the key or paste it into a chat.

## 3. Ask

- "Fix the high and critical Lumina issues in this repo."
- "What did last night's Lumina test find?"
- "Snooze the Lumina issues about rate limiting for two weeks."
- "Run the Juice Shop test again and tell me when it's done."

## Tools

| Tool | What it does |
| --- | --- |
| `get_overview` | Credits, verified domains, applications with their open issues, running tests |
| `list_tests`, `get_test` | Tests, their status, what the testers are doing, findings so far |
| `get_finding` | A finding in full, as Markdown |
| `list_issues`, `get_issue` | Issues across tests, and one in full with everything needed to fix it |
| `triage_issue` | resolve, snooze, ignore or reopen an issue |
| `request_rerun`, `request_new_test` | A link for a person to approve a test and its price |

## License

MIT
