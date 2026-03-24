# Decision log

This file records all analytical decisions made during the study, in chronological order. It is updated in real time — before moving to the next step, not after.

**Rule:** every decision that affects the analysis, the metrics, the inclusion criteria, or the interpretation must be entered here. If data had already been seen when the decision was made, the entry is flagged POST-HOC.

---

## How to add an entry

Copy the template below, fill in all fields, and commit this file together with any code or notebook changes that implement the decision.

```
---
**Date:**
**Decision:**
**Alternatives considered:**
**Rationale:**
**Data seen at time of decision:** yes / no
**Status:** PRE-REGISTERED / UNREGISTERED / POST-HOC
---
```

- **PRE-REGISTERED** — decision was declared in the pre-registration document before data access.
- **UNREGISTERED** — decision was made before data access but is not explicitly covered in the pre-registration. Acceptable; must be documented here.
- **POST-HOC** — decision was made after any analytical access to the fire or LULC data. Must be disclosed in the manuscript methods section.

---

## Entries

---
**Date:** 2026-03-15
**Decision:** Define unit of analysis as individual 30m pixels (not aggregated spatial units such as hexagonal grids).
**Alternatives considered:** Hexagonal grid at 20 km² (used in related prior work); administrative units; regular grid at coarser resolution.
**Rationale:** Pixel-level analysis preserves the full temporal history of each location and avoids spatial aggregation decisions that would introduce additional degrees of freedom. The 40-year series at 30m resolution is sufficient to compute interval metrics at the pixel level.
**Data seen at time of decision:** no
**Status:** PRE-REGISTERED

---
**Date:** 2026-03-15
**Decision:** Restrict analysis to stable native vegetation — pixels classified as native vegetation in every year of MapBiomas LULC Collection 10 (1985–2024).
**Alternatives considered:** All native vegetation pixels regardless of stability; pixels native in at least 90% of years; separate analysis for stable vs. transitional pixels.
**Rationale:** Including pixels that transitioned to anthropogenic use would conflate fire regime of natural vegetation with fire used for land conversion. Stability ensures that interval structure reflects ecological fire dynamics, not land-use change events.
**Data seen at time of decision:** no
**Status:** PRE-REGISTERED

---
**Date:** 2026-03-15
**Decision:** Adopt the three-type interval typology: left-censored (1985 → first fire), uncensored (fire → next fire), right-censored (last fire → 2024), plus fully censored for pixels with frequency = 0.
**Alternatives considered:** Use only uncensored intervals (standard approach); treat censored intervals as missing data and exclude; model all intervals jointly with survival analysis (Weibull).
**Rationale:** Censored intervals carry real ecological information — the left-censored interval reflects the fire-free period before the series began, the right-censored interval reflects ongoing recovery or exclusion, and fully censored pixels represent the extreme case of fire exclusion. Excluding them would structurally bias the characterization toward high-frequency pixels. Survival analysis was considered but rejected in favor of direct metrics to keep the approach transparent and reproducible without distributional assumptions.
**Data seen at time of decision:** no
**Status:** PRE-REGISTERED

---
**Date:** 2026-03-15
**Decision:** Explore a threshold space of 4 minimum values (0, 1, 2, 3 years) × 4 maximum values (15, 20, 25, 30 years) = 16 combinations, rather than adopting fixed thresholds.
**Alternatives considered:** Single threshold pair (≤2 years / ≥20 years) based on literature; continuous sensitivity curve; expert elicitation for threshold priors.
**Rationale:** Precise fire interval thresholds for regime disruption in the Cerrado are not universally established. Treating thresholds as parameters and exploring the full plausible space ensures that conclusions are robust to this uncertainty. Including t_min = 0 covers the most permissive case (only consecutive-year burns are disruptive at the lower end).
**Data seen at time of decision:** no
**Status:** PRE-REGISTERED

---
**Date:** 2026-03-15
**Decision:** Use IBGE 2025 official boundary for the Cerrado biome extent.
**Alternatives considered:** IBGE 2018 boundary (used in prior work); MapBiomas biome layer.
**Rationale:** The 2025 boundary is the most current official delineation. Using it ensures consistency with future studies and policy instruments.
**Data seen at time of decision:** no
**Status:** PRE-REGISTERED

---
**Date:** 2026-03-15
**Decision:** Use MapBiomas Fire Collection 4 (1985–2024) for annual burned area and MapBiomas LULC Collection 10 (1985–2024) for the stable native vegetation mask.
**Alternatives considered:** MapBiomas Fire Collection 3 (used in prior work, ends 2023); MODIS burned area (coarser resolution, shorter series).
**Rationale:** Collection 4 extends the series to 2024, providing a full 40-year window. Collection 10 is the current LULC release and is temporally consistent with Collection 4.
**Data seen at time of decision:** no
**Status:** PRE-REGISTERED

---

---
**Date:** 2026-03-15
**Decision:** Apply a native vegetation class filter to `fire_{year}` rasters. A pixel is considered burned in a given year only if its value in `fire_{year}` corresponds to a native vegetation class in LULC Collection 10. Pixels burned under non-native classes are treated as unburned.
**Alternatives considered:** Use all valid (non-zero) values in `fire_{year}` regardless of class — simpler but would include fire events in pasture or cropland pixels that were misclassified or transitional in the LULC layer.
**Rationale:** The stable native vegetation mask (Filter 1) ensures the pixel was native throughout the series, but the `fire_{year}` class reflects the LULC state at the time of burning. Applying the native class filter ensures consistency: only fire events occurring on native vegetation are counted as burns in the interval series. This is coherent with the study's focus on fire regimes of native vegetation.
**Data seen at time of decision:** no
**Status:** UNREGISTERED — added to pre-registration document on same date

---
**Date:** 2026-03-15
**Decision:** Native vegetation classes for Filter 3 confirmed as: 3, 4, 5, 6, 11, 12, 29, 32, 49, 50 (MapBiomas LULC Collection 10).
**Alternatives considered:** Including class 1 (anthropic forest) — rejected as it does not represent native vegetation in Col 10.
**Rationale:** Classes confirmed against the official MapBiomas Collection 10 legend before any data access.
**Data seen at time of decision:** no
**Status:** UNREGISTERED — pre-data, same date as pre-registration

---
**Date:** 2026-03-15
**Decision:** All native vegetation classes are aggregated into a single category for the purpose of fire interval analysis. No distinction is made between forest (e.g., class 3), savanna (e.g., class 4, 12), and grassland/campo formations (e.g., class 12, 11) at this stage.
**Alternatives considered:** Stratify analysis by broad vegetation type (forest, savanna, grassland) from the outset, given known differences in fire resistance and resilience among these types.
**Rationale:** The primary objective of this study is to characterize fire interval structure relative to fire frequency across stable native vegetation as a whole. Introducing vegetation type stratification at this stage would increase complexity and is beyond the current research question. However, it is explicitly acknowledged that different native vegetation types in the Cerrado have distinct fire resistance and resilience characteristics — forest formations are generally more fire-sensitive, while savanna and grassland formations are fire-adapted. Separating analyses by broad vegetation type (forest, savanna, grassland) is a well-justified extension for future work and would require reclassifying the LULC classes used in the stable vegetation mask accordingly.
**Data seen at time of decision:** no
**Status:** UNREGISTERED — pre-data, same date as pre-registration

---
**Date:** 2026-03-16
**Decision:** Switch from full in-memory array to block processing with numpy memmap. Fire stack is written to disk incrementally in blocks of BLOCK_ROWS rows. Frequency is also computed block by block from the memmap.
**Alternatives considered:** Downsample raster to coarser resolution; process by tile; use Dask arrays.
**Rationale:** Full array allocation (40 × 82933 × 71227 uint8 = 220 GiB) caused MemoryError. Memmap writes directly to disk with no RAM limit. Block size is configurable (default 500 rows ≈ 1.4 GB per year band in RAM) and can be adjusted for available memory. No analytical change — output is identical to the original approach.
**Data seen at time of decision:** yes — MemoryError occurred after mask was loaded (shape and pixel count observed)
**Status:** POST-HOC — must be disclosed in manuscript methods

---
**Date:** 2026-03-16
**Decision:** First analytical access to fire data — notebook 01 completed successfully. Results recorded here for traceability. No analytical decisions made based on these results yet.
**Observations:**
- Stable native vegetation pixels: 1,040,061,856
- Never burned (freq=0, fully censored): 499,659,522 (48.0%)
- Burned at least once: 540,402,334 (52.0%)
- Mean frequency: 2.82
- Max frequency: 40 (46,237 pixels burned every year)
- Frequency distribution is strongly right-skewed — majority of pixels at low frequencies
**Data seen at time of decision:** yes — this entry records the first data access
**Status:** POST-HOC — first data observation

---
**Date:** 2026-03-16
**Decision:** Notebook 02 completed — interval metrics computed. Results recorded for traceability. No analytical decisions made based on these results yet.
**Observations:**
- Left-censored intervals (freq > 0): 540,402,334 — mean 10.58 yrs, median 6.0
- Right-censored intervals (freq > 0): 540,402,334 — mean 10.78 yrs, median 7.0
- Mean uncensored interval (freq >= 2): 403,409,399 pixels — mean 6.35 yrs, median 4.67
- CV uncensored intervals (freq >= 3): 320,026,773 pixels — mean 0.569, median 0.557
- MCBY > 0: 540,402,334 pixels — mean 1.54, max 40
- Fully censored (freq=0): 1,071,699,161 pixels
**Note:** Fully censored count (1,071,699,161) is larger than freq=0 from notebook 01 (499,659,522). Discrepancy requires investigation before proceeding — see next entry.
**Data seen at time of decision:** yes
**Status:** POST-HOC

---
**Date:** 2026-03-16
**Decision:** Flag discrepancy in fully_censored count for investigation. Notebook 01 reported 499,659,522 pixels with freq=0 within the stable native vegetation mask. Notebook 02 reports 1,071,699,161 fully censored pixels — roughly double. Likely cause: notebook 02 is applying fc = (freq_block == 0) to ALL pixels in the raster (including those outside the stable vegetation mask), not just pixels where frequency was set to 0 by the mask. The stable mask must be applied explicitly in notebook 02 before computing fully_censored.
**Data seen at time of decision:** yes
**Status:** POST-HOC — bug fix required before notebook 03

---
**Date:** 2026-03-16
**Decision:** Fix fully_censored.npy to apply stable native vegetation mask. Notebook 02 incorrectly marked all pixels with freq=0 as fully censored, including the 4,867,006,935 pixels outside the stable mask. Corrected in-place via notebook 02c: only pixels inside the mask with freq=0 are valid fully censored observations. All other outputs (interval_mean, interval_cv, interval_left, interval_right, interval_mcby, n_uncensored) verified correct by notebook 02b diagnostic — zero valid values outside the mask.
**Alternatives considered:** Rerun full notebook 02 with explicit mask application; accept the error and filter downstream.
**Rationale:** In-place fix is sufficient and avoids reprocessing ~2 hours of computation. The bug affected only fully_censored and had no downstream effect on float outputs because pixels outside the mask have freq=0, which caused fc=True and NaN assignment in all float arrays.
**Data seen at time of decision:** yes
**Status:** POST-HOC — bug identified after first data access

---
**Date:** 2026-03-16
**Decision:** In notebook 03, disruption classification will be tested under two interval scope approaches: (A) uncensored intervals only, and (B) uncensored + censored intervals combined. Both approaches will be computed and compared. This extends the pre-registration (§6, which specified uncensored only) to include a sensitivity test with censored intervals.
**Alternatives considered:** Uncensored only as pre-registered; censored only.
**Rationale:** The pre-registration declares uncensored intervals as the primary scope for threshold application (§6.3). Including censored intervals as a second approach tests whether the additional temporal information at the edges of the series changes the disruption classification for a meaningful proportion of pixels. This is a methodological sensitivity test, not a change to the primary analysis.
**Data seen at time of decision:** yes — notebooks 01 and 02 completed
**Status:** POST-HOC — extends pre-registration scope

---
**Date:** 2026-03-16
**Decision:** A pixel is classified as disruptive (by excess or exclusion) if ANY of its intervals falls outside the functional zone for a given threshold pair. A single short interval (≤ t_min) or a single long interval (≥ t_max) is sufficient to classify the pixel as disruptive.
**Alternatives considered:** Majority rule (>50% of intervals outside zone); proportion-based continuous score (P_short + P_long).
**Rationale:** The ecological rationale for this choice is that even a single very short interval can cause irreversible structural change in fire-sensitive vegetation, and a single very long interval can allow woody encroachment that alters fire behavior for subsequent years. A majority rule would mask these ecologically meaningful events. The proportion-based approach (P_short, P_long) remains available as a continuous metric from notebook 02 outputs and can be used in downstream analyses without changing the binary classification here.
**Data seen at time of decision:** yes
**Status:** POST-HOC — clarifies pre-registration §6.3


----
Date: 2026-03-16
Decision: Abandon the 16-combination (t_min × t_max pairs) classification approach in favor of two independent axes of disruption, each with its own metric and interval scope.
Alternatives considered: 16 paired threshold combinations as originally planned in notebook 03 draft; binary disruptive/functional classification per pair.
Rationale: The two disruption axes are ecologically independent. A pixel can show only short-interval disruption, only long-interval exclusion, or both — and these are not jointly determined by a single functional zone. Treating t_min and t_max as pairs conflates two separate ecological phenomena and forces a combined classification that obscures their independent occurrence. Separating the axes allows each to be analyzed and mapped on its own terms, with robustness evaluated independently per axis.
Data seen at time of decision: yes
Status: POST-HOC — replaces pre-registration §6 combined threshold approach

Date: 2026-03-16
Decision: Short disruption axis — metric is COUNT of uncensored intervals ≤ t_min (integer, 0 to n_uncensored). Four rasters, one per t_min value (0, 1, 2, 3). Censored intervals are excluded from this axis.
Rationale: Censored intervals are lower bounds on the true interval length — a left-censored interval of 2 years may in reality be longer. Using censored intervals to detect short-interval disruption would risk false positives. Uncensored intervals are fully observed and provide unambiguous evidence of short recurrence. Count (not binary) captures intensity: a pixel with 5 short intervals is ecologically different from one with 1.
Data seen at time of decision: yes
Status: POST-HOC

Date: 2026-03-16
Decision: Long disruption axis — metric is LENGTH (in years) of the longest fire-free interval per pixel, considering ALL interval types: uncensored, left-censored, right-censored, and fully censored. Single raster (long_max_interval.npy). The four t_max values (15, 20, 25, 30) are applied as reading thresholds on this raster, not as separate rasters.
Rationale: For detecting prolonged fire exclusion, any fire-free period is valid evidence regardless of its position in the series. Censored intervals are lower bounds on the true interval — including them is conservative (the true exclusion may be even longer). A single raster of maximum interval length is more informative than four binary rasters and allows flexible threshold application in downstream analysis. Fully censored pixels (freq=0) have a minimum interval of 40 years and are naturally captured.
Data seen at time of decision: yes
Status: POST-HOC
---
Date: 2026-03-16
Decision: Correct interval length calculation in Parts B and C of notebook 03. Previous version used raw gap (distance between fire index positions) as the interval length. Corrected to gap - 1 = actual fire-free years between consecutive burns. Left-censored, right-censored, and fully censored intervals are already expressed in fire-free years and require no correction. This fix enables t_min=0 to correctly detect consecutive burns (0 fire-free years between fires, gap=1) and aligns all axes to the same unit.
Alternatives considered: Redefine t_min values to absorb the offset (t_min=1 for consecutive burns) — rejected because it would make the threshold values unintuitive and inconsistent with the ecological literature where intervals are expressed in fire-free years.
Rationale: Bug identified by testing the sequence 0000011101100000011100000000010001001000 — t_min=0 detected no short intervals despite visible consecutive burns. Root cause: gap=1 for consecutive burns, and the criterion was gap <= 0 (never true). Correct criterion is free_years = gap - 1 <= t_min, i.e. gap <= t_min + 1.
Data seen at time of decision: yes
Status: POST-HOC — bug fix

------
Date: 2026-03-16
Decision: Notebook 03 completed and verified. Results recorded for traceability.
Observations:
Short disruption axis (% of all mask pixels):
t_min=0 (consecutive burns): 12.30% with >= 1 event, 7.61% with >= 2
t_min=1 (<=1 yr free):       23.59% with >= 1 event, 16.13% with >= 2
t_min=2 (<=2 yrs free):      27.81% with >= 1 event, 19.85% with >= 2
t_min=3 (<=3 yrs free):      30.17% with >= 1 event, 22.15% with >= 2
Long disruption axis (freq > 0 pixels only, n=540,402,334):
mean max interval: 19.55 yrs, median: 19.0 yrs

= 15 yrs: 63.16% — >= 20 yrs: 49.39% — >= 25 yrs: 33.51% — >= 30 yrs: 19.03%
Fully censored pixels (freq=0): all assigned long_max=40 yrs — correct by definition.
Distribution of long_max shows peaks at multiples of the series length (22, 25, 36, 39 yrs)
suggesting right-censored and left-censored intervals concentrating at those values.
Data seen at time of decision: yes
Status: POST-HOC — results observation

-----
Date: 2026-03-16
Decision: Document two analytical extensions beyond the core paper, to be developed in parallel or as subsequent work.
EXTENSION 1 — Protected areas and indigenous territories as analysis units (within paper scope as case studies)
All UCs and TIs of the Brazilian Cerrado above a minimum area threshold (exact threshold TBD — defined as minimum number of stable native vegetation pixels within the polygon, not total polygon area) will be used as analysis units. For each polygon, the internal composition of fire regime metrics will be characterized: distribution of interval metrics per pixel, proportion of pixels by disruption class, and internal heterogeneity. The goal is to assess whether polygons with similar total burned area or frequency show different internal regime compositions — and whether internal heterogeneity correlates with ecological or governance characteristics. Selected contrasting cases (5–10 polygons) will be presented as case studies in the paper.
EXTENSION 2 — Stable vegetation patches as ecological units (dashboard / future work)
The stable native vegetation raster will be segmented into contiguous patches using rook connectivity (4-neighbor, sides only — no diagonals). This is a conservative choice that requires genuine physical contiguity between pixels to define a patch. For each patch, the internal mosaico of fire regimes will be characterized — proportions of pixels by disruption class, internal heterogeneity of interval metrics, and spatial distribution of regime types within the patch. The ecological hypothesis motivating this analysis is that within-patch regime heterogeneity may reflect intermediate disturbance dynamics, where coexistence of different regime types within a patch supports higher biodiversity than homogeneous regimes. A minimum patch size threshold will be defined before analysis. This extension is more naturally suited to a dashboard or interactive tool than a conventional paper, as it requires exploration of individual patches in spatial context.
Data seen at time of decision: yes
Status: POST-HOC — conceptual extension, not in original pre-registration
---
**Date:** 2026-03-16
**Decision:** Compute integrated interval distributions combining uncensored, left-censored, right-censored, and fully censored intervals for each frequency class. Primary approach: descriptive — censored intervals included as observed values (lower bounds), explicitly flagged as such. Secondary approach (Kaplan-Meier survival estimator) deferred — to be decided after primary results are examined. Frequency classes 0–10 computed with full interval-level detail (histogram per class). Classes 11–40 computed with summary statistics only (mean, SD, quantiles per type). Rationale: censored intervals represent a substantial fraction of the total information — especially for freq=0 (fully censored, 48% of mask) and freq=1–3 where censored intervals outnumber uncensored — and excluding them produces a systematically incomplete picture of the temporal fire history.
**Data seen at time of decision:** yes
**Status:** POST-HOC

---
Date: 2026-03-16
Decision: Notebook 04 completed. Results recorded for traceability.
Observations:
Ratio of censored to uncensored intervals per frequency class:
freq=0 : inf (499,659,522 fully censored — no uncensored intervals)
freq=1 : inf (273,985,870 censored — no uncensored intervals)
freq=2 : 2.00 (2 censored per uncensored)
freq=3 : 1.00 (equal censored and uncensored)
freq=4 : 0.67 — freq=5: 0.50 — freq=10: 0.22
Key finding: for freq=0 and freq=1 (636M pixels, 61% of mask), frequency-based
analysis captures zero interval information. For freq=2-3 (141M pixels), censored
intervals outnumber or equal uncensored. Only from freq=4 onward do uncensored
intervals dominate. This quantifies the structural incompleteness of frequency
as a regime descriptor for the majority of stable native vegetation pixels.
Censored interval means are consistently longer than uncensored means within
each frequency class — e.g. freq=2: uncensored mean=9.7 yrs vs left=14.2,
right=14.1 yrs — confirming that censored intervals carry distinct and
complementary information about longer fire-free periods.
Data seen at time of decision: yes
Status: POST-HOC — results observation

-----
Date: 2026-03-19
Decision: Finalize operational research objective after reviewing results from notebooks 01–04 and existing literature (Arruda et al. 2024, Segura-Garcia et al. 2025, Ramos-Neto & Pivello 2000).
Operational objective:
"We quantify the temporal fire regime information concealed by fire frequency in stable native vegetation of the Brazilian Cerrado over a 40-year period (1985–2024), by characterizing the complete interval typology — uncensored, censored, and fully censored intervals — across all frequency classes and demonstrating that censored intervals are both structurally unavoidable and ecologically distinct from those captured by frequency alone."
Scope decisions confirmed:

Unit of analysis: pixel (30m)
No spatial drivers or causal attribution
No temporal trends
Restriction to stable native vegetation is a methodological choice with direct ecological interpretation consequences — documented as a strength, not merely a filter
Paper scope: Pergunta A only (methodological with ecological implication)
Pergunta B (spatial characterization of disruption) deferred to future work

Relationship to pre-registration: the pre-registration stated the central question in broader terms. This operational objective refines and narrows that question based on the analytical results obtained. It does not contradict the pre-registration but specifies it. This refinement will be noted explicitly in the manuscript methods section.
Data seen at time of decision: yes — all notebooks completed
Status: POST-HOC — objective refined after data analysis

-----
Date: 2026-03-19
Decision: Define narrative structure and result presentation conventions for Paper A.

UNIT OF PRESENTATION: Pixel counts will be converted to area (km² or Mha) using the 30m pixel size (1 pixel = 900 m² = 0.0009 km²). No recalculation needed — direct multiplication. Key values:

Total stable native vegetation mask: ~936,056 km² (93.6 Mha)
freq=0: ~449,694 km² (45.0 Mha)
freq=1: ~123,294 km² (12.3 Mha)
freq=0+1 combined: ~572,987 km² (57.3 Mha, 61.2% of mask)


NARRATIVE DISTINCTION between freq=0 and freq=1:

freq=0 exclusion from conventional analyses is an analytically justifiable choice (no fire event = no frequency signal) but ecologically consequential — it treats the most extreme fire exclusion regime as absence of data. Correctable by explicit methodological inclusion. Presented as important ecological context.
freq=1 is the deeper and more original argument: even when included in frequency analysis, these pixels (136M pixels, 12.3 Mha, 13.2% of mask) have zero uncensored intervals — the information content of their fire regime is structurally zero under frequency-based approaches. Two freq=1 pixels with entirely opposite fire histories (early vs. late single burn) are indistinguishable. Not correctable by including freq=0 — requires changing the unit of analysis to intervals. This is the central methodological contribution.


RESULT SELECTION confirmed: only results responding to the three objective components (structural, quantitative, ecological) enter Paper A. Spatial patterns, disruption metrics (short_tmin, long_max), MCBY by class, and ecoregion comparisons deferred to Paper B or future work.
Data seen at time of decision: yes
Status: POST-HOC — narrative and presentation decisions after data analysis
-----
Date: 2026-03-20
Decision: Conceptual figure (Fig 1) finalized — v3 accepted.
Structure: 6 rows (freq=0,1,2,4,6,8) × 2 columns (schematic timeline + gamma distributions).
Key design decisions:

freq=0: single FC bar + no-data text box (zero interval information)
freq=1: single generic trajectory (fire at t=20, off-centre intentional); no pixel A/B labelling
freq=2,6,8: L and R distributions combined as "L & R" annotation when medians are equal
freq=4: L, U, R annotated separately (medians 7, 4, 6 — all distinct)
Fire event marker (▼) added to legend
Times New Roman throughout; 170mm width; 300 dpi
Eixo y sem valores numéricos (relative frequency only)
Summary box at bottom restates key pattern for reader

Output files:
D:\Projetos\Fire_Interval\figures\fig_conceptual_v3.pdf
D:\Projetos\Fire_Interval\figures\fig_conceptual_v3.png
Script: 06_conceptual_figure_v3.py
Status: POST-HOC — figure design iterated after viewing data

-----

Date: 2026-03-23
Decision: Add relative error analysis as Section 3.3 of Paper A results.
MOTIVATION:
Fire frequency implicitly encodes a mean interval estimate as T/n (T = observation
period, n = frequency). This estimate assumes evenly spaced fire events — an
assumption our data demonstrate to be systematically violated. Quantifying the
relative error of T/n against observed mean uncensored intervals constitutes a
direct, empirical measure of how much information frequency conceals about
interval structure.
This analysis was motivated by the forthcoming MapBiomas Fire product that will
include a mean interval layer calculated as T/n. The paper does not cite this
product explicitly — the result is framed as a general information-theoretic
assessment of frequency-derived interval estimates, applicable to any
frequency-based fire regime characterization.
ANALYTICAL DECISION — POST-HOC:
Relative error defined as:
eps_n = |T/n - I_obs| / I_obs
where I_obs = observed mean uncensored interval for frequency class n,
derived from part_b_summary.csv (interval_type == 'uncensored').
KEY RESULTS FROM INITIAL RUN (freq=2–10, from part_b_summary.csv):
freq=2:  T/n=20.0 yrs, I_obs=9.7 yrs,  error=106%
freq=5:  T/n=8.0 yrs,  I_obs=5.04 yrs, error=59%
freq=8:  T/n=5.0 yrs,  I_obs=3.28 yrs, error=52% (minimum observed)
freq=10: T/n=4.0 yrs,  I_obs=2.60 yrs, error=54%
ARTEFACT IDENTIFIED — POST-HOC:
Initial notebook 07 computed I_obs for freq=11–40 from interval_mean.npy,
which stores mean of ALL interval types (censored + uncensored) per pixel.
This produced a spurious discontinuity at freq=10→11 (error dropping from
54% to 9%). This source is INCORRECT for this analysis. Discarded.
PATTERN OF INTEREST — POST-HOC:
Relative error decreases from freq=2 to freq=8 (106% → 52%) but then
slightly increases from freq=8 to freq=10 (52% → 54%). This non-monotonic
pattern is ecologically interpretable: high-frequency pixels are not a
random sample of native vegetation — they represent sites where fires cluster
temporally, making T/n a worse estimator despite higher n. This observation
motivates extending the analysis beyond freq=10 to confirm whether the
inflection is real or a boundary effect.
EXTENSION DECISION — POST-HOC:
Extend DETAIL_FREQS in notebook 02 from range(0,11) to range(0,16) to
obtain correct mean uncensored intervals for freq=11–15 from the same
source as freq=2–10 (part_b_summary.csv). This extension was motivated
by observing the inflection in the error curve at freq=8–10.
NOTEBOOK CHANGES REQUIRED:
notebook 02: DETAIL_FREQS = list(range(0, 16))
SUMMARY_FREQS = list(range(16, 41))
notebook 07: change freq filter from <= 10 to <= 15
change high-freq fallback from range(11,41) to range(16,41)
FILES TO BACKUP BEFORE RERUN:
classification/part_b_summary.csv
classification/part_b_interval_distributions.csv
Data seen at time of decision: yes — fig_relative_error.pdf and
relative_error_by_freq.csv reviewed before this entry
Status: POST-HOC — all decisions made after seeing initial results


-----

Date: 2026-03-23
Decision: Diagnostic results from verify_part_b.py — findings and implications.
DIAGNOSTIC SUMMARY:

part_b_summary.csv: 164 rows, freq=0–40, all 4 interval types present per class
part_b_interval_distributions.csv: 1723 rows, freq=0–19 (detail classes only)
No gaps in frequency coverage
No discontinuity at freq=10→11 (10.3%) or freq=20→21 (9.4%) — artefact
from interval_mean.npy confirmed absent when using part_b_summary.csv
44 NaN values in summary = fully_censored rows for freq>0 — expected

MONOTONICITY CONFIRMED:
Mean uncensored interval decreases monotonically from freq=2 (9.70 yrs)
to freq=40 (0.00 yrs). The apparent inflection at freq=8–10 observed in
the initial relative error curve was a boundary artefact caused by mixing
data sources (part_b_summary for freq<=10, interval_mean.npy for freq>10).
With consistent data source, no inflection exists.
RIGHT-CENSORED < UNCENSORED FOR freq=15–26 — STRUCTURAL EXPLANATION:
Check 7 shows right_censored mean falls below uncensored mean starting at
freq=15. This is a structural/geometric consequence of high fire frequency:
with many events distributed across a 40-year series, the last event tends
statistically to occur close to 2024, producing a short right-censored
interval. This is the geometric inverse of the freq=1 case (where the single
event tends to be far from both series boundaries, producing long censored
intervals). This does NOT invalidate the paper's argument — the key
asymmetry is that LEFT-censored remains systematically longer than
uncensored across all frequency classes examined.
ECOLOGICAL HYPOTHESIS — NOT TESTABLE WITH CURRENT DATA:
The short right-censored intervals at high frequency classes could
additionally reflect a concentration of recent fire events (post-2010)
in high-frequency pixels, consistent with the shift in fire season
documented by Arruda et al. (2024). However, separating this ecological
signal from the structural geometric effect requires analysis of the
temporal distribution of events within high-frequency pixels — specifically,
testing whether the last event is closer to 2024 than expected by chance.
This requires the fire stack at pixel level and is deferred to future work.
Registered here as a direction for Paper B or a focused methodological note.
NOTEBOOK 07 UPDATE REQUIRED:
Use part_b_summary.csv for freq=2–20 entirely.
Remove dependency on interval_mean.npy.
Filter: freq_class >= 2 AND freq_class <= 20 AND interval_type == 'uncensored'
High-freq fallback (interval_mean.npy) no longer needed for this analysis.
Data seen at time of decision: yes — verify_part_b.py output reviewed
Status: POST-HOC
-----
*[New entries go below this line]*
