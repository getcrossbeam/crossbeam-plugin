# ELG context — Co-Sell Copilot (compiled subset)

A self-contained subset of Crossbeam's ELG plays and foundations, covering only what this skill
runs: the plays it executes and the foundations it needs. It is not the full catalog.

---

## Primitives this skill uses

**Ecosystem-Led Growth (ELG):** a go-to-market motion that uses the partner ecosystem to attract, convert, and grow customers.

**Account Mapping Matrix (AMM):** a 3×3 of your segments (rows) × the partner's segments (columns). Co-Sell Copilot lives in the **My Opportunities** row, plus one reciprocal cell:

|  | Partner Opportunities | Partner Customers |
|---|---|---|
| **My Opportunities** | Joint Opportunity → co-sell (coordinate) | Your opp is the partner's customer → pull intel / reference / voucher |
| **My Customers** | Your customer is the partner's opp → reciprocal give (cross-sell for them) | — |

**Segments:** Prospect (fits ICP, not engaged), Opportunity (in your pipeline), Customer (paying).

**Attribution:** sourced (partner generated it) vs influenced (partner touched it). Co-selling converts influence into attributable outcomes.

**Partner types:** tech partners (integrations; joint value = combined functionality) and channel/service partners (can sell on partner paper, cutting procurement friction).

---

## Tools this skill calls (Crossbeam MCP)

> Verify exact tool names and params against the connected MCP server on the first call; versions differ. Match on the suffixes below.

| Tool | Returns | Use here |
|---|---|---|
| `find_partner_recommendations` | The matching deal/account and recommended partners, each with `ei_signals` (`recent_wins`, `long_term_relationship`, `new_contacts`, `missing_contacts`) | The spine: who can help, ranked, with the position signals already computed. Resolve the account here too (fuzzy match returns `ClarificationRequired` candidates). |
| `find_partner_contacts` | Partner-shared contacts at an account: priority-role `insights`, an `in_own_crm` flag, `partner_populations`, `last_activity_at` | Altitude, the coverage gap (`in_own_crm: false`), intel/reference targets. |
| `find_overlap_partners` | Partners with partner-level metrics | Partner prioritization across a book. |
| `find_overlaps` | Accounts overlapping with partners, by population intersection | The reciprocal give (your customer = partner's open opp); marketplace overlap. |
| Deal Navigator link tool (if exposed) | A link to the co-sell workspace for an opp | Optional hand-off for a coordinated joint opportunity. Not present on every install. |

This skill is **read-only**: every tool is a `find_*` / `get_*`. It produces intelligence and drafts; the send/log/track happens in the company's own CRM, chat, or the linked co-sell workspace.

**Position signals come straight from the tools** — do not infer them. `ei_signals` give win-direction and recency (`recent_wins`, `long_term_relationship`, `new_contacts`) and the multi-thread flag (`missing_contacts`); `find_partner_contacts` gives altitude (priority-role `insights`) and the coverage gap (`in_own_crm: false`).

---

## Play records this skill runs

**P19. No-partner-attached alert** · Trigger: an opp reaches mid/late stage with no partner attached. · Cell: my opp × partner customers/opps. · Tools: `find_partner_recommendations`. · Action: surface candidate partners; prompt the rep — "what do you need from partner A/B/C to progress this?"

**P20. Partner intro request** · Trigger: rep needs partner help on a specific account (intel, intro, procurement) — whether breaking in or progressing. · Cell: my opp × partner customers. · Tools: `find_partner_contacts`, `find_partner_recommendations`. · Action: identify the right partner and contact; package the request (account, partner type, the ask); route it.

**P21. Co-sell joint opportunity** · Trigger: both you and a partner are selling the same account. · Cell: my opp × partner opp. · Tools: `find_partner_recommendations`, `find_partner_contacts`, and a Deal Navigator link tool if the install exposes one. · Action: align deal teams, agree a next-best-action on the better-together story, track in the co-sell workspace. · Proof: co-sold deals close faster, win more, larger ACV, retain better.

**P22. Partner intel gathering** · Trigger: a deal in flight where a partner already has the account. · Cell: my opp × partner customer. · Tools: `find_partner_contacts`. · Action: ask the partner rep how the prospect buys — procurement, pricing dynamics, decision-makers, gotchas; feed the rep, adjust forecast. · Proof: partner-influenced deals carry higher ACV (general ELG finding).

**P23. Backchannel reference** · Trigger: a late-stage deal needs a credibility push. · Cell: my opp × partner customer. · Tools: `find_partner_contacts`. · Action: ask a partner with a strong relationship at the account to endorse you. · Proof: a credible partner endorsement shortens late-stage deals (general ELG finding).

**P24. Partner vouchers** · Trigger: an open opp that is an existing customer of a partner; need urgency without discounting yourself. · Cell: my opp × partner customer. · Tools: `find_partner_recommendations`. · Action: let the partner extend an initial discount/voucher so they look like the hero and create urgency. · Proof: a partner-extended voucher creates urgency without discounting your own price (general ELG finding).

**P26. Hyperscaler marketplace co-sell** · Trigger: the deal can transact via a hyperscaler cloud marketplace. · Cell: my opp × hyperscaler customers. · Tools: `find_overlaps` (account resolution via the `find_*` fuzzy match). · Action: identify marketplace overlap, run rep-to-rep alignment and co-sell; leverage simplified procurement, committed-spend drawdown, seller assist. · Proof: cloud marketplaces ~$50B throughput by end-2025; 5–10% take rate.

**P25. Cross-sell into your customer (the reciprocal give)** · Trigger: reciprocity — a partner is pursuing one of your customers, or you want something to offer back. · Cell: my customer × partner opp. · Tools: `find_overlaps`. · Action: offer to help the partner sell or expand into one of your customers; deepens the better-together story and fuels the value loop. · Proof: the AMM reciprocal cell.

---

## Proof library (use only to persuade; always attribute; never invent or reattribute)

- Co-sold deals close faster, win more, carry larger ACV, retain better (general ELG finding).
- Cloud marketplaces: ~$50B throughput by end-2025; 5–10% take rate.

Named customer results are deliberately not stored here. To cite a specific company's
numbers, find a current published case study first and attribute it to that source — never
recall one from memory, and never attach a metric to a company you have not just verified.

---

## Guardrails

- Overlap data is visible only if the partner shares it. Absence of data is not absence of overlap — say "not shared by the partner," never "no overlap."
- Respect data-access tiers; don't expose field-level partner data the relationship doesn't warrant.
- Every co-sell ask requires partner consent and a real "better-together" truth. Do not manufacture a joint-value story that doesn't exist.
- Read-only / draft-only. Route any drafted message through the company's approved path; never send directly.
