---
name: ecosystem-powered-pipeline-prioritization
description: Scans a configured list of accounts for Crossbeam ecosystem intelligence and signals — partner deal activity, overlap populations, and recent partner movements — then ranks accounts by signal strength and surfaces the ones worth acting on. Optionally generates an outreach angle per account based on the ecosystem context. Use whenever someone wants to scan accounts for partner signals, prioritize their pipeline using ecosystem data, run a daily or weekly signal scan, or identify which accounts have buying motion their team can't see. Not for one-off account lookups — use the Ecosystem-Informed Account Brief for that.
---

<readme>

# README — Ecosystem-Powered Pipeline Prioritization

Find the accounts on your list that are worth acting on now — surfaced by real partner deal activity, not modeled intent.

## What it does

Takes a list of accounts you care about, scans Crossbeam's ecosystem intelligence and signals across each one, and returns a ranked list of the accounts with the most meaningful partner activity — with an optional outreach angle per account so your team knows how to engage.

This isn't a report you build once. Run it on a schedule and you'll know every morning which accounts just moved.

## What Crossbeam Ecosystem Signals are

Ecosystem Signals are partner-driven events that surface real buying motion on your accounts. When a partner opens or closes a deal on an account you're tracking, that's a signal. When multiple partners are active on the same account in the same window, that's a convergence — and convergences tend to mean something is actually happening.

Unlike intent data or behavioral signals, Ecosystem Signals come from actual deal activity inside your partner network. They're not modeled or inferred — a partner worked this account, or they didn't.

## The value of layering Ecosystem Signals with your existing signals

Most teams already have a way to qualify accounts — intent scores, engagement data, pipeline stage, product usage. Crossbeam Ecosystem Signals add a layer those sources can't see.

An account that scores high on intent but also has three partners with active deals is a fundamentally different conversation than intent alone. That convergence — your existing signals plus Crossbeam's ecosystem layer — surfaces accounts that no single source would show you. Ecosystem Signals are valuable on their own. Combined with what you already know, they give you the most complete picture of which accounts have real buying motion happening right now.

## What you need

**Required**
- **Crossbeam connector** — the source of ecosystem intelligence and signals. Connect from the Claude connector directory or at crossbeam.com. Make sure you're authenticated to the right org before running.

**Required — account source**
You need a list of accounts to scan. Configure where that list comes from in the skill's Configuration section before running.

**Optional but recommended**
Crossbeam Ecosystem Signals stand on their own — real partner deal activity is a signal worth acting on regardless of what else you know about an account. But if you want a 360-degree view, layering Ecosystem Signals with your existing qualification and intent data gives you a more complete picture. An account showing partner activity AND scoring high on intent AND having recent engagement is a convergence of signals that no single source could surface alone.

## Before this works — what your partners need to share

**For signals to appear, your partner must:**

- **Have a CRM connected to Crossbeam**
- **Be syncing and sharing the following fields with you:**
  - **Deal open date**
  - **Deal close date**
  - **Deal is closed**
  - **Deal is won**

> **Note:** Your company only needs to sync these fields with Crossbeam — you do not need to share them back with your partner.

**To receive contact information on signals, your partner must also:**

- **Be syncing and sharing CRM contact data**

**If contact data is included, signals may contain:**

- **Contact Name**
- **Contact Title**

## Setup: what to configure before your first run

**Step 1 — Connect Crossbeam**
Connect from the Claude connector directory or at crossbeam.com. Authenticate to the right org.

**Step 2 — Define your account source**
Fill in where your account list comes from and how it will be pulled. This is your starting list — Ecosystem Signals layer on top to show which accounts have active partner motion. Be specific: name the source, the segment or filter you're using, and how Claude should access it (e.g. via a connected CRM connector, a manual CSV upload, a named account list you'll paste in).

> **Fill in before sharing:** `Account source: [e.g. "Open opportunities in Salesforce, Enterprise segment, pulled via Salesforce connector" or "Top 300 prospect accounts — will paste as a list" or "Named account list uploaded as CSV"]`

**Step 3 — Set your signal lookback window**
How far back should the scanner look for partner activity on each account? Set this based on your typical sales cycle and how fresh you want the signals to be. A shorter window (30 days) keeps results focused on what's moving right now. A longer window (60–90 days) is better for enterprise deals that move slowly or where you want to catch signals you may have missed.

> **Fill in before sharing:** `Signal lookback window: [e.g. 30 days / 60 days / 90 days]`

**Step 4 — Set your Crossbeam partner tags (optional)**
By default the scanner picks up signals from every partner you're connected to in Crossbeam. That's useful for broad coverage — but if you have a defined set of strategic or co-sell partners whose signals matter most for pipeline, filtering to their tags keeps the output focused on the relationships that are most likely to move deals. This is worth configuring if you have tiered partner programs, active co-sell motions, or specific ISV relationships your sales team is already aligned on.

> **Fill in before sharing:** `Strategic partner tags: [e.g. Tier 1, Co-Sell — or leave blank to include all partners]`

**Step 5 — Set your output destination**
Decide where the ranked list gets delivered. Think about who needs to see it and in what format.

> **Fill in before sharing:** `Output destination: [e.g. in-chat / Slack channel / Google Doc / slide deck / other]`

**Step 6 — Set your volume confirmation threshold (optional)**
If your account list is large, this sets the point at which the scanner pauses and asks you to confirm before scanning. It is worth setting if you are pointing the skill at a broad segment and want to avoid accidentally running across thousands of accounts on a first try. Leave it blank for no limit.

> **Fill in before sharing:** `Volume confirmation threshold: [e.g. 100 accounts — or leave blank for no limit]`

**Step 7 — Run it once and adjust**
After a first run, ask Claude to help you refine the scoring weights, change the lookback window, or trim the output format to what your team actually reads.

## How to run it

> "Run the ecosystem pipeline scanner"
> "Scan my account list for partner signals"
> "Which of my accounts have ecosystem activity this week?"
> "Run the scanner and give me outreach angles for the top accounts"

## What to expect

- **Ranked output:** accounts sorted by signal strength
- **Signal summaries per account:** what's moving, which partners, what it likely means
- **Outreach angles (optional):** suggested entry point per account based on ecosystem context
- **No partner contact details surfaced directly:** notes they're available and routes through your partnerships lead
- **Accounts with no signals:** noted in output, not silently dropped
- **Partnerships lead note:** before acting on any signal, check with your partnerships lead — they may already have an active motion

## Not sure how to set something up?

Just ask Claude — it can walk you through connecting tools, finding your Crossbeam partner tags, setting up a scheduled run, or adapting the output format.



## A note on the data

All partner data comes from what your partners have shared with you in Crossbeam under your sharing rules. The skill only surfaces what is already visible to you, and never guesses at data a partner has not shared.

</readme>

<instructions>

> **Structural tags.** `<readme>`, `<instructions>`, and `<output_template>` delimit sections of
> this file for the agent reading it. They are not content: never echo a tag in your output, and
> where an `<output_template>` is given, reproduce what it contains without the surrounding tags.

# Skill Instructions

Scans a configured list of accounts against Crossbeam's ecosystem intelligence and signals, ranks by signal strength, and surfaces accounts worth acting on. One output: a ranked list with signal summaries, plus optional outreach angles.

---

## Configuration
> These values are set once by whoever deploys this skill. Claude reads them at runtime — do not change them mid-conversation.

**Account source:** [fill in — name the source, the segment or filter, and how Claude should access it. e.g. "My open opportunities, Enterprise segment" or "Top 300 prospect accounts" or "Named account list"]

**Signal lookback window:** [fill in — how far back to look for partner activity. e.g. 30 days / 60 days / 90 days]

**Strategic partner tags (optional):** [fill in — e.g. Tier 1, Co-Sell. Configure this if you have a tiered partner program or active co-sell motions and want signals filtered to those relationships. Leave blank to include all partners.]

**Output destination:** [fill in — e.g. in-chat / Slack channel / Google Doc / slide deck / other]

**Volume confirmation threshold (optional):** [fill in — e.g. 100 accounts. Claude pauses and confirms before scanning if the list exceeds this. Leave blank for no limit.]

---

Crossbeam MCP is required. The scanner works with Crossbeam alone. For a more complete picture, the account source can reflect your team's existing qualification logic — intent scores, pipeline stage, or any other filter — so that Ecosystem Signals are layered on top of what you already know about each account.

---

## Step 1 — Get the account list

Get the account list from the configured account source. If no source is configured, stop and tell the user they need to define or share one source or connector before running — do not pull from any source automatically.

If the list exceeds the configured volume confirmation threshold, stop and tell the user how many accounts were found and ask them to confirm before proceeding.

For each account, collect at minimum: account name and domain. Domain makes Crossbeam matching more reliable — include it whenever the source has it.

---

## Step 2 — Pull Crossbeam ecosystem data for each account

For each account on the list, run the following. If the account isn't found in Crossbeam, note it and move to the next — don't stop the scan.

> **Tool names:** the Crossbeam MCP tool-name prefix varies per installation (e.g. `Crossbeam:`, `mcp__Crossbeam__`). Match on the suffixes below rather than the full name, and confirm the actual tool surface on the first call — tool sets differ between installs.

**2a — Resolve the account in Crossbeam**
```
get_account_context(account_domain: "example.com")
```
Accepts `account_domain`, `account_name` (fuzzy), or `account_id`. Domain is the most reliable. Extract the account's CRM `record_id` / `account_id` — Step 2c needs it.

**2b — Get partner overlap**
```
find_overlap_partners(account_id: "<record_id>", limit: 100)
```
**Always pass `limit`.** The tool defaults to `limit: 10`, so an account overlapping more partners than that silently returns only the first page, with no error and no truncation flag. Pass `limit: 100` and paginate with `page` until the partner list is complete.

If strategic partner tags are configured, filter in the same call — this tool takes the tag directly, so no second call and no manual intersect is needed:
```
find_overlap_partners(account_id: "<record_id>", partner_tag_name: "<tag>", limit: 100)
```
An ambiguous tag returns `ClarificationRequired` with candidates; resolve it once at the start of the scan and reuse the confirmed `partner_tag_id` for every remaining account rather than re-prompting per account.

**`partner_tag_name` takes a single tag, not a list.** Configuration invites several (e.g. "Tier 1, Co-Sell"). If more than one is configured, make one call per tag and union the results by partner before scoring. Passing only the first tag silently drops partners carrying only the others, and because those partners' signals then never enter scoring, the account can be reported as "no signals detected in window" when it actually has some. If no tags are configured, omit the tag argument to get every partner that shares the account.

Capture: partner name, population name (interpret intent over exact string — "Pipeline", "Active Opportunities" = open opportunity; "Clients", "Active Customers" = customer).

**2c — Get ecosystem activity signals**
```
get_ecosystem_activity(record_ids: ["<record_id>"], resource_type: "accounts")
```
This tool filters by CRM **record ID**, not by domain — use the `record_id` from Step 2a. Optionally narrow with `partner_names` / `partner_ids`, or with `event_types`, whose valid values are `partner_deal_opened`, `partner_deal_closed_won`, and `partner_greenfield_deal_closed_won`. If strategic partner tags are configured, pass the tagged partner names from Step 2b.

**Resolve `partner_names` before the scan, then pass `partner_ids`.** If any name resolves to zero or multiple partners the tool returns `ClarificationRequired` and **no events at all** — which this step would otherwise read as "no activity" and score 0, producing the exact false negative Step 2b warns about. Resolve the tagged partner names once at the start of the scan, then pass the confirmed `partner_ids` for every account, so an ambiguous name cannot re-prompt once per account across a long run.

**The tool has no date-range parameter** — it returns events newest first. Apply the configured lookback window yourself by discarding events older than it, and paginate (`page`, `limit`) only until events fall outside the window.

Capture: event type (deal opened / deal closed won / greenfield), partner name, date, contact name and title if available.

---

## Step 3 — Score and rank accounts

Score each account based on ecosystem signal strength. This scoring adds a dimension to the user's existing qualification logic — it doesn't replace it.

**Score in-window motion, not static overlap breadth.** This is the difference between a scanner
that works and one that just re-sorts your biggest accounts. Large, well-known companies overlap
dozens of partners each. If per-partner population counts enter the score, that near-constant
breadth term dominates and buries the deals you are scanning for.

**Signal scoring. Only in-window motion originates a score:**
- +3 per partner with a deal **closed won** on this account inside the lookback window
- +2 per partner with a deal **opened** on this account inside the lookback window
- +2 bonus if 3 or more partners have **in-window activity** (convergence signal)
- +1 bonus if any in-window event occurred in the last 14 days (recency bonus)

**Modifiers. These apply only to an account that already scored above zero** on the terms above.
They separate accounts that both have motion; they can never lift a dormant account above an
active one. If an account has no in-window activity, skip this block entirely.
- +2 per partner with a **currently open opportunity** on this account — **capped at +4 total**.
  An open opp is real, but it is a standing state rather than new motion; two partners is enough
  to establish it. **Count only partners whose open opportunity was not already scored as an
  in-window `deal opened` above.** A deal opened in the window and still open is one event, not two;
  scoring it in both places lets recent opens outrank closed-won, inverting the priority below.
- +2 per partner carrying a configured strategic partner tag — **capped at +4 total**

**An account with no in-window partner activity scores exactly 0.** Standing open opportunities
and strategic tags do not change that, because gating them is the only thing that makes the
ranking check below actually hold.

**Worked check before you rank.** Account A: two partners closed won in-window = 3 + 3 = **6**.
Account B: two partners opened deals in-window and both are still open = 2 + 2 = **4**, and the
open-opportunity modifier adds **nothing**, because both partners were already counted as in-window
opens. A ranks above B, which is correct — closed won is the strongest signal. If your arithmetic
puts B first at 8, you double-counted the opens.

**Populations do not score.** Membership in a prospect or other population earns zero. Overlap
breadth is useful context, so report it next to the score ("38 overlapping partners") — never
inside it.

**Rank** accounts highest score first. Accounts with zero signals go to the bottom of the list —
include them but mark as "no signals detected in window."

**Check the ranking before you present it.** An account with no in-window partner activity must
score 0 and must sort below every account that has any. If a zero-activity account outranks one
with a live partner deal, the scoring has been misapplied — recompute rather than presenting it.
Likewise, if event volume alone is floating an account to the top, look at what those events are:
a stream of one- and two-seat add-ons at a partner's existing customer is not buying motion, and
should not outrank a genuine new-business deal. Say so when you demote it.

---

## Step 4 — Produce the ranked output

Deliver to the configured output destination. Format:

<output_template>

---
**ECOSYSTEM PIPELINE SCAN**
Date: [today] · Lookback: [configured window] · Accounts scanned: [N] · Accounts with signals: [N]

---
**[Rank]. [Account Name]** — Signal score: [N]
**What Crossbeam shows:** [partner name] — [population] — [event type, date]
**Why it matters:** [1–2 sentences interpreting what the partner activity implies about this account — buying motion, stack change, entry point. Be specific to the signals, not generic.]

---
[repeat for each account with signals]

---
**No signals detected:**
[list account names only]

</output_template>

Keep signal summaries tight — one clear interpretation per account, not a list of raw data points. The rep should be able to read the "Why it matters" line and immediately understand why this account is on the list.

Do not surface partner owner contact details in the output. If signals suggest a partner play, note that contact details are available in Crossbeam and recommend the user coordinate with their partnerships lead before engaging — they may already have an active motion or relationship context on this account.

---

## Step 5 — Outreach angles (optional)

Only run this step if the user explicitly requests outreach angles — either at run time ("give me outreach angles") or as a configured default.

For each account in the ranked output, generate a suggested outreach angle based on the ecosystem context:

- What the partner activity implies about the buying situation (e.g. stack change in progress, vendor evaluation underway, new relationship forming)
- What the entry point is for this account given those signals (e.g. reach out now before the stack solidifies, reference the shared partner relationship as context)
- What to avoid (e.g. don't mention specific partner deal details — you're not supposed to know the specifics, only that there's activity)

Keep angles to 2–3 sentences. These are talking points, not drafted messages. If the user wants a drafted message, route them to the Ecosystem-Informed Outreach Writer skill (or any outreach-drafting skill they have installed).

Note on every outreach angle: confirm with your partnerships lead before engaging — they'll know if there's already a motion in place and can help frame the approach.

---

## Step 6 — Offer to schedule

If the user hasn't already set up a scheduled run, offer once at the end: "Want me to set this up to run automatically on a cadence?" If yes, ask for the frequency (daily, weekly, specific day/time) and output destination, then set up the scheduled task.

---

## Rules

- One output: the ranked list. No deep-dive briefs — route those to the Ecosystem-Informed Account Brief skill.
- Always run Crossbeam ecosystem steps (Steps 2a–2c) for every account on the list.
- Do not pull the account list from any source not explicitly configured. If no source is configured, stop.
- Volume confirmation threshold must be respected — never scan more than the threshold without explicit user confirmation.
- Do not surface partner owner contact details. Note availability and route through partnerships lead.
- Outreach angles are optional — only generate if explicitly requested.
- Partnerships lead coordination note must appear wherever outreach is suggested.
- Strategic partner tag IDs are set by the user in Configuration — do not hardcode them.
- Accounts not found in Crossbeam are noted, not silently skipped.
- The output is for internal use only.
- **Don't imply a partner sees what you see.** Crossbeam surfaces what partners have shared with you under your sharing rules. That is not the same as the partner having confirmed the signal, or being able to see this account from their side. Report ecosystem signals as what the data shows, not as partner-stated fact.

</instructions>
