# Exponential Moving Average-Based Input Drift Compensation for Autoencoder-Based Quality Monitoring in Wire Harness Manufacturing

All analyses are in `content/ema_drift_compensation.ipynb`. Run the notebook from top to bottom. Tables and figures appear inline; figures are not saved as separate files.

For local execution with Python 3.13.5, install the pinned analysis requirements and open JupyterLab from the repository root:

```bash
python -m pip install -r requirements.txt
jupyter lab content/ema_drift_compensation.ipynb
```

The six CSVs in `content/data` contain 9,829 waveforms: 9,803 normal and 26 defective. CSV row order is acquisition order. The notebook reports dataset fingerprints and package versions.

Each dataset uses its first five known-normal waveforms to initialize a fixed reference, scaler, and AE. Monitoring processes all subsequent waveforms in acquisition order. Compensation uses the drift estimate from preceding observations; the current reconstruction-error decision controls the update for the next waveform. Final scaling uses the 10th percentile of positive initial standard deviations as a minimum divisor. Threshold calibration retains ordinary standardization without that minimum divisor and sets the threshold to three times the mean of five held-out errors.

The existing JupyterLite workflow uses `requirements-jupyterlite.txt` and is triggered manually. In its browser file list, open `ema_drift_compensation.ipynb`; the accompanying CSVs are under `data`. Browser package versions may differ from the local environment.
