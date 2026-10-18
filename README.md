# Aging-clock checker

**Author:** Riddhika D · riddhika.d9b@gmail.com · GitHub: [@riddhikad9b-ux](https://github.com/riddhikad9b-ux)

A small Colab notebook that runs several published epigenetic aging clocks (through the open-source `pyaging` package) on public DNA-methylation datasets from NCBI GEO, then shows how far each clock is from chronological age and how much the clocks disagree with each other.

**Status: v0.3, verified by a full fresh run (2026-10-18).** A demonstration on three datasets, not a benchmark. Read the limits below before quoting any number.

## What it does
- Loads public GEO datasets (Illumina 450k) and chronological ages.
- Runs Horvath2013, AltumAge, GrimAge2, PCGrimAge and DunedinPACE via `pyaging`.
- Reports **error in years only for clocks trained to predict age** (Horvath2013, AltumAge). GrimAge-type clocks (trained on mortality risk) and DunedinPACE (a pace of aging) are shown by correlation and offset, not as errors.
- Reports how far apart the age-scale clocks are on the same samples.

## Main findings so far (author's Colab runs, 2026-10-17)
| Clock | Sperm, n=37 | GSE41169 controls, n=33 | GSE42861 controls, n=100 |
|---|---|---|---|
| Horvath2013, mean abs error (years) | 45.9 | 3.00 | 4.31 |
| AltumAge, mean abs error (years) | 51.4 | 1.29 | 4.74 |

- **Tissue matters most.** The same clocks that are within about 1-5 years in blood are off by about 46-51 years in sperm.
- **Clocks can agree with each other and both be wrong** (Horvath2013 vs AltumAge differ by 6.9 years on sperm while both are 46-51 years off).
- **GrimAge-type clocks sit above chronological age** in both blood datasets (GrimAge2 about +10 to +12 years, PCGrimAge about +14 to +17 years). Inputs were verified present, the offset is nearly constant across ages, and the cause is **unresolved**.

Full table and details: [RESULTS.md](RESULTS.md).

## Limits (please read)
- Three small, single-study datasets (sperm n=37; blood n=95 and a random 100 controls). Narrow age ranges.
- Training overlap was **not checked**. One dataset's contact is the author of the Horvath clock, and AltumAge's error rose from 1.29 to 4.74 years between the two blood datasets, which is consistent with an optimistic first number but is not proof.
- GrimAge offsets are unexplained. Several clocks are marked research-use-only and not yet author-approved in `pyaging`.
- Controls and patients are reported separately where relevant; a difference between them was not tested.
- v0.3 was run top to bottom in a fresh Colab session and reproduced the numbers in RESULTS.md. Environment: Python 3.13, pandas 3.0.6 (`pip` upgraded pandas for `pyaging`; Colab warns this conflicts with its pinned 2.2.3 but the run worked). The last notebook cell prints package versions.
- Nothing here is medical advice or a measure of anyone's real health.

## How to run
1. Open `aging_clock_checker_v0.3.ipynb` in Google Colab (File → Upload notebook).
2. Runtime → Disconnect and delete runtime, then Run all. Takes roughly 15-25 minutes (two large GEO downloads).
3. If Colab restarts from lack of memory, run from the top again; each dataset cell frees its table.

## Files
- `aging_clock_checker_v0.3.ipynb`: the notebook (contains the analysis module).
- `RESULTS.md`: results table, GrimAge diagnostics, limits.

## Data and software credits
Datasets: GEO GSE139307 (via pyaging example data), GSE41169, GSE42861. Clocks and package: `pyaging` and the clock papers cited in each clock's metadata (`adata.uns['<clock>_metadata']`).
