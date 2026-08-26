# Crossbeam Plugin

Five ready-made Skills that bring live partner data from [Crossbeam](https://www.crossbeam.com/) into your revenue team's workflows: partner overlap, co-sell context, and ecosystem signals, wrapped in the judgment for what to do with them.

**MCP connections provide the data; Skills provide the judgment and the Ecosystem-Led Growth best practices.** The Crossbeam MCP server returns partner overlaps, co-sell context, and ecosystem signals. These skills encode an expert workflow on top of it: which partner to work and why, the right play, the next best action to take.

Every skill here reads live Crossbeam data, so the [Crossbeam MCP server](https://help.crossbeam.com/en/articles/12601327-crossbeam-mcp-server-limited-availability) is a prerequisite for all of them. See [Requirements](#requirements) below.

Each skill is plain Markdown, so you can **read it here on GitHub** before you install anything. Skills live under [`skills/`](skills/), each with a `SKILL.md` plus any supporting `references/` files. Everything a human needs in order to set a skill up lives in the `<readme>` section at the top of its `SKILL.md`.

## Skills in this plugin

### Co-sell and prospecting

| Skill | What it does |
|---|---|
| [Co-Sell Copilot](skills/crossbeam-co-sell-copilot/SKILL.md) | Name an account or a stuck deal. It finds the best-positioned partner, picks the play (intro, intel, reference, voucher, or co-sell), and drafts the rep-to-rep ask, with something to give back. |
| [Ecosystem Prospecting (EQL Finder)](skills/crossbeam-ecosystem-prospecting/SKILL.md) | Turns the ecosystem into a ranked prospect list: who to work, why (which partner, customer vs. deal), the angle, and the warm path in. |

### Account intelligence and prioritization

| Skill | What it does |
|---|---|
| [Ecosystem-Informed Account Brief](skills/ecosystem-informed-account-brief/SKILL.md) | Produces a structured account brief enriched with Crossbeam ecosystem intelligence: partner overlap, ecosystem relationships, and recent partner activity signals. |
| [Ecosystem-Powered Pipeline Prioritization](skills/ecosystem-powered-pipeline-prioritization/SKILL.md) | Scans a list of accounts for partner signals (deal activity, overlap populations, recent partner movement), then ranks them and surfaces the ones worth acting on. |

### Outreach

| Skill | What it does |
|---|---|
| [Partner Alignment Outreach](skills/partner-alignment-outreach/SKILL.md) | Turns recent closed-won deals into partner-alignment outreach. Finds which partners overlap each won account and drafts a short alignment email to each partner rep. |

## Install as a plugin

If your agent supports the plugin format, install all five at once:

```
/plugin marketplace add getcrossbeam/crossbeam-plugin
/plugin install crossbeam-plugin@crossbeam
```

Restart your client and the skills are available in every session. Your agent picks the right one automatically based on what you ask. Use `/plugin` to browse or disable individual skills, and `claude plugin update crossbeam-plugin` to pull the latest version.

## Install in any other agent

Skills are an open standard, not a single vendor's format. To load one by hand:

1. Download the whole skill folder from [`skills/`](skills/), not just the `SKILL.md`. Two of the skills depend on files in a `references/` subfolder and will not work without them.
2. Load it the way your tool documents. Most platforms take an uploaded folder or a path on disk.

The Crossbeam MCP requirement below applies wherever you run them.

## Requirements

**The [Crossbeam MCP server](https://help.crossbeam.com/en/articles/12601327-crossbeam-mcp-server-limited-availability) must be connected and authenticated.** All five skills read live partner data through it. This plugin does not bundle the connector: set it up separately, and the skills will tell you if it is missing. For details on connecting Crossbeam to your AI tool, see our [help documentation](https://help.crossbeam.com).

## Configure before you use

**These skills will not run until you configure them.** Each one opens with a Configuration block of `[fill in ...]` placeholders: where your account or deal list comes from, your ICP, your exclusions, your product name. Until you fill those in, the skills are designed to stop and ask rather than guess at a source. That is correct behavior, not a bug, but it does mean a fresh install can look like it is refusing to work.

Open the `SKILL.md` for each skill, fill in its Configuration block, and reinstall or reload before sharing with your team. Your assistant can help you work through the placeholders.

## Feedback

Have feedback or a use case we have not built yet? Share it with our team [here](https://docs.google.com/forms/d/e/1FAIpQLScPr15cLPv3HZniTbid7QBXraCLPBAP8rJGB-fxDEzMWw_Wjg/viewform).

## License

Released under the [MIT License](LICENSE).
