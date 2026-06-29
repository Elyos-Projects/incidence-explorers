# Competitive + Improvement Analysis — `incidence-explorers`

> Scope: plain-language, accessible dashboards/explainers built from **aggregate** SEER\*Explorer,
> GLOBOCAN/GCO, and USCS cancer-incidence statistics. Medium risk overall; HIGH for any
> patient-facing content. Analysis dated 2026-06-29. Web-researched; URLs cited inline.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually rigorous — it already names most of the traps (counts-vs-rates, standard-population
mismatch, GLOBOCAN-as-estimates, `<16` suppression, registry lag, no causal claims, WCAG 2.2 AA). The
review below confirms what holds and flags the one place where the plan is materially **wrong**, plus
several places it is under-specified.

### 1a. CRITICAL — GLOBOCAN license is CC BY-NC-**ND**, not "CC BY-NC" (and that breaks the output license)

This is the single most important finding. The task brief and the plan both treat GLOBOCAN/IARC as
**CC BY-NC**. Web research indicates IARC/GCO content is published under **Creative Commons
Attribution-NonCommercial-NoDerivatives 3.0 IGO (CC BY-NC-ND 3.0 IGO)** — the **ND (NoDerivatives)**
clause, plus NC, plus a no-endorsement / no-IARC-logo condition
(https://gco.iarc.who.int/, https://www-dep.iarc.fr/about-the-gco; license family corroborated at
https://creativecommons.org/cc-licenses/). The plan's open question "confirm GCO non-commercial / citation
/ derivative limits" is correct to exist, but the plan **underestimates the consequence**:

- **The project's core activity is making derivatives.** Re-plotting GCO numbers into new charts,
  recomputing/normalizing, building "explainers" around them, and especially **publishing derived
  datasets/tables** are all derivative works. A strict reading of **ND forbids exactly this** for
  GLOBOCAN-sourced figures.
- **Output-license conflict (Plan §"Output licensing", §Scope).** The plan says it will publish derived
  datasets/content as **CC-BY-4.0**. You **cannot relicense GLOBOCAN-derived data as CC-BY-4.0**: that
  silently strips both NC and ND. SEER/USCS (U.S. federal works, effectively public domain) *can* be
  CC-BY-4.0; GLOBOCAN cannot. A blanket "all derived tables are CC-BY-4.0" is therefore **incorrect and
  non-compliant**. The license register must be **per-source**, and output licensing must be
  **per-source-segregated** (or GLOBOCAN reduced to fair-use-style display-only with no derived-data
  redistribution).
- **Practical resolutions to add to the plan:** (a) treat GCO as **display/reference-only with mandatory
  citation**, redistributing *no* derived GCO tables; or (b) **prefer the underlying GLOBOCAN journal
  publication / IARC data-release terms**, which sometimes grant broader reuse than the GCO web UI; or
  (c) **request explicit written permission** from IARC for the specific reuse; or (d) **lean on SEER/USCS
  for anything requiring redistribution** and use GCO only where its global coverage is irreplaceable.
  Whichever path, the M0/M1 license-register gate must encode ND, NC, no-endorsement, and the mandated
  citation string — not just "attribution required."

> Note: Our World in Data re-publishes GCO-derived series under its own CC BY but **explicitly warns that
> third-party (WHO/IARC) source terms still apply** (https://ourworldindata.org/faqs). That is precisely
> the trap this project must not fall into by stamping CC-BY-4.0 on GCO derivatives.

### 1b. Age-standardization / rates vs counts vs ASR — strong, with two gaps

Correct and well-handled: typed `Measure`, `CountOrRate`, an `AgeStandard` type, and a **comparability
guard** that refuses to co-plot incompatible standards without a labelled caveat. The standard-population
trap (SEER/USCS **2000 U.S. Std** vs GLOBOCAN **World/Segi**) is explicitly named. Gaps to add:

- **Three-way distinction, not two.** The plan frames "counts vs rates." It should explicitly carry
  **crude rate vs age-specific rate vs age-standardized rate (ASR)** as distinct, and never let a crude
  rate and an ASR share an axis. ASR is also itself standard-dependent (ASR-World ≠ ASR-US2000).
- **SEER\*Explorer now ships preliminary delay-adjusted age-adjusted rates/trends through 2024**
  (https://seer.cancer.gov/statistics-network/explorer/application.html). The pipeline must capture
  **delay-adjustment status** and **preliminary-vs-final** as provenance fields, or it will mislabel
  vintage and mix delay-adjusted with non-adjusted series.

### 1c. SEER/USCS suppression of small counts — correct threshold, extend the rationale

The `<16` rule is right and is shared by **both** SEER and USCS. USCS documents the dual rationale —
**rate instability** (a count ≤16 yields a standard error ≥25% of the rate / a 95% CI as wide as the rate)
**and patient confidentiality** (https://www.cdc.gov/united-states-cancer-statistics/technical-notes/suppression.html).
The plan's fail-closed guard and explicit blocking of complementary-cell/differencing reconstruction is
exactly right. Add: **complementary suppression** (when only one cell in a row is suppressed, the row
total can re-expose it — suppress a second cell), and note USCS also suppresses **rates based on
small populations**, not only small case counts.

### 1d. Confidence intervals / GLOBOCAN uncertainty — mostly right, sharpen one point

Good: CIs for SEER/USCS rates, and "modelled estimate — uncertainty" for GLOBOCAN. Sharpen: **GLOBOCAN
does not publish conventional CIs** — its estimates are *modelled* from registry data of varying quality,
so the honest treatment is a **data-quality / methods-uncertainty flag** (and "not for short-term trend
claims"), not a fabricated interval. The plan should state the product will **never synthesize a CI** for
a GLOBOCAN value. (Caveat matrix already gestures at this; make it explicit.)

### 1e. No-causal-claims, data currency, accessibility — complete

- **No causal claims** + descriptive-only/structural-context disparity framing: present and gated by
  statistics + advocate review. Good.
- **Data currency / registry lag:** explicitly stated as a non-goal of real-time framing, with vintage on
  every figure and a refresh cadence. Good — just ensure the **vintage field distinguishes
  diagnosis-year from release-year** (SEER incidence typically lags ~2–4 years).
- **Accessibility:** WCAG 2.2 AA, chart-plus-text-table, ARIA, keyboard, contrast — as acceptance
  criteria, not polish. This is genuinely ahead of every government competitor (see §2).

### 1f. Other completeness notes

- **Mortality denominator subtlety:** USCS/SEER mortality comes from NCHS, incidence from registries —
  different geographic coverage and years. The comparability guard should block naive incidence-vs-mortality
  overlays that imply a survival inference.
- **"Validate against upstream within tolerance"** is good, but for GLOBOCAN the ND clause may even
  constrain *how* you reproduce/redraw figures; resolve §1a first.
- **Helpline verification + dated `lastVerified`:** present and correct; region-appropriateness called out.

---

## 2. Competitive landscape

| Tool | What it is | Strengths | Weaknesses (our opening) |
|---|---|---|---|
| **SEER\*Explorer** (NCI) — https://seer.cancer.gov/statistics-network/explorer/ | Authoritative U.S. interactive incidence/mortality/survival/prevalence, APC/AAPC, delay-adjusted to 2024 | Gold-standard data; custom graphs/tables; downloadable; public, no DUA; now preliminary 2024 | Built **for analysts**; jargon (APC, delay-adjustment, relative survival) unexplained; not plain-language; accessibility is basic; no "how to read this" layer; U.S.-only |
| **CDC U.S. Cancer Statistics Data Viz** — https://www.cdc.gov/united-states-cancer-statistics/dataviz/ | Official federal incidence+mortality, state/county/congressional-district | Most complete U.S. registry coverage; documented suppression + technical notes | Dense; analyst-oriented; caveats buried in technical-note PDFs; limited narrative; accessibility uneven |
| **IARC Global Cancer Observatory / GLOBOCAN** — https://gco.iarc.who.int/ | Global modelled estimates, 185 countries, 36 cancers (Cancer Today/Tomorrow/Over Time) | Only credible global comparator; rich maps/bars | **CC BY-NC-ND** (reuse-hostile, see §1a); estimates routinely misread as counts; methods opaque to lay users; not plain-language |
| **Our World in Data — Cancer** — https://ourworldindata.org/cancer | Narrative global cancer explainers + embeddable Grapher charts (mostly IHME/GBD, some GCO) | Excellent plain-language narrative; **CC BY**; embeddable; strong design | Uses **GBD/IHME modelled** data (different from registry SEER/GCO); global not patient-facing; no medical-safety framing; no per-cancer "talk to your care team" layer; third-party source terms still apply |
| **Cancer Research UK — statistics** — https://www.cancerresearchuk.org/health-professional/cancer-statistics | UK incidence/mortality/survival + "Cancer Stats Explained" + plain-English patient site | Has a genuine **plain-language "stats explained"** layer + patient-facing CancerHelp content | UK-only; "explained" pages aimed at health professionals; not an interactive accessible dashboard over raw aggregates; charity (not neutral infra) |
| **NCI State Cancer Profiles** — https://statecancerprofiles.cancer.gov/ | Interactive U.S. maps/graphs to national/state/county incidence, mortality, screening, risk factors | Geographic granularity; ties incidence to screening/risk-factor context | Planner/policymaker audience; map-heavy and analyst-facing; no lay interpretation layer; accessibility limited |
| **The Cancer Atlas (ACS + IARC), 4th ed. 2025** — https://canceratlas.cancer.org/ | Curated visual narrative of global cancer burden + risk factors + action | Beautiful, accessible, plain narrative; expert-authored; patient/public-friendly framing | **Static editorial publication**, not a live queryable dashboard; not provenance-per-figure/reproducible; global, not personal-statistic interpretation |

**Pattern:** the authoritative tools (SEER\*Explorer, USCS, GCO, State Cancer Profiles) are **accurate but
analyst-facing**; the accessible products (OWID, Cancer Atlas, CRUK "explained") are **either global/editorial
or not reproducible per-figure**. **No one combines** authoritative registry aggregates +
provenance-per-number + plain-language caveat-as-you-show-it + WCAG 2.2 AA + medical-safety framing.

---

## 3. Gaps we can fill

1. **"Teach the caveat as you show the number."** Every competitor buries methods in a PDF; we inline the
   right caveat per figure (counts≠rates, standard-pop, screening artifact, GLOBOCAN-is-an-estimate).
2. **Provenance-per-assertion / reproducibility.** No competitor binds each rendered statistic to
   source + release + exact query params + extraction date + URL such that a reader can re-run it.
3. **Accessibility as a gate.** Government tools are weak on WCAG 2.2 AA, chart+data-table parity, AT
   testing. This directly serves the beneficiaries (low-literacy, AT users) the others underserve.
4. **Medical-safety interpretation layer.** "This is a population statistic, not your prognosis,"
   persistent "not medical advice — talk to your care team," refusal of individual-risk framings — none of
   the data tools do this; the patient-facing ones (Cancer Atlas, CRUK) aren't reproducible dashboards.
5. **Screening-artifact literacy.** "Incidence rose" mis-told as "risk rose." A first-class, reusable
   explainer pattern none of them foregrounds at the chart level.
6. **License-correct global reuse.** By solving the GCO ND/NC problem cleanly (§1a) and being explicit
   about source-segregated output licensing, we become the *trustworthy-to-reuse* reference — something
   even OWID hedges on for IARC data.
7. **Cross-source comparability guard.** A guard that refuses to co-plot ASR-World vs ASR-US2000, or
   incidence-vs-mortality-implying-survival, is a literacy feature no one ships.

---

## 4. Differentiators to win

- **Strongest single differentiator: provenance-bound, caveat-inline, accessibility-gated plain-language
  interpretation — "no source, no render; no caveat, no chart."** This is the union of correctness +
  trust + reach that *no existing tool* offers, and it is enforced (tests/CI), not promised.
- **Medical-safety-gated patient-facing content** (oncologist + advocate blocking sign-off) — a trust
  moat editorial sites approximate but data tools entirely lack.
- **Reproducibility** (re-run the query upstream) → credible for journalists/educators who must fact-check.
- **Neutral, open infrastructure** (MIT code, source-segregated CC-BY content) vs charity/agency-branded
  silos — reusable by libraries, newsrooms, advocacy orgs, and other Elyos sibling projects.
- **Comparability + screening-artifact guards** turn statistical correctness into a *product feature*.

---

## 5. Claude API leverage — and the hard boundaries

**Where Claude clearly helps (prose/transform/accessibility from already-computed stats):**

1. **Plain-language explainer drafting from computed numbers.** Stats come from `lib/stats` (code); Claude
   turns a structured `{measure, value, CI, ageStandard, caveats[]}` record into grade-~8 prose with the
   caveat woven in. Claude drafts; oncologist + advocate sign off; numbers never originate in the model.
2. **Per-region / per-audience scaling of one reviewed explainer.** Re-voice an approved template for a
   different reading level, a different region's helplines, or a different cancer site **as drafts for
   review** — multiplying expensive expert-reviewed content without re-deriving stats.
3. **Accessibility generation.** Draft alt-text, chart-equivalent **data-table captions**, ARIA summaries,
   and "what this chart is saying" text equivalents from the underlying data table — directly advancing
   WCAG 2.2 AA chart+table parity.
4. **Caveat selection assist + content-safety lint.** Suggest which caveat-matrix entries apply, and
   pre-screen drafts for advice/diagnosis/prognosis phrasings (Claude as a *first-pass* lint, human gate final).
5. **Extraction/normalization assist** of messy upstream tables into typed provenance records — **with
   validation against the upstream published figure** (the number must match source within tolerance).

> Model/pricing/caching specifics: consult the Claude API skill before implementation; route everything
> through the plan's thin provider-neutral LLM client so the agent-neutral core stays vendor-free.

**Where Claude must NOT decide (hard gates — these are correctness/safety boundaries, not preferences):**

- **No statistic is model-generated.** Every number comes from an ingested aggregate source with
  provenance; Claude never computes, estimates, interpolates, un-suppresses, or "fills in" a value.
- **No causal or medical claims.** Claude never asserts etiology, prognosis, individual risk, screening or
  treatment guidance. Patient-facing substance is **oncologist + patient-advocate** decided (blocking veto).
- **Factual accuracy + license are human-verified.** Claude may *draft* attribution/citation strings and
  flag license constraints, but the **license register and the GCO ND/NC determination (§1a) are a human
  gate**; Claude's read of a license is never authoritative.
- **Stats from code, not LLM.** Age-standardization, CI handling, comparability, and suppression decisions
  live in tested TypeScript, not in a prompt.

---

## 6. Ten concrete optimizations

1. **Fix the license model (highest priority).** Encode **CC BY-NC-ND 3.0 IGO** for GCO in the license
   register; make output licensing **per-source** (SEER/USCS → CC-BY-4.0/PD-safe; GLOBOCAN →
   display/reference-only, **no derived-data redistribution**) and add a CI check that fails if a GCO-derived
   table is exported under CC-BY-4.0.
2. **Add a `derivativeAllowed` + `redistributable` flag per source**, evaluated at *render and export*
   time — not just at ingestion — so an ND source can be shown-with-citation but never repackaged.
3. **Promote ASR to a three-way type:** crude / age-specific / age-standardized(`standardPop`); the
   comparability guard blocks crude-vs-ASR and ASR-World-vs-ASR-US2000 on one axis.
4. **Capture delay-adjustment + preliminary/final + diagnosis-year-vs-release-year** as first-class
   provenance fields (SEER\*Explorer now ships preliminary delay-adjusted 2024 data).
5. **Never synthesize a CI for GLOBOCAN**; render a data-quality/methods-uncertainty badge + "not for
   short-term trends" instead, enforced by the caveat matrix for any GCO figure.
6. **Add complementary-suppression** to the small-cell guard (suppress a second cell when a row total can
   re-expose the first), plus small-population rate suppression — not only `<16` counts.
7. **Block incidence↔mortality overlays that imply survival**; require an explicit labelled caveat (and
   note differing NCHS-mortality vs registry-incidence coverage/vintage).
8. **Ship embeddable, accessible widgets** (iframe/web component) with provenance + caveat baked in — so a
   newsroom or library can drop a *correct* chart in one line (out-competes OWID's reuse story on
   correctness; gated to non-ND sources).
9. **Build a comprehension-check harness early** (the M2 kill-gate) as a reusable instrument — before/after
   "read this statistic correctly" — and treat it as the headline outcome metric, not pageviews.
10. **Provide a machine-readable provenance/caveat API** (JSON-LD per figure) so downstream tools and an
    MCP server (see §7) can consume statistics *with* their caveats and citations.

---

## 7. Parallel & perpendicular spin-offs

**Parallel (same engine, adjacent corpora) — direct ties to sibling Elyos plans:**

- **`open-data-explainers`** — the provenance + caveat-matrix + accessibility engine is **domain-agnostic**;
  generalize it to any public aggregate dataset (the cancer pipeline is the proving ground).
- **`disparities-analysis`** — reuse this project's descriptive-only, structural-context, no-group-essentialism
  framing and the comparability guard; share the advocate-review pattern.
- **`screening-coverage-trends`** — the **screening-artifact caveat** is the shared crown jewel; pair
  incidence trends with screening-coverage to *explain* (not causally claim) apparent rises.
- **`prevention-info-open`** — link descriptive incidence patterns to *evidence-based, expert-reviewed*
  prevention info (never causal/individual), reusing the medical-safety + helpline-verification gates.
- **`oncology-data-literacy` / `stats-for-clinicians` / `cancer-dataset-datasheets`** — share the
  "how to read cancer statistics" module and the per-source datasheet/provenance schema; avoid duplication.

**Perpendicular (new product surfaces from the same core):**

- **Embeddable widgets** (§6.8): correctness-guaranteed chart components for newsrooms/libraries/advocacy sites.
- **MCP server** exposing `getStatistic(site, measure, region, …) → {value, CI, ageStandard, caveats[],
  source, license}` so other agents/tools consume **provenanced, caveat-tagged, license-aware** cancer
  stats and **cannot** retrieve a value without its caveats/citation — a literacy guardrail at the API layer.
- **A reusable "statistical-honesty caveat matrix"** as a standalone open spec other health-data projects adopt.
- **Plain-language readability + accessibility lint** packaged as a shareable check for sibling deeds.

---

## 8. Open questions

1. **GCO ND/NC resolution (blocking).** Confirm CC BY-NC-ND 3.0 IGO applies to the specific GCO data we
   ingest; decide display-only vs seek-IARC-permission vs prefer-journal/data-release-terms vs lean-on-SEER.
   Nothing GCO-derived ships until this is recorded in the license register. (Resolves §1a.)
2. **Output-license segregation.** Confirm the per-source output-licensing scheme and the CI check that
   prevents GCO derivatives from being stamped CC-BY-4.0.
3. **Pilot cancer site** (already M0-gated): which site maximizes misinterpretation-value × data-availability
   × reviewer-availability × tractable complexity — and is its data *redistributable* (favoring SEER/USCS)?
4. **GBD/IHME as a fourth source?** OWID uses GBD; it is CC BY and globally modelled. In scope (broader,
   CC-BY-friendly global comparator) or out (avoid mixing modelling frameworks with registry data)?
5. **Readability target** (~grade 8) — confirm with the health-literacy partner; pair with a measurable
   instrument.
6. **Helpline scope** — which regions/languages, verification + re-check cadence (a U.S. line is wrong for
   a non-U.S. reader).
7. **Comprehension instrument** — what before/after check credibly measures "reads it more correctly"
   without itself collecting sensitive data?
8. **MCP/widget governance** — how do caveats and license constraints travel with a statistic once it
   leaves our UI, and how is misuse downstream detected/limited?

---

### Source URLs

- SEER\*Explorer: https://seer.cancer.gov/statistics-network/explorer/ ·
  https://seer.cancer.gov/statistics-network/explorer/application.html
- CDC U.S. Cancer Statistics Data Viz: https://www.cdc.gov/united-states-cancer-statistics/dataviz/ ·
  Suppression: https://www.cdc.gov/united-states-cancer-statistics/technical-notes/suppression.html
- IARC Global Cancer Observatory / GLOBOCAN: https://gco.iarc.who.int/ ·
  https://www-dep.iarc.fr/about-the-gco
- Our World in Data — Cancer: https://ourworldindata.org/cancer · Reuse/FAQ: https://ourworldindata.org/faqs
- Cancer Research UK statistics: https://www.cancerresearchuk.org/health-professional/cancer-statistics
- NCI State Cancer Profiles: https://statecancerprofiles.cancer.gov/
- The Cancer Atlas (ACS + IARC): https://canceratlas.cancer.org/
- Creative Commons license family: https://creativecommons.org/cc-licenses/
