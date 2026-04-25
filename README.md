# Bearing Health Index

Vibration-based bearing fault detection and Remaining Useful Life (RUL) estimation using real accelerometer data from industrial test rigs.

Classifies bearing faults across 4 failure modes with 99% accuracy (F1: 0.99 macro avg) and estimates degradation stage using a health index built from fault severity progression.

---

## Problem

Unexpected bearing failures cause unplanned downtime in industrial machinery. Visual inspection is reactive — by the time a fault is visible, damage has already occurred. This project builds a health index from raw vibration signals that detects degradation early and estimates how much useful life remains.

---

## Data

- **CWRU Bearing Dataset** — Motor bearings with EDM-induced faults (ball, inner race, outer race) at three severity levels (0.007", 0.014", 0.021" diameter) at 12kHz. [Source](https://engineering.case.edu/bearingdatacenter)

**Note on RUL approach:** True RUL estimation requires run-to-failure data (e.g., NASA IMS). As a proxy, this project uses CWRU fault severity progression (three severity levels per fault type) as a degradation timeline to construct a health index and estimate remaining life stages. This is a valid demonstration of the RUL methodology. NASA IMS integration is planned as a future extension when storage constraints are resolved.

---

## Methodology

1. **Feature extraction** — Time-domain features (RMS, kurtosis, crest factor, peak-to-peak) and frequency-domain features (FFT spectral centroid, bearing defect frequencies) extracted from raw accelerometer signals
2. **Health index construction** — Rolling degradation score built from feature trends across the run-to-failure timeline
3. **Fault classification** — CNN-LSTM model trained on CWRU data to classify fault type (normal, ball fault, inner race fault, outer race fault)
4. **RUL estimation** — Health index slope used to estimate remaining cycles before failure threshold is crossed
5. **Dashboard** — Power BI dashboard showing rolling health index, alert thresholds, predicted failure date, and fault type probability

---

## Results

- 99% fault classification accuracy across 4 bearing conditions on held-out test set (1,878 samples)
- Perfect recall on Normal and Inner Race conditions; Ball fault recall 98%
- Health index tracks degradation across 3 severity stages — updated after Notebook 04
- Dashboard provides actionable maintenance window recommendation per bearing

---

## Stack

- Python: TensorFlow/Keras, scikit-learn, pandas, numpy
- Signal processing: scipy.signal, FFT, envelope analysis
- Visualization: Power BI
- Data: CWRU Bearing Data Center (NASA IMS planned extension)

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
