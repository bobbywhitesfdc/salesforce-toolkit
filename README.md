# Salesforce Toolkit

A [Claude Code](https://claude.ai/code) plugin marketplace collecting open-source Salesforce skills — CLI-driven plugins today, with room for non-CLI skills over time.

Each entry is a Claude Code **skill**. Skills that drive a Salesforce CLI (`sf`) plugin bootstrap that CLI plugin for you — with your confirmation — on first use, so you install one skill and it provisions what it needs.

## Install

Add the marketplace once:

```
/plugin marketplace add bobbywhitesfdc/salesforce-toolkit
```

Then install any skill:

```
/plugin install <skill>@salesforce-toolkit
```

Or run `/plugin` with no arguments and browse the **Discover** tab.

## Catalog

The skills this marketplace advertises. Install any **Available** skill with the command shown; **Coming soon** entries are being migrated and will light up here as they land.

| Skill | What it does | SF CLI plugin (topic) | Install | Status |
| --- | --- | --- | --- | --- |
| `sf-bulk-analyzer` | Analyze Bulk API V1/V2 job failures; reverse engineer ETL upload field mappings | `sf-bulk-analyzer` (`sf bulk`) | `/plugin install sf-bulk-analyzer@salesforce-toolkit` | ✅ Available |
| `sf-secret-inject` | Securely inject secrets into External Credential principals via the Named Credentials REST API | `sf-secret-inject` (`sf secret`) | `/plugin install sf-secret-inject@salesforce-toolkit` | ✅ Available |
| `sf-agentpmd` | McCabe cyclomatic complexity & LOC analysis for AgentScript (`.agent`) bundles | `sf-agentpmd` (`sf agentpmd`) | `/plugin install sf-agentpmd@salesforce-toolkit` | ✅ Available |

> The **Skill** column is the Claude plugin's install id. The **SF CLI plugin** column is the underlying `sf` plugin (and its `sf` topic) that the skill bootstraps on first use.

## How updates work

Skills track their source repositories. Run `/plugin marketplace update salesforce-toolkit` to pull the latest. A skill's underlying Salesforce CLI plugin updates separately via `sf plugins install <name>`.

## License

MIT — see [LICENSE](LICENSE).
