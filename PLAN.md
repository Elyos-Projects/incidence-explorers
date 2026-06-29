# Incidence-Explorers — PLAN.md

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) ·
> Lane: donated · Risk tier: **medium overall; HIGH for any patient-facing educational content**

Plain-language, accessible dashboards and explainers built **only** from **aggregate, open-access,
de-identified** cancer-incidence statistics (primarily **SEER\*Explorer** and **GLOBOCAN / Global
Cancer Observatory**), so that patients, families, advocates, journalists, students, and small public-
health teams can read cancer statistics correctly. The output is education, never medical advice; it
carries provenance on every assertion and is reviewed by a credentialed oncologist **and** a patient
advocate before any patient-facing content ships.

---

## Executive summary

Cancer statistics are everywhere and almost universally misread. A headline that "cancer X is rising"
usually conflates more screening with more disease; "rates" get confused with "counts"; modeled
GLOBOCAN *estimates* get treated as exact case tallies; age-standardised rates computed against
different standard populations get compared head-to-head; and a 5-year *relative survival* figure gets
read by a frightened family as a personal prognosis. The numbers are public and authoritative, but the
**interpretation layer** that makes them safe and useful for a non-specialist barely exists. This
project builds that layer: trustworthy, plain-language explainers and accessible dashboards over
aggregate cancer-incidence data, with the statistical caveats built into the product rather than buried
in a methods PDF.

The single most important design fact is the **data boundary**. The project uses **only** open-access,
aggregate, de-identified statistics — **SEER\*Explorer** published aggregate tables, **GLOBOCAN /
GCO** country-level estimates, and **US Cancer Statistics (USCS)** public aggregates. **Individual-
level / case-level / controlled-access data is OUT OF SCOPE**: the SEER *Research Data* case-level
database (which requires a signed SEER Research Data Agreement), CDC restricted-use files, any
record-level patient data, and any source needing a DUA/IRB are explicitly excluded. We do not
attempt to re-identify, un-suppress, or reconstruct small-cell values; we honour every upstream
suppression rule (e.g. counts based on fewer than 16 cases) and add our own where aggregation could
re-create a small cell. This is the **headline license/privacy gate** and the first build item is the
data-governance layer that enforces it, not a feature.

The second design fact is the **medical-safety boundary**. Anything a patient or family member might
read about a cancer is **HIGH risk-tier** and **does not ship without sign-off by a credentialed
oncologist AND a patient advocate** (blocking expert review — no sign-off, no ship). Every such surface
carries persistent **"This is general education, not medical advice — talk to your care team"**
framing, routes nothing toward diagnosis or treatment decisions, and any crisis/helpline information
is independently verified and dated before publication. Every quantitative assertion is **provenance-
backed**: it traces to a specific source, data release/version, the exact query parameters, an
extraction date, and a URL — a statistic that cannot cite its source does not render.

Honesty note: **no partner organisation, beneficiary, or expert reviewer is yet secured.** Every
delivery-dependent task is marked `TO BE SECURED` with `verifiedNeed: false`. The project is *not*
"shipped" on merge; it ships only when real beneficiaries (a patient-advocacy org, a public-library /
health-literacy program, a small public-health team, or a newsroom) actually use the explainers, with
the data, license, and medical-safety gates independently verified.

**Partner-acquisition plan (dated) and build-vs-mothball decision rule.** Outreach is time-boxed
against the build rather than left open-ended: by **2026-08-31**, a pilot cancer site/topic is selected
(criteria in *Open questions*) and ≥ 3 candidate partner/beneficiary organisations are in active
conversation; by **2026-10-31**, a credentialed oncologist **and** a patient-advocate reviewer are
secured (M3 gate); by **2026-12-31**, a steward/beneficiary for last-mile adoption is secured (M5
gate). **Decision rule:** if no oncologist + advocate reviewer pair is secured by the M3 entry date,
patient-facing content work does not start and the project holds at M2 (non-patient-facing data
literacy + dashboards, which a general reviewer can clear). If no beneficiary is secured by
**2027-03-31** (~M4 completion), the project does **not** publish into a vacuum: it either (a)
**pivots** the last-mile beneficiary (e.g., contribute the "how to read cancer statistics" literacy
module + dashboards to an existing OER/health-literacy commons) or (b) **mothballs** to maintenance-
only, recorded in governance. The data-literacy explainer and dashboards remain valuable reference
artifacts in either case.

---

## Problem & beneficiaries

**Who is helped (directly):**
- **Patients and families** trying to make sense of a new diagnosis's statistics without being misled
  or frightened by numbers they can't contextualise.
- **Patient advocates and small advocacy orgs** (especially rare-cancer groups) who need accurate,
  plain, citable statistics for their communities and materials.
- **Journalists and students** who report on or learn from cancer data and routinely misinterpret it.
- **Small/under-resourced public-health teams, librarians, and health-literacy educators** who need
  trustworthy, accessible explainers they can hand to the public.

**Who is helped (ultimately):** the **general public's cancer-statistics literacy** — fewer scares
from screening artifacts, less fatalism from misread survival figures, better-informed questions for
clinicians.

**The need.** Authoritative aggregate data exists (NCI/SEER, IARC/GLOBOCAN, CDC/USCS) and is free, but
it is presented for analysts: dense tables, statistical jargon (age-standardised rate, APC/AAPC,
relative survival, joinpoint), and methods caveats that a non-specialist cannot apply. The result is
predictable, repeated misreading in public discourse. A plain-language, provenance-cited,
accessibility-first interpretation layer — that *teaches the caveat as it shows the number* — is
missing.

**Verified need / partner:** **TO BE SECURED.** No specific advocacy org, library/health-literacy
program, public-health team, or newsroom has yet agreed to adopt or co-develop the explainers, and no
oncologist/advocate reviewer is yet engaged. Target partner profiles: a **rare-cancer or general
cancer advocacy organisation**; a **public-library or health-literacy program**; a **small local/
regional public-health unit**; a **health/science newsroom or data-journalism desk**; and an academic
**oncology or cancer-epidemiology group** willing to provide credentialed review. Until one is secured,
the project builds the agent-neutral data-governance + statistics-correctness + dashboard platform and
one *illustrative* cancer-site explainer **for review only**, and marks all delivery/adoption work `TO
BE SECURED`. Outreach is **dated, not open-ended** (see the partner-acquisition plan above), governed
by a **build-vs-mothball/pivot decision rule** if dates slip.

---

## Goals and non-goals

**Goals**

- Build an open-source pipeline + dashboards + explainers that present **aggregate** cancer-incidence
  statistics accurately and in plain language, with every assertion provenance-cited.
- Make statistical correctness a *product feature*: distinguish incidence / mortality / prevalence /
  survival; counts vs rates; age-standardisation and its standard-population trap; confidence intervals
  and estimate uncertainty (GLOBOCAN modelled estimates ≠ exact counts); no causal claims.
- Ship a **data-governance layer** that provably uses only aggregate/open-access/de-identified sources,
  verifies each source's license, and honours/extends small-cell suppression to prevent re-
  identification.
- Produce a flagship **"How to read cancer statistics"** literacy explainer (medium risk; no per-
  cancer medical claims) plus per-cancer-site explainers (HIGH risk; oncologist + advocate reviewed).
- Meet **WCAG 2.2 AA** and a plain-language readability target so the output is genuinely usable by a
  non-specialist, including assistive-tech users.
- Reach real beneficiaries and record genuine outcomes (people read statistics more correctly; a
  partner adopts the explainers), with the medical-safety and data gates independently verified.

**Non-goals**

- **Not medical advice**, diagnosis, prognosis, screening recommendations, or treatment guidance for
  any individual — ever.
- **Not** an individual-risk calculator, "your odds" estimator, or anything that maps a population
  statistic onto a specific person.
- **Not** a user of **individual-level / case-level / controlled-access** data (no SEER Research Data
  case files, no CDC restricted-use files, no record-level patient data, no DUA/IRB-gated source).
- **Not** a research-grade statistical-analysis platform or a replacement for SEER\*Stat / GCO — it
  *presents and explains* published aggregate statistics; it does not invent new estimates or run novel
  epidemiological modelling.
- **Not** a tool that makes **causal/etiological** claims ("X causes cancer Y") or attributes
  disparities to inherent group traits.
- **Not** a real-time surveillance or outbreak-tracking system; cancer registry data lags by years and
  the product states its data vintage explicitly.

---

## Success metrics (outcomes)

Outcome-centric and beneficiary-first. Vanity metrics (pageviews, sessions, "users") are explicitly
**not** success.

| Metric | Baseline | Target (pilot) | How measured |
|---|---|---|---|
| Provenance coverage of quantitative assertions | none | **100%** of rendered statistics carry a resolvable source + data-release + query-params + extraction-date citation | Automated provenance-coverage test in CI (a claim without a `Source` does not render) |
| Source license verified before any data ships | none | **100%** of enabled sources have a recorded, verified reuse term + attribution string | License register + CI check (no unverified source enabled) |
| Controlled-access / individual-level data used | none | **0** (hard gate) — only allow-listed aggregate sources can be ingested | Source allow-list enforcement test; PR review |
| Small-cell / re-identification protection | none | **0** rendered values derived from suppressed/`<16` cells; **0** un-suppression or small-cell reconstruction | Suppression test suite; aggregation-safety check |
| Expert sign-off before patient-facing content ships | none | **100%** of HIGH-tier content has recorded **oncologist + patient-advocate** sign-off | Governance/reviewers ledger |
| "Not medical advice" + care-team framing present | none | **100%** of patient-facing surfaces; **0** advice/diagnosis/prognosis phrasings (lint + reviewer) | Content-safety lint + expert review checklist |
| Crisis/helpline info verified & dated | none | **100%** of helpline entries verified against the official source and carry a `lastVerified` date | Helpline-verification check + reviewer audit |
| Statistical-caveat coverage | none | **100%** of charts show the right caveat (counts-vs-rates, age-standardisation + standard pop, CI/estimate uncertainty, screening artifact where relevant) | Caveat-coverage test against a per-chart caveat matrix |
| Plain-language readability | n/a | core explainers meet the target reading level (proposal: ~grade 8) and a plain-language reviewer pass | Readability metric + human plain-language review |
| Accessibility | n/a | **WCAG 2.2 AA** on all dashboards/explainers; charts have text-table + ARIA equivalents | Automated a11y scan + manual AT testing |
| Comprehension improvement (the real outcome) | unmeasured | beneficiaries read a sample statistic **more correctly** after using an explainer (e.g., ≥ X-point lift on a short before/after comprehension check) | Pilot comprehension check with the partner |
| Partner adoption (Definition of Shipped) | 0 | ≥ 1 partner org embeds/uses ≥ 1 explainer with its community, outcome recorded | Steward-attested adoption log |

The **defining success outcome** (Definition of Shipped): a real beneficiary organisation uses the
explainers with its community and people demonstrably read the statistics more correctly, with the
data/license/medical-safety guardrails independently verified — not "the dashboard merged."

---

## Scope

**In scope**

- A **data-governance layer**: a source allow-list (aggregate/open-access only), a per-source license
  register, provenance capture, and small-cell/suppression enforcement — built first.
- A **data-ingestion pipeline** for **aggregate** tables from **SEER\*Explorer**, **GLOBOCAN / GCO**,
  and **USCS** public aggregates, with full provenance (source, dataset version/release, query
  parameters, extraction date, URL) and validation against the upstream published figures.
- A **statistics-correctness layer**: typed measures (incidence/mortality/prevalence/survival),
  counts-vs-rates handling, age-standardisation metadata (which standard population), confidence-
  interval / estimate-uncertainty handling, and a per-figure **caveat matrix**.
- **Accessible dashboards** (WCAG 2.2 AA) with chart + text-table equivalents, and inline caveats.
- A flagship **"How to read cancer statistics"** literacy explainer (medium risk) and **per-cancer-
  site explainers** (HIGH risk, oncologist + advocate reviewed) with persistent "not medical advice"
  framing and verified crisis/helpline references.
- An **eval/quality harness** (provenance coverage, caveat coverage, readability, a11y, suppression).
- Published open datasets/derived tables (CC-BY-4.0) and code (MIT), with attribution preserved.

**Out of scope (explicitly will NOT do)**

- **Individual-level / case-level / controlled-access data** of any kind: SEER Research Data case files
  (DUA-gated), CDC restricted-use files, record-level patient data, biobank/clinical-record data, or
  any source requiring a DUA/IRB. (Aggregate published statistics only.)
- **Re-identification, un-suppression, or small-cell reconstruction**, and any aggregation/cross-tab
  that could re-create a suppressed small cell.
- **Medical advice, diagnosis, prognosis, screening/treatment recommendations**, or individual-risk
  estimation.
- **Causal/etiological claims** and disparity narratives that attribute outcomes to inherent group
  traits (disparities are presented descriptively with structural-context framing only).
- **Novel epidemiological modelling** or new estimates — the project presents and explains *published*
  aggregate statistics; it does not produce its own incidence estimates.
- **Real-time/surveillance** framing — data vintage and registry lag are stated explicitly.
- Any source whose license/reuse terms have not been verified and recorded.

When a request or feature idea falls in the out-of-scope set (e.g., "show me my personal risk," "break
this down to a tiny sub-county cell," "tell me if I should get screened"), the product declines that
framing, explains why in plain terms, and redirects to the appropriate resource (a clinician, the
official screening guidance body, or the upstream data tool).

---

## Solution approach & architecture

**Stack.** TypeScript, ESM, pnpm workspaces (per Elyos conventions). Static-first web delivery
(Astro or Next.js static export) so dashboards are cacheable, auditable, and need no server holding
data. Data pipeline in TypeScript (typed measures, provenance records). Accessible visualisation via a
charting approach that emits **both** an SVG chart **and** an equivalent data table with ARIA
(charts are never the only representation). Anthropic Claude (model selection/pricing per the Claude
API skill) may assist drafting explainer prose and extraction **behind a thin provider-neutral LLM
client** so the core stays vendor-neutral — but **no statistic is generated by the model**; numbers
come only from the ingested aggregate sources, and the model never has the final word on a medical
claim (expert review does). Code license **MIT**; content/datasets **CC-BY-4.0**.

**Components**

1. **Data-governance layer (`lib/governance`) — built first.** Not a doc; an enforced subsystem.
   - *Source allow-list:* only explicitly allow-listed **aggregate** sources can be ingested; anything
     case-level/controlled-access is rejected at ingestion. The allow-list is the hard data boundary.
   - *License register:* each source has a recorded, verified reuse term, required attribution string,
     and any non-commercial/derivative constraints; an unverified source cannot be enabled (CI check).
   - *Suppression & re-identification guard:* honours upstream suppression (e.g. SEER/USCS counts based
     on `<16` cases), refuses to display suppressed values, and **blocks aggregations/cross-tabs that
     would reconstruct a small cell**. Fail-closed: ambiguous cell size → suppress.

2. **Provenance store (`lib/provenance`).** Every ingested figure is a `DataPoint` bound to a `Source`
   record: source name, dataset/release version (e.g. *SEER\*Explorer* release date, *GLOBOCAN 2022*),
   exact **query parameters** (cancer site, sex, race/ethnicity grouping, year/period, age-standard,
   measure), extraction date, URL/DOI, and license. A `DataPoint` with no resolvable `Source` cannot be
   rendered (citation-coverage test enforces this).

3. **Statistics-correctness layer (`lib/stats`).** Typed `Measure` (`incidence | mortality |
   prevalence | survival`), `CountOrRate`, `AgeStandard` (e.g. *2000 US Std* for SEER vs *World (Segi/
   Doll)* for GLOBOCAN), and uncertainty (CI bounds, or "modelled estimate — uncertainty" for
   GLOBOCAN). A **comparability guard** refuses to plot two series with incompatible standards/measures
   on the same axis without an explicit, labelled caveat. A per-figure **caveat matrix** maps each
   figure type to its mandatory caveats (counts≠rates, age-standardisation + standard pop, CI/estimate
   uncertainty, **screening/diagnostic artifact** where incidence trends are screening-sensitive,
   registry lag/data vintage).

4. **Dashboard/visualisation layer (`app/`).** Static, accessible dashboards: each chart ships with an
   equivalent **data table**, ARIA labelling, keyboard navigation, sufficient contrast, and inline
   caveats from the matrix. No chart renders a suppressed or unprovenanced value.

5. **Explainer content layer (`content/`).**
   - *Data-literacy explainer* ("How to read cancer statistics") — **medium** risk, general reviewer;
     no per-cancer medical claims.
   - *Per-cancer-site explainers* — **HIGH** risk; persistent "general education, not medical advice —
     talk to your care team" framing; verified, dated crisis/helpline references; **oncologist +
     patient-advocate sign-off required before ship**. Every quantitative claim cites its `DataPoint`.

6. **Eval/quality harness (`scripts/eval.ts`).** Automated gates: provenance coverage, caveat
   coverage, suppression safety, content-safety lint (no advice/diagnosis/prognosis phrasings),
   helpline verification, readability, and a11y — plus a human plain-language + expert review loop.

**Key decisions**

- The data boundary (aggregate-only, allow-listed) and suppression guard are a first-class, tested
  subsystem and a release gate — not a policy doc.
- **No statistic is model-generated**; numbers come only from ingested aggregate sources, with
  provenance; the LLM only assists prose/extraction behind a neutral client and never overrides expert
  review.
- Static-first delivery: no server holds patient or restricted data because there is none — the threat
  surface is deliberately minimised.
- Depth-first on one cancer site + the literacy explainer, expert-reviewed, before breadth.
- Accessibility and plain language are acceptance criteria, not polish.

---

## Data, licensing & compliance

**This section leads with the binding cancer guardrails.** They gate everything downstream.

**1. Aggregate / open-access / de-identified ONLY (hard boundary).** The project ingests *only*
allow-listed **aggregate** statistics:
- **SEER\*Explorer** — NCI's public interactive aggregate cancer-statistics tool (incidence,
  mortality, survival, prevalence). **Public, no DUA.** *In scope.*
- **GLOBOCAN / Global Cancer Observatory (GCO)** — IARC/WHO country-level **modelled estimates** of
  incidence/mortality/prevalence (e.g. GLOBOCAN 2022). **Aggregate, public.** *In scope* — flagged as
  *estimates with modelling uncertainty*, not exact counts, and **not** suitable for short-term trend
  claims.
- **US Cancer Statistics (USCS)** — CDC/NCI combined public aggregates (incl. via CDC WONDER public-
  use). *In scope*, honouring USCS suppression rules.

**OUT OF SCOPE (hard exclusion):** the **SEER Research Data** case-level database (requires a signed
**SEER Research Data Agreement / DUA**), CDC **restricted-use** files, any **record-level / individual-
level** patient data, biobank or clinical-record data, and **any source requiring a DUA or IRB
approval**. These are rejected at the ingestion allow-list, not merely discouraged.

**2. Per-source license verification (recorded before ship).** Each source's reuse terms are verified
and recorded in a **license register** before any of its data is enabled: the reuse permission,
required **attribution string**, and any **non-commercial or no-derivatives** constraint. Notes by
source (to be verified, not assumed):
- *SEER\*Explorer / NCI / USCS:* U.S. federal government works are generally not copyright-protected in
  the U.S.; attribution to NCI/CDC and the data release is still required, and **redistribution terms
  and any citation requirements are verified and recorded** per the tool's stated terms.
- *GLOBOCAN / GCO (IARC/WHO):* IARC/WHO publish specific **terms of use and a required citation**;
  reuse (including any **non-commercial** condition and the mandated attribution) is **verified and
  recorded before GCO data is enabled** — not assumed open. If terms forbid a given reuse, that data is
  not shipped.
An unverified source cannot be enabled (CI gate). The project is non-commercial and open (CC-BY-4.0
content), which keeps it inside typical non-commercial-research terms, but each source is still checked.

**3. Provenance on every assertion.** Every rendered statistic resolves to a `Source` record (source,
dataset release/version, exact query parameters, extraction date, URL/DOI, license, age-standard,
measure). **No source, no render** — enforced by a citation-coverage test. Provenance also makes the
data **reproducible**: a reader can re-run the same query against the upstream tool.

**4. Re-identification / small-cell protection.** Cancer aggregates suppress small cells precisely to
protect patient privacy. We **honour all upstream suppression** (e.g. SEER/USCS counts based on `<16`
cases), **never display** suppressed values, **never attempt to un-suppress or infer** a small cell,
and **block any aggregation/cross-tabulation that could reconstruct one** (the complementary-cell and
differencing attacks). The suppression guard is **fail-closed**: if a cell's underlying count is
unknown or near threshold, it is suppressed. We do not produce sub-national breakdowns finer than the
source publishes.

**5. No medical advice — patient-facing content is education only (HIGH, expert-gated).** Any patient-
facing surface carries persistent **"This is general education, not medical advice — talk to your care
team"** framing; contains **no** diagnosis, prognosis, individual-risk, screening, or treatment
guidance; and **ships only after sign-off by a credentialed oncologist AND a patient advocate**. A
content-safety lint blocks advice/diagnosis/prognosis phrasings; the expert review is a hard,
**blocking** gate (no sign-off, no ship).

**6. Verified crisis/helpline information.** Any crisis, distress, or helpline reference (e.g. a
cancer-support or mental-health line) is **verified against the official source, region-appropriate,
and dated** (`lastVerified`), and re-checked on the maintenance cadence. An unverified or stale
helpline entry is withheld.

**7. Statistical-honesty rules (no misleading numbers).** Distinguish incidence vs mortality vs
prevalence vs survival; never present counts as rates or vice-versa; always label the **age-
standardisation standard population** and **refuse to compare across incompatible standards** without a
caveat; show **confidence intervals / uncertainty** and mark **GLOBOCAN as modelled estimates**, not
counts; flag **screening/diagnostic artifacts** when an incidence trend is screening-sensitive; state
**registry lag / data vintage**; make **no causal claims**; present **disparities descriptively** with
structural context, never as inherent group traits.

**Output licensing.** Code: **MIT**. Content, explainers, and derived datasets: **CC-BY-4.0**, with
upstream attribution strings preserved per the license register. Expert reviewers credited (with
consent, version-scoped) in a reviewers ledger.

**PII stance.** The product **stores no patient data** (there is none — only aggregate statistics) and
collects **no end-user personal data** beyond minimal, privacy-respecting analytics if any (proposal:
none / privacy-preserving only). No secrets, tokens, or PII in logs, receipts, or committed files
(Elyos rule).

---

## Quality, review & risk gates

**This section leads with the cancer guardrails as gates.**

**Risk tier.** **Medium overall** (aggregate-data dashboards/explainers requiring statistical
accuracy), with a **HIGH** sub-surface: **any patient-facing cancer content**. Pure pipeline/infra/a11y
tasks are low–medium; the data-literacy explainer (no per-cancer medical claims) is medium; **anything
a patient/family reads about a specific cancer is HIGH and requires credentialed expert sign-off before
merge** (`docs/good-deed-definition.md`).

**Required reviews before a deed is "done":**

- **Maintainer** review on all PRs (engineering quality, agent-neutral core, no secrets/PII in logs,
  tests + CI green).
- **Data-governance review** — source is allow-listed and **aggregate-only**; license verified and
  recorded; provenance complete; suppression/re-identification guard satisfied. (Gate for any data
  task.)
- **Statistics review** — measures, age-standardisation, CIs/estimate uncertainty, comparability, and
  the **caveat matrix** are correct for every figure. (Gate for any chart/figure.)
- **Credentialed expert sign-off (HIGH tier) — BLOCKING** — a **credentialed oncologist AND a patient
  advocate** sign off **before** any patient-facing content ships. No sign-off, no ship. Sign-off is
  **version-scoped** (attaches to a specific content version + citation set; edits require re-sign-off).
- **Plain-language + accessibility review** — readability target met and WCAG 2.2 AA verified (charts
  have text-table/ARIA equivalents).

Every patient-facing surface is labelled **"general education, not medical advice — talk to your care
team."**

**Definition of Shipped (project):** a real beneficiary organisation uses the explainers with its
community and people demonstrably read the statistics more correctly, with: only aggregate/open-access
sources used (verified), every assertion provenanced, suppression honoured, all patient-facing content
oncologist- + advocate-signed-off and "not medical advice"-framed, crisis/helpline info verified,
WCAG 2.2 AA met, and the outcome recorded by the steward.

---

## Roadmap & milestones

Phased: data-governance + statistics-correctness first; dashboards next; patient-facing content only
behind expert review; beneficiary adoption last and gated on a secured partner.

- **M0 — Foundation & data-governance (cold-start).**
  *Goal:* the data boundary, license register, provenance model, suppression guard, and statistical-
  honesty + "not medical advice" specs exist before any feature, plus an agent-neutral skeleton.
  *Exit:* repo + pnpm/TS/ESM + CI green; **source allow-list (aggregate-only) enforced**; license
  register schema + first entries; provenance model; suppression/re-identification guard with tests;
  statistical-honesty spec + caveat matrix drafted; "not medical advice" framing wired in; **pilot
  cancer site/topic selected** (criteria in *Open questions*) so M1+ builds against a real corpus and
  reviewer profile.

- **M1 — Aggregate ingestion + provenance.**
  *Goal:* ingest aggregate SEER\*Explorer + GLOBOCAN (+USCS) tables with full provenance, validated
  against upstream published figures.
  *Exit:* ingestion for the pilot site's incidence (and mortality/survival where published) with
  provenance records; **values validated to match the upstream tool** within tolerance; suppression
  honoured end-to-end; **0 controlled-access sources** (allow-list test green); license verified for
  each enabled source.

- **M2 — Statistics-correctness + accessible dashboards.**
  *Goal:* correct, accessible visualisation with caveats — no patient-facing medical claims yet.
  *Exit:* typed measures, age-standardisation metadata, CI/estimate-uncertainty display, comparability
  guard, and per-figure caveat matrix enforced; dashboards meet **WCAG 2.2 AA** with chart+table
  equivalents; provenance + caveat coverage tests green. **Kill-gate:** a small comprehension check
  (does an explained chart read more correctly than a raw table?) trends positive before investing in
  HIGH-tier content.

- **M3 — Patient-facing explainers (HIGH, expert-gated).**
  *Goal:* the data-literacy explainer (medium) + the first per-cancer-site explainer (HIGH), reviewed.
  *Exit:* "How to read cancer statistics" explainer shipped (general review); first cancer-site
  explainer drafted, fully provenanced, "not medical advice"-framed, crisis/helpline references
  verified, and **oncologist + patient-advocate signed off** (blocking). Content-safety lint green.

- **M4 — Eval, hardening & beneficiary readiness.**
  *Goal:* prove quality and ready for a real beneficiary.
  *Exit:* eval harness green (provenance, caveat, suppression, content-safety, helpline, readability,
  a11y); plain-language review passed; comprehension-check instrument ready; adoption/onboarding
  runbook written.

- **M5 — Publication & handoff (the deed).**
  *Goal:* a real beneficiary adopts and benefits.
  *Exit (Definition of Shipped):* a secured partner uses ≥ 1 explainer with its community; comprehension
  improvement recorded; data/license/medical-safety guardrails independently verified. *(Gated on a
  secured partner — TO BE SECURED.)*

- **M6 — Sustain & refresh (post-delivery).**
  *Goal:* durable maintenance and data-release refresh.
  *Exit:* maintenance rotation + ops runbook; **data-refresh cadence** tied to upstream releases (SEER/
  GLOBOCAN update periodically — re-ingest, re-validate, re-date provenance, re-verify helplines, and
  **re-sign-off** changed HIGH-tier content); outcomes dashboard (comprehension/adoption, not
  pageviews); expansion process for additional cancer sites, expert-gated.

Dependencies flow M0 → M1 → M2 → M3 → M4 → M5; the **pilot-site decision is made in M0 and gates M1–
M6** (it fixes the data corpus and the reviewer profile); M3 patient-facing content blocks on M0
governance, M2 stats/a11y, and the **secured oncologist + advocate**; M5 blocks on a secured
beneficiary.

---

## Work breakdown

The itemized, schema-mapped backlog lives in **`TASKS.md`**: ~17 tasks across milestones M0–M6 plus a
future backlog, each mapped to the Elyos Task JSON schema, with per-task acceptance criteria for the
most important items, milestone Definitions of Done, and a complete example Task JSON for the first M0
task (the data-governance / source allow-list + license register). The first build item is the **data-
governance layer** (aggregate-only allow-list, license register, suppression guard), reflecting its
status as the headline gate; the **pilot-site selection** and an early **comprehension kill-gate** at
M2 are sequenced so HIGH-tier patient-facing investment is gated on both a chosen corpus and an early
usefulness check.

---

## Governance, roles & stakeholders

- **Maintainer (Owner): TBD.** Owns architecture, the agent-neutral core, CI, and merge quality.
- **Reviewers / rotation:** at least one engineering reviewer; a **data-governance reviewer** (license/
  provenance/suppression); and a **statistics reviewer** (measures/age-standardisation/CIs/caveats),
  who may be the same epidemiologically-literate person.
- **Credentialed expert reviewers (HIGH tier): TO BE SECURED** — a **credentialed oncologist** AND a
  **patient advocate** (ideally cancer-community-embedded). Both sign off patient-facing content before
  it ships; tracked in a reviewers ledger with credentials + consent. **Independence/COI:** a reviewer
  recuses from any content where they have a material conflict (e.g., a product/therapy they're tied
  to). Sign-off is **version-scoped** and does not carry forward to edits. **Name-use limits:** a
  reviewer's name/credentials may be published only for the specific versions they approved, with
  consent, and may not imply endorsement of unreviewed content. **Disagreement fallback:** the
  oncologist holds a **veto on medical safety** and the advocate a **veto on patient-appropriateness/
  framing** — a maintainer cannot override a "do not ship" on substance; disagreement is logged and
  escalated to Elyos governance / a second reviewer; contested content does not ship.
- **Steward (last-mile owner): TO BE SECURED** — owns the beneficiary relationship and the adoption +
  outcome recording that constitutes shipping.
- **Partner / requestor: TO BE SECURED** — the advocacy org / library-health-literacy program / public-
  health team / newsroom that adopts the explainers.
- **Community / board:** edge-cases (e.g., a borderline disparity framing, a new source's license)
  go through Elyos governance.

---

## Dependencies & integrations

- **Data sources (aggregate, allow-listed):** SEER\*Explorer (NCI), GLOBOCAN / Global Cancer
  Observatory (IARC/WHO), US Cancer Statistics (CDC/NCI, incl. CDC WONDER public-use) — each with
  verified, recorded reuse terms and provenance.
- **External services:** Anthropic Claude API (prose/extraction assist behind the neutral LLM client;
  never generates statistics); static hosting/CDN; CI.
- **Elyos pieces:** `packages/schema` (Task JSON), `CLAUDE.md` work rules + refusal guardrails,
  `docs/good-deed-definition.md` (risk tiers), `planning/ROADMAP.md` (Track 8 cancer guardrails),
  Elyos governance for license/edge-case decisions.
- **Sibling Elyos plans for house style / overlap:** `oncology-data-literacy`, `stats-for-clinicians`,
  `disparities-analysis`, `screening-coverage-trends`, `cancer-dataset-datasheets` — reuse provenance/
  datasheet patterns and avoid duplicating effort.
- **Human/decision dependencies (critical path):** the **pilot-site decision (M0, gates M1–M6)**; a
  secured **oncologist + patient-advocate** reviewer pair (blocks M3 patient-facing content); a secured
  **beneficiary/steward** (blocks M5).

---

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Accidentally ingesting controlled-access / individual-level data (esp. SEER Research Data) | Medium | Critical | **Aggregate-only allow-list** enforced at ingestion (CI gate); explicit exclusion of DUA/IRB sources; PR review; no source enabled without license verification | Data-governance reviewer |
| Re-identification via small cells / un-suppression / differencing | Medium | Critical | Honour upstream suppression (`<16`); never display/infer suppressed cells; **block aggregations that reconstruct small cells**; fail-closed suppression guard; tests | Data-governance reviewer |
| Patient reads content as medical advice / prognosis | High | Critical | Persistent "not medical advice — talk to your care team" framing; **no** individual-risk/diagnosis/prognosis content; content-safety lint; **oncologist + advocate blocking sign-off**; redirect to clinician | Oncologist + Advocate |
| Misleading statistic (counts-vs-rates, age-standardisation mismatch, GLOBOCAN estimate treated as count, screening artifact read as rising risk) | High | High | Typed measures; **comparability guard**; mandatory **caveat matrix** per figure; CI/estimate-uncertainty display; statistics review gate | Statistics reviewer |
| Source license violated (esp. GCO/IARC non-commercial or citation terms) | Medium | High | **Verify + record each source's reuse terms before enabling** (license register, CI gate); preserve required attribution; drop data if terms forbid the reuse | Data-governance reviewer / Maintainer |
| Stale data / unstated registry lag presented as current | Medium | Medium | Provenance carries data release/vintage; product states registry lag; **refresh cadence** re-ingests/re-validates on upstream releases; staleness flag | Maintainer |
| Crisis/helpline info wrong or out of date | Low | High | Verify against official source, region-appropriate, dated (`lastVerified`); re-check on cadence; withhold if unverified/stale | Advocate / Maintainer |
| Disparity data framed as inherent group trait (causes harm/discrimination) | Medium | High | Descriptive-only + structural-context framing; **no causal claims**; advocate + statistics review; refusal guardrail | Advocate + Statistics reviewer |
| No beneficiary secured → cannot reach Definition of Shipped | High | High | Honest `TO BE SECURED`/`verifiedNeed:false`; **dated partner-acquisition plan**; **build-vs-mothball/pivot rule** (~2027-03-31): pivot literacy module into an OER/health-literacy commons or mothball — don't publish into a vacuum | Steward / Maintainer |
| No oncologist + advocate reviewer secured → HIGH content blocked | Medium | High | Recruit via advocacy orgs, oncology depts, cancer-epi groups; **hold at M2** (non-patient-facing) — never ship HIGH content without dual sign-off | Maintainer |
| LLM "helpfully" invents a statistic or a medical claim | Medium | Critical | **No statistic is model-generated** (numbers only from ingested sources with provenance); provenance-coverage test; expert review is final word on medical claims | Maintainer / Oncologist |
| Inaccessible charts exclude the very people being helped | Medium | Medium | WCAG 2.2 AA acceptance criterion; chart + text-table + ARIA equivalents; automated + manual AT testing | Maintainer |

---

## Security & privacy

**Threat surface (deliberately small).** The product holds **no patient data and no controlled data**
— only published aggregate statistics — and is **static-first**, so the classic data-breach surface is
minimised by construction. The real risks are: (a) **data-integrity/misuse** — ingesting a forbidden
source or rendering a misleading/unprovenanced statistic; (b) **re-identification** via small-cell
mishandling; (c) **license breach**; (d) supply-chain/secrets in CI; (e) an end-user treating content
as medical advice.

**Controls.**
- **Source allow-list** (aggregate/open-access only) enforced at ingestion — the primary control;
  controlled-access/individual-level sources cannot enter the pipeline.
- **Suppression/re-identification guard** — honour upstream suppression, block small-cell
  reconstruction, fail-closed.
- **Provenance + license register** — no render without a resolvable source; no source enabled without
  verified reuse terms (CI gates).
- **Content-safety controls** — "not medical advice" framing enforced; advice/diagnosis/prognosis lint;
  **blocking oncologist + advocate sign-off** for patient-facing content; verified/dated helplines.
- **Statistical-integrity controls** — typed measures, comparability guard, caveat matrix, CI/estimate
  uncertainty, no causal claims (statistics review gate).
- **Standard hygiene** — no secrets/tokens/PII in logs, receipts, or committed files (Elyos rule);
  dependency + secret scanning in CI; if any end-user analytics exist, privacy-preserving only.

**Abuse/misuse prevention.** The product refuses individual-risk/diagnosis/screening framings, refuses
to produce finer-grained breakdowns than the source publishes, and refuses causal/etiological or
group-essentialist disparity claims — enforced and tested, not merely documented.

---

## Sustainability & maintenance

After delivery, a named **maintenance rotation** owns the platform and the **steward** owns the
beneficiary relationship + outcome tracking. The defining maintenance reality for cancer statistics is
**periodic upstream releases**: SEER\*Explorer, USCS, and GLOBOCAN update on their own cadences, so the
project runs a **refresh cycle** — re-ingest the new release, re-validate against the upstream tool,
re-date provenance, re-verify crisis/helpline references, re-run the eval/caveat/suppression gates, and
**re-obtain oncologist + advocate sign-off for any changed HIGH-tier content** (version-scoped sign-off
does not carry forward). Stale data is flagged with its vintage rather than served silently as current.
Outcomes are tracked as **comprehension/adoption** (not pageviews) on an outcomes dashboard. Expansion
to additional cancer sites follows a documented, expert-gated process and only after the first is
stable.

---

## Open questions

- **Pilot cancer site/topic — decided in M0, not deferred** (it gates M1–M6: data corpus + reviewer
  profile). Selection criteria, scored before M1: (1) **high public-misinterpretation value** (a site
  where screening artifacts / survival misreads are common and harmful); (2) **data availability** in
  the allow-listed aggregate sources at the needed granularity (without forcing small cells); (3) an
  **available oncologist + advocate reviewer** for that site (rare-cancer communities are often more
  reviewer-accessible and underserved); (4) **tractable statistical complexity** for a first build.
  The specific site is `TO BE SECURED`, but the **decision rule + deadline (2026-08-31) are fixed**.
- **GLOBOCAN / IARC reuse terms** — confirm the exact GCO terms of use (non-commercial conditions,
  mandated citation, derivative limits) before enabling GCO data; if terms forbid a reuse, that data is
  not shipped. (Blocks enabling GCO.)
- **Readability target** — confirm the plain-language reading-level target (proposal: ~grade 8) with
  the advocate/health-literacy partner.
- **Crisis/helpline scope** — which helplines, for which regions/languages, and the verification +
  re-check cadence (region-appropriateness is essential; a US line is wrong for a non-US reader).
- **Comprehension-measurement instrument** — what before/after check credibly measures "reads the
  statistic more correctly," co-designed with the partner and avoiding any data collection that itself
  raises privacy concerns.
- **Multilingual / i18n** — defer to backlog or include a second language in pilot? (Adds per-language
  expert + plain-language review.)
- **Lane** — donated by default; is there a future funded lane for paid expert-review hours (hard
  budget cap) without compromising reviewer independence?

---

## References

- Proposal / portfolio entry: `planning/ROADMAP.md` (Track 8b — `incidence-explorers`; Track 8 cancer
  guardrails)
- Elyos work rules & refusal guardrails: `CLAUDE.md`
- Good-deed definition & risk tiers: `docs/good-deed-definition.md`
- Task JSON schema: `packages/schema/src/schemas.ts`
- Sibling Elyos plans (house style + overlap): `planning/projects/public-official-guide/{PLAN,TASKS}.md`,
  `planning/projects/open-data-datasheets/{PLAN,TASKS}.md`
- Aggregate data sources (terms to verify per *Data, licensing & compliance*): SEER\*Explorer (NCI),
  Global Cancer Observatory / GLOBOCAN (IARC/WHO), US Cancer Statistics (CDC/NCI; CDC WONDER public-use)
- Re-identification / small-cell suppression: SEER/USCS suppression of statistics based on fewer than
  16 cases

---

## Appendix A — Improvements applied

The following 25 specific improvements were identified during planning and have been **applied** to
the plan above (not left as suggestions). Each lists what changed and where.

1. **Aggregate-only allow-list made an enforced ingestion gate, not a policy.** Promoted the data
   boundary from prose to a tested `lib/governance` allow-list with a CI check (Architecture §1, Data
   §1, Risks). *Applied.*
2. **Explicit exclusion list for controlled-access data.** Named SEER Research Data (DUA), CDC
   restricted-use files, record-level/biobank data, and any DUA/IRB source as hard exclusions (Scope,
   Data §1). *Applied.*
3. **Per-source license register with CI enforcement.** Added a license register requiring a verified
   reuse term + attribution string before a source is enabled; unverified → not enabled (Data §2,
   metrics, M0/M1 tasks). *Applied.*
4. **GLOBOCAN/IARC terms called out specifically.** Flagged GCO non-commercial/citation terms as
   "verify before enabling" with a blocking open question, rather than assuming open (Data §2, Open
   questions, Risks). *Applied.*
5. **GLOBOCAN treated as modelled estimates, not counts.** Added typed estimate-uncertainty handling
   and a caveat so GCO is never presented as exact counts or used for short-term trends (Architecture
   §3, Data §7). *Applied.*
6. **Age-standardisation standard-population trap addressed.** Added an `AgeStandard` type + a
   comparability guard that refuses to co-plot incompatible standards (US 2000 vs World) without a
   caveat (Architecture §3, Data §7). *Applied.*
7. **Provenance includes exact query parameters + data release.** Provenance records the site/sex/race/
   year/age-standard/measure query params and dataset version, making every figure reproducible
   against the upstream tool (Architecture §2, Data §3). *Applied.*
8. **"No source, no render" enforced by a test.** Provenance-coverage is a CI gate and a success metric
   at 100%, not an aspiration (Architecture §2, metrics, Quality gates). *Applied.*
9. **Small-cell reconstruction / differencing attack explicitly blocked.** Suppression guard blocks
   aggregations/cross-tabs that could re-create a suppressed cell, fail-closed (Data §4, Security).
   *Applied.*
10. **Dual blocking expert review (oncologist AND patient advocate).** Made patient-facing content HIGH
    and gated on *both* reviewers with veto rights and a disagreement fallback (Quality gates,
    Governance). *Applied.*
11. **Content-safety lint for advice/diagnosis/prognosis phrasings.** Added an automated lint as a gate
    plus a 0-phrasing metric, so "not medical advice" is enforced, not just declared (Data §5,
    metrics, eval harness). *Applied.*
12. **Crisis/helpline verification + dating + region-appropriateness.** Made helplines verified against
    official sources, dated (`lastVerified`), region-correct, and re-checked on cadence (Data §6,
    metrics, Sustainability). *Applied.*
13. **No statistic is model-generated.** Hard rule that numbers come only from ingested aggregate
    sources; the LLM only assists prose/extraction behind a neutral client and never overrides expert
    review (Architecture key decisions, Risks). *Applied.*
14. **Screening/diagnostic-artifact caveat is mandatory where relevant.** The caveat matrix flags
    screening-sensitive incidence trends so "rising" isn't misread as rising true risk (Architecture
    §3, Data §7). *Applied.*
15. **Registry lag / data vintage stated explicitly.** Product states data vintage and registry lag;
    no real-time/surveillance framing (Scope non-goals, Data §7, Risks). *Applied.*
16. **No causal/etiological claims + structural disparity framing.** Added explicit non-goals and a
    descriptive-only, structural-context rule with advocate review for disparities (Scope, Data §7,
    Risks). *Applied.*
17. **WCAG 2.2 AA + chart-plus-text-table equivalents as acceptance criteria.** Accessibility is a gate
    (the beneficiaries include AT users), with charts never the sole representation (Goals,
    Architecture §4, metrics, M2). *Applied.*
18. **Plain-language readability target + human review.** Added a readability target (~grade 8) and a
    plain-language reviewer pass as a gate (Goals, metrics, Quality gates). *Applied.*
19. **Comprehension outcome metric (the real success), not pageviews.** Defined a before/after
    comprehension check as the headline outcome and an M2 kill-gate; vanity metrics excluded (Success
    metrics, Roadmap). *Applied.*
20. **Static-first architecture to minimise threat surface.** Chose static delivery with no server
    holding data, since there is no patient/restricted data to hold (Architecture, Security). *Applied.*
21. **Dated partner-acquisition plan + build-vs-mothball/pivot rule.** Time-boxed outreach with a
    concrete pivot (donate the literacy module to an OER/health-literacy commons) or mothball decision
    so the project never ships to no one (Executive summary, Problem, Risks, M5). *Applied.*
22. **Version-scoped expert sign-off + re-sign-off on refresh.** Sign-off attaches to a content version/
    citation set and does not carry forward; data-release refreshes trigger re-sign-off of changed
    HIGH content (Quality gates, Governance, Sustainability). *Applied.*
23. **Data-refresh cadence tied to upstream releases.** Maintenance re-ingests/re-validates/re-dates on
    SEER/GLOBOCAN/USCS releases, re-verifies helplines, and re-runs all gates (Sustainability, M6).
    *Applied.*
24. **Overlap with sibling cancer projects acknowledged + reused.** Cross-referenced
    `oncology-data-literacy`, `stats-for-clinicians`, `disparities-analysis`,
    `screening-coverage-trends`, `cancer-dataset-datasheets` to reuse patterns and avoid duplication
    (Dependencies). *Applied.*
25. **Refusal behaviour for out-of-scope user asks.** The product declines individual-risk/diagnosis/
    screening and over-granular requests, explains why, and redirects to a clinician / official
    guidance / the upstream tool (Scope, Security abuse-prevention). *Applied.*

---

## Review sign-off

**Reviewer:** planning author (senior staff engineer + TPM), self-review pass on 2026-06-28.
**Method:** checked the draft against (a) the PLAN_SPEC 17-section structure, (b) the Elyos cancer
guardrails in `CLAUDE.md` + `planning/ROADMAP.md` Track 8, (c) the good-deed definition + risk tiers,
and (d) the Task JSON schema for TASKS.md mapping. Corrections made during review:

- **Completeness:** confirmed all 17 required H2 sections are present and ordered per PLAN_SPEC; *Data,
  licensing & compliance* and *Quality, review & risk gates* both **lead with the binding cancer
  guardrails** as required.
- **Correctness — data boundary:** verified the plan only admits **aggregate/open-access/de-identified**
  sources and explicitly excludes SEER Research Data (DUA), CDC restricted-use files, and any record-
  level/DUA/IRB source — consistent with the binding guardrail. Suppression/re-identification handling
  added and made fail-closed.
- **Correctness — medical safety:** verified patient-facing content is **HIGH risk** and gated on a
  **blocking oncologist + patient-advocate** dual sign-off, with persistent "not medical advice — talk
  to your care team" framing and **verified, dated crisis/helpline** info — matching the guardrail.
- **Correctness — statistics:** confirmed the plan distinguishes incidence/mortality/prevalence/
  survival, counts vs rates, age-standardisation (with the standard-population comparability trap),
  CIs/estimate uncertainty (GLOBOCAN as modelled estimates), screening artifacts, registry lag, and a
  no-causal-claims rule.
- **Honesty:** confirmed no partner/expert/beneficiary is invented — all are `TO BE SECURED` with a
  dated acquisition plan and a build-vs-mothball/pivot rule; success is outcome-based (comprehension +
  adoption), not vanity metrics.
- **Provenance:** confirmed every quantitative assertion is gated on a resolvable source ("no source,
  no render") and that no statistic is model-generated.
- **Schema readiness:** confirmed TASKS.md maps cleanly to `packages/schema/src/schemas.ts` with a
  schema-valid example Task JSON and `verifiedNeed:false` throughout (no partner secured).

**Outcome:** the plan is internally consistent, complete against the spec, and faithful to the binding
cancer guardrails. **Sign-off: APPROVED as Draft v0.1.0** for community/governance review. **Still
needs a human decision:** pilot cancer site, GLOBOCAN/IARC reuse terms, securing the oncologist +
advocate reviewers and a beneficiary, and the readability target.
