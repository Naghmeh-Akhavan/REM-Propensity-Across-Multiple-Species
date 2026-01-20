# REM Architecture Analysis (Human, Mouse, and Rat)

This repository contains MATLAB scripts used to analyze REM–NREM cycling structure across **mouse**, **rat**, and **human** sleep recordings, with a focus on:

- **Long-wake filtering** (cycle DROP rule)
- **GMM-based classification** of REM cycles as **Single** vs **Sequential**
- **REM propensity analysis** using a function from the NREM distribution
- **Increasing-zone analysis** (global min → first local max in propensity curve)
- **Distribution of REM bouts/time across the normalized sleep episode** (deciles)
- Generation of manuscript figures and CSV summary tables

All scripts are designed to be **toolbox-free** for mixture fitting (custom EM), except where noted.


## 1) Data Requirements

### Required input files / tables

#### Human (combined MAT tables)
Place these in your `combined_out/` folder (or update paths in scripts):
- `Combined_BigResultTable.mat`  
  Contains cycle-level summary variables (e.g., `NREM`, `REMpre`, `IREM`, `REMpost`, identifiers).
- `Combined_BigStageDetailsTable.mat`  
  Contains stage-block details per cycle with at least:
  - `REM_Cycle_Index`
  - `Stage`
  - `Duration`
  - (optional) `ord` ordering field

#### Mouse/Rat (base CSV tables)
Each dataset should be a cycle-level CSV containing (at minimum):
- `Subject_Index`, `REM_Cycle_Index`
- `NREM`, `REMpre`, `IREM`, `REMpost`
- long-wake support columns (one of):
  - `MaxWakeSec`, `MaxWakeBlockSec`, `MaxWakeInIREMSec`, etc. (preferred)

Examples (update paths as needed):
- `BigResultTable_mouse_light.csv`
- `BigResultTable_mouse_dark.csv`
- `BigResultTable_rat_light.csv`
- `BigResultTable_rat_dark.csv`

---

## 2) Key Definitions

### Cycle timing
- `IREM` = inter-REM interval (NREM + Wake + Movement), in seconds  
- `REMpre` = REM bout duration (used as REM duration for many plots), seconds  
- `REMpost` = REM bout duration used in several propensity-related plots, seconds  

### Long-wake DROP rule (cycle exclusion)
A REM cycle is **dropped** if it contains a **single continuous wake block** of duration ≥ `WAKE_BREAK_MIN` minutes.

- Human: computed from `StageDetails` table using `Stage == 5` blocks  
- Mouse/Rat: uses best available “max wake block” column; if not present, falls back to total `Wake`

### Single vs Sequential classification (GMM on NREM)
- Convert NREM to minutes: `NREM_min = NREM/60`
- Fit **2-component GMM** to `z = log(NREM_min)`
- “Small-NREM component” is the Gaussian with smaller mean `mu`
- A cycle is labeled:
  - **Sequential** if `P(small | z) ≥ 0.5`
  - **Single** otherwise

### Propensity function (hazard-style)
Given a fitted CDF `F(t)` of NREM duration,
\[
p(t,\Delta)=\frac{F(t+\Delta)-F(t)}{1-F(t)}.
\]
Default: `Δ = 30 s`.

### Increasing zone
For each REMpre bin (or fixed per dataset):
- compute propensity curve `p(t)`
- define increasing zone as:
  - from **global minimum** of `p(t)` to the **first local maximum after that minimum**

Some scripts use **precomputed increasing-zone windows** for reproducibility.

---

## 3) How to Run

1. Open MATLAB
2. Set your working directory to the repo root
3. Update the `combinedFolder` / CSV paths at the top of scripts
4. Run scripts directly (each script is self-contained)



## Author

**Naghmeh Akhavan** - January 2026

---


*For questions or issues, please refer to the comments within individual MATLAB files or contact the author Naghmeh Akhavan (nakhavan@umich.edu).*

