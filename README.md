<div align="center">

# EMA-Based Input Drift Compensation

**Autoencoder-based quality monitoring with real factory data**

[![Launch JupyterLite Lab](https://img.shields.io/badge/Launch-JupyterLite%20Lab-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jwsong0620.github.io/260901_DriftAE/lab/index.html?path=ema_drift_compensation.ipynb)
[![View notebook](https://img.shields.io/badge/View-Analysis%20Notebook-24292F?style=for-the-badge&logo=github&logoColor=white)](https://github.com/jwsong0620/260901_DriftAE/blob/main/content/ema_drift_compensation.ipynb)

[Overview](#overview) · [Datasets](#datasets) · [Run the analysis](#run-the-analysis) · [Repository structure](#repository-structure)

</div>

## Overview

Waveforms recorded during wire harness crimping can drift as production progresses. These changes can cause a fixed autoencoder (AE) to flag normal products as defective, increasing false alarms in factory quality monitoring.

This repository accompanies the manuscript **“Exponential Moving Average-Based Input Drift Compensation for Autoencoder-Based Quality Monitoring in Wire Harness Manufacturing.”** It provides the datasets and a single executable notebook for evaluating whether exponential moving average (EMA)-based input compensation can reduce drift-induced false alarms while preserving defect detectability, without retraining the AE during monitoring.

The analysis uses **six datasets collected during production at a real wire harness manufacturing factory**. All tables and figures are generated inline in one notebook.

| Factory datasets | Recorded waveforms | Measurement points per waveform |
| :---: | :---: | :---: |
| **6** | **9,829** | **200** |

### What the notebook covers

- Sequential waveform drift across the six production datasets.
- The effect of compensation on reconstruction errors and false alarms for normal samples.
- Sensitivity to the EMA smoothing factor at fixed detection thresholds.
- Defect detectability after compensation, using the observed defective samples.

Each dataset has its own AE, reference, and scaler. During monitoring, each incoming waveform is compensated using the drift estimate from preceding observations. The estimate is updated for the next waveform only when the current waveform passes the reconstruction-error threshold.

## Datasets

The CSV files in [`content/data`](https://github.com/jwsong0620/260901_DriftAE/tree/main/content/data) contain measured crimping waveforms in **acquisition order**. Each row represents one crimping operation.

| File | Acquisition date | Normal | Defective | Total |
| :--- | :--- | ---: | ---: | ---: |
| `20230426.csv` | April 26, 2023 | 1,484 | 1 | 1,485 |
| `20230427.csv` | April 27, 2023 | 1,024 | 2 | 1,026 |
| `20230428.csv` | April 28, 2023 | 2,946 | 10 | 2,956 |
| `20230503.csv` | May 3, 2023 | 1,199 | 8 | 1,207 |
| `20230504.csv` | May 4, 2023 | 1,288 | 0 | 1,288 |
| `20230505.csv` | May 5, 2023 | 1,862 | 5 | 1,867 |
| **Total** | | **9,803** | **26** | **9,829** |

| Columns | Description |
| :--- | :--- |
| `data1`–`data200` | The 200 measured waveform points, before standardization or drift compensation. |
| `label` | `0`: normal; `1`: defective. Labels are based on crimp force monitoring (CFM) results and manual verification. |
| Other columns | Supplementary fields; not used as AE inputs. |

The first five known-normal waveforms in each dataset are used for initialization. Monitoring evaluates subsequent waveforms in acquisition order, including defective samples. The counts above include the initialization samples.

## Run the analysis

### In your browser — JupyterLite Lab

**[Open the analysis in JupyterLite Lab →](https://jwsong0620.github.io/260901_DriftAE/lab/index.html?path=ema_drift_compensation.ipynb)**

No local Python installation is required.

1. Open `ema_drift_compensation.ipynb` if it does not open automatically. The CSV files are in the accompanying `data` folder.
2. Select **Run → Run All Cells** and allow the analysis to finish.
3. Read the tables and figures directly below the corresponding cells.

### On your computer — JupyterLab

The local analysis environment uses **Python 3.13.5**. From the repository root, install the pinned dependencies and launch JupyterLab:

```bash
python -m pip install -r requirements.txt
jupyter lab content/ema_drift_compensation.ipynb
```

Run the notebook from top to bottom. Tables and figures appear inline; no separate figure files are saved.

For the pinned analysis environment, use the local instructions above. Browser package versions may differ. The notebook reports package versions and SHA-256 dataset fingerprints to identify the environment and input files used.

<details>
<summary><strong>Reproducibility settings</strong></summary>

Each dataset uses its first five known-normal waveforms to initialize a fixed reference, scaler, and AE. These remain fixed during monitoring; only the drift estimate is updated.

Final scaling uses the 10th percentile of positive initial standard deviations as a minimum divisor. Threshold calibration uses ordinary standardization without that minimum divisor and sets the threshold to three times the mean of five held-out reconstruction errors. The notebook contains the complete implementation and parameter settings.

</details>

## Repository structure

```text
260901_DriftAE/
├── content/
│   ├── ema_drift_compensation.ipynb   # Complete analysis with inline results
│   └── data/
│       ├── README.md                 # Data format and label definitions
│       └── *.csv                     # Six sequential crimping datasets
├── requirements.txt                 # Pinned local analysis environment
├── requirements-jupyterlite.txt     # Browser deployment dependencies
├── .github/workflows/deploy.yml      # JupyterLite build and deployment
└── README.md
```

<details>
<summary><strong>Updating the browser version</strong></summary>

The JupyterLite site is built using `requirements-jupyterlite.txt`. After pushing changes to GitHub, run the **Build and Deploy** workflow manually from the repository’s **Actions** tab to update the browser version.

</details>
