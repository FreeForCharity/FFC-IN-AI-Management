# FFC-IN-AI-Management

Centralized AI agent configuration management for all Free For Charity repositories.

## What This Repo Does

This repository is the **single source of truth** for AI agent configuration across all FFC repos (~30 repositories across 3 GitHub organizations). It provides:

- **Validated templates** for 4 AI tools: Claude Code, GitHub Copilot, Google Gemini, and a universal agent baseline
- **MCP server configurations** for Cloudflare, GitHub, Sentry, Playwright, and Microsoft Learn
- **Custom Claude Code agents** for DNS audits, site health checks, cross-repo sync, and more
- **Audit and sync scripts** to track and deploy configurations across all repos
- **Managed settings** for machine-level Claude Code configuration

## Architecture: Base + Overlay

```
templates/base/          <-- Goes into EVERY repo (Next.js, GitHub Pages)
templates/overlays/      <-- Extra rules for special repo types (PowerShell, infra)
```

Most FFC repos are Next.js static sites deployed to GitHub Pages. The **base template** covers all of them. Only infrastructure repos (like FFC-Cloudflare-Automation) need the overlay.

## What Gets Deployed to Each Repo

| File | Tool That Reads It | Purpose |
|------|-------------------|---------|
| `AGENTS.md` | All AI tools | Universal baseline instructions |
| `CLAUDE.md` | Claude Code (CLI + IDE) | Claude-specific directives |
| `GEMINI.md` | Google Gemini | Gemini-specific guidance |
| `.github/copilot-instructions.md` | GitHub Copilot | Copilot-specific instructions |
| `.claude/settings.json` | Claude Code | Permissions (allow/deny commands) |
| `.claude/rules/*.md` | Claude Code | Behavioral rules, auto-loaded by file path |
| `.claude/agents/*.md` | Claude Code | Custom agent definitions |
| `.copilot/mcp-config.json` | GitHub Copilot Agent | MCP server reference config |

## Quick Start

### Audit all repos

```powershell
.\scripts\Audit-AIConfigs.ps1
```

Scans all FFC repos and generates `inventory/audit-report.md`.

### Sync templates to a single repo

```powershell
.\scripts\Sync-AIConfigs.ps1 -RepoName FFC-EX-legioninthewoods.org -DryRun
```

### Sync templates to all repos of a type

```powershell
.\scripts\Sync-AIConfigs.ps1 -RepoType base -DryRun
```

Remove `-DryRun` to create PRs.

## Directory Structure

```
templates/base/              Standard template for all repos
templates/overlays/          Extra files for special repo types
agents/                      Custom Claude Code agent definitions
mcp/                         MCP server configurations
managed/                     Windows machine-level settings
inventory/                   Repo inventory and audit reports
scripts/                     PowerShell audit and sync automation
docs/                        Architecture and reference documentation
```

## Documentation

- [Architecture Guide](docs/architecture.md) - How the base + overlay system works
- [File Reference](docs/file-reference.md) - Which AI tool reads which file
- [MCP Server Reference](docs/mcp-server-reference.md) - Available MCP servers
- [Custom Agents Guide](docs/custom-agents-guide.md) - How to create and use custom agents
- [Sync Guide](docs/sync-guide.md) - Running audit and sync scripts

## Organizations Covered

| Organization | Repos | Primary Type |
|---|---|---|
| FreeForCharity | ~28 | Charity websites (Next.js), infrastructure tools |
| koenig-childhood-cancer-foundation | 1 | Charity website (Next.js) |
| clarkemoyer | ~8 | Research sites, personal projects |

## License

GNU Affero General Public License v3.0 - see [LICENSE](LICENSE)
