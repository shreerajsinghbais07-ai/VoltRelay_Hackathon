# VoltRelay Energy — Data Analytics Hackathon

## Project Overview

End-to-end data analytics project for the VoltRelay Energy Hackathon.

**Company:** VoltRelay Energy — synthetic battery-swapping network for EV 2W/3W delivery riders  
**Cities:** Bengaluru · Delhi NCR · Hyderabad · Jaipur · Mumbai · Pune  
**Dataset period:** January 2024 – June 2025  
**Simulation seed:** 59 500

---

## Project Structure

```
VoltRelay_Hackathon/
├── VoltRelay_Data_Analytics_Hackathon.ipynb   ← Main reproducible notebook
├── VoltRelay_Final_Analytics_Report_VoltRelay.docx  ← Final business report
├── VoltRelay_Hackathon_Script.py              ← Standalone Python validation script
├── build_notebook.py                          ← Notebook generator script
├── build_report.py                            ← Report generator script
│
├── data/
│   ├── raw/                                   ← Original CSVs (not moved; kept in root)
│   └── cleaned/                               ← Cleaned datasets (generated)
│
├── figures/                                   ← All chart PNG files (generated)
├── models/                                    ← Model artefacts directory
├── outputs/                                   ← CSV outputs (KPIs, RFM, churn, etc.)
└── README.md
```

**Raw data files** (in root — do not move):
- `swap_events.csv` — 3,877,013 rows × 22 cols
- `station_hourly_status.csv` — 1,487,712 rows × 14 cols
- `support_tickets.csv` — 44,000 rows × 12 cols
- `riders.csv` — 20,000 rows × 12 cols
- `batteries.csv` — 6,500 rows × 13 cols
- `city_daily_context.csv` — 3,282 rows × 12 cols
- `stations.csv` — 152 rows × 22 cols
- `fleet_partners.csv` — 12 rows × 12 cols

---

## Setup

### Requirements

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn statsmodels python-docx nbformat lxml
```

### Python Version
Python 3.12.x recommended. The project uses only standard data science libraries.

---

## How to Run the Notebook

1. Ensure all 8 raw CSV files are in the **same directory** as the notebook.
2. Open `VoltRelay_Data_Analytics_Hackathon.ipynb` in Jupyter (or VS Code / Google Colab).
3. **Restart kernel → Run All Cells**.
4. Expected: **ZERO errors**. All outputs, charts, and cleaned datasets generated automatically.

### Google Colab
Upload all CSV files to Colab session storage or mount Google Drive, then run the notebook. Change the path prefix if using Drive:
```python
import os
os.chdir("/content/drive/MyDrive/VoltRelay_Hackathon")
```

---

## Analytical Framework (9 Steps)

| Step | Description |
|---|---|
| 1 | Environment & Data Loading |
| 2 | Data Inspection & Quality Audit |
| 3 | Data Cleaning |
| 4 | EDA & Business KPIs (BQ1–BQ5) |
| 5 | RFM Analysis |
| 6 | Observations & Hypotheses |
| 7 | Formal Churn Analysis + Hypothesis Testing |
| 8 | Predictive Modelling + ML Sanity Check |
| 9 | Business Analytics & Recommendations |

---

## Major Findings

### 1. Kyron Battery Anomalous Degradation (BQ4) — **STRONG**
- Kyron batteries show **−22 pp lower SOH** than Amptek/Cellora after controlling for age and swap cycles (OLS, p < 0.001, R² = 0.78)
- **87.8%** of all Kyron batteries have been retired vs **0%** for other suppliers
- Characterisation: "anomalous degradation not explained by observed age/cycle exposure — requires quality investigation"
- Does NOT constitute proof of a manufacturing defect on synthetic data

### 2. Service Failure Concentration (BQ2) — **MODERATE**
- Dominant failure type: `failed_no_charged_battery` (87% of all failures) — an inventory problem
- Jaipur (7.85%) and Delhi NCR (7.35%) failure rates vs Mumbai (4.45%)
- Failures concentrate in commute windows: 06–09h and 17–20h
- Failure rate spiked to >12% in April–May 2024

### 3. Rider Churn Driven by Recency (BQ6) — **STRONG (structural)**
- 37.9% churn rate in the 60-day outcome window
- `recency_pre` (days since last swap) is the dominant predictor (RF importance = 0.68)
- Simple rule `recency_pre > 7 days`: F1 = 0.935, captures 95.8% of churners
- Best ML model ROC-AUC = 0.979 — but structurally related to churn definition
- **H1 (failure → churn) is NOT independently supported** once recency is controlled (p = 0.775)
- **H2** (city → churn): p = 0.053, not significant; **H3** (plan → churn): p = 0.517, not significant

---

## Three Recommendations

| # | Recommendation | Evidence Strength |
|---|---|---|
| 1 | Recency-triggered re-engagement outreach (7/14-day thresholds + A/B test) | Strong |
| 2 | Peak-window operational audit at top-10 failure stations in Jaipur/Delhi NCR | Moderate |
| 3 | Kyron battery quality review and managed phase-out (SOH < 70%) | Strong |

---

## Key Data Quality Notes

| Issue | Resolution |
|---|---|
| Firmware v3.2.0 timestamp bug (+5h30m) | Applied ONLY to records 2025-03-10 to 2025-04-14 |
| SOC/SOH > 100 (sensor drift) | Set to NaN |
| Negative km_since_last_swap | Set to NaN + flagged |
| home_city spelling inconsistencies | Mapped to 6 canonical names |
| Test stations STN-TST-01/02 | Flagged and excluded from analytics (not deleted) |
| CSAT 65.6% missing | Retained; MNAR noted |

---

## Limitations

1. **Synthetic data** — no external validation possible
2. **Observational design** — no causal identification
3. **Structural recency-churn relationship** — high ML AUC partly structural
4. **Single churn cutoff** — results may vary with different cutoff choices
5. **CSAT MNAR** — missing-not-at-random; analysis excluded
6. **No confirmed cost data** — contribution margin per swap not calculable

---

## Final Audit

| Check | Status |
|---|---|
| Dataset check | PASS |
| Cleaning check | PASS |
| EDA check | PASS |
| RFM check | PASS |
| Churn check | PASS |
| H1 check (+ confound control) | PASS |
| H2 check | PASS |
| H3 check | PASS |
| Recency-rule benchmark | PASS |
| ML check | PASS |
| Battery age/cycle check | PASS |
| Recommendation check | PASS |
| Report check | PASS |
| Notebook reproducibility | PASS |

---

*VoltRelay Energy Data Analytics Hackathon — Analytical rigour over artificial sophistication.*
