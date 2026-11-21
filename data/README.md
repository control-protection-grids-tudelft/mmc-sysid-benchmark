# Dataset

This folder holds RTDS/RSCAD simulation data from the onshore MMC station of the CIGRE B4.57 MMC-HVDC benchmark: 11 test cases plus the benchmark time-domain record (56 files, about 49 MB).

- [`manifest.csv`](manifest.csv) lists every file with its test case, signal, number of rows, size and SHA-256 hash.
- [`SHA256SUMS`](SHA256SUMS) lets you check file integrity: `sha256sum -c SHA256SUMS`, run from this folder.

The files are the original RSCAD exports, with their names and contents unchanged. Only the folders were reorganised.

## Layout

```
data/
├── time_domain/
│   ├── benchmark_IGDQ/                    # record used for the time-domain benchmark (Sec. 2.4)
│   ├── Test1_CT_LRnom_Pnom_Noise01/
│   ├── Test2_CT_LRnom_Pnom_Noise095/
│   ├── Test3_CT_LRnom_Pnom_Noise05/
│   ├── Test4_CT_LRnom_P05_Noise01/
│   └── Test5_CT_LR054006_P05_Noise01/
├── frequency_domain/
│   ├── Test6_FT_LRnom_P05_Noise01/
│   ├── Test7_FT_LRnom_P05_Noise095/
│   ├── Test8_FT_LRnom_P05_Noise05/
│   ├── Test9_FT_LRnom_P05_Noise00/
│   ├── Test10_FT_LRnom_Pnom_Noise00/
│   └── Test11_FT_LR054006_P05_Noise00/
├── manifest.csv
└── SHA256SUMS
```

### Test-case naming

`Test<n>_<type>_<LR>_<P>_Noise<k>`

| Field | Values | Meaning |
|---|---|---|
| `type` | `CT` / `FT` | time-domain record / frequency-domain sweep |
| `LR` | `LRnom` / `LR054006` | nominal / modified L–R parameters (the modified-parameter case) |
| `P` | `Pnom` / `P05` | nominal / 0.5 p.u. active-power setpoint |
| `Noise<k>` | `00`, `01`, `05`, `095` | injected-noise setting 0, 0.1, 0.5, 0.95 |

## Time-domain files (`time_domain/`)

Each CSV holds one signal. There are two columns, `Time` in seconds and the signal value, with a header in RSCAD format (`"Subsystem #1|CTLs|Vars|<signal>"`). Every record has 20,000 samples at Ts = 50 µs, covering 0–1 s.

**`benchmark_IGDQ/`**: the input–output record used in the time-domain benchmark of the paper.

| File | Signal | Role |
|---|---|---|
| `DATA_IGDQ_0.csv` | `Vdpu1` | input $v_d$ (p.u.) |
| `DATA_IGDQ_1.csv` | `Vqpu1` | input $v_q$ (p.u.) |
| `DATA_IGDQ_2.csv` | `Idpu1` | output $i^\Delta_d$ (p.u.) |
| `DATA_IGDQ_3.csv` | `Iqpu1` | output $i^\Delta_q$ (p.u.) |
| `DATA_IGDQ.svg` | | RSCAD plot of the record |

Paper split: samples 1–10,000 for estimation, 10,001–20,000 for validation.

**`Test1`–`Test5` (`CT`)**: each folder contains 8 signals plus an RSCAD plot (`Data_exported.svg`).

| File | Signal | Description |
|---|---|---|
| `Data_exported_0.csv` | `VD1REF` | d-axis voltage reference (input) |
| `Data_exported_1.csv` | `VQ1REF` | q-axis voltage reference (input) |
| `Data_exported_2.csv` | `id1` | d-axis current (output) |
| `Data_exported_3.csv` | `iq1` | q-axis current (output) |
| `Data_exported_4.csv` | `VRMS1pu` | AC RMS voltage (p.u.) |
| `Data_exported_5.csv` | `VDCLINK1` | DC-link voltage |
| `Data_exported_6.csv` | `PMMC1` | MMC active power |
| `Data_exported_7.csv` | `QMMC1` | MMC reactive power |

## Frequency-domain files (`frequency_domain/`)

Each folder has one `Data_exported.csv` holding the 2 × 2 *dq* admittance sweep. Values are comma-separated and padded with spaces. Each row is one injection frequency, and these columns are included:

| Columns | Meaning |
|---|---|
| `VDCp, Pdc/MVA, VRMSpu, theta, Pac/MVA, Qac/MVA` | steady-state operating point |
| `FREQ[tr]` | injection frequency (Hz) |
| `Mag_DD, Phase_DD` … `Mag_QQ, Phase_QQ` | magnitude and phase (deg) of $Y_{dd}, Y_{dq}, Y_{qd}, Y_{qq}$ |
| `Vref, Pref, Qref, VDCref` | controller setpoints |

> **Parsing note:** the header line appears more than once in a file because RSCAD appends one per sweep run. Skip any line that starts with `VDCp` when reading.

| Test | Unique frequencies | Range | Rows |
|---|---|---|---|
| Test6 | 30 | 13–100 Hz | 30 |
| Test7, Test8, Test9 | 70 | 1–1000 Hz | 70 |
| Test10 | 70 | 1–1000 Hz | 170 (repeated sweeps) |
| Test11 | 70 | 1–1000 Hz | 140 (repeated sweeps) |

## License and citation

The data are released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). If you use them, please cite the paper and this repository (see [`../CITATION.cff`](../CITATION.cff)).
