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
Date: 2026-03-16
Decision: Switch from full in-memory array to block processing with numpy memmap. Fire stack is written to disk incrementally in blocks of BLOCK_ROWS rows. Frequency is also computed block by block from the memmap.
Alternatives considered: Downsample raster to coarser resolution; process by tile; use Dask arrays.
Rationale: Full array allocation (40 × 82933 × 71227 uint8 = 220 GiB) caused MemoryError. Memmap writes directly to disk with no RAM limit. Block size is configurable (default 500 rows ≈ 1.4 GB per year band in RAM) and can be adjusted for available memory. No analytical change — output is identical to the original approach.
Data seen at time of decision: yes — MemoryError occurred after mask was loaded (shape and pixel count observed)
Status: POST-HOC — must be disclosed in manuscript methods


*[New entries go below this line]*
