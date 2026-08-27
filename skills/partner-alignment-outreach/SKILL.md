---
name: partner-alignment-outreach
description: Turn recent closed-won deals into partner-alignment outreach. Pulls closed-won deals from a connected data source or a pasted list, uses the Crossbeam MCP to find which partners overlap each won account and who owns the relationship on the partner side, then drafts a short alignment email to each partner rep as copy-ready text. Use whenever someone wants to notify partners about closed-won deals, run post-win co-sell follow-up, "let partners know we won X", "draft partner outreach for recent wins", set up weekly partner-alignment outreach, or align with partner reps after deals close, even if they don't mention Crossbeam, overlaps, or email explicitly.
---

<readme>

# Partner Alignment Outreach

Turn recent closed-won deals into partner alignment emails, one per deal, addressed to the right rep at the right partner.

## What it does

1. Pulls your recent closed-won deals from a connected data source or a pasted list
2. Looks up each won account in Crossbeam to find which partners overlap on it and who owns the relationship on their side
3. Drafts a short, rep-to-rep alignment email for each deal, delivered as copy-ready text in chat

Nothing is sent automatically. You review every draft before it goes anywhere.

## What you need

**Required**
- **Crossbeam connector:** connect it from your tool's connector directory or at crossbeam.com. The skill won't run without it. Make sure you're authenticated to the right org before starting.

**For pulling deals: any one of these works**
- A connected data source (a report or pasted export of recent closed-won deals)
- A pasted list of account names

## Setup

**Connect Crossbeam**
Connect from your tool's connector directory or at crossbeam.com. Authenticate to the right org. That's all that's required.

## Optional configuration (set once, runs every time)

Everything below can be left blank. The skill asks at runtime for anything it needs and applies sensible defaults.

**Strategic partner tags:** which Crossbeam tags identify your priority partners (e.g. Tier 1, Co-Sell). The skill will weight those partners higher when multiple partners overlap the same account. Leave blank to consider all partners equally.

## Defaults (all adjustable)

| Setting | Default |
|---|---|
| Lookback window | [e.g. last 4 weeks] |
| Minimum deal size | [e.g. $5,000] |
| Deal types | [e.g. new business + expansion, renewals excluded] |
| Max deals per run | [e.g. 100] |
| Emails per deal | [e.g. 1, best-positioned partner rep only] |
| Strategic partner tags | [e.g. none, all partners weighted equally] |
| Volume confirmation threshold | [e.g. 25 deals] |

The volume confirmation threshold is a safety check. If more deals qualify than this number, the skill stops, reports the count, and waits for you to confirm before generating anything. It exists so a run against an unintended scope cannot quietly turn into a bulk batch.

## How to run it

The skill detects your connected tools automatically. You have a few options for how to kick it off:

- **Run it as-is:** uses all defaults: "Run partner alignment outreach"
- **Specify a time window:** "Run partner alignment outreach for deals closed this week"
- **Filter by deal size:** "Run partner alignment outreach for deals over $20k"
- **Focus on specific partners:** "Run partner alignment outreach, prioritize partners tagged Tier 1"
- **Include renewals:** "Run partner alignment outreach, include renewals"
- **Limit the run:** "Run partner alignment outreach, just the top 5 deals"
- **Paste a list:** "Run partner alignment outreach for these accounts: [paste]"

Mix and match any of these in a single request.

## What to expect

- **Deals with no overlap:** normal outcome. Not every won account has a partner in Crossbeam. These are noted in the summary.
- **Overlaps with no owner email:** also normal. Partner data quality varies. Some partners don't share owner fields. The skill filters these out and explains why in the summary so you're not left wondering why a particular partner was skipped.
- **Multiple overlapping partners per deal:** the skill picks the single best-positioned rep based on partner population (open opportunity, customer, prospect), owner title, and whether the partner matches your strategic tags. One draft per deal. That ordering is a default, not a fixed rule: if you care more about mutual-customer alignment than co-sell pipeline, say so and the skill will reweight for the session.
- **Recipient confirmation:** before any drafts are created, the skill shows you who it plans to write to, one line per deal. Nothing is generated until you confirm, and you can drop anyone who looks wrong from the run.
- **Volume check:** if more deals qualify than your confirmation threshold, the skill reports the count and stops rather than proceeding.
- **Drafts in chat:** all drafts are delivered as copy-ready text. Nothing is ever sent.

## Setting it up as a recurring run

Once you've run it once and are happy with the output, ask your assistant to schedule it as a weekly task (e.g., Monday mornings to catch the prior week's wins).

## A note on the data

All overlap data comes from what your partners have chosen to share with you in Crossbeam. The skill only surfaces what's already visible to you under your sharing rules. It won't surface or guess at data your partners haven't shared.

## Not sure how to set something up?

If you're unsure how to connect a tool, find your Crossbeam partner tags, or adapt the defaults for your team's workflow, just ask your assistant. It can walk you through any of it.

</readme>

<instructions>

> **Structural tags.** `<readme>`, `<instructions>`, and `<output_template>` delimit sections of
> this file for the agent reading it. They are not content: never echo a tag in your output, and
> where an `<output_template>` is given, reproduce what it contains without the surrounding tags.

## Technical Reference

# Partner Alignment Outreach

When a deal closes, the partners who overlap on that account are the people who most want to know, and the ones most likely to return the favor. This skill turns a list of recent closed-won deals into one short, specific alignment email per deal, addressed to the right rep at the right partner.

The flow: pull recent closed-won deals, find the partner with the strongest position on each account and the rep who owns it, draft a concise alignment email, deliver as copy-ready text (never sent) plus a summary.

## Configuration
> Optional defaults set by whoever deploys this skill. All fields can be left blank. The skill asks at runtime for anything not configured here.

**Strategic partner tags (optional):** [fill in, e.g. Tier 1, Co-Sell. Leave blank for all partners.]
**Your company / product name:** [fill in, e.g. the name your reps use for your product]

## Defaults

Apply these unless the user specifies otherwise.

| Setting | Default |
|---|---|
| Lookback window | [e.g. last 4 weeks] |
| Minimum deal size | [e.g. $5,000] |
| Deal types | [e.g. new business + expansion, renewals excluded] |
| Max deals per run | [e.g. 100; if context runs tight, process at least 20 largest first] |
| Emails per deal | [e.g. 1, to the single best-positioned partner rep] |
| Sending | Never. Drafts only. |
| Strategic partner tags | [e.g. none, all partners considered equally] |
| Volume confirmation threshold | [e.g. 25 deals] |

**Volume guardrail:** if the number of qualifying deals exceeds the confirmation threshold, stop and tell the user how many deals were found and how many drafts would be created. Wait for explicit confirmation before proceeding. This prevents accidentally running a bulk send against an unintended scope.

## Step 0 — Inventory available tools

Check what's connected before starting. Three things matter:

1. **Crossbeam MCP** (required). Look for tools whose names contain `find_overlapping_partners`, `find_overlapping_accounts_and_leads`, `get_account_context`, `find_partner_shared_contacts`, `get_ecosystem_activity`, or `get_partner_sharing_status` (used in Step 2 to confirm the partnership is active; optional, skip the check if absent). The tool-name prefix varies per installation. Match on these suffixes and confirm the actual surface on the first call, since tool sets differ between installs. If no Crossbeam MCP is connected, stop and tell the user to connect the Crossbeam connector (available in your tool's connector directory or at crossbeam.com) and authenticate before running. Nothing else in this skill works without it. Do not proceed past this step until Crossbeam is confirmed connected.
2. **A deal source** (flexible). Connected deal data, a pasted list, or nothing. If nothing, ask the user to paste their recent closed-won deals.

Don't ask the user which tools they have. Detect, then only ask if there's genuinely no way to get deals.

## Step 1 — Pull recent closed-won deals

Pull closed-won deals from the configured deal source. For each deal, collect at minimum: **account name**, **deal amount**, **close date**, plus **account domain** and **deal type** when available. Domain makes Crossbeam matching more reliable, so include it whenever the source has it.

Apply the configured defaults: close date within the lookback window, amount at or above the minimum, deal type not a renewal (unless overridden). Sort largest first.

If the user hasn't specified a deal source, ask them before proceeding. If no deal source is found, stop and tell the user to paste their deal list.

If zero deals qualify, stop and report "No qualifying closed-won deals in the window." Don't draft anything.

## Step 2 — Find the best partner rep for each deal

For each deal, call the Crossbeam `find_overlapping_partners` tool with the account's domain (preferred) or name. On the first call of a run, inspect the actual response shape before assuming field names. Crossbeam MCP versions differ in what they expose. If the tool returns a ClarificationRequired for an ambiguous account, prefer retrying with the domain rather than interrupting the run. If still ambiguous, skip the deal and note it in the summary.

From the overlapping partners, you're looking for two things per partner: **what segment/population the account sits in on the partner's side** (open opportunity, customer, prospect) and **who owns the account on the partner's side** (owner name, email, title).

**Filter out** partners where:
- No partner-side owner email is exposed. Before discarding, try `find_partner_shared_contacts` for that account and partner. It may surface a partner-shared contact who can serve as the recipient.
- The owner email's domain matches the won account's own domain (that "owner" is the customer, an unassigned bucket, or an integration user; a data artifact, not a person to email).
- The owner is obviously a system account (emails like `integration@`, `api@`, `no-reply@`, `gtmops@`).

Partners filtered out for any of these reasons are normal. Data quality varies across partnerships. Note them in the summary under "skipped: no qualifying rep" so the user understands why no draft was created for that partner, rather than assuming no overlap exists.

**Confirm the partnership is live before drafting.** For each partner that survives the filter, call `get_partner_sharing_status` with the partner and the won account. It returns `partnership_status` (`active` or `inactive`) and, when the account resolves, `sharing_status` (`shared`, `not_shared`, or `not_present`).

- **`partnership_status: inactive`:** do not draft. A rep-to-rep alignment note on a dormant partnership is worse than no note. Report it as "skipped: partnership inactive."
- **`sharing_status`** is your own sharing, not the partner's. It says whether you share this account with them. It does not tell you what they share with you, so never use it to explain a missing partner-side owner email. Its use here is framing, in Step 3.
- **`ClarificationRequired`:** the partner name, the account, or both resolved to zero or several candidates, so neither status field comes back. Do not read that as inactive and do not drop the partner. Because this call runs once per surviving partner per won deal, an ambiguous partner name would otherwise prompt the user once per deal. Resolve each ambiguous name once, reuse the confirmed IDs for every remaining deal in the run, and treat any name still unresolved as "status unconfirmed." Draft it under the unconfirmed rule in Step 3.
- **No `sharing_status` in the response** (the account did not resolve, or the tool returned only `partnership_status`): treat it as unconfirmed rather than as `not_shared`, and use the unconfirmed wording in Step 3.

If the tool is not present in this installation, skip this check and proceed. Note in the summary that partnership status could not be confirmed.

**Scoring configuration note**
The scoring below reflects a default prioritization strategy. Before running at scale, the user should confirm this matches how they actually think about partner prioritization. Common adjustments:
- If co-sell pipeline is the primary goal, the +3 for open opportunities is the right anchor. No change needed.
- If the goal is mutual customer alignment (e.g. expansion, onboarding), consider making customer population the top signal.
- If strategic partner tags are the primary filter, the +2 tag bonus may be enough to surface the right partners without relying on population scoring at all.

If the user wants to adjust the weighting, ask them once before running and apply their preference for the session.

**Score** the survivors:
- +3 if the account is in a partner population/segment indicating an open or joint opportunity. Population names vary by partner. Interpret intent rather than matching exact strings. Names like "Pipeline", "Active Opportunities", "Open Deals" should score the same as "Open Opportunities".
- +2 if it's in a customer population. Names like "Active Customers", "Clients", "Accounts" should score the same as "Customers".
- +1 otherwise (prospect, unknown, or any population that doesn't clearly indicate opportunity or customer status)
- +1 bonus if the owner's title reads rep-level: Account Executive, AE, Account Manager, CSM, Partner Manager, or similar
- +2 bonus if the partner has a tag matching any of the user's configured strategic_partner_tags (e.g., "Tier 1", "Co-Sell"). This ensures strategic partners surface to the top even when overlap data is thinner.

Pick the highest score. Tie-break: open opportunity > customer > prospect; then strategic tag > no tag; then prefer the partner with the higher partner score or engagement metrics if the response exposes them.

If no partner survives the filter, skip the deal and count it in the summary. That's a normal outcome, not an error. Many won accounts simply have no actionable partner overlap.

## Step 3 — Draft the alignment email

The recipient is the **partner rep**, never the customer. Read the draft back with that in mind. A single sentence that addresses them as the account ("congrats on choosing us") is a hard failure.

Voice: rep-to-rep, director-level, concise (90-130 words), no emojis, no em dashes, no partnership jargon ("synergy", "flagship", "premier", "unlock", "leverage" are banned). Plain verbs.

Vary the opening across the run. Rotate between patterns like:
1. "As a partner of [Partner Co], I'm writing to align on our recent win at [Account]..."
2. "We just closed [new business / an expansion] at [Account], and given our partnership, I wanted to share what we learned..."
3. "To move our work with [Partner Co] forward, I'm sharing a few specifics from our recent win at [Account]..."

Body. Match the scenario from Step 2:
- **Partner has an open opp on the account:** "I see you have an active opportunity with [Account]. We just closed there. Happy to make an intro or share what moved the deal to help your cycle."
- **Account is the partner's customer:** "[Account] is now a mutual customer. Worth comparing notes on what resonated and where our teams can support each other there."
- **Otherwise:** "I see you also work with [Account]. Now that they're our customer, we can trade notes on the buying committee and timing."

**Do not claim mutual visibility you don't have.** These templates, and phrasings like "Crossbeam shows you have an open opportunity," presume the partner can see this account from their side. Use the `sharing_status` from Step 2 to check that assumption:

- **`shared`:** the templates above work as written. Referencing what Crossbeam shows is fair.
- **`not_shared` or `not_present`:** you do not share this account with that partner, so they may not see the win at all. Drop any "Crossbeam shows" framing and state the context plainly instead: "We recently closed [Account]. You may not see it on your side, so flagging it directly." Do not tell the user their sharing rules are wrong. Just write the email so it reads correctly either way.
- **Unconfirmed** (tool absent, `ClarificationRequired`, or no `sharing_status` returned): write it the same way as `not_shared`. Plain framing reads correctly whether or not the partner can see the account, so it is the safe default when you do not know. "Crossbeam shows" is the only phrasing that needs `shared` to be true.

Only reference facts the overlap data actually shows. Don't invent details about the partner's deal stage, their champion, or their history with the account.

Close with "Best," and nothing after it. The sender's email signature completes it.

Subject line. Vary across the run, pick the most specific fit:
- `[Account] + [Partner Co]: aligning on a mutual win`
- `[Account]: quick note from [Your Company]`
- `[Account] is now a mutual customer`
- `We just closed [Account], worth a quick sync?`

If the draft will become an HTML email, use `<br>` for line breaks. No `<html>` or `<body>` wrapper.

## Step 4 — De-AI pass (hard gate before delivery)

Re-read every draft before it goes anywhere. This is a hard stop: no draft is delivered until it passes both checks.

1. **Zero em dashes.** Remove every em dash (—) and every en/hyphen dash used like one. Restructure the sentence with a comma, a period, or two sentences. Don't just swap the character.
2. **No AI-sounding filler.** Cut or rewrite phrases that mark an email as machine-written: "I hope this finds you well", "I wanted to reach out", "just reaching out", "touching base", "circle back", "delve", "robust", "seamless", "streamline", "excited to", "thrilled to", "navigate", "landscape", "game-changer", "at the end of the day", "win-win". Also kill the rule-of-three flourish ("faster, smarter, and more aligned") and any sentence that compliments the recipient generically.

The test: would a busy rep read this and assume a human typed it between calls? If not, rewrite until yes.

## Step 5 — Deliver the drafts

**Before creating any drafts, confirm recipients with the user.**
Present a summary list of who will receive drafts:
- [Account] → [Partner name] / [Rep name] / [Rep email]

Ask the user to confirm before proceeding. If anything looks wrong, give them the opportunity to remove it from the run. Only create drafts after explicit confirmation.

**What the gate blocks, precisely.** Sending is never permitted. Copy-ready text in chat is not a send, so you may show drafts in the same turn as confirmation, but the recipient table must appear above the drafts so the user checks each address before copying. The volume guardrail always blocks: if the qualifying count exceeds the threshold, report the count and stop. A blanket "I trust you, don't check with me" does not satisfy either gate. Report the count and the recipients anyway.

**Default delivery:** produce all drafts as copy-ready text in chat, one section per deal with recipient, subject, and body.

## Step 6 — Report

End with a compact summary:
- Deals pulled / drafts created / deals skipped (and why: no overlap, no qualifying rep, ambiguous match)
- One line per draft: `[Account] → [Partner Co] / [Rep name]`
- Any tool errors

Don't repeat full email bodies in the summary. They're in the drafts.

## Recurring runs

This works well as a weekly cadence (e.g., Monday mornings, catching the prior week's wins). If the user runs it manually and seems happy, offer once to set it up as a scheduled task. If any connector returns auth errors during a scheduled run, stop and report clearly so the user can reconnect. Don't partially process.

## Guardrails

- Drafts only. Never send email.
- Only surface partner data that Crossbeam's sharing rules already expose to this user. Never speculate about partner data you can't see.
- Deal amounts and account lists are sensitive. Keep them out of any output that isn't going to the user themselves.

</instructions>
