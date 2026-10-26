# Used datasets

One public download of the tables that were actually read. Each study keeps its own folder. An institutional table is stored once for each distinct version. The same file is not copied into every study that read it.

Institutional row keys are hashes. These files do not contain names, student numbers, class, or college. UKZN source keys were replaced with SHA-256. OULAD keys are the public student keys from Kuzilek, Hlosta, and Zdrahal, Scientific Data 2017 (CC BY 4.0). Where offerings are pooled, the offering code follows `::`.

Missing scores are left missing.

## Where each study reads

| Study | Folder | Version |
|---|---|---|
| Machine-learning early warning | `ml-early-warning/private` | Institutional effect tables, T=2–5, 2020–2025. Training years 2020–2023 have had columns with no finite value removed. Widths are 116, 232, 389, 581. |
| Same study | `ml-early-warning/private-raw` | Raw-column version used beside the effect tables. |
| Same study | `ml-early-warning/oulad` | Pooled OULAD, presentations 2013B, 2013J, 2014B, 2014J. Widths are 115, 231, 376, 548. |
| Same study | `ml-early-warning/ukzn` | UKZN effect tables. Identifiers are SHA-256. |
| LSTM recall experiments | `ml-early-warning/private` | Same effect version. Not copied again. |
| Tabular foundation model | `ml-early-warning/private` and `tabpfn-admit/oulad-align-t2` | Same institutional effect version, plus the OULAD T=2 offerings used there. |
| Landing search | `landing/advanced` and `landing/advanced_raw` | Two later versions, T=2 and T=3 only. |
| Knowledge tracing, before the effect tables | `kt-eventstream` | Hashed `SID` plus knowledge-state summaries. This is an earlier version, not a copy of `ml-early-warning/private`. |
| Cross-table decisions | `decision-transfer/` | Separate sources below. |

## Decision-transfer sources

| Folder | What was read |
|---|---|
| `oulad` | Open University tables. `studentVle.csv` is split into `studentVle.csv.part00` and the following parts, in order. Join them to recover the original file. |
| `physionet2012` | PhysioNet/CinC 2012 measurements, split into `measurements.csv.part00` and `measurements.csv.part01`. https://physionet.org/content/challenge-2012/1.0.0/ |
| `cmapss` | NASA C-MAPSS turbofan runs FD001–FD004. https://data.nasa.gov/dataset/cmapss-jet-engine-simulated-data |
| `ncmapss` | N-CMAPSS unit table and column list that were read. The original multi-gigabyte archive is the public NASA file; it is not copied again. |
| `backblaze` | Backblaze 2023 drive table that was read. `drives.csv` is split into `drives.csv.part00` and the following parts, in order. |
| `uci697` | UCI 697, Predict Students' Dropout and Academic Success. https://doi.org/10.24432/C5MC89 |
| `sepsis2019` | PhysioNet/CinC 2019 hourly table that was read. https://physionet.org/content/challenge-2019/1.0.0/ |
| `private` | Institutional fields those runs actually read: hashed `SID`, year, grade, and process scores or completion times. |

The deep-learning runs score seven sources, then two further public tables that were turned into the same matrices. The seven are PhysioNet 2012, OULAD with withdrawal as the positive class, C-MAPSS, OULAD with failure as the positive class, UCI 697, the 2019 sepsis table, and the institutional process table. Withdrawal and failure are two labels on the OULAD files already in `oulad/`. N-CMAPSS and Backblaze are the two further tables.

`grade_label` is 1 for fail and 0 for pass. Withdrawn students are not in the institutional or OULAD modeling tables.

To restore a split file, concatenate the parts in name order. On Windows:

```
cmd /c copy /b studentVle.csv.part00+studentVle.csv.part01+studentVle.csv.part02+studentVle.csv.part03+studentVle.csv.part04+studentVle.csv.part05 studentVle.csv
```

The same pattern restores `drives.csv` from its four parts and `measurements.csv` from its two parts. Each part stays under 80 MB so the repository can be cloned without Git LFS.

## Not in this release

- PhysioNet 2019. The source terms do not allow redistribution.
- Grade-file columns that name a person, a class, or a college.
- Offline score files that were not read.
- Raw platform logs and the full N-CMAPSS archive. The modeling tables made from them are included instead.
- A second or third copy of the institutional effect tables.

## Splits used with these tables

Institutional effect and raw tables: train 2020–2023 (3,633 students, 140 fails), freeze the warning ratio on 2024 (871 students, 85 fails), test once on 2025 (910 students, 93 fails).

OULAD modeling tables: train `2013B` (3,336 rows, 1,241 fails) and `2013J` (6,153 rows, 1,956 fails), freeze on `2014B` (4,153 rows, 1,458 fails), test once on `2014J` (5,714 rows, 1,525 fails).
