# EMA-Based Input Drift Compensation

This repository provides the code and data to reproduce the analyses presented in the manuscript **“Exponential Moving Average-Based Input Drift Compensation for Autoencoder-Based Quality Monitoring in Wire Harness Manufacturing.”**

Waveform drift during production can cause an autoencoder to flag normal products as defective. Using six datasets collected at a real wire harness manufacturing factory, the code evaluates EMA-based input drift compensation to reduce false alarms while preserving defect detectability, without retraining the autoencoder during monitoring.

The analysis can be run directly in your browser using JupyterLite Lab. Open `ema_drift_compensation.ipynb` and select **Run → Run All Cells** to view the tables and figures.

[![Open in JupyterLite Lab](https://img.shields.io/badge/Open%20in-JupyterLite%20Lab-F37626)](https://jwsong0620.github.io/260901_DriftAE/lab/index.html?path=ema_drift_compensation.ipynb)

Code and data DOI: [10.5281/zenodo.22689470](https://doi.org/10.5281/zenodo.22689470)
