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

## Install tiers (`tiers.yaml`)

Adding a marketplace never installs anything on its own — the two-step model (add, then install) is deliberate. But downstream tooling that provisions a whole team's environment often wants a *policy*: which skills are safe to install automatically, which to merely recommend, and which to just list. This marketplace publishes that policy as a sidecar, [`tiers.yaml`](tiers.yaml), at the repo root:

| Tier | Meaning |
| --- | --- |
| `auto` | A consumer's setup step may headless-install the **skill** without prompting. Safe because installing the skill does **not** install its `sf` CLI plugin — that stays opt-in behind the skill's own consent-gated Preflight on first use. No CLI lands without a yes. |
| `recommended` | Surface and nudge (a setup prompt, a session-start hint), but require an explicit opt-in before installing. |
| `listed` | Discoverable through the marketplace only; no nudge. A catalog skill absent from `tiers.yaml` is treated as `listed`. |

Why a sidecar and not a field in `marketplace.json`? That file has a fixed schema and rejects custom keys, so the tier map lives beside it instead. Consumers fetch `tiers.yaml` over raw HTTP (`https://raw.githubusercontent.com/bobbywhitesfdc/salesforce-toolkit/main/tiers.yaml`) and degrade gracefully when it's unreachable — treating every skill as `listed`. The convention is generic: any marketplace can ship a `tiers.yaml` with the same three tiers, and any consumer tool can honor it.

The current policy: `sf-bulk-analyzer` and `sf-secret-inject` are `auto`; `sf-agentpmd` is `recommended`.

## Staying up to date

There's no push notification when skills are added or revised — you either pull on demand, or opt in to automatic updates.

### Check for updates manually

```
/plugin marketplace update salesforce-toolkit
```

This refreshes the catalog (new skills appear under `/plugin` → **Discover**) and fetches new versions of the skills from their source repositories. Run `/reload-plugins` to pick up updates in the current session without restarting. A skill's underlying `sf` CLI plugin updates separately via `sf plugins install <name>`.

### Opt in to automatic updates

Enable auto-update so the marketplace refreshes in the background at startup. Toggle it in the UI — `/plugin` → **Marketplaces** → select `salesforce-toolkit` → **Enable auto-update** — or pre-configure it in your Claude Code settings (`~/.claude/settings.json` for user scope, or `.claude/settings.json` for a single project). This one block both registers the marketplace *and* turns on auto-update:

```json
{
  "extraKnownMarketplaces": {
    "salesforce-toolkit": {
      "source": { "source": "github", "repo": "bobbywhitesfdc/salesforce-toolkit" },
      "autoUpdate": true
    }
  }
}
```

Auto-update runs in the background shortly after startup and does not block launch; the running session keeps its launch-time versions until you run `/reload-plugins` or start a new session.[^autoupdate]

[^autoupdate]: Known limitation: `autoUpdate` reliably refreshes the catalog and covers fresh installs, but may not re-install *already-installed* plugins — see [anthropics/claude-code#61854](https://github.com/anthropics/claude-code/issues/61854). Until that resolves, `/plugin marketplace update salesforce-toolkit` (then `/reload-plugins`) is the guaranteed way to move an existing install to a new version.

## License

MIT — see [LICENSE](LICENSE).
