# Incidence-Explorers — TASKS.md

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

Backlog for **Incidence-Explorers** (slug: `incidence-explorers`): plain-language, accessible
dashboards and explainers built **only** from **aggregate, open-access, de-identified** cancer-
incidence statistics (SEER\*Explorer, GLOBOCAN/GCO, USCS). See `PLAN.md` for full context.

**Binding cancer guardrails (gate everything):** aggregate/open-access/de-identified data **only**
(individual-level / controlled-access / SEER Research Data DUA / IRB sources **OUT OF SCOPE**); verify
each source's license before enabling; honour + extend small-cell suppression (no re-identification);
**provenance on every assertion**; **no medical advice** — patient-facing content is education only,
carries "not medical advice — talk to your care team," uses **verified, dated** crisis/helpline info,
and **ships only after blocking sign-off by a credentialed oncologist AND a patient advocate**
(`riskTier: high`). No partner/expert/beneficiary is yet secured, so delivery-dependent tasks carry
`requestor: TO BE SECURED` and `verifiedNeed: false` (currently `false` everywhere).

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against `packages/schema/src/schemas.ts`.
Field mapping:

- **id** — stable slug `incidence-explorers-<area>-NNN` (e.g. `incidence-explorers-governance-001`).
- **title** — the task title in the milestone table.
- **project** — `incidence-explorers`.
- **type** — one of `code | research | writing | data | design-spec | maintenance`.
- **lane** — `donated` (default; any `funded` task must add `fundedBudgetUsd` with a hard cap).
- **priority** — `high | medium | low`.
- **domain** — array, e.g. `["cancer","epidemiology","open-data","health-literacy","software"]`.
- **riskTier** — `low | medium | high`. **Any patient-facing cancer content is `high`** (oncologist +
  advocate sign-off); the data-literacy explainer (no per-cancer medical claims) and most data tasks
  are `medium`; pure infra/a11y is `low`.
- **urgent** — boolean (no urgent tasks at cold-start).
- **deliverable** — `pr | dataset | document | translation`.
- **tokenEstimate** — `small | medium | large` (maps to the Size column).
- **status** — `open | in-progress | review | delivered | done` (all start `open`).
- **context / objective / acceptanceCriteria[] / resources[] / output** — per task.
- **requestor** — partner/steward/expert; `TO BE SECURED` where unknown.
- **verifiedNeed** — `true` only once a specific partner/need is confirmed; otherwise `false`
  (currently `false` everywhere — no partner secured).
- **outputLicense** — code: `MIT`; content/explainers/datasets: `CC-BY-4.0`; docs: `CC-BY-4.0`.

Size legend: small ≈ `small`, med ≈ `medium`, large ≈ `large`.
Reviewer **"Expert (onco+advocate)"** = credentialed oncologist **AND** patient advocate (HIGH-tier
blocking sign-off, **TO BE SECURED**). **"Data-gov"** = data-governance reviewer (license/provenance/
suppression). **"Stats"** = statistics reviewer (measures/age-standardisation/CIs/caveats).

---

## Milestone M0 — Foundation & data-governance (cold-start)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| incidence-explorers-governance-001 | Data-governance layer spec + source allow-list (aggregate-only) + license register | design-spec | medium | medium | document | — | Data-gov + Maintainer |
| incidence-explorers-site-002 | Pilot cancer site/topic selection (scored against explicit criteria) — gates M1–M6 | research | small | medium | document | — | Maintainer + Stats |
| incidence-explorers-repo-003 | Monorepo + pnpm + TS/ESM + static-site + CI (build/test/lint) skeleton | code | small | low | pr | — | Maintainer |
| incidence-explorers-stats-spec-004 | Statistical-honesty spec + per-figure caveat matrix + "not medical advice" framing spec | design-spec | medium | medium | document | — | Stats + Maintainer |
| incidence-explorers-suppression-005 | Suppression / re-identification guard (honour `<16`, block small-cell reconstruction, fail-closed) | code | medium | medium | pr | 001, 003 | Data-gov |

**Acceptance criteria — key tasks**

- **incidence-explorers-governance-001** (data-governance spec + allow-list + license register)
  - Defines the **aggregate-only allow-list**: SEER\*Explorer, GLOBOCAN/GCO, USCS (incl. CDC WONDER
    public-use) — and **explicitly excludes** SEER Research Data (DUA), CDC restricted-use files, any
    record-level/individual-level/biobank data, and any DUA/IRB-gated source.
  - Specifies the **license register** schema: per source, the verified reuse term, required
    attribution string, and any non-commercial/no-derivatives constraint; an unverified source **cannot
    be enabled** (CI gate). Flags **GLOBOCAN/IARC terms** as verify-before-enable.
  - Specifies the **provenance record** (source, dataset release/version, exact query parameters,
    extraction date, URL/DOI, license, age-standard, measure) and the **"no source, no render"** rule.
  - Reviewed by the data-governance reviewer (recorded).

- **incidence-explorers-site-002** (pilot site selection)
  - Scores candidate cancer sites against explicit criteria: (1) high public-misinterpretation value
    (screening-artifact / survival-misread prone); (2) data availability in allow-listed aggregate
    sources without forcing small cells; (3) an available oncologist + advocate reviewer for the site;
    (4) tractable statistical complexity for a first build.
  - Records the decision (or shortlist + the fixed 2026-08-31 decision deadline) with rationale; the
    chosen site becomes a dependency for M1 ingestion and M3 content.

- **incidence-explorers-stats-spec-004** (statistical-honesty + caveat matrix)
  - Defines typed measures (incidence/mortality/prevalence/survival), counts-vs-rates rules, age-
    standardisation metadata + the **standard-population comparability rule** (no co-plotting US-2000
    vs World standards without a caveat), CI/estimate-uncertainty handling (**GLOBOCAN = modelled
    estimates, not counts**), and the **per-figure caveat matrix** (counts≠rates, age-standardisation,
    CI/uncertainty, **screening artifact**, registry lag).
  - Defines the **"not medical advice — talk to your care team"** framing requirement and the
    no-causal-claims / descriptive-disparities rule.

- **incidence-explorers-suppression-005** (suppression / re-identification guard)
  - Honours upstream suppression (e.g. SEER/USCS counts based on `<16` cases); **never displays**
    suppressed values; **blocks aggregations/cross-tabs that could reconstruct a suppressed cell**
    (differencing/complementary-cell attacks).
  - **Fail-closed:** unknown or near-threshold cell size → suppress. Unit tests cover both.

**M0 Definition of Done:** data-governance spec + **aggregate-only allow-list** + license-register
schema (with first entries + GLOBOCAN-terms flag) merged; statistical-honesty spec + caveat matrix +
"not medical advice" framing spec merged; suppression/re-identification guard implemented with tests;
TS/ESM static-site skeleton + green CI; **pilot cancer site selected (or shortlisted with the fixed
2026-08-31 decision)** so M1+ builds against the right corpus and reviewer profile.

---

## Milestone M1 — Aggregate ingestion + provenance

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| incidence-explorers-license-006 | Per-source license verification + recorded attribution (SEER/USCS + GLOBOCAN/IARC terms) | research | small | medium | document | 001 | Data-gov + Maintainer |
| incidence-explorers-ingest-007 | Aggregate ingestion for pilot site (SEER\*Explorer + GLOBOCAN [+USCS]) with full provenance | data | large | medium | dataset | 001, 002, 005, 006 | Data-gov + Stats |
| incidence-explorers-validate-008 | Upstream validation: ingested values match the source tool within tolerance + provenance-coverage test | code | medium | medium | pr | 007 | Stats + Data-gov |

**Acceptance criteria — key tasks**

- **incidence-explorers-license-006** (source license verification)
  - Each enabled source has a recorded, **verified** reuse term + required attribution string + any
    non-commercial/no-derivatives constraint in the license register.
  - **GLOBOCAN/GCO (IARC/WHO) terms verified before enabling** GCO data; if terms forbid the intended
    reuse, that data is **not shipped**. SEER/USCS (U.S. federal) attribution + citation requirements
    recorded.
  - No source is enabled in the pipeline until its register entry is verified (CI gate).

- **incidence-explorers-ingest-007** (aggregate ingestion + provenance)
  - Ingests **only allow-listed aggregate** tables for the pilot site (incidence, plus mortality/
    survival where published); **0 controlled-access/individual-level sources** (allow-list test green).
  - Every `DataPoint` carries a complete provenance record (source, release/version, query params,
    extraction date, URL/DOI, license, age-standard, measure); GLOBOCAN points flagged as **modelled
    estimates**.
  - Upstream suppression honoured end-to-end; no suppressed/`<16` value enters the dataset.

- **incidence-explorers-validate-008** (validation + provenance coverage)
  - Ingested values **match the upstream tool** (SEER\*Explorer / GCO) within a documented tolerance;
    discrepancies are investigated, not silently accepted.
  - **Provenance-coverage test:** 100% of `DataPoint`s resolve to a `Source`; a point without a source
    fails CI ("no source, no render").

**M1 Definition of Done:** the pilot site's aggregate statistics are ingested with complete provenance,
**validated against the upstream tool**, with each source's **license verified + recorded**,
suppression honoured, and **0 controlled-access sources** (allow-list + provenance-coverage tests
green).

---

## Milestone M2 — Statistics-correctness + accessible dashboards

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| incidence-explorers-stats-009 | Statistics-correctness layer: typed measures, age-standard + comparability guard, CI/estimate uncertainty | code | large | medium | pr | 004, 007 | Stats |
| incidence-explorers-dashboard-010 | Accessible dashboards (WCAG 2.2 AA): chart + text-table + ARIA, inline caveats from the matrix | code | large | medium | pr | 009 | Maintainer + Stats |
| incidence-explorers-eval-011 | Eval gates: provenance + caveat coverage + suppression + content-safety lint (CI) | code | medium | medium | pr | 009, 010 | Data-gov + Stats |

**Acceptance criteria — key tasks**

- **incidence-explorers-stats-009** (statistics-correctness layer)
  - Typed `Measure`/`CountOrRate`/`AgeStandard`/uncertainty; the **comparability guard refuses** to
    co-plot incompatible measures/standards without an explicit labelled caveat.
  - GLOBOCAN rendered as **modelled estimates with uncertainty**, never as exact counts; CIs shown
    where the source publishes them.

- **incidence-explorers-dashboard-010** (accessible dashboards)
  - Every chart ships an **equivalent data table** + ARIA labelling + keyboard navigation + sufficient
    contrast; charts are never the only representation. **WCAG 2.2 AA** verified (automated + manual AT).
  - Each figure shows its **mandatory caveats** from the matrix (counts≠rates, age-standardisation +
    standard pop, CI/estimate uncertainty, screening artifact where relevant, registry lag/vintage).
  - No suppressed or unprovenanced value renders.

- **incidence-explorers-eval-011** (eval gates)
  - CI gates for **provenance coverage (100%)**, **caveat coverage (100% against the matrix)**,
    **suppression safety**, and a **content-safety lint** (no advice/diagnosis/prognosis phrasings).
  - A failing gate fails the build.

**M2 Definition of Done:** correct, accessible dashboards (WCAG 2.2 AA, chart+table equivalents) with
enforced caveats, comparability guard, CI/estimate-uncertainty display, and green provenance/caveat/
suppression/content-safety gates — **no patient-facing medical claims yet**. The M2 **comprehension
kill-gate** (an explained chart reads more correctly than a raw table) trends positive before HIGH-tier
content investment, else pause and reassess.

---

## Milestone M3 — Patient-facing explainers (HIGH, expert-gated)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| incidence-explorers-literacy-012 | "How to read cancer statistics" data-literacy explainer (no per-cancer medical claims) | writing | medium | medium | document | 004, 010 | Stats + Maintainer |
| incidence-explorers-explainer-013 | Pilot cancer-site explainer (provenanced, "not medical advice," verified helplines) — oncologist + advocate signed off | writing | large | high | document | 008, 010, 012 | Expert (onco+advocate) |
| incidence-explorers-helplines-014 | Crisis/helpline reference set: verified against official sources, region-appropriate, dated | data | small | high | dataset | 002 | Expert (onco+advocate) |

**Acceptance criteria — key tasks**

- **incidence-explorers-literacy-012** (data-literacy explainer — medium)
  - Plain-language (target ~grade 8) explanation of incidence vs mortality vs prevalence vs survival,
    counts vs rates, age-standardisation, CIs/estimate uncertainty (incl. GLOBOCAN estimates), and
    screening artifacts — each illustrated with a provenanced dashboard figure.
  - Makes **no per-cancer medical claims** (stays medium-risk); still carries "general education" framing.

- **incidence-explorers-explainer-013** (pilot cancer-site explainer — HIGH, blocking)
  - Every quantitative claim cites its `DataPoint` (provenance complete); **no** diagnosis/prognosis/
    individual-risk/screening/treatment guidance; persistent **"general education, not medical advice —
    talk to your care team"** framing; content-safety lint green.
  - Crisis/helpline references are the **verified, dated** entries from task 014 and region-appropriate.
  - **Signed off by a credentialed oncologist AND a patient advocate** (version-scoped, recorded in the
    reviewers ledger) **before ship** — blocking; no sign-off, no ship.

- **incidence-explorers-helplines-014** (crisis/helpline set — HIGH)
  - Each helpline verified against its **official source**, region/language-appropriate, and stamped
    with a `lastVerified` date; unverified or stale entries are withheld.
  - Reviewed by the advocate (and oncologist where clinical).

**M3 Definition of Done:** the data-literacy explainer is shipped (general review); the first cancer-
site explainer is fully provenanced, "not medical advice"-framed, uses verified/dated helplines, passes
the content-safety lint, and is **oncologist + patient-advocate signed off** (blocking).

---

## Milestone M4 — Eval, hardening & beneficiary readiness

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| incidence-explorers-readiness-015 | Full eval + readability + a11y verification + comprehension instrument + adoption runbook | code | medium | medium | pr | 011, 012, 013 | Maintainer + Stats + Expert (onco+advocate) |

**Acceptance criteria — incidence-explorers-readiness-015**
- All eval gates green (provenance, caveat, suppression, content-safety, helpline verification,
  readability, a11y); plain-language review passed.
- A credible **before/after comprehension instrument** is ready (co-designed with the prospective
  partner; collects no privacy-sensitive end-user data).
- An **adoption/onboarding runbook** for a beneficiary org is written.

**M4 Definition of Done:** the product passes all quality gates, is **WCAG 2.2 AA** + plain-language
verified, has a comprehension-measurement instrument and an adoption runbook, and is beneficiary-ready.

---

## Milestone M5 — Publication & handoff (the deed)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| incidence-explorers-partner-016 | Secure beneficiary/steward + independent guardrail audit (data/license/medical-safety) | research | medium | high | document | 013, 015 | Steward + Expert (onco+advocate) + Data-gov |
| incidence-explorers-handoff-017 | Adoption: partner uses an explainer with its community; comprehension improvement recorded | maintenance | medium | high | document | 016 | Steward + Expert (onco+advocate) |

**Acceptance criteria — key tasks**

- **incidence-explorers-partner-016** (secure beneficiary + audits)
  - A real beneficiary org (advocacy / library-health-literacy / public-health / newsroom) is secured;
    steward assigned; `verifiedNeed` flips to `true`.
  - Independent audit confirms: **only aggregate/open-access sources used** (allow-list), each license
    verified, every assertion provenanced, suppression honoured, all patient-facing content **oncologist
    + advocate signed off** and "not medical advice"-framed, helplines verified.
  - Driven by the **dated partner-acquisition plan**; if no beneficiary by **~2027-03-31**, apply the
    **build-vs-mothball/pivot rule** (PLAN exec summary): pivot the literacy module into an OER/health-
    literacy commons or mothball to maintenance-only — recorded in governance — rather than publish into
    a vacuum.

- **incidence-explorers-handoff-017** (closed loop — the deed)
  - The partner demonstrably uses ≥ 1 explainer with its community; a **comprehension improvement** is
    recorded; "not medical advice" framing upheld throughout; guardrails independently verified.

**M5 Definition of Done:** project-level **Definition of Shipped** met — a real beneficiary uses the
explainers and people read the statistics more correctly, with data/license/medical-safety guardrails
independently verified and the outcome recorded. *(Gated on a secured partner — TO BE SECURED.)*

---

## Milestone M6 — Sustain & refresh (post-delivery)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| incidence-explorers-ops-018 | Ops runbook + data-refresh cadence (re-ingest/re-validate/re-sign-off) + outcomes dashboard | maintenance | medium | medium | document | 017 | Maintainer + Steward + Expert (onco+advocate) |

**Acceptance criteria — incidence-explorers-ops-018**
- Runbook covers deploy, the **data-release refresh cycle** (re-ingest the new SEER/GLOBOCAN/USCS
  release, re-validate against the upstream tool, re-date provenance, re-verify helplines, re-run all
  gates) and **re-sign-off of changed HIGH-tier content** (version-scoped sign-off does not carry
  forward).
- Outcomes dashboard tracks **comprehension/adoption** (not pageviews); named maintenance rotation;
  documented, expert-gated process for adding the next cancer site.

**M6 Definition of Done:** project sustainably maintained with a refresh cadence tied to upstream
releases, re-sign-off of changed HIGH content, outcomes tracked, a rotation owning it, and a gated
expansion process.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
|---|---|---|---|---|---|---|
| incidence-explorers-site2-019 | Second cancer-site explainer pack | writing | large | high | document | Only after M6; full provenance + oncologist + advocate sign-off |
| incidence-explorers-i18n-020 | Internationalisation + community translations of explainers | writing | medium | high | translation | Per-language plain-language + expert review; region-correct helplines |
| incidence-explorers-disparities-021 | Descriptive disparities views (structural-context framing, no causal/essentialist claims) | data | medium | high | dataset | Overlaps `disparities-analysis`; advocate + stats review |
| incidence-explorers-trends-022 | Incidence-trend views with APC/AAPC + joinpoint caveats and screening-artifact flags | code | medium | medium | pr | Overlaps `screening-coverage-trends`; stats review |
| incidence-explorers-datasheets-023 | Datasheets for the derived published datasets (provenance + license) | data | small | medium | dataset | Reuse `cancer-dataset-datasheets` / `open-data-datasheets` patterns |

---

## Example task JSON

Complete, schema-valid Task JSON for the first M0 task (`incidence-explorers-governance-001`):

```json
{
  "id": "incidence-explorers-governance-001",
  "title": "Data-governance layer spec + source allow-list (aggregate-only) + license register",
  "project": "incidence-explorers",
  "type": "design-spec",
  "lane": "donated",
  "priority": "high",
  "domain": ["cancer", "epidemiology", "open-data", "data-governance", "health-literacy", "software"],
  "riskTier": "medium",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "Incidence-Explorers builds plain-language, accessible dashboards and explainers from AGGREGATE, open-access, de-identified cancer-incidence statistics (SEER*Explorer, GLOBOCAN/GCO, USCS). The binding cancer guardrails make the data boundary the headline gate: individual-level / case-level / controlled-access data is OUT OF SCOPE (SEER Research Data requires a signed DUA; CDC restricted-use files, record-level/biobank data, and any DUA/IRB source are excluded). Each source's license must be verified before its data is enabled, small-cell suppression must be honoured to prevent re-identification, and every rendered statistic must carry provenance. This cold-start task specifies and bounds that data-governance layer before any ingestion or feature is built. No partner organisation, beneficiary, or oncologist/advocate reviewer is yet secured.",
  "objective": "Write the authoritative data-governance specification that all later ingestion and rendering code must implement and be tested against: the AGGREGATE-ONLY source allow-list and the explicit controlled-access exclusion list; the per-source license register (verified reuse term + required attribution + non-commercial/no-derivatives constraints, with GLOBOCAN/IARC terms flagged verify-before-enable); the provenance record (source, dataset release/version, exact query parameters, extraction date, URL/DOI, license, age-standard, measure); the 'no source, no render' rule; and the small-cell suppression / re-identification boundary.",
  "acceptanceCriteria": [
    "Defines an aggregate-only source allow-list (SEER*Explorer, GLOBOCAN/GCO, USCS incl. CDC WONDER public-use) and an explicit exclusion list (SEER Research Data/DUA, CDC restricted-use files, any record-level/individual-level/biobank data, any DUA/IRB-gated source), enforced at ingestion",
    "Specifies the license-register schema: per source, the verified reuse term, required attribution string, and any non-commercial/no-derivatives constraint; an unverified source cannot be enabled (CI gate); GLOBOCAN/IARC terms flagged verify-before-enable",
    "Specifies the provenance record (source, dataset release/version, exact query parameters, extraction date, URL/DOI, license, age-standard, measure) and the 'no source, no render' (citation-coverage) rule",
    "Specifies the small-cell suppression / re-identification boundary: honour upstream suppression (e.g. counts based on fewer than 16 cases), never display or infer suppressed cells, block aggregations that could reconstruct one, fail-closed when cell size is unknown",
    "States that no statistic is model-generated (numbers come only from ingested aggregate sources) and that patient-facing content is out of scope for this task (handled HIGH-tier with oncologist + advocate sign-off downstream)",
    "Reviewed and signed off by the data-governance reviewer (recorded)"
  ],
  "resources": [
    "planning/projects/incidence-explorers/PLAN.md",
    "planning/ROADMAP.md",
    "CLAUDE.md",
    "docs/good-deed-definition.md",
    "packages/schema/src/schemas.ts"
  ],
  "output": "A reviewed data-governance specification document (the aggregate-only allow-list + controlled-access exclusion list, the license-register schema with first entries and the GLOBOCAN-terms flag, the provenance-record schema with the 'no source, no render' rule, and the small-cell suppression / re-identification boundary) - the contract that incidence-explorers-suppression-005 (suppression guard), incidence-explorers-license-006 (license verification), and incidence-explorers-ingest-007 (ingestion + provenance) implement and are tested against.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```
