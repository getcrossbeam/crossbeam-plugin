---
name: crossbeam-ecosystem-prospecting
description: >
  Turn your partner ecosystem into a ranked, reasoned prospecting list — find the prospects worth working now, why
  each one, and the warm way in. Crosses your prospects against partners' customers and open deals to surface
  ecosystem-qualified leads (EQLs), scores and ranks them, enriches each with second-party partner context, generates
  a per-account angle and the warm-path contact, and can alert you when an account heats up. Read-only and draft-only:
  it produces a list and first-touch drafts and hands off to your MAP or sequencer; it never sends. Use whenever a rep
  wants better leads or top-of-funnel direction, including asks like "find me EQLs", "who should I prospect", "which of
  my prospects are partner customers", or "what changed on my accounts". Works on your whole prospect base, a segment
  or territory, or a pasted list. The user does not need to mention partners or know what an EQL is. Always use this
  skill for ecosystem prospecting and lead prioritization rather than a raw data lookup.
---

<readme>

# Ecosystem Prospecting (powered by Crossbeam)

Turn your partner ecosystem into a ranked prospecting list: who to work now, why each one, and the warm way in.

## What it does

1. Reads your request and **picks the partner pools most likely to produce good leads** — the partners whose customers fit what you're selling, inferred from the prompt and your partner tags, not a fixed list
2. Pulls your prospects that sit in those partners' customer and open-deal populations — the **EQLs**
3. **Excludes accounts you already sell to**, keeps net-new prospects as the prime list, and flags any that are already open deals in your pipeline
4. Scores and ranks for fit, so you work the strongest signal first rather than reading a raw overlap dump
5. Enriches each lead with the *why* — which partner, customer or live deal — and **flags where Crossbeam already has contacts**, including net-new buyers you don't have in your CRM
6. Gives a per-account angle and the warm path, and skips accounts a partner is mid-deal on
7. Hands the list, and optional first-touch drafts, to your MAP or sequencer

Read-only and draft-only. The list and drafts are produced for you to review; the actual push or send happens in your own tools.

## What you need

**Required**
- **Crossbeam connector** — the source of partner overlap and contact data. Connect from your tool's connector directory or at crossbeam.com. Authenticate to the right org first. The skill will not run without it.

**For the prospect population — any one of these works**
- A description ("my whole prospect base", "enterprise prospects in DACH", "accounts in my territory")
- A pasted list of companies to vet for ecosystem signal
- A CRM report or filter, or your Crossbeam prospect population

**Optional but recommended**
- **Partner tags in Crossbeam** that group partners by integration surface, tier, or theme (e.g. a tag for the partners your product plugs into) — these let the skill pick the right pool automatically
- A CRM or warehouse connector — for segment/territory resolution and to know what you already own
- A MAP or sequencer connector — to hand the list and drafts off directly
- Other GTM skills you have installed — an ICP or persona skill, a sales playbook, your voice skill — so ranking and angles match how your team sells

## Before your first run: what to configure

### 1. Prospect source
Where your prospects come from — a CRM filter, a warehouse query, your Crossbeam prospect population, or a pasted list.

### 2. ICP definition (sharpens ranking)
What a good-fit account looks like, so EQL strength is weighted by fit, not just ecosystem presence.

> **Fill in before sharing:** `ICP: [e.g. B2B SaaS, 200-2000 employees, North America, RevOps or Sales buyer]`

### 3. Anchor partners or partner tag (optional)
The partners — or the partner tag — whose customers are the best fit for what you sell. Leave blank and the skill infers it from the prompt and your tags.

> **Fill in before sharing:** `Anchor partners / tag: [e.g. partners tagged "Integration Partner" — or blank to infer]`

### 4. Exclusions (recommended)
Accounts never to prospect — your own partners and your investors — so a rep never cold-emails one.

> **Fill in before sharing:** `Exclude: [partner accounts, investor accounts — or blank]`

### 5. Destination (optional)
Where the list should go — a MAP, a sequencer, a CSV, or just the chat.

> **Fill in before sharing:** `Destination: [e.g. a named MAP/sequencer, CSV, or chat]`

## Defaults (all adjustable)

| Setting | Default |
|---|---|
| EQL definition | Prospects that are an anchor partner's customer or open opportunity |
| Customers | Excluded (you already sell to them) |
| Open opportunities | Included but flagged as already-in-motion |
| Ranking | Composite EQL score, highest first |
| List size | Deep pull (up to the full qualified pool), not a fixed cap |
| Contact enrichment | Top of the ranked list, not every account |
| First-touch drafts | Off — list only, unless asked |
| Conflict handling | Flag prospects a partner is mid-deal on; do not enroll |
| Sending / pushing | Never automatic. Review, then confirm. |

## How to run it

- **Launch a product:** "Find me leads for our [product] launch" — the skill anchors on the partners that product plugs into
- **Top of funnel:** "Who should I prospect this week?"
- **Vet a list:** "Which of these accounts have ecosystem signal? [paste]"
- **By partner:** "Which of my prospects are customers of [partner]?"
- **Segment / territory:** "Best ecosystem-qualified prospects in my enterprise territory"

## A note on the data

All partner data comes from what your partners have shared with you in Crossbeam under your sharing rules. The skill only surfaces what is already visible to you, never guesses at data a partner has not shared, and never sends or pushes anything without your review.

</readme>

<instructions>

> **Structural tags.** `<readme>`, `<instructions>`, and `<output_template>` delimit sections of
> this file for the agent reading it. They are not content: never echo a tag in your output, and
> where an `<output_template>` is given, reproduce what it contains without the surrounding tags.

## Technical Reference

# Ecosystem Prospecting (powered by Crossbeam)

Top-of-funnel motion. Turn the prospect population into a ranked EQL list, each row carrying partner context, our-side segment, contact availability, the angle, and the warm path. Read-only and draft-only.

The flow: read the prompt and **select the anchor partner pools** -> pull our prospects in those partners' customer/open-opp populations (customers excluded) -> label our-side segment (prospect vs open-opp) -> score, rank, and run the exclusion pass -> enrich the top N with contacts and the warm path -> generate angle -> output the ranked list and optional drafts -> hand off.

## ELG plays

Implements ELG plays P7, P8, P11, and P12. Play records and the foundations subset are embedded in `references/elg-context.md` — read it before executing. It is a self-contained subset of Crossbeam's ELG plays and foundations, not the full catalog.

- **EQL generation (P7)** — surface prospects exhibiting ecosystem behaviour: Steps 1-2.
- **Second-party enrichment (P8)** — replace third-party firmographics with partner context: Step 5.
- **PDR/SDR partner sequences (P11)** — angle, warm path, conflict check: Step 6.
- **Signal-triggered outbound (P12)** — alert on new ecosystem signals: Step 8 (degrades gracefully if the signal tool is absent).

## Configuration
> Set once by whoever deploys this skill. The agent reads these at runtime — do not change them mid-conversation.

**Prospect source:** [fill in — CRM filter, warehouse query, Crossbeam prospect population, or "user will paste"]
**ICP:** [fill in — what a good-fit account looks like]
**Anchor partners / partner tag (optional):** [fill in — partners or a tag whose customers fit the product. Blank = infer from the prompt.]
**Exclusions (recommended):** [fill in — your own partner accounts and investor accounts; never prospect these]
**Destination (optional):** [fill in — MAP/sequencer, CSV, or chat]
**Your company / product name:** [fill in — your own product, e.g. the name your reps use for it]

## Verified tool surface

Confirm exact names/params against the connected server on the first call. Tools observed in production:
- `find_overlaps` — account lists by population intersection. `our_segments` filters YOUR side; the `partners` arg takes either an array of partner specs (each with `id`/`name` + `segments`) or an EcosystemFilter (`tag_name`/`tag_id`, `segments`, `partner_scores`). Returns each account's `partner_names`/`partner_segments` (the PARTNER's segment) and a `total_count`. **The row does not say which of YOUR segments the account is in** — see Step 3. Large reports may return `RetryLater`; fuzzy names return `ClarificationRequired`, and a named partner that doesn't exist returns `is_no_match: true`.
- `find_overlap_partners` — partners that share a given account; supports `partner_tag_name`. Use to confirm which partners carry a tag.
- `find_partner_contacts` — partner-shared contacts at an account, with priority-role `insights` and an `in_own_crm` flag. One call per account.
- `find_partner_recommendations` — EI-recommended partners + `ei_signals` for a named deal/account.
- `find_new_accounts` — pipeline-generation accounts: companies your partners sell to that you could go after. `pipeline_type` classifies each row — `net_new` (absent from your CRM entirely), `prospect` (in one of your prospect populations, overlaps a partner's customers, no open opp), `not_in_my_populations` (in your CRM but in no population, usually a population-rule gap). Supports `partner_names`/`partner_ids`, `partner_tag_name`/`partner_tag_id`, and `sort_by`. **The tag argument WIDENS rather than narrows:** combined with `partner_names`/`partner_ids` it returns accounts for partners matching *either* filter, not both. So a tag plus a named pool pulls in partners outside that pool. To restrict to an intersection, pass one filter and discard non-members yourself. **This is the only tool that reaches true net-new whitespace — `find_overlaps` cannot.**
- `search_crossbeam_knowledge` — ELG/product/blog content.

No ecosystem-activity / signal tool is guaranteed present — Step 8 degrades to an on-demand re-scan.

This skill is **read-only**: it produces a ranked list and optional drafts; the push/send happens in the company's own MAP, sequencer, or CRM.

## Step 0 — Inventory tools

Confirm the Crossbeam MCP is connected and authenticated; if not, stop and say so. Note whether a CRM/warehouse, a MAP/sequencer, and any GTM skills (ICP/persona, playbook, voice) are present — each is used when available, never hard-depended on.

## Step 1 — Select the anchor pools (the judgment that beats a raw dump)

Do **not** default to "all partners, sort by overlap breadth" — that floats megacorps with many partners to the top and buries fit. Instead, pick the partner pools whose customers are the best leads for what's being prospected:

1. **Read the prompt for the product/goal.** "Leads for our [X] launch" → the partners that relate to X.
2. **Prefer a partner tag.** If a tag groups the relevant partners (e.g. an integration-surface tag, a tier, a theme), anchor on it: pass `partners: { tag_name: "<tag>", segments: ["customers","open_opportunities"] }` to `find_overlaps`. Confirm a fuzzy tag with the user (or enumerate members via `find_overlap_partners(partner_tag_name=...)`) so the report is transparent about *which* partners it used and *why*.
3. **Named partners.** If the user names partners, resolve them. If a named partner returns `is_no_match` (not in the ecosystem), **say so plainly** — do not silently drop it. ("X and Y aren't partners in your Crossbeam ecosystem; anchoring on the ones that are.")
4. **Fallback.** No tag or names → anchor on strategic-tagged or high-`partner_score` partners, and/or weight by ICP fit in Step 4. State the basis.

Resolve the prospect population in parallel: CRM/warehouse when connected, else the Crossbeam **prospects** population; a pasted list is vetted directly.

## Step 2 — Pull the EQL pool (customers excluded)

```
find_overlaps(
  list_name: "<product> launch leads — <anchor> customers that are our prospects",
  our_segments: ["prospects"],
  partners: <anchor set with segments: ["customers","open_opportunities"]>,
  sort_by: "partner_overlap_count", sort_order: "desc",
  limit: 100   // paginate via has_more; pull deep, do not stop at a token cap
)
```
Excluding `customers` from `our_segments` drops accounts you already sell to (lower value, per design). Paginate to the full qualified pool — more good leads is the goal; the cap is ICP fit, not an arbitrary number.

## Step 3 — Label the our-side segment (add the open-opp pass)

`find_overlaps` returns the partner's segment, not yours, so a single call can't tell a prospect from an open opp on your side. Step 2 already pulled the `our_segments: ["prospects"]` pass — **that result is your prime lead list; do not re-query it here.** Make one additional call to separate the in-motion accounts:
- **Pass A (prospects)** — reuse the Step 2 result as-is → the prime lead list.
- **Pass B (open opps)** — repeat the Step 2 call with `our_segments: ["open_opportunities"]` (same `partners` anchor set) → accounts already in your pipeline.
Label Pass A as prime; flag anything in Pass B as **already-in-motion** (lower priority, or hand to crossbeam-co-sell-copilot). Default the lead list to prospects only.

**Net-new-to-CRM whitespace needs a different tool.** `find_overlaps` is an *intersection*: it only
returns accounts already in your CRM/populations, so it can never surface an account a partner has
that you have never entered. Do not imply an overlap list contains net-new names.

That whitespace *is* reachable — just not here. `find_new_accounts` with `pipeline_type: "net_new"`
returns a partner's customers that are absent from your CRM entirely. When the user asks for
genuinely new names, call it rather than reframing the overlap list, and say which tool each part of
the answer came from. Carry these caveats: `net_new` rows have no `account_id`, so `domain` is the
key; free plans return only the first page; and an empty result has several possible causes (no
partner sharing greenfield data, plan tier, owner-scoped access, or a rebuild lag of 2–24 hours), so
hedge rather than telling the user they have no net-new accounts.

## Step 4 — Score and rank (the value over a raw overlap dump)

Compute a composite **EQL score** per account:
- **Anchor breadth** — how many anchor partners hold the account as a customer (3 > 2 > 1).
- **Relationship type** — partner's **customer** outranks partner's **open opp** (stronger proof and a warmer reference).
- **ICP fit** — consult an ICP/persona skill if present, else the configured ICP. Down-rank or drop non-ICP that the pool inevitably contains (for a product-SaaS ICP: government, education, PE/VC, GSI/IT-services).
- **Reachability** — a priority-role partner contact exists (from Step 5, for the top of the list).
- **Recency** — recent overlap where available.

Rank highest-first and tier (A/B/C). Never present an unranked dump.

**Exclusion pass (judgment, not data):** drop accounts that are themselves your **partners** or your **investors** (configured exclusion list, plus any account whose name or domain makes it obvious it is a partner, reseller, or investor entity rather than a buyer), and de-dupe segment artifacts (a customer record vs a self-serve duplicate). A rep must never cold-prospect a partner or an investor.

**Watch for population definitions that contradict the segment you asked for.** A population can be configured with a segment that does not match what its accounts actually are — for example a population segmented as `prospects` whose members are also in a customers population. When an account surfaces as a prospect but also appears in a customers population, trust the customers membership and exclude it. Say that you did, and name the population, so the user can fix the definition at source. Silently ranking an existing customer as a prospect is the worst failure this skill can produce.

## Step 5 — Enrich the top N with contacts and second-party context (P8, contact flag)

For the **top N ranked** (not the whole list — one call per account):
```
find_partner_contacts(account_id|account_name, partner_id: <anchor partner>)
```
Per account, flag:
- **Contacts available?** Y/N and count.
- **Priority roles** — economic_buyer, decision_maker, executive_sponsor, technical_buyer (from `insights`).
- **`in_own_crm`** — `false` = a **net-new** buyer you don't have (the strongest warm path); `true` = coverage you already hold.
- **Second-party context** — which partner, customer vs open opp, partner segment/AE where exposed.

**Data hygiene:** validate each contact's email domain against the account domain; drop mismatches (partner-side artifacts).

## Step 6 — Angle and conflict check (P11)

Per top account: a one-line better-together angle grounded in the specific partner relationship (the account already runs/buys the anchor partner → the joint value). Use the playbook/voice skill if present. **Conflict check:** if the account is one a partner is actively working as an open opp, don't cold-prospect — flag for a coordinated co-sell motion. Optionally draft a first touch (off by default) through the de-AI gate in Step 9.

## Step 7 — Output and hand-off

Deliver a ranked list, one row per account: EQL tier/score · anchor partner(s) + relationship type · our-side segment (prospect / open-opp flag) · contacts available + best warm-path contact (and net-new vs known) · angle · exclusion/conflict flags. Keep it scannable; for large lists provide the `list_link` and/or a CSV.
- **MAP/sequencer present:** offer to push the list (and drafts). Confirm destination and rows with the user; never push automatically.
- **Else:** deliver as list, CSV, or chat per the configured destination.

## Step 8 — Signal alerts (P12, graceful degradation)

If a signal/ecosystem-activity tool is exposed, surface accounts that just changed (a partner closed it, a new overlap appeared), timed to the signal. **If absent, do not fail** — offer an on-demand re-scan (re-run Steps 2-3 and diff against the prior list).

## Step 9 — Report and de-AI gate

Compact summary: anchor pools used and why · pool size / `total_count` · prospects vs open-opps · excluded (customers / partners / investors / non-ICP) and counts · top N enriched for contacts · any tool errors, `is_no_match` partners, or `RetryLater` waits. Don't repeat the full list.

Any drafted touch passes the de-AI gate: zero em dashes; cut machine filler ("just reaching out", "I hope this finds you well", "excited to", "circle back", "seamless", the rule-of-three flourish). Would a rep believe a human wrote it? If not, rewrite.

## Recurring runs

Works as a weekly "who's new / who heated up" pass. Offer once to schedule after a clean run. On an auth error mid-run, stop and report.

## Guardrails

- Read-only and draft-only. The Crossbeam MCP exposes `find_*` / `get_*` tools only. Produce the list and drafts; the push/send happens in the company's own MAP, sequencer, or CRM. Never push or send automatically.
- Value over a raw lookup is the point: select the pool deliberately, label the segment, score and rank, enrich the *why*, flag contacts, give an angle. Never hand back an unranked overlap dump — that is what the MCP already does.
- Exclude what you already sell to (customers) and what you must not prospect (your partners, your investors).
- Only surface partner data Crossbeam's sharing rules already expose. Absence of data is not absence of overlap — say "not shared by the partner," never "no overlap."
- Never cold-prospect into an account a partner is actively working — flag for co-sell.
- Net-new-to-CRM whitespace is out of `find_overlaps`' reach (intersection only). Reach it with `find_new_accounts` (`pipeline_type: "net_new"`), and never imply an overlap list already contains it.
- Every angle rests on a real better-together truth. Proof points persuade only, always attributed, never invented.
- Generic and vendor-neutral. No publisher-internal product or skill names. The Crossbeam brand (skill name, "powered by Crossbeam"), Crossbeam MCP tool names, and public case-study companies are fine — the shared interface and public proof every installer has.
- Prospect lists and partner data are sensitive — keep them out of any output not going to the user.

</instructions>
