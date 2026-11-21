# mmc-sysid-benchmark

**A Survey and Simulation-Based Benchmark of System Identification for Converter-Dominated Power Systems**

Code and data for a controlled benchmark of system-identification (SI) methods on a high-fidelity grid-connected modular multilevel converter (MMC) model, simulated in RTDS/RSCAD. The benchmark accompanies a structured review of SI methods in three families:

1. **Classical SI**: subspace (N4SID-CVA, FDSID), prediction-error (PEM), polynomial (ARX, OE, BJ), transfer-function (TFEST), process and impulse-response models
2. **Nonlinear and ML-based SI**: NLARX and Hammerstein–Wiener (NLHW) models with wavelet, sigmoid, tree-partition, neural-network, SVM, regression-tree and Gaussian-process nonlinearities
3. **Hybrid and physics-informed SI**: a physics-based baseline combined with a learned neural correction

All methods use the same dataset, input–output definitions, preprocessing, validation split and error metrics (MSE, RMSE, NMSE, FPE, fit %), and the benchmark also reports execution times.

> **Scope.** The numerical results hold only for the implementations and operating conditions evaluated here. They are not a universal ranking of SI methods.

---

## Test system

- **Model:** onshore MMC station of the CIGRE B4.57 point-to-point MMC-HVDC benchmark. It is connected to a balanced AC grid through a low-frequency transformer and to an ideal DC bus.
- **Control:** an outer P/Q loop and an inner loop for output and circulating current
- **Key parameters:** 200 submodules per arm, $L_\text{arm}$ = 0.029 p.u., $C_\text{SM}$ = 10 mF, 60 Hz, 220 kV secondary, 266.667 MVA per phase
- **MIMO structure:** 2 × 2 *dq*-frame admittance. The inputs are the *dq* voltage perturbations ($v_d$, $v_q$) and the outputs are the *dq* current responses ($i^\Delta_d$, $i^\Delta_q$) at the PCC.

| Experiment | Content | Use |
|---|---|---|
| Frequency domain | ≈ 100 complex frequency-response points (99 frequencies) | N4SID, PEM, FDSID, ARX, OE; noise and Monte Carlo study (SNR 20/30/40 dB, 100 runs) |
| Time domain | 20,000 samples, $T_s$ = 50 µs; first 50 % for estimation, last 50 % for validation | Classical, nonlinear and hybrid methods |

## Data

The dataset is hosted externally:
**[Google Drive: dataset folder](https://drive.google.com/drive/folders/1ja6PIDCBjcrZljfRsmoqGAZ5CM7Yuj7T)**

Download it into `data/`. See [`data/README.md`](data/README.md) for the expected layout.

## Repository structure

<!-- TODO: adjust to the actual repository contents -->
```
mmc-sysid-benchmark/
├── data/                  # dataset (downloaded separately, see data/README.md)
├── classical/             # frequency- and time-domain linear SI (Sec. 2)
├── nonlinear_ml/          # NLARX, NLHW and ML estimators (Sec. 3)
├── hybrid/                # physics-informed / hybrid identification (Sec. 4)
├── utils/                 # metrics (MSE, RMSE, NMSE, FPE, fit), plotting
├── results/               # generated figures and tables
├── CITATION.cff
├── LICENSE
└── README.md
```

## Requirements

<!-- TODO: confirm versions -->
- MATLAB R20xx or later with the **System Identification Toolbox** (classical and nonlinear methods)
- *(if applicable)* Python ≥ 3.10 with the packages in `requirements.txt` (hybrid / neural models)
- RTDS/RSCAD is needed **only** to regenerate the data. The supplied dataset is enough to reproduce all results.

## Reproducing the results

<!-- TODO: replace with actual entry-point scripts -->
| Paper item | Script |
|---|---|
| Figs. 3–6, Table 2 (frequency domain) | `classical/run_frequency_domain.m` |
| Figs. 7–8, Monte Carlo study (noise / parameters) | `classical/run_noise_montecarlo.m` |
| Figs. 13–16, 19, Table 4 (time domain) | `classical/run_time_domain.m` |
| Figs. 17–18, 20–21, Tables 6–7 (NLARX / NLHW) | `nonlinear_ml/run_nonlinear.m` |
| Figs. 22–23 (hybrid) | `hybrid/run_hybrid` |

## Citation

If you use this code or dataset, please cite the paper. GitHub's **"Cite this repository"** button produces the entry from [`CITATION.cff`](CITATION.cff).

```bibtex
@unpublished{singh2026sysid,
  author = {Singh, Sunny and Mishra, Saurabh and Reyes Dreke, Victor Daniel and Rane, Rahul and Leki{\'c}, Aleksandra},
  title  = {A Survey and Simulation-Based Benchmark of System Identification for Converter-Dominated Power Systems},
  note   = {Preprint submitted to Elsevier},
  year   = {2026}
}
```

Please also cite the data and software themselves, not only the paper, when you reuse them.

## Authors

- **Sunny Singh**: TU Delft (corresponding author, s.singh-6@tudelft.nl)
- **Saurabh Mishra**: Indian Institute of Technology (BHU), Varanasi
- **Victor Daniel Reyes Dreke**: TU Delft
- **Rahul Rane**: TU Delft
- **Aleksandra Lekić**: TU Delft

Faculty of Electrical Engineering, Mathematics and Computer Science, Delft University of Technology, The Netherlands.

## Acknowledgement

This work was carried out within the **MITIGATE-HARM** project, funded by the CETPartnership under the Joint Call 2024. CETPartnership research projects are co-funded by the European Commission (Grant Agreement No. 101069750) and the national funding organisations listed at <https://cetpartnership.eu/>.

## License

Code: [MIT](LICENSE). Data: CC BY 4.0 (see [`data/README.md`](data/README.md)).
