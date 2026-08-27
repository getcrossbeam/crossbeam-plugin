# ELG context — Ecosystem Prospecting (compiled subset)

A self-contained subset of Crossbeam's ELG plays and foundations, covering only what this skill
runs, with the tool surface and mechanics verified against the live Crossbeam MCP. It is not the
full catalog.

---

## Primitives this skill uses

**Ecosystem-Led Growth (ELG):** a GTM motion that uses the partner ecosystem to attract, convert, and grow customers.

**Account Mapping Matrix (AMM) — the bottom row is where EQLs come from:**

|  | Partner Opportunities | Partner Customers |
|---|---|---|
| **My Prospects** | EQL — a partner is actively selling them | EQL — they are a partner's customer (top marketing source) |

**Segments:** Prospect (fits ICP, not engaged, not a customer), Opportunity (in your pipeline), Customer (paying). This skill excludes Customers and treats Prospects as the prime list, Open Opportunities as already-in-motion.

**EQL (Ecosystem Qualified Lead):** a lead more likely to convert because of behaviour in your ecosystem — uses a complementary tool, is a partner's customer, or is being actively pursued by a partner. A first-class qualification tier alongside MQL/PQL. Proprietary second-party data.

**Second-party data:** data shared directly by a partner. Not first-party (yours) or third-party (brokered/buyable). The unique asset behind the play.

---

## Tools this skill calls (verified live surface)

| Tool | Returns / use here | Notes from production |
|---|---|---|
| `find_overlaps` | Accounts by population intersection. `our_segments` filters YOUR side; `partners` takes an array of partner specs OR an EcosystemFilter (`tag_name`/`tag_id`, `segments`, `partner_scores`). The workhorse for surfacing EQLs. | Row carries the PARTNER's segment (`partner_segments`) + `total_count`, **not** your segment — label prospect vs open-opp with a two-pass call (Step 3). `ClarificationRequired` on fuzzy names; `is_no_match: true` when a named partner isn't in the ecosystem; `RetryLater` on big reports. limit max 100, paginate via `has_more`. |
| `find_overlap_partners` | Partners that share a given account; supports `partner_tag_name`. | Use to confirm/enumerate which partners carry an anchor tag, for a transparent report. |
| `find_partner_contacts` | Partner-shared contacts at an account, priority-role `insights`, `in_own_crm` flag. | One call per account → enrich the top N only. `in_own_crm:false` = net-new buyer (best warm path). Validate contact email domain vs account domain (partner-side artifacts occur). |
| `find_partner_recommendations` | EI-recommended partners + `ei_signals` for a named deal/account. | Deal-centric; mainly for the co-sell skill. |
| `search_crossbeam_knowledge` | ELG/product/blog content. | Methodology/how-to fallback. |

No guaranteed ecosystem-activity/signal tool → P12 alerts degrade to an on-demand re-scan.

This skill is **read-only**: produces a ranked list and optional drafts; the push/send happens in the company's own MAP, sequencer, or CRM.

---

## The anchor-pool principle (Step 1)

Sorting all overlaps by raw partner-overlap count surfaces the most-connected megacorps and buries fit — the wrong leads. The skill instead **selects the partner pools whose customers fit what's being sold**, preferring a partner **tag** that groups them (e.g. an integration-surface tag), then named partners, then strategic/high-score partners. The pool choice is the judgment that makes the list good; state which partners were used and why.

## Scoring (Step 4)

Composite EQL score = anchor breadth (how many anchor partners hold the account as customer) + relationship type (customer > open opp) + ICP fit (consult an ICP skill if present; drop gov/edu/PE-VC/GSI for a product-SaaS ICP) + reachability (priority-role contact exists) + recency. Tier A/B/C. Then an exclusion pass removes your own partners and investors, and de-dupes segment artifacts.

---

## Play records this skill runs

**P7. EQL generation** · Trigger: "find me ecosystem-qualified leads / better leads"; top-of-funnel fill. · Cell: prospects × partner opps/customers (bottom row). · Tools: `find_overlaps`. · Action: surface prospects exhibiting ecosystem behaviour; tag as EQL; route to MAP/sales. Optionally run retroactively to prove conversion lift. · Proof: EQLs convert higher; back-testable in a spreadsheet.

**P8. Second-party enrichment** · Trigger: "enrich this prospect list" / generic outreach underperforming. · Cell: prospects × all partner segments. · Tools: `find_overlaps`, `find_partner_contacts`. · Action: replace third-party firmographics with partner context — which partner, customer vs opp, partner AE, segment; flag contact availability and net-new (`in_own_crm:false`) buyers. · Proof: proprietary, un-buyable second-party data.

**P11. PDR/SDR partner sequences** · Trigger: SDR/BDR outbound prep; "who should I prospect and how?" · Cell: prospects × partner customers. · Tools: `find_overlaps`, `find_partner_contacts`. · Action: enroll overlapping contacts in a partner-specific sequence leading with joint value; surface the partner AE; check for sequence conflicts first. · Proof: partner-specific sequences outperform generic outbound on overlapping accounts (general ELG finding).

**P12. Signal-triggered outbound** · Trigger: "alert me when a prospect heats up"; automated workflow. · Cell: prospects × partner opps/customers. · Tools: a signal/ecosystem-activity tool if exposed. · Action: on a new ecosystem signal (partner closed the account, tech-stack change, new overlap), fire personalized outreach or notify the rep/partner; time the touch to the signal. If no signal tool, degrade to on-demand re-scan. · Proof: second-party partner data is a lens you cannot scrape or buy.

---

## Proof library (persuade only; always attribute; never invent or reattribute)

- EQLs convert higher than cold leads; lift is back-testable on your own closed data.
Named customer results are deliberately not stored here. To cite a specific company's
numbers, find a current published case study first and attribute it to that source — never
recall one from memory, and never attach a metric to a company you have not just verified.

---

## Guardrails

- Overlap data is visible only if the partner shares it. Absence of data is not absence of overlap — say "not shared by the partner," never "no overlap."
- Exclude customers (already sold), partners, and investors. Never cold-prospect an account a partner is actively working — flag for co-sell.
- Always select the pool deliberately, label the segment, score, rank, and explain — never an unranked dump.
- True net-new-to-CRM whitespace is out of `find_overlaps` reach (intersection only) — never imply otherwise.
- Read-only / draft-only. Route any drafted touch through the company's approved path; never send directly.
