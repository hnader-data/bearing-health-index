# Bearing Health Index

Vibration-based bearing fault detection and Remaining Useful Life (RUL) estimation using real run-to-failure sensor data from industrial test rigs.

Classifies bearing faults across 4 failure modes with 99% accuracy (F1: 0.99 macro avg)


---

## Problem

Unexpected bearing failures cause unplanned downtime in industrial machinery. Visual inspection is reactive — by the time a fault is visible, damage has already occurred. This project builds a health index from raw vibration signals that detects degradation early and estimates how much useful life remains.

---

## Data

- **NASA IMS Bearing Dataset** — 4 bearings tested continuously to failure at 20kHz sampling, 1-second snapshots every 10 minutes. [Source](https://data.nasa.gov/dataset/ims-bearings)
- **CWRU Bearing Dataset** — Motor bearings with EDM-induced faults (ball, inner race, outer race) at 12-48kHz. [Source](https://engineering.case.edu/bearingdatacenter)

---

## Methodology

1. **Feature extraction** — Time-domain features (RMS, kurtosis, crest factor, peak-to-peak) and frequency-domain features (FFT spectral centroid, bearing defect frequencies) extracted from raw accelerometer signals
2. **Health index construction** — Rolling degradation score built from feature trends across the run-to-failure timeline
3. **Fault classification** — CNN-LSTM model trained on CWRU data to classify fault type (normal, ball fault, inner race fault, outer race fault)
4. **RUL estimation** — Health index slope used to estimate remaining cycles before failure threshold is crossed
5. **Dashboard** — Tableau dashboard showing rolling health index, alert thresholds, predicted failure date, and fault type probability

---

## Results

- 99% fault classification accuracy across 4 bearing conditions on held-out test set (1,878 samples)
- Perfect recall on Normal and Inner Race conditions; Ball fault recall 98%
- Dashboard provides actionable maintenance window recommendation per bearing

---

## Stack

- Python: TensorFlow/Keras, scikit-learn, pandas, numpy
- Signal processing: scipy.signal, FFT, envelope analysis
- Visualization: Tableau
- Data: NASA IMS, CWRU Bearing Data Center

---

## Structure

```
bearing-health-index/
├── data/               # Links to NASA/CWRU sources (raw data not committed)
├── notebooks/          # Exploration and feature engineering
├── src/                # Feature extraction, model training, health index
├── visualizations/     # Tableau dashboard screenshots
├── requirements.txt
└── README.md
```

---

## How to Run

```bash
git clone https://github.com/hnader-data/bearing-health-index
cd bearing-health-index
pip install -r requirements.txt
jupyter notebook notebooks/01_feature_extraction.ipynb
```

---

## Background

Mechanical Engineering BSc with experience in wireline services maintenance at ADNOC (Abu Dhabi). Bearing diagnostics and vibration analysis are core to rotating equipment maintenance — this project applies that domain knowledge to a data-driven health monitoring pipeline.
