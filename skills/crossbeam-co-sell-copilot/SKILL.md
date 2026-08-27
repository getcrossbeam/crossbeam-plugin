---
name: crossbeam-co-sell-copilot
description: >
  Help a rep win, unblock, or break into an account by bringing in the right partner — find who can help, work out
  the specific co-sell play, and draft the outreach that gets it moving. Works on a single account, a list, or a
  pipeline filter. Finds the partner best positioned on the account, qualifies how strong that position is, pinpoints
  the partner contact and the right play (intro, intel, a backchannel reference, a voucher for urgency, a joint-deal
  coordination, or a marketplace co-sell), and drafts the ask with something to offer back. Read-only and draft-only:
  it hands off to your own tools, chat, or co-sell workspace and never sends. Use whenever a rep wants help on a deal or
  account, including asks like "who can help me win [Account]", "I need a partner intro", "no partner attached", "deals
  stuck in stage 3", "how do I break into [Account]", or "create urgency without discounting". The user does not need to
  mention partners. Always use this skill to win, unblock, or break into accounts with a partner.
---

<readme>

# Co-Sell Copilot (powered by Crossbeam)

Win, unblock, or break into an account with a partner: find who can help, work out the play, and draft the ask — with something to offer back.

## What it does

1. Takes whatever you give it — a single account ("who can help me win [Account]", "break into [Account]"), a pipeline filter ("deals stuck in stage 3"), or a list — and resolves it against your connected data, or against the partner-ecosystem data if nothing else is available
2. Consults your other GTM context if you have it installed — sales playbook, ICP, voice skill — so the play and the wording match how your team sells
3. Finds which partner is best positioned on each account and qualifies how strong that position really is
4. Works out the specific play: intro to a stakeholder you are missing, intel on how the account buys, a backchannel reference, a voucher for urgency, coordinating a joint deal, or a marketplace co-sell — and flags when no partner adds anything
5. Finds something to offer the partner back
6. Drafts a short, rep-to-rep ask for each account, with a hand-off link to the co-sell workspace where it fits

Read-only and draft-only. Nothing is sent; you review every draft and the actual send, log, or track happens in your own tools.

## What you need

**Required**
- **Crossbeam connector** — the source of partner position, contacts, and activity. Connect from your tool's connector directory or at crossbeam.com. Authenticate to the right org before running. The skill will not run without it.

**For the accounts or deals — any one of these works**
- A specific account or deal named in your request ("who can help me win [Account]", "help me on the [Account] deal")
- A pipeline filter ("deals stuck in stage 3", "open enterprise deals closing this quarter")
- A pasted list, a report, or a data export

Connected data is preferred — it is the right tool for any pipeline filter and it gives the skill your own contacts on each deal, which sharpens the play. With no connected data available, the ecosystem data resolves named accounts; for a pipeline filter, name or paste the deals.

**Optional but recommended**
- A reciprocity source (a field, a sheet, or a note tracking what you owe each partner and what they owe you) — lets the draft reference the running give-and-get
- Other GTM skills you have installed — a sales playbook, an ICP or buying-center skill, your voice skill. If present, this skill reads them so the play and the wording match how your team sells.

## Before your first run: what to configure

### 1. Account or deal source
Where accounts and deals come from — a report or filter, a data export, or a pasted list. Connected data is preferred (pipeline filters and your-side contacts).

### 2. Buying-center map (optional, sharpens the play)
The roles that make a complete buying committee for your sale. The skill compares partner contacts and your contacts against this to name the gap a partner can fill.

> **Fill in before sharing:** `Buying center: [e.g. economic buyer (CRO/CFO), champion (VP Sales), technical eval (RevOps), procurement]`

### 3. Strategic partner tags (optional)
Tier 1 or co-sell partners to weight up so their plays surface first.

> **Fill in before sharing:** `Strategic partner tags: [e.g. Tier 1, Co-Sell — or leave blank for all partners]`

### 4. Reciprocity source (optional)
Where give-and-get balance per partner lives, if anywhere. Leave blank if not tracked.

> **Fill in before sharing:** `Reciprocity source: [e.g. Salesforce field, a Google Sheet, a note — or blank]`

## Defaults (all adjustable)

| Setting | Default |
|---|---|
| Position-strength floor | Medium — accounts where the best partner is Weak are reported but not drafted |
| Scope | Single account, a list, or open pipeline — as given |
| Max accounts per run | 100 |
| Drafts per account | 1 — the single best-positioned partner rep |
| Reciprocal give | On — offer something back where the data supports it |
| Volume confirmation threshold | 25 accounts |
| Sending | Never. Drafts only. |

## How to run it

- **Single account, plain language:** "Who can help me win [Account]?" / "How do I break into [Account]?"
- **Stuck pipeline:** "I have 5 deals stuck in stage 3, can you help me unlock them?"
- **No partner attached:** "This opp hit solution review with no partner — what now?"
- **Urgency:** "Create urgency on the [Account] deal without discounting"
- **Pasted list:** "Run crossbeam-co-sell-copilot on these deals: [paste]"
- **Focus partners:** "Co-sell-copilot, prioritize partners tagged Tier 1"

## What to expect

- **Accounts with no usable partner position:** a normal outcome, reported not drafted. The skill tells you when a partner adds nothing rather than manufacturing an ask.
- **One artifact:** a recommended partner, the right contact, the suggested play, and a draftable ask per account — plus a co-sell workspace link where the play is a joint deal.
- **A specific play, not "a partner overlaps here."** The output names what the partner can do that you cannot do yourself.
- **Drafts only**, to the partner rep, never to the prospect.

## Setting it up as a recurring run

Works well as a weekly pass over open pipeline, or fed an at-risk list from your forecast tool. Ask your assistant to schedule it after a clean manual run.

## A note on the data

All partner data comes from what your partners have shared with you in Crossbeam under your sharing rules. The skill only surfaces what is already visible to you, and never guesses at data a partner has not shared.

</readme>

<instructions>

> **Structural tags.** `<readme>`, `<instructions>`, and `<output_template>` delimit sections of
> this file for the agent reading it. They are not content: never echo a tag in your output, and
> where an `<output_template>` is given, reproduce what it contains without the surrounding tags.

## Technical Reference

# Co-Sell Copilot (powered by Crossbeam)

Co-sell motion across the full deal lifecycle — break-in, progress, unblock, accelerate. For each account or deal, find the partner with the strongest real position, qualify it, classify the specific play, attach a reciprocal give, and draft a rep-to-rep ask with a co-sell workspace hand-off where it fits. Read-only and draft-only.

The flow: resolve the account(s)/deal(s) -> for each, find positioned partners and rank by strength -> qualify (recency, altitude, win-direction) and map coverage against your contacts -> classify the play -> drop accounts below the strength floor or with no incremental value -> find a reciprocal give -> read reciprocity state if configured -> draft the ask -> de-AI gate -> confirm recipients -> deliver -> report.

## ELG plays

This skill implements ELG plays P19, P20, P21, P22, P23, P24, P26, and the reciprocal give P25. The play records and the foundations subset this skill needs are embedded in `references/elg-context.md` — read it before executing. It is a self-contained subset of Crossbeam's ELG plays and foundations, not the full catalog.

How the plays map to the play classification in Step 3:
- **No partner attached** at a mid/late stage: surface candidates and ask the rep what they need to progress — **P19**.
- **Multi-thread** (reach a stakeholder you are missing): package the partner intro request — **P20** — and gather buying intel from the partner inside the account — **P22**.
- **Vouch** (a contact you already have): partner backchannel endorsement — **P23**.
- **Voucher** (urgency without discounting): the partner extends an initial voucher so they are the hero — **P24**.
- **Coordinate** (partner is also selling the account): align deal teams on the better-together next step and hand off to the co-sell workspace — **P21** (use a Deal Navigator link tool if the install exposes one).
- **Marketplace** (deal can transact via a hyperscaler cloud marketplace): identify marketplace overlap and run rep-to-rep co-sell — **P26**.
- The reciprocal give is the AMM reciprocal cell — reciprocal influence on, or a lead into, one of your customers the partner wants — **P25**.

## Configuration
> Set once by whoever deploys this skill. The agent reads these at runtime — do not change them mid-conversation.

**Account or deal source:** [fill in — report/filter, data export, or "user will paste"]
**Buying center (optional):** [fill in — roles that make a complete buying committee for your sale]
**Strategic partner tags (optional):** [fill in — e.g. Tier 1, Co-Sell. Leave blank for all partners.]
**Reciprocity source (optional):** [fill in — where give/get balance per partner lives, or blank]
**Your company / product name:** [fill in — your own product, e.g. the name your reps use for it]

## Defaults

Apply unless the user overrides.

| Setting | Default |
|---|---|
| Position-strength floor | Medium (Weak accounts reported, not drafted) |
| Max accounts per run | 100 |
| Drafts per account | 1, to the single best-positioned partner rep |
| Reciprocal give | On |
| Volume confirmation threshold | 25 accounts |
| Sending | Never. Drafts only. |

**Volume guardrail:** if qualifying accounts exceed the threshold, stop and report how many were found and how many drafts would be created. Wait for explicit confirmation.

## Step 0 — Inventory available tools

1. **Crossbeam MCP (required).** The verified tools on the current surface: `find_partner_recommendations` (partners + `ei_signals` for a deal/account), `find_partner_shared_contacts` (partner-shared contacts at an account, ranked by EI recommendation score; sorted by priority-role signals, then key_contact signals, then data completeness; `in_own_crm` flag identifies net-new contacts not yet in your records), `find_overlapping_partners` (partners that share a given account, sorted by partner-level metrics such as `partner_score` or `deal_size_with_partner`), `find_overlapping_accounts_and_leads` (account lists, including the reciprocal give; supports `our_segments`/`our_populations` filters and ecosystem-wide or per-partner scans), `search_crossbeam_knowledge`. Match on these suffixes; the prefix varies per installation and tool sets differ between installs, so confirm what is present on the first call. Account resolution is built into the `find_*` tools via fuzzy `account_name` / `account_domain` matching — they return `ClarificationRequired` with candidate records — so no separate account-lookup tool is needed. If no Crossbeam MCP is connected, stop and tell the user to connect and authenticate. Do not proceed until confirmed.
2. **An account/deal source (flexible).** Connected data (preferred — exposes your contacts on the deal), or a pasted list.
3. **A reciprocity source (optional).** Only if configured. Read-only.
4. **Other GTM skills (optional).** Consult any installed skill that carries GTM context: an ICP, persona, or buying-center skill (committee definition for Step 2); a sales-playbook skill (frame the play and the ask); a voice skill for the sender (match the draft in Step 6); an account-brief or forecast/pipeline skill (richer context, and the natural source of an at-risk list to feed this skill). Consult when present; never hard-depend — the skill runs fully without any of them.

Detect, do not interrogate. Only ask if there is genuinely no way to get the accounts or deals.

## Step 1 — Intake: resolve the accounts or deals

Detect the input shape, resolve to a concrete set, and confirm anything fuzzy before spending tool calls.

- **Shape A — a single named account or deal** ("who can help me win [Account]", "break into [Account]", "help me on the [Account] deal"). Resolve that one.
- **Shape B — a pipeline filter** ("deals stuck in stage 3", "open enterprise deals closing this quarter"). Turn it into a concrete list, then confirm.
- **Shape C — an explicit list** (pasted, a report, a configured view). Use it directly.

**Resolution, in order of preference:**
1. **Connected data (preferred, and the right tool for pipeline filters).** A filter like "stage 3" is a pipeline query — run it against whatever is connected. This is also where you get your contacts on each deal, which Step 2 needs.
2. **Crossbeam (fallback for named accounts).** With no connected data, resolve a named account by passing it to a `find_*` tool's `account_name` / `account_domain` (they fuzzy-match and return `ClarificationRequired` candidates to disambiguate). State the limit honestly: this resolves named accounts but will not enumerate your pipeline by stage — if the user gave a pipeline filter and nothing is connected, ask them to name or paste the deals.
3. **Neither.** Ask the user to name or paste the accounts/deals. Do not guess.

For each, capture **account name**, **domain** when available, and — for an open deal — **stage**, **amount**, and **the contacts already on the opportunity** (name, title, role). Your-side contacts make the coverage map possible; without them the play is coarser. Confirm any fuzzy set. If the set exceeds the volume threshold, stop and confirm.

## Step 2 — Find the best-positioned partner and qualify the position

Overlap is not co-sell leverage. The partner must be positioned strongly enough, and must be able to do something you cannot already do yourself. The Crossbeam tools return the signals you need directly — use them rather than inferring.

**2a — Resolve the account.** The `find_*` tools fuzzy-match on `account_name` / `account_domain` and return `ClarificationRequired` with candidate records when a name is ambiguous. When that happens, pick by the user's intent — for an expansion or renewal, the record in a **customers** population, not a prospect or self-serve duplicate — confirming with the user if it is unclear. Retry with the confirmed `account_id`.

**2b — Find partners positioned on the account, ranked.** Call `find_partner_recommendations(deal_or_account_name)`. It returns the matching deal(s)/account and the recommended partners, each with `ei_signals`:
- `recent_wins` — the partner closed a deal with this account in the last 3 months (strong: live and winning).
- `long_term_relationship` — the account has been the partner's customer 2+ years (strong: credible to vouch).
- `new_contacts` — the partner added contacts at this account recently (active).
- `missing_contacts` — the partner holds key contacts here that are not yet in your records (the multi-thread signal).
Partners with no active signals are de-prioritized, not dropped. Weight up strategic-tagged partners if tags are configured.

**2c — Pull the partner's contacts and your coverage.** For each candidate, call `find_partner_shared_contacts(account_id, partner_name)`. Each contact carries `insights` / `partner_insights` (priority roles: `decision_maker`, `economic_buyer`, `economic_decision_maker`, `executive_sponsor`, `technical_buyer`, `primary_contact`; secondary: `key_contact`), an `in_own_crm` flag, `partner_populations` (customer / open opp / prospect), and `last_activity_at`.

**2d — Score Partner Position Strength** from those signals:
- **Win-direction / credibility** — Strong if `recent_wins` or `long_term_relationship` is set, or the partner holds the account as a **customer**; Medium if an open opportunity; Weak if prospect-only or dormant.
- **Altitude** — Strong if the partner holds a priority-role contact (economic buyer, decision-maker, executive sponsor) in your buying center; Medium if mid-level; Weak if junior or out-of-center only.
- **Incremental coverage** — Strong if `missing_contacts` is set and the priority-role contacts show `in_own_crm: false` (the partner reaches people you do not); Weak if every useful contact is already `in_own_crm: true` (the partner adds no reach).

Combine: **Strong** (two-plus axes Strong, none Weak), **Medium** (no Weak, not enough for Strong), **Weak** (any axis Weak). Pick the highest-tier partner per account; tie-break by active `ei_signals`, then strategic tag, then most recent `last_activity_at`.

**2e — Name the coverage gap.** From 2c, list the priority-role contacts the partner holds that are `in_own_crm: false`, especially economic buyers and the roles in your configured buying center. That gap is what the partner can open and you cannot — it drives the play in Step 3. If every priority contact is already `in_own_crm: true`, the partner adds no reach: treat as no incremental value.

## Step 3 — Classify the play and apply the floor

Pick the single most valuable play for each account from its position and coverage:

- **No partner attached** — open opp at a mid/late stage with no partner currently engaged: surface candidates and frame what the rep needs to progress (P19), then proceed to the fitting ask below.
- **Multi-thread** — the partner holds a stakeholder you lack: package an intro request (P20), optionally paired with buying intel (P22). Highest-value play.
- **Vouch** — the partner knows a contact you already have, and you need credibility late-stage: backchannel reference (P23).
- **Voucher** — the account is the partner's customer and you need urgency without discounting: partner voucher (P24).
- **Coordinate** — the partner has an active open opp of their own here: align the joint opportunity (P21) and hand off to the co-sell workspace.
- **Marketplace** — the deal can transact via a hyperscaler the account already buys through: marketplace co-sell (P26).
- **No incremental value** — the partner only knows contacts you already own, has no open motion, adds no altitude and no marketplace path. There is nothing to co-sell. Treat as below the floor.

Accounts whose best partner is below the Medium strength floor, **or** classified "no incremental value," are **reported, not drafted**, with the reason in a phrase. Do not ask a partner to help where they add nothing.

## Step 4 — Find a reciprocal give (P25)

For each account that cleared, find one thing to offer back. Between reps on live deals, the strongest trade is reciprocal influence, not a cold lead. Use `find_overlapping_accounts_and_leads(partner)` and prefer an account where the partner has an open opp and you are the customer or strongly positioned — you can advocate for them as you are asking them to advocate for you. Fall back to a lead (an account you are active in and the partner is not yet). If neither exists, set the give to none. Never invent one.

## Step 5 — Read reciprocity state (only if a source is configured)

If configured, read current give-and-get balance for this partner (read-only) and capture one usable fact for the draft. If not configured, skip silently.

## Step 6 — Draft the ask

The recipient is the **partner rep** who owns the relationship — never the prospect, never the partner contact inside the account, never your own customer. A draft addressed to the account is a hard failure. Resolve the recipient from the partner owner in the contact/overlap data; apply hygiene (skip if no partner-side owner email; skip if the owner domain matches the prospect's; skip system accounts like `integration@`, `no-reply@`).

**Voice:** rep-to-rep, director-level, concise (90 to 140 words). No emojis. No em dashes. No partnership jargon — "synergy," "leverage," "unlock," "flagship," "premier" are banned. Plain verbs.

**Structure:** one-line frame (you partner with their company, you are working this account) -> the play from Step 3, grounded in real position, without claiming data you cannot see -> the give from Step 4 (or a reciprocity fact from Step 5) -> one CTA, an easy yes or no.

**Match the body to the play:**
- **Multi-thread** -> name the stakeholder you are missing and ask for the intro or the door; offer the give.
- **Vouch** -> name the shared contact and ask for the endorsement; offer the give.
- **Voucher** -> propose the partner extend an initial voucher to create urgency, positioning them as the hero.
- **Coordinate** -> propose aligning on the committee and timing; if the install exposes a Deal Navigator link tool, offer to open the joint opp in the co-sell workspace (P21).
- **Marketplace** -> note the shared marketplace and propose rep-to-rep co-sell through it.
- **No give available** -> run the same ask without the trade and say you will look for a way to return the favor. Do not fake a give.

**ELG framing:** lead with a real "better-together" reason the partner should help. You may cite one proof point from `references/elg-context.md`, attributed to its source, only when genuinely relevant and true — never invent or reattribute a metric. If there is no real better-together truth, make the plain ask instead.

Only reference facts the Crossbeam data and your own deal data actually show. Do not invent a partner's deal stage, a contact's disposition, or relationship history.

## Step 7 — De-AI pass (hard gate before delivery)

Re-read every draft. No draft is delivered until it passes both checks.
1. **Zero em dashes.** Remove every em dash and dash used like one; restructure with a comma, a period, or two sentences.
2. **No machine-written filler.** Cut "I hope this finds you well," "I wanted to reach out," "just reaching out," "touching base," "circle back," "delve," "robust," "seamless," "streamline," "excited to," "thrilled to," "navigate," "landscape," "game-changer," "win-win," the rule-of-three flourish, and generic compliments.

The test: would a busy rep read this and assume a human typed it between calls? If not, rewrite until yes.

## Step 8 — Confirm recipients, then deliver

**Before creating any drafts, confirm the run.** One summary line per account:
- [Account] ([amount], [stage] if a deal) -> [Partner] / [Rep name] / [Rep email] · Position: [Strong/Medium] · Play: [no-attach / multi-thread / vouch / voucher / coordinate / marketplace] · Give: [give account or none]

Ask the user to confirm; let them drop any line where the partner, rep, or play looks wrong. Only create drafts after explicit confirmation.

**What the gate blocks, precisely.** Sending anything is a hard stop: never send, even if a send tool exists. Copy-ready text in chat is not a send, so you may show it in the same turn — but the confirmation summary must appear **above** the drafts, so the user reads who each one is addressed to before they copy anything. Never present drafts with the recipient list buried below them or omitted.

**Default delivery:** produce all drafts as copy-ready text in chat, one block per account with recipient, subject, and body. For a Coordinate play, include the Deal Navigator link.

Subject lines — vary, pick the most specific:
- `[Account]: can you help us reach [stakeholder]?`
- `[Account] — we are both in, worth aligning?`
- `Quick hand on [Account]?`

## Step 9 — Report

Compact summary:
- Accounts scanned / drafts created / accounts skipped, with the reason (no partner / not shared by partner / below floor / no incremental value / no qualifying rep)
- One line per draft: `[Account] -> [Partner] / [Rep name] · Play: [type] · Give: [account or none]`
- Accounts below the floor or with no incremental value listed separately so the user sees what was deliberately not asked and why
- Any tool errors

Do not repeat full email bodies — they are in the drafts.

## Guardrails

- Read-only and draft-only. The Crossbeam MCP exposes `find_*` / `get_*` tools only. Deliver intelligence and drafts; the send, log, or track happens in the company's own tools, chat, or the linked co-sell workspace. Never claim end-to-end execution. Never send email.
- Only surface partner data Crossbeam's sharing rules already expose to this user. Never speculate about data you cannot see. Absence of data is not absence of overlap — say "not shared by the partner," not "no overlap."
- Every ask must rest on a real "better-together" truth and the partner's consent to be approached. Do not manufacture a joint-value story. Proof points are for genuine persuasion only, always attributed, never invented.
- Resolve accounts/deals from connected data when available (the right tool for pipeline filters); otherwise resolve named accounts from Crossbeam and ask the user to name or paste deals when a pipeline filter cannot be run. Confirm any fuzzy set before drafting.
- Recipient is always the partner rep. Never the prospect, never the partner contact, never your own customer.
- Never draft where the partner is below the strength floor or adds no incremental value. Reporting it is correct; asking is a failure.
- Generic and vendor-neutral. No publisher-internal product or skill names. Crossbeam MCP tool names and public case-study companies are fine — they are the shared interface and public proof every installer has.
- The reciprocity source is read-only. This skill does not write give/get state.
- Deal amounts, pipeline, and partner data are sensitive — keep them out of any output not going to the user.

</instructions>
