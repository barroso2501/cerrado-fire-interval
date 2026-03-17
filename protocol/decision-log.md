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


*[New entries go below this line]*
*[New entries go below this line]*
