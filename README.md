# Medilytics — Healthcare Revenue Intelligence Dashboard

A multi-role, interactive revenue analytics dashboard built with Streamlit, Plotly, and Pandas. Provides real-time insights into revenue leakage, claim denial risk, billing anomalies, and financial forecasting across hospital departments.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Running the App](#running-the-app)
- [User Roles & Login Credentials](#user-roles--login-credentials)
- [Dashboard Pages](#dashboard-pages)
- [Data Files](#data-files)
- [Tech Stack](#tech-stack)


---

## Overview

Medilytics is a healthcare revenue cycle management (RCM) intelligence platform designed for hospital finance teams. It provides role-based access so CFOs, RCM analysts, department heads, and insurance teams each see only the data and pages relevant to their function.

The platform analyses 60,000 real-world claim records across 5 departments (Cardiology, Neurology, Orthopedics, Emergency, General Medicine) and 5 insurance types, offering actionable insights on:

- Where revenue is leaking and why
- Which claims are at highest risk of denial
- Where billing anomalies exist in the system
- What revenue is projected for the next 6 months

---

## Features

- **Role-Based Access Control** — 4 distinct roles with tailored page access
- **7 Analytics Pages** — Executive overview, leakage analysis, denial prediction, anomaly detection, forecasting, CFO strategy, insurance analytics
- **Animated Login Page** — ECG wave background, gold brand heading, CSS-only animations
- **PDF Export** — Save any page as a PDF file directly from the browser
- **Interactive Charts** — All charts built with Plotly, fully interactive with hover tooltips
- **Sidebar Filters** — Date range, department, insurance type filters per page
- **Department Head View** — Scoped to a single department with doctor-level breakdown
- **ARIMA Forecasting** — 6-month revenue projection with confidence bands
- **Billing Anomaly Detection** — Z-score outlier detection on Claim Gap and Payment Gap
- **Denial Risk ML Model** — Logistic regression with feature importance (ROC-AUC 0.66)

---

## Project Structure

```
medilytics/
│
├── app.py                          # Entry point — run this
├── Login.py                        # Login page with animated background
├── sidebar.py                      # Sidebar navigation, filters, logo
├── chart_config.py                 # Shared Plotly theme, KPI helpers, CSS loader
├── pdf_export.py                   # PDF download button (html2canvas + jsPDF)
├── style.css                       # Global dark theme stylesheet
│
├── Executive_Dashboard.py          # Executive Overview page
├── Revenue_Leakage_Analysis.py     # Revenue Leakage Analysis page
├── Claim_Denial_main.py            # Claim Denial Risk Prediction page
├── billing_anomaly.py              # Billing Anomaly Detection page
├── forecast_dashboard.py           # Revenue Forecasting page
├── cfo_strategic.py                # CFO Strategic Dashboard page
├── insurance_view.py               # Insurance Analytics page
│
├── New_logo-removebg-preview.png   # Medilytics logo (place in root folder)
│
└── data/
    ├── users.csv                   # User credentials and roles
    ├── modified_dataset.csv        # Main claims dataset (60,000 rows)
    ├── updated_cleaned_claim_dataset.csv   # Extended dataset with gap columns
    ├── pre_processed_data.csv      # Pre-processed data for denial model
    ├── denial_model_predictions.csv        # Denial probability per claim
    ├── denial_feature_importance.csv       # Logistic regression coefficients
    ├── denial_model_metrics.csv            # Model evaluation metrics
    ├── monthly_revenue_history.csv         # 23 months of historical revenue
    ├── revenue_forecast.csv                # 6-month ARIMA forecast
    └── monthly_kpi_dataset.csv             # Monthly KPI aggregates
```

---

## Installation

### Prerequisites

- Python 3.9 or higher
- pip

### Steps

**1. Clone or download the project**

```bash
git clone <your-repo-url>
cd medilytics
```

Or simply unzip the project folder.

**2. Install dependencies**

```bash
pip install streamlit pandas plotly pillow
```

Full dependency list:

| Package | Purpose |
|---|---|
| `streamlit` | Web app framework |
| `pandas` | Data manipulation |
| `plotly` | Interactive charts |
| `pillow` | Image handling |

---

## Running the App

```bash
streamlit run app.py
```

The app will open automatically in your browser at `http://localhost:8501`.

> **Note:** Make sure your terminal is in the project root folder (the one containing `app.py`) when running this command.

---

## User Roles & Login Credentials

The app uses role-based access. Each user sees only the pages relevant to their role.

| Username | Password | Role | Department Access |
|---|---|---|---|
| `Rahul Sharma` | `pass123` | CFO | All departments — all 7 pages |
| `Anita Verma` | `pass456` | RCM | All departments — 5 pages |
| `Neha Gupta` | `pass888` | RCM | All departments — 5 pages |
| `Amit Joshi` | `pass258` | RCM | All departments — 5 pages |
| `Dr Kiran Reddy` | `pass789` | Department Head | Cardiology only |
| `Dr Sanjay Kulkarni` | `pass321` | Department Head | Neurology only |
| `Dr Arvind Sharma` | `pass654` | Department Head | Orthopedics only |
| `Dr Deepak Joshi` | `pass999` | Department Head | Emergency only |
| `Dr Suresh Nair` | `pass147` | Department Head | General Medicine only |
| `Rohit Malhotra` | `pass777` | Insurance Team | Insurance Analytics only |

### Role Access Matrix

| Page | CFO | RCM | Dept Head | Insurance Team |
|---|:---:|:---:|:---:|:---:|
| Executive Overview | ✓ | ✓ | ✓ (dept only) | — |
| Revenue Leakage | ✓ | ✓ | ✓ (dept only) | — |
| Claim Denial Risk | ✓ | ✓ | ✓ (dept only) | — |
| Billing Anomaly | ✓ | ✓ | ✓ (dept only) | — |
| Revenue Forecasting | ✓ | ✓ | — | — |
| CFO Strategic View | ✓ | — | — | — |
| Insurance Analytics | — | — | — | ✓ |

---

## Dashboard Pages

### Executive Overview
High-level hospital revenue summary. Shows monthly revenue trend, revenue by department and insurance type, and denial rate heatmap. Department Heads see a scoped view with doctor-level breakdown.

**Key metrics:** Total Revenue · Net Received · Revenue Leakage · Approval Rate

---

### Revenue Leakage Analysis
Identifies and quantifies the gap between expected and actual revenue across departments, insurance types, and time. Highlights charge capture efficiency bottlenecks.

**Key metrics:** Total Leakage · Leakage Rate · Avg Leakage Index · Revenue at Risk

**Key insight:** Total leakage ₹46.85 Cr (25.7% of expected revenue)

---

### Claim Denial Risk Prediction
Logistic Regression model (ROC-AUC 0.66) predicts denial probability per claim. Shows risk distribution by department and insurance, feature importance of denial drivers, and a priority review list of high-risk claims.

**Key metrics:** High-Risk Claims · High-Risk Rate · Potential Loss · Top Risk Department

**Key insight:** Government insurance has the highest denial rate at 26.7%

---

### Billing Anomaly Detection
Z-score outlier detection on Claim Gap (billed vs approved) and Payment Gap (approved vs received). Flags anomalies at the 2σ threshold with severity classification (Critical / High / Normal).

**Key metrics:** Total Anomalies · Claim Gap Anomalies · Payment Gap Anomalies · Revenue Loss

**Key insight:** 14.8% of claims flagged as anomalous; Cardiology has the highest average Claim Gap

---

### Revenue Forecasting
ARIMA(1,1,1) model trained on 23 months of historical data. Displays 6-month forward projection with ±10% confidence bands, month-over-month growth, and a dual-axis claims volume vs leakage chart.

**Key metrics:** Latest Monthly Revenue · Avg Historical Revenue · 6-Month Forecast Total · Projected Growth

---

### CFO Strategic Dashboard *(CFO only)*
Board-level view combining all financial dimensions. P&L comparison across departments, collection efficiency heatmap, full revenue vs forecast overlay, and a revenue vs denial rate bubble chart by payer.

**Key metrics:** 8 KPIs including Expected Revenue, Actual Revenue, Collection Rate, Leakage Rate, 6-Month Forecast

---

### Insurance Analytics *(Insurance Team only)*
Three-tab deep dive into payer performance. Tab 1: payer mix and revenue trends. Tab 2: denial rate and ML denial probability per payer. Tab 3: settlement turnaround, leakage, and revenue at risk per insurer.

**Key insight:** Self-Pay has the lowest denial rate; Government has the highest at 26.7%

---

## Data Files

| File | Rows | Key Columns |
|---|---|---|
| `modified_dataset.csv` | 60,000 | Claim_ID, Department, Insurance_Type, Expected_Revenue, Actual_Revenue, Revenue_Leakage, Denial_Flag |
| `updated_cleaned_claim_dataset.csv` | 60,000 | All above + Claim_Gap, Payment_Gap, High_Risk_Claim, Revenue_Loss |
| `pre_processed_data.csv` | 60,000 | Encoded features for ML model |
| `denial_model_predictions.csv` | 60,000 | Claim_ID, Denial_Probability, Risk_Level |
| `denial_feature_importance.csv` | ~15 | Feature, Coefficient |
| `denial_model_metrics.csv` | 1 | Accuracy, Precision, Recall, F1, ROC_AUC |
| `monthly_revenue_history.csv` | 23 | Month, Actual_Revenue |
| `revenue_forecast.csv` | 6 | Month, Forecast_Revenue |
| `monthly_kpi_dataset.csv` | 23 | Month, Total_Claims, Revenue_Leakage |

### Departments
`General Medicine` · `Emergency` · `Cardiology` · `Orthopedics` · `Neurology`

### Insurance Types
`Corporate` · `Government` · `Private` · `Self-Pay` · `Unknown`

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Frontend / App** | Streamlit 1.x |
| **Charts** | Plotly Express + Plotly Graph Objects |
| **Data** | Pandas, NumPy |
| **ML Model** | Scikit-learn (Logistic Regression) |
| **Forecasting** | ARIMA(1,1,1) via Statsmodels |
| **PDF Export** | html2canvas + jsPDF (CDN, browser-side) |
| **Styling** | Custom CSS (dark theme, Rajdhani + Inter fonts) |
| **Authentication** | CSV-based credential store with Streamlit session state |

---

## Key Business Insights

- **₹46.85 Cr** total revenue leakage detected (25.7% of expected)
- **98.4%** collection rate across all departments
- **Government insurance** has the highest denial rate at **26.7%**
- **Self-Pay** has the lowest denial rate at **4.7%**
- **Cardiology** has the highest average Claim Gap (₹13,151)
- **14.8%** of claims flagged as billing anomalies
- **3,469** Claim Gap anomalies (5.8%) above the 2σ threshold of ₹33,683
- **1,597** Payment Gap anomalies (2.7%) above the 2σ threshold of ₹13,527

---

## Notes

- The app runs fully offline — no external API calls are made during operation
- PDF export requires an internet connection to load the html2canvas and jsPDF libraries from CDN
- The login system is credential-based for demonstration; for production use, replace with a proper authentication system
- All data is pre-processed and static; the app does not write back to any data files

---

*Medilytics — Healthcare Revenue Intelligence*
