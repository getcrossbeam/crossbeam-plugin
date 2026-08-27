---
name: ecosystem-informed-account-brief
description: Surface Crossbeam's ecosystem intelligence for any account — partner overlap, ecosystem activity signals, and partner-shared contacts. Use standalone when you need an ecosystem-first view of an account, or drop it into an existing account brief to add the partner layer your other sources can't provide. Triggers automatically when asking for an account brief while using the plugin.
---

<readme>

# README — Ecosystem-Informed Account Brief

Add Crossbeam's ecosystem intelligence to any account view — partner overlap, deal signals, partner-shared contacts, and partner reccomendations in one structured output.

## What it does

Pulls everything Crossbeam knows about an account: which partners overlap, what population they're in, what's moving in your ecosystem around this account (deals opened, closed won, greenfield signals), which contacts partners have shared, and if there are any partners recommended to leverage on the account. That's the layer no other source has — use it standalone or drop it into an existing brief to make the rest of your account intelligence more actionable.

**When using the plugin:** asking for a brief on any account will automatically surface this ecosystem context. You don't need to set anything up beyond connecting Crossbeam.

## What you need

**Required**
- **Crossbeam connector** — connect from your tool's connector directory or at crossbeam.com. Make sure you're authenticated to the right org before running.

## Setup: what to configure before your first run

**Step 1 — Connect Crossbeam**
Connect the Crossbeam connector from your tool's connector directory. That's all that's required.

**Step 2 — Set your Crossbeam partner tags (optional)**
If you want the brief to focus on specific partners — Tier 1s, active co-sell partners, strategic ISVs — fill in which Crossbeam tags identify them in the Configuration section.

> **Fill in before sharing:** `Strategic partner tags: [e.g. Tier 1, Co-Sell]`

Tags need to match what's already applied in Crossbeam. If left blank, the brief surfaces all ecosystem relationships with no filter.

**Step 3 — Customize the brief template (optional)**
The brief template is a starting point. Remove sections that don't apply to your team, rename fields to match your product, and fill in `[your product]` wherever it appears. Ask your assistant to help you adapt it.

**Step 4 — Run it once and adjust**
After your first run, ask your assistant to help you refine any sections based on what was most useful.

## How to run it

> "Brief me on [Account]"
> "Give me an overview of [Account]"
> "What do we know about [Account]?"

You can also paste in an existing account brief and ask your assistant to enrich it with ecosystem data — the skill will add the Crossbeam layer to what you already have.

## What to expect

- **Ecosystem intelligence called out explicitly** — where Crossbeam data changes the picture (a partner with an open deal, a greenfield signal, a risk from ecosystem movement), the brief calls it out rather than burying it.
- **Partner-shared contacts** — contacts your partners have shared on this account, ranked by role (economic buyer, decision maker, executive sponsor, technical buyer). Net-new contacts not yet in your records are flagged.
- **No ecosystem relationships found:** normal for some accounts. The brief notes it clearly.

## Related

- **Meeting prep** — if you have a specific upcoming meeting with known attendees and a meeting-prep skill installed, use that instead.

## A note on the data

All partner data comes from what your partners have shared with you in Crossbeam under your sharing rules. The skill only surfaces what is already visible to you, and never guesses at data a partner has not shared.

</readme>

<instructions>

> **Structural tags.** `<readme>`, `<instructions>`, and `<output_template>` delimit sections of
> this file for the agent reading it. They are not content: never echo a tag in your output, and
> where an `<output_template>` is given, reproduce what it contains without the surrounding tags.

# Skill Instructions

Produces a structured account brief from Crossbeam's ecosystem intelligence. One output: the brief.

---

## Configuration
> These values are set once by whoever deploys this skill. The agent reads them at runtime — do not change them mid-conversation.

**Strategic partner tags (optional):** [fill in — e.g. Tier 1, Co-Sell. Leave blank to surface all ecosystem relationships.]

**Your product name:** [fill in — e.g. Crossbeam]

---

## Step 1 — Resolve the account

Infer the domain from the account name if not provided. If ambiguous, ask once.

---

## Step 2 — Pull ecosystem data via Crossbeam MCP

Run all four calls. If the account isn't found in Crossbeam, note it and stop — there is no ecosystem data to surface.

> **Tool names:** the Crossbeam MCP tool-name prefix varies per installation (e.g. `Crossbeam:`, `mcp__Crossbeam__`). Match on the suffixes below rather than the full name, and confirm the actual tool surface on the first call — tool sets differ between installs.

**2a — Resolve the account**
```
get_account_context(account_domain: "example.com")
```
Accepts `account_domain`, `account_name` (fuzzy), or `account_id`. Domain is the most reliable. Extract the account's CRM `record_id` / `account_id` from the match — Step 2c needs it. If several accounts come back, pick by the user's intent and confirm if unclear.

**2b — Get partner overlap**
```
find_overlapping_partners(account_id: "<record_id>", limit: 100)
```
**Always pass `limit`.** The tool defaults to `limit: 10`, so an account overlapping more partners than that silently returns only the first page, with no error and no truncation flag. Pass `limit: 100` and paginate with `page` until the partner list is complete.

If strategic partner tags are configured, filter in the same call — this tool takes the tag directly, so no second call and no manual intersect is needed:
```
find_overlapping_partners(account_id: "<record_id>", partner_tag_name: "<tag>", limit: 100)
```
An ambiguous tag returns `ClarificationRequired` with candidates; present them and retry with `partner_tag_id`. If no tags are configured, omit the tag argument to get every partner that shares the account.

**`partner_tag_name` takes a single tag, not a list.** Configuration invites several (e.g. "Tier 1, Co-Sell"), so if more than one is configured you cannot pass them in one call. Make one call per configured tag and union the results by partner, de-duplicating. Passing only the first tag silently drops partners that carry only the others, which shows up as partners missing from the brief rather than as an error.

For each match capture: partner name, population name, and partner owner name if available.

**2c — Get ecosystem activity signals**
```
get_ecosystem_activity(record_ids: ["<record_id>"], resource_type: "accounts")
```
This tool filters by CRM **record ID**, not by domain — use the `record_id` from Step 2a. Optionally narrow with `partner_names` (fuzzy; ambiguous names return `ClarificationRequired` — resolve before proceeding) or `event_types`, whose valid values are `partner_deal_opened`, `partner_deal_closed_won`, and `partner_greenfield_deal_closed_won`. If strategic partner tags are configured, pass the tagged partner names from Step 2b.

Capture: event type (deal opened / deal closed won / greenfield), partner name, date, contact context if available.

Flag signals that should surface in the brief:
- A partner with an open opportunity on this account = co-sell signal, surface in ECOSYSTEM RELATIONSHIPS and NEXT STEPS
- A partner who closed won on this account = mutual customer signal, surface in ECOSYSTEM RELATIONSHIPS
- A greenfield signal = net new opportunity via partner, surface in GROWTH OPPORTUNITIES
- A partner going dark or no recent activity where there was previously overlap = risk signal, surface in RISKS & CONCERNS

**2d — Get partner-shared contacts**
```
find_partner_shared_contacts(account_id: "<record_id>", limit: 50)
```
Call once per overlapping partner from Step 2b — pass `partner_name` each time to avoid the same contact appearing multiple times across partners. For each contact capture: name, title, which partner shared them, `in_own_crm` flag (false = not yet in your records), and `last_activity_at`.

Determine each contact's role from `partner_insights` first (partner-specific signals), then fall back to `insights` (network-wide signals). Priority roles to surface: `economic_buyer`, `decision_maker`, `economic_decision_maker`, `executive_sponsor`, `technical_buyer`, `primary_contact`. Secondary: `key_contact`. Contacts with no role signal are surfaced last, ranked by data completeness.

In the brief, list priority-role contacts first. Flag contacts where `in_own_crm` is false as net-new. If no contacts are returned for a partner, note it and move on. If the tool returns no contacts across all partners, note "No partner-shared contacts available."

---

## Step 3 — Synthesize the brief

Using all data gathered, produce the brief below. Rules:
- **Surface Crossbeam's contribution explicitly.** Where ecosystem data changes the picture — a partner with an open deal, movement in the ecosystem, a greenfield signal, a risk — call it out directly. Don't let it disappear into a bullet. The rep should be able to see at a glance what Crossbeam added to this brief that they couldn't get anywhere else.
- Lead with what's most actionable.
- If you can't answer something with confidence, write "No information" — never guess.
- Be succinct. No filler. No generic statements that apply to any account.
- Tailor next steps and discovery questions to any context the user provided.
- Risks & Concerns must be affirmatively justified — if no risks, cite the evidence. An empty section without justification is a failed brief.

---

<output_template>

## TLDR
**[Account]** — [Domain]

Ecosystem: [overlapping partners, or "none identified"] · Recent ecosystem activity: [most notable signal, or "none"]

⚡ [Recommended next action — one sentence.]

---
## ECOSYSTEM RELATIONSHIPS
**What Crossbeam shows:**
- [Partner name] — [Population]

**Recent ecosystem activity:**
- [Date] — [Partner name] — [Event: deal opened / deal closed won / greenfield] — [Contact context if available]

**Partner-shared contacts:**
List priority-role contacts first (economic buyer, decision maker, executive sponsor, technical buyer, primary contact), then key contacts, then any remaining contacts with role unknown. For each:
- [Name] — [Title] — [Role] — shared by [Partner name] — [★ Net-new / Already in your records]

If no contacts were returned: "No partner-shared contacts available."

**Why it matters:**
Call out specifically what the ecosystem data means for this account — a partner with an open opp is a co-sell opportunity, a closed won is a mutual customer worth aligning on, a greenfield is a net new intro. Net-new contacts surfaced by partners are warm paths into the account worth prioritizing. If nothing is actionable, say so plainly.

If no overlap: "No ecosystem relationships identified in Crossbeam."
If no activity: "No recent ecosystem activity."

---
## RISKS & CONCERNS
Tag each with severity: 🔴 High / 🟡 Medium / 🟢 Low.

- **Ecosystem risk signals:** [severity] — e.g. partner closing competing deal, key partner going dark, no ecosystem movement where expected
- **Other risks:** [severity]

---
## GROWTH OPPORTUNITIES
- Ecosystem opportunities: [partner-influenced plays, co-sell potential, greenfield signals from Crossbeam]
- Known expansion signals:
- Untapped use cases:

---

---
## DISCOVERY QUESTIONS
3–4 sharp, specific questions based on gaps in the data and the account's profile.

</output_template>

---

## Rules

- One output: the brief.
- Always run the Crossbeam ecosystem steps (Step 2) — if the account isn't found in Crossbeam, note it and stop.
- Surface Crossbeam's contribution explicitly in the brief — ecosystem signals should never be invisible.
- Strategic partner tag IDs are set by the user in Configuration — do not hardcode them.
- Risks & Concerns must be affirmatively justified, never empty.
- The brief is for internal use only.
- **Don't imply a partner sees what you see.** Crossbeam surfaces what partners have shared with you under your sharing rules. That is not the same as the partner having confirmed the signal, or being able to see this account from their side. Report ecosystem signals as what the data shows, not as partner-stated fact.
- If the user mentions a specific upcoming meeting with known attendees, suggest a meeting-prep skill instead, if one is installed.

</instructions>
