# 🏭 Industrial Predictive Maintenance Platform

**AI-driven failure prediction, automated diagnostics, and real-time maintenance alerting for Industrial IoT systems.**

## 👤 Portfolio Implementation

**Customized by:** Ali Faiz  
**Role:** Frontend Developer & AI Automation Developer  
**GitHub:** https://github.com/Alifaizllc  
**LinkedIn:** https://www.linkedin.com/in/ali-khattak

This repository is a customized portfolio implementation of a publicly available predictive-maintenance project. The automation credentials, Google Sheet reference, Telegram destination, n8n instance metadata, and temporary API tunnel URLs have been removed so the project can be configured with your own environment.

---
---

## 📌 Overview

The **Industrial Predictive Maintenance Platform** predicts industrial machine failures **before they happen** and automates the entire maintenance response — from sensor reading to technician alert — using a modern data engineering and machine learning stack.

Instead of reactive repairs or fixed maintenance schedules, this system continuously evaluates live sensor data, predicts whether a machine will fail **within the next 24 hours**, classifies the **expected failure type**, generates **AI-powered maintenance recommendations**, and pushes **real-time alerts** to maintenance teams — with results visualized in a **Power BI dashboard**.


---

## 🎯 Objectives

- ✅ Predict whether a machine will fail within 24 hours
- ✅ Identify the expected failure type
- ✅ Generate automated maintenance recommendations
- ✅ Send real-time alerts to maintenance teams
- ✅ Provide operational insights through dashboards

---

## 🏗️ System Architecture

```
 ┌────────────────────┐
 │  Industrial Sensors │
 └──────────┬──────────┘
            ▼
 ┌────────────────────┐
 │   Google Sheets     │  ← Data Source
 └──────────┬──────────┘
            ▼
 ┌────────────────────┐
 │   n8n Workflow      │  ← Orchestration
 └──────────┬──────────┘
            ▼
 ┌────────────────────┐
 │   FastAPI Service   │  ← Model Serving (via ngrok)
 │  ┌────────────────┐ │
 │  │ Model 1:       │ │  Failure Prediction (0/1)
 │  │ Random Forest  │ │
 │  └───────┬────────┘ │
 │          ▼           │
 │  ┌────────────────┐ │
 │  │ Model 2:       │ │  Failure Type Classification
 │  │ Random Forest  │ │
 │  └────────────────┘ │
 └──────────┬──────────┘
            ▼  (if failure predicted)
 ┌────────────────────┐
 │  Gemini AI Agent    │  ← Condition analysis + recommendation
 └──────────┬──────────┘
            ▼
 ┌────────────────────┐
 │   Telegram Bot      │  ← Real-time alert to maintenance team
 └──────────┬──────────┘
            ▼
 ┌────────────────────┐
 │  Power BI Dashboard │  ← Operational insights & KPIs
 └────────────────────┘
```

---

## 🧰 Tech Stack

| Layer                  | Technology                          |
|-------------------------|--------------------------------------|
| Data Source              | Google Sheets                        |
| Workflow Orchestration   | n8n                                   |
| Machine Learning          | Python, scikit-learn (Random Forest) |
| API Layer                 | FastAPI                              |
| API Exposure               | ngrok                                |
| AI Analysis                 | Google Gemini AI Agent             |
| Notifications                | Telegram Bot API                  |
| Visualization                  | Power BI                        |

---

## 📊 Dataset

Industrial IoT sensor dataset — **24,042 records** across four machine types.

**Machine types:** `CNC` · `Pump` · `Compressor` · `Robotic Arm`

### Features

| Feature | Description |
|---|---|
| `machine_type` | Type of industrial machine |
| `vibration_rms` | Root-mean-square vibration amplitude |
| `temperature_motor` | Motor temperature reading (°C) |
| `current_phase_avg` | Average electrical phase current |
| `pressure_level` | Operating pressure |
| `rpm` | Rotational speed |
| `operating_mode` | Current operational state |
| `hours_since_maintenance` | Hours elapsed since last maintenance |
| `ambient_temp` | Ambient environmental temperature |

### Targets

| Target | Type | Values |
|---|---|---|
| `failure_within_24h` | Binary | `0` = Normal, `1` = Failure |
| `failure_type` | Multi-class | `bearing`, `electrical`, `hydraulic`, `motor_overheat`, `none` |

---

## 🤖 Machine Learning Pipeline

1. **Data Cleaning** — duplicate removal, missing value imputation, outlier handling
2. **Feature Engineering** — thermal delta, vibration-to-RPM ratio, maintenance urgency ratio
3. **Label Encoding** — categorical variables encoded for tree-based models
4. **Train-Test Split** — 80/20 stratified split
5. **Model Training** — Random Forest Classifier (two stages)
6. **Model Evaluation** — accuracy, precision, recall, F1-score, ROC-AUC
7. **Model Deployment** — served via FastAPI, exposed through ngrok

### Model Architecture

- **Model 1 — Failure Prediction:** binary classifier → `failure_within_24h`
- **Model 2 — Failure Type Classification:** multi-class classifier → `failure_type` (invoked only when Model 1 predicts failure)

> ℹ️ Metrics are illustrative of the evaluation methodology — replace with your actual training results.

---

## 🔌 API Reference (FastAPI)

| Endpoint | Method | Description |
|---|---|---|
| `/predict/failure` | `POST` | Returns `failure_within_24h` prediction + probability |
| `/predict/failure-type` | `POST` | Returns predicted `failure_type` + class probabilities |
| `/health` | `GET` | Service health check |
| `/model-info` | `GET` | Model version & feature schema |


---

## ⚙️ n8n Workflow

1. **Google Sheets Trigger** — polls for new sensor rows
2. **HTTP Request** → `/predict/failure`
3. **IF Node** — branches on `failure_within_24h == 1`
4. **HTTP Request** → `/predict/failure-type` *(failure branch only)*
5. **Gemini AI Agent Node** — generates condition analysis & recommendation
6. **Telegram Node** — sends alert to maintenance team
7. **Write Node** — logs results for Power BI reporting

---

## 📲 Sample Telegram Alert

```
ALERT: Predictive Maintenance System
Machine: CNC-14 | Type: CNC
Status: FAILURE PREDICTED (Probability: 0.91)
Failure Type: Bearing Wear
Recommendation: Replace spindle bearing within 12
hours. Reduce load to 60% until serviced.
Time: 2026-07-19 09:42
```

---

## 📈 Power BI Dashboard

- KPI cards — machines monitored, active alerts, failures (24h)
- Failure trend line chart by machine type
- Failure type distribution (pie chart)
- Machine health heatmap
- Recent alerts & recommendations table

---

## 📂 Project Structure

```
industrial-predictive-maintenance/
├── data/
│   └── sensor_data.csv
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_feature_engineering.ipynb
│   └── 03_model_training.ipynb
├── models/
│   ├── failure_prediction_rf.pkl
│   └── failure_type_rf.pkl
├── api/
│   ├── main.py
│   ├── schemas.py
│   └── preprocessing.py
├── n8n/
│   └── workflow.json
├── dashboard/
│   └── powerbi_report.pbix
├── requirements.txt
└── README.md
```

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/industrial-predictive-maintenance.git
cd industrial-predictive-maintenance
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run the FastAPI service
```bash
uvicorn api.main:app --reload --port 8000
```

### 4. Expose the API with ngrok
```bash
ngrok http 8000
```

### 5. Import the n8n workflow
Import `My workflow.json` into your n8n instance and configure:
- Google Sheets credentials
- FastAPI (ngrok) endpoint URL
- Gemini AI API key
- Telegram Bot token & chat ID

### 6. Open the Power BI dashboard
Open `dashboard/powerbi_report.pbix` and point it to your results data source.

---



## 🔮 Future Improvements

- [ ] Replace Google Sheets with a real streaming source (MQTT/Kafka)
- [ ] Deploy FastAPI to a persistent cloud host (AWS/Azure/GCP)
- [ ] Add model drift detection & automatic retraining
- [ ] Support multiple prediction horizons (6h, 24h, 72h)
- [ ] Explore LSTM/TCN models for time-series degradation trends
- [ ] Add authentication & role-based access control
- [ ] Build a technician feedback loop to improve model accuracy

link dashboard 
https://app.powerbi.com/view?r=eyJrIjoiOWU5YTQ0NTMtYTQ4Mi00OWFiLTlhNDEtOTI1N2UzYzAxMDZkIiwidCI6IjJiYjZlNWJjLWMxMDktNDdmYi05NDMzLWMxYzZmNGZhMzNmZiIsImMiOjl9

---


