# Pre-registration: Fire Frequency and Interval Structure in the Brazilian Cerrado

**Status:** Registered  
**Version:** 0.1  
**Date:** 2026-03-15  
**Authors:** [to be completed before submission]  
**OSF project:** https://osf.io/swn2v  
**OSF DOI:** https://doi.org/10.17605/OSF.IO/THJFM  
**Zenodo archive:** https://doi.org/10.5281/zenodo.19051060  

---

## 1. Title

**What does fire frequency reveal — and conceal — about fire regimes in stable native vegetation of the Brazilian Cerrado over 40 years?**

---

## 2. Research questions

### 2.1 Central question

Does fire frequency reliably indicate functional fire regimes in stable native vegetation of the Brazilian Cerrado, or do interval-level patterns reveal disruptions — by excess, exclusion, or both — that frequency conceals?

### 2.2 Sub-questions

**Q1 — Sufficiency:** Under what conditions is fire frequency sufficient to characterize a fire regime? That is, when do pixels sharing similar frequency also share similar interval structure?

**Q2 — Concealment:** What additional temporal structure — interval length, regularity, and consecutiveness — is necessary to distinguish regimes that share similar frequency?

---

## 3. Background and rationale

Fire frequency (total number of fire events per pixel over the study period) is the most widely used metric to characterize fire regimes in the Cerrado. However, frequency is an aggregate measure that collapses temporal information: two pixels with identical frequency may have radically different interval structures — one with regular, evenly spaced burns and another with consecutive fires followed by prolonged exclusion.

This study argues that interval structure carries ecologically relevant information beyond what frequency captures, particularly in relation to known functional thresholds for the Cerrado ecosystem. Very short intervals (≤ ~2–5 years) and very long intervals (≥ ~15–30 years) are associated with regime disruption — the former with degradation of vegetation structure and nutrient dynamics, the latter with woody encroachment and loss of fire-adapted species. Because these thresholds are not precisely established in the literature, the analysis will treat them as parameters explored across a plausible range rather than as fixed values (see Section 6).

A key contribution of this study is the explicit inclusion of fire-free pixels (frequency = 0) as **fully censored intervals**, rather than excluding them from analysis. These pixels represent the extreme case of fire exclusion and are invisible to frequency-based characterizations.

---

## 4. Study design

### 4.1 Unit of analysis

Individual pixels at 30-meter spatial resolution.

### 4.2 Spatial scope

Brazilian Cerrado biome (official IBGE 2025 boundary).

### 4.3 Temporal scope

1985–2024 (40-year series).

### 4.4 Inclusion criteria

Pixels classified as **stable native vegetation**: areas mapped as native vegetation (any class) in every year of the MapBiomas Land Use and Land Cover series (Collection 10) throughout the full study period. This excludes:

- Areas that transitioned to anthropogenic land use at any point
- Areas that underwent regeneration after prior land use (secondary vegetation)
- Transitions between native vegetation types are retained (e.g., savanna to forest)

### 4.5 Pixel inclusion and burned-year logic

A binary burned/unburned time series is constructed for each pixel through three sequential filters:

**Filter 1 — Stable native vegetation mask**
The persistent native vegetation raster (derived from MapBiomas LULC Collection 10) contains value `1` for pixels classified as native vegetation in every year of the series, and `nodata` (0) otherwise. Only pixels with value `1` are retained for analysis.

**Filter 2 — Fire detection**
For each year, the `fire_{year}` raster contains the LULC Collection 10 class of the burned area, and `nodata` (0) for unburned pixels. A pixel is considered burned in a given year if it has a valid (non-zero, non-nodata) value in `fire_{year}`.

**Filter 3 — Native vegetation class**
An additional filter is applied to `fire_{year}`: only pixels whose burned class corresponds to a native vegetation class in LULC Collection 10 are considered burned. Pixels burned under non-native classes (e.g., pasture, cropland) are treated as unburned for interval calculation purposes.

The result for each included pixel is a 40-year binary vector (1 = burned, 0 = unburned) from which all interval types are derived.

### 4.6 Data sources

| Dataset | Version | Use |
|---|---|---|
| MapBiomas Fire | Collection 4 | Annual burned area, 1985–2024 |
| MapBiomas LULC | Collection 10 | Stable native vegetation mask |

---

## 5. Interval typology

For each pixel, fire intervals are classified into three types based on their observability within the 40-year series. For a pixel with frequency **n**:

| Type | Count | Definition |
|---|---|---|
| Left-censored | 1 (if n > 0) | From 1985 to the first fire event. The true interval began before the series. |
| Uncensored | n − 1 (if n > 1) | Between consecutive fire events. Fully observed. |
| Right-censored | 1 (if n > 0) | From the last fire event to 2024. The true interval extends beyond the series. |
| Fully censored | 1 (if n = 0) | The pixel did not burn in 40 years. The interval is at least 40 years. |

**Total intervals per pixel** = n + 1 for n > 0; = 1 for n = 0.

Fire frequency captures only the n − 1 uncensored intervals and is blind to the 2 censored intervals per pixel and to all fully censored pixels.

---

## 6. Ecological thresholds

The Cerrado literature suggests that very short and very long fire-free intervals are associated with regime disruption. However, precise thresholds are not universally established. To avoid adopting a single arbitrary standard, this study will explore a range of ecologically plausible threshold combinations.

### 6.1 Threshold space

| Parameter | Values to test |
|---|---|
| Minimum interval (short disruption) | 0, 1, 2, 3 years |
| Maximum interval (long disruption) | 15, 20, 25, 30 years |

All 16 combinations of minimum × maximum thresholds will be evaluated (4 minimum values × 4 maximum values). Results will be reported across the full threshold space, not for a single combination.

### 6.2 Functional zone

For a given threshold pair (t_min, t_max), an interval is classified as:

- **Short-disruptive:** length ≤ t_min
- **Functional:** t_min < length < t_max
- **Long-disruptive:** length ≥ t_max

### 6.3 Pixel-level disruption classification

For each threshold combination, each pixel will be assigned a disruption profile based on the proportion of its uncensored intervals falling in each category, and the character of its censored intervals. A pixel will be classified as:

- **Robustly functional:** within the functional zone across all threshold combinations
- **Robustly disruptive:** outside the functional zone across all threshold combinations
- **Threshold-sensitive:** classification changes depending on the threshold pair adopted

---

## 7. Metrics

All metrics are computed at the pixel level from uncensored intervals, unless otherwise noted. Censored intervals are used in a supplementary characterization layer.

### 7.1 Primary metrics (uncensored intervals only)

| Metric | Symbol | Description |
|---|---|---|
| Mean interval length | MIL | Arithmetic mean of uncensored intervals (years) |
| Coefficient of variation | CV | Standard deviation / mean of uncensored intervals. Captures temporal regularity. |
| Maximum consecutive burned years | MCBY | Longest run of consecutive years with fire. Captures burst disruption. |
| Proportion short-disruptive | P_short | Fraction of uncensored intervals ≤ t_min |
| Proportion long-disruptive | P_long | Fraction of uncensored intervals ≥ t_max |

### 7.2 Censored interval characterization (supplementary)

| Metric | Description |
|---|---|
| Left-censored interval length | Years from 1985 to first fire |
| Right-censored interval length | Years from last fire to 2024 |
| Fully censored flag | Binary: pixel never burned (frequency = 0) |

---

## 8. Analytical approach

### 8.1 Core analysis

1. Compute all metrics (Section 7) for every pixel meeting inclusion criteria.
2. For each of the 16 threshold combinations, classify pixels by disruption profile (Section 6.3).
3. Identify the subset of pixels where frequency alone would misclassify the regime — i.e., pixels with similar frequency but different disruption profiles.
4. Characterize the spatial distribution of robustly functional, robustly disruptive, and threshold-sensitive pixels across the Cerrado.

### 8.2 Frequency sufficiency test

For each frequency class (n = 1, 2, 3, ...), assess the internal heterogeneity of interval structure metrics. High heterogeneity within a frequency class indicates that frequency is insufficient to characterize the regime for that class.

### 8.3 Sensitivity analysis

Report the proportion of pixels that change disruption classification as thresholds vary. This constitutes the formal robustness assessment.

---

## 9. What this study does not do

To be explicit about scope:

- This study **does not** test ecological consequences of regime types (vegetation response, biodiversity effects). That is a separate and subsequent question.
- This study **does not** model fire probability or use survival analysis (e.g., Weibull distributions).
- This study **does not** aggregate to landscape units (hexagonal grids or similar). The unit is the pixel.
- This study **does not** assess drivers of fire regime (climate, land tenure, land use). Spatial patterns may be described but causal attribution is not claimed.

---

## 10. Deviations protocol

Any analytical decision made after first access to the fire data will be recorded in `protocol/decision-log.md` with:

- Date
- Description of the decision
- Alternatives considered
- Rationale
- Flag: **POST-HOC** (data had been seen before the decision was made)

Post-hoc decisions are not prohibited, but they must be disclosed. The pre-registration text will not be altered retroactively. Deviations will be reported transparently in the methods section of the manuscript.

---

## 11. Repository and archival plan

| Platform | Purpose | Action |
|---|---|---|
| GitHub | Code, notebooks, decision log | Continuous commits; one commit per analytical decision |
| OSF | Pre-registration | Submit this document before data access; DOI generated |
| Zenodo | Archival snapshots | Release v0.1 (protocol), v1.0 (submission), v1.x (revisions) |

GitHub repository: https://github.com/barroso2501/cerrado-fire-interval

---

## 12. Pre-registration checklist

Before submitting to OSF, confirm:

- [ ] Research questions finalized (Section 2)
- [ ] Threshold space fully declared (Section 6.1)
- [ ] All metrics defined (Section 7)
- [ ] Deviations protocol understood by all authors (Section 10)
- [ ] GitHub repository created and linked
- [ ] Fire data **not yet accessed** for analysis

---

*This document was drafted on 2026-03-15 prior to any analytical access to MapBiomas Fire Collection 4 data.*
