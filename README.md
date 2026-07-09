# koalr-mcp

**Koalr MCP server** — connect your engineering metrics, deploy risk scoring, and DORA performance data to Claude, Cursor, Windsurf, and any MCP-compatible AI assistant.

[![npm version](https://img.shields.io/npm/v/koalr-mcp)](https://www.npmjs.com/package/koalr-mcp)
[![smithery badge](https://smithery.ai/badge/koalr-mcp)](https://smithery.ai/server/koalr-mcp)

## Quick Start

### Generate an API Key

1. Go to [app.koalr.com/settings/api-keys](https://app.koalr.com/settings/api-keys)
2. Create a new API key with the scopes you need
3. Copy the key (shown once) — it starts with `koalr_`

### Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "koalr": {
      "command": "npx",
      "args": ["-y", "koalr-mcp@latest"],
      "env": {
        "KOALR_API_KEY": "koalr_your_key_here"
      }
    }
  }
}
```

### Cursor

Add to your Cursor MCP settings (`~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "koalr": {
      "command": "npx",
      "args": ["-y", "koalr-mcp@latest"],
      "env": {
        "KOALR_API_KEY": "koalr_your_key_here"
      }
    }
  }
}
```

### Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "koalr": {
      "command": "npx",
      "args": ["-y", "koalr-mcp@latest"],
      "env": {
        "KOALR_API_KEY": "koalr_your_key_here"
      }
    }
  }
}
```

### Claude Code (CLI)

```bash
claude mcp add koalr \
  -e KOALR_API_KEY=koalr_your_key_here \
  -- npx -y koalr-mcp@latest
```

## Environment Variables

| Variable        | Description                                          | Default                 |
| --------------- | ---------------------------------------------------- | ----------------------- |
| `KOALR_API_KEY` | Your Koalr API key (required). Starts with `koalr_`. | —                       |
| `KOALR_API_URL` | Koalr API base URL (for self-hosted or local dev)    | `http://localhost:3001` |
| `MCP_TRANSPORT` | Transport mode: `stdio` (default) or `http`          | `stdio`                 |
| `PORT`          | Port for HTTP transport mode                         | `3010`                  |

## Available Tools

### Deploy Risk

| Tool                       | Description                                                    |
| -------------------------- | -------------------------------------------------------------- |
| `score_pr_for_deploy_risk` | Score a pull request 0–100 using 36 research-validated signals |

### Organization

| Tool                     | Description                                                       |
| ------------------------ | ----------------------------------------------------------------- |
| `get_org_health`         | Comprehensive org health: DORA tier, cycle time, teams, incidents |
| `get_well_being_summary` | Developer well-being: focus time, meeting load, burnout signals   |

### DORA Metrics

| Tool               | Description                                            |
| ------------------ | ------------------------------------------------------ |
| `get_dora_summary` | Deploy frequency, lead time, change failure rate, MTTR |
| `get_dora_trend`   | Weekly trend for any DORA metric                       |

### Pull Requests

| Tool              | Description                                     |
| ----------------- | ----------------------------------------------- |
| `get_pr_summary`  | Cycle time, throughput, review health metrics   |
| `get_open_prs`    | Currently open PRs with age and risk indicators |
| `get_at_risk_prs` | PRs at risk of being long-running or blocked    |

### Teams

| Tool                | Description                          |
| ------------------- | ------------------------------------ |
| `list_teams`        | All teams with IDs and member counts |
| `get_team`          | Team-level DORA and flow metrics     |
| `list_team_members` | Team roster with GitHub logins       |

### Repositories

| Tool                | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| `list_repositories` | All repos with health scores                                 |
| `get_repository`    | Repo metrics: deployment frequency, cycle time, contributors |

### Developers

| Tool                    | Description                                      |
| ----------------------- | ------------------------------------------------ |
| `get_developer`         | Individual developer metrics and recent activity |
| `list_top_contributors` | Most active contributors by commits and PRs      |

### Search

| Tool     | Description                                      |
| -------- | ------------------------------------------------ |
| `search` | Search developers, repos, PRs, and teams by name |

### Coverage

| Tool                   | Description                            |
| ---------------------- | -------------------------------------- |
| `get_coverage_summary` | Test coverage by repository with trend |

### Incidents

| Tool                    | Description                                        |
| ----------------------- | -------------------------------------------------- |
| `list_recent_incidents` | Recent incidents from PagerDuty/OpsGenie with MTTR |

### AI Adoption

| Tool                      | Description                             |
| ------------------------- | --------------------------------------- |
| `get_ai_adoption_summary` | GitHub Copilot and Cursor usage metrics |
| `get_ai_adoption_trend`   | AI tool adoption trend over time        |

## Example Prompts

Once connected, you can ask your AI agent things like:

- "What is our team's DORA performance tier this month?"
- "Show me all PRs that have been open for more than 3 days"
- "Which developers have the highest PR throughput on the backend team?"
- "How is our AI coding tool adoption trending?"
- "What was our MTTR for incidents last quarter?"
- "Find the auth-service repository and show me its deployment frequency"

## HTTP Transport (Remote Hosting)

For hosting the MCP server remotely (e.g., at `mcp.koalr.com`):

```bash
MCP_TRANSPORT=http PORT=3010 KOALR_API_KEY=koalr_... node dist/index.js
```

The server exposes `POST /mcp` and `GET /mcp` endpoints following the MCP Streamable HTTP transport spec.

## Local Development

```bash
# From repo root
pnpm install
pnpm exec turbo build --filter=koalr-mcp

# Run in stdio mode (test with MCP Inspector)
KOALR_API_KEY=koalr_... node apps/mcp/dist/index.js

# Run in HTTP mode
MCP_TRANSPORT=http KOALR_API_KEY=koalr_... node apps/mcp/dist/index.js
```
