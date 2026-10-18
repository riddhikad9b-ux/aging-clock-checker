# Aging-clock checker: results note (2026-10-17)

Run by the user in Colab with `pyaging` (Python 3.13, pandas 3.0.6). Verified: `aging_clock_checker_v0.3.ipynb` was run top to bottom in a fresh Colab session on 2026-10-18 with no errors, and its summary reproduced every number below. Analysis code was also tested on simulated data.
Chronological-age error is reported only for clocks trained to predict age. Mortality and pace clocks are shown by correlation and offset.

## Datasets
- GSE139307: 37 sperm samples (pyaging example data).
- GSE42861: 100 randomly chosen healthy controls ('Normal') of 335, peripheral blood leukocytes, Illumina 450k, ages 20-70, mean 51 (random seed 0).
- GSE41169: 95 whole-blood samples, Illumina 450k (33 controls, 62 schizophrenia patients), ages 18-65, mean 31.6.

## Age-scale clocks (MAE in years; correlation with age)
| Clock | Sperm n=37 | GSE41169 controls n=33 | GSE41169 all n=95 | GSE42861 controls n=100 |
|---|---|---|---|---|
| Horvath2013 | 45.9; r 0.27 | 3.00; r 0.94 | 2.95; r 0.93 | 4.31; r 0.91 (bias +1.90) |
| AltumAge | 51.4; r 0.21 | 1.29; r 0.99 | 2.59; r 0.95 | 4.74; r 0.95 (bias +0.62) |
Horvath2013 vs AltumAge mean absolute difference: 6.85 y (sperm), 3.19 y (GSE41169 controls), 3.4 y (GSE41169 all), 4.16 y (GSE42861 controls).

## Other clocks (not errors)
- PCGrimAge offset from chronological age: sperm +14.48 y (r 0.99; not meaningful, wrong tissue); GSE41169 controls +17.25 y (r 0.96); all +18.63 y (r 0.92).
- GrimAge2 offset: sperm -8.86 y (r 0.87; not meaningful, wrong tissue); GSE41169 controls +11.90 y (r 0.94); all +14.59 y (r 0.85).
- Second blood dataset GSE42861 (n=100): PCGrimAge offset +13.6 y mean (+18.1 at age 30, +11.7 at 60), r 0.94; GrimAge2 offset +10.0 y mean (+14.6 at 30, +8.1 at 60), r 0.90. Both GrimAge-type offsets are positive in both blood datasets, so the offset is probably not specific to one dataset; cause still unconfirmed (clock port or calibration are candidates).
- DunedinPACE (pace): r with age 0.58 (GSE41169 controls), 0.50 (GSE41169 all), 0.29 (GSE42861 controls), 0.04 (sperm).
GrimAge diagnostics (blood, n=95): inputs verified (no missing features; stored age equals real age). Adding 10 y to the age input shifts PCGrimAge by about 6.6 y and GrimAge2 by about 5.9 y. The offset is nearly constant across age (PCGrimAge +19.5 y at age 20, +16.5 y at 60; GrimAge2 +15.5 y at 20, +12.4 y at 60). Cause unresolved; candidates (untested): normalisation of this dataset, port calibration, study batch effect. An independent blood dataset would separate these.

## What the data support
- Tissue mismatch is the largest effect: errors of about 46-51 y in sperm versus about 1-3 y in blood for the same clocks.
- Clocks can agree with each other (6.9 y apart on sperm) and still both be far from truth.

## Limits
- AltumAge error rose from 1.29 y (GSE41169 controls) to 4.74 y (GSE42861 controls); this is consistent with the first number being flattered by training overlap, but overlap was not checked.
- GSE41169's contact is Steve Horvath: possible overlap with clock training data, not checked. The low AltumAge error (1.29 y) may also be flattered by overlap.
- Narrow, young age range; small groups; one study per tissue.
- Controls vs patients difference (AltumAge 1.29 vs 2.59 y) could be disease effect or noise; not tested.
- GrimAge/DunedinPACE labelled "research use only" by pyaging. Nothing here is medical advice.
