# Sequential crimping datasets

The six CSVs contain raw measured waveforms collected on April 26, April 27, April 28, May 3, May 4, and May 5, 2023.

- One row represents one crimping operation; CSV row order is acquisition order.
- `data1`–`data200` contain the 200 waveform measurement points before standardization or drift compensation.
- `label` is 0 for normal and 1 for defective, based on CFM results and manual verification.
- Additional columns are not used as AE inputs.

The files contain 9,829 waveforms: 9,803 normal and 26 defective. The notebook identifies the input version with SHA-256 fingerprints. Monitoring begins after the fifth initial known-normal waveform in each dataset; subsequent defective samples remain in the evaluated sequence.
