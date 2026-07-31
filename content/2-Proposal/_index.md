---
title : "Proposal"
date: ""
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

#### Project Proposal

*Project proposal detailing the background, core objectives, AWS cloud architecture, data warehouse model, backtesting engine, and expected outcomes.*

---

#### Project Overview Summary

| Field | Details |
|:---|:---|
| **Project Title** | END-TO-END DATA PIPELINE & BACKTESTING ENGINE FOR STOCK MARKET DIRECTION |
| **Authors** | Vương Nhật Minh - 2212094, Phạm Đức Hoài Nam - 2212157, Doãn Anh Khôi - 2352601, Trần Quốc Khánh - 2311538, Nguyễn Huy Lượng - 2311997 |
| **Supervisor** | Lữ Hoàn Thiện |
| **Start Date** | June 15, 2026 |
| **End Date** | August 15, 2026 |

---

### 1. Project Overview & Core Objectives

The system is designed to automate the complete end-to-end lifecycle from stock market data ingestion, data cleansing & standardization, machine learning model training, backtesting strategy simulation, to serving daily stock price directional predictions.

#### 1.1. Core Problem Statement
* **Objective:** Binary classification of next-day stock price directional movement T+1.
* **Target Label Definition:**
  * **Label 1 (Up / Bullish):** (T+1) > (T)
  * **Label 0 (Down or Bearish/Sideways):** (T+1) <= (T)

#### 1.2. Business Value Extension (Optional / Future Scope)
* **Top Stock Recommendation:** Rank and suggest top potential stock portfolios daily based on the model's prediction confidence scores.

#### 1.3. System Goals
* Fully automate the Data Pipeline and MLOps lifecycle on AWS cloud infrastructure.
* Provide a low-latency, interactive web dashboard for end-users via Streamlit.

---

### 2. System Architecture (AWS Architecture)

The solution operates on a **Serverless Architecture** divided into **three main pipelines** for maximum performance and scalability:

#### Pipeline A – Historical Backfill
Collects historical NASDAQ stock data from 1962 to the present, applies Feature Engineering, and stores it as yearly Apache Parquet files (`processed/YYYY.parquet`) in S3.

#### Pipeline B – Daily Increment (Data Pipeline)
This pipeline runs every trading day and consists of several stages using **5 Lambda functions** with a **Fan-Out architecture**:

1. **EventBridge** triggers `nasdaq-daily-collector` (Lambda Producer) on a daily schedule.
2. `nasdaq-daily-collector` reads `tickers.json` from S3, splits tickers into chunks (CHUNK_SIZE=100), and pushes each chunk as an SQS message to `daily-collector-queue`.
3. **SQS Fan-Out**: `nasdaq-collector-producer` (Lambda Consumer) consumes SQS messages in parallel and fetches OHLCV market data from Yahoo Finance, writing raw Parquet files to `S3 Raw Zone` (`raw/{SYMBOL}.parquet`).
4. `nasdaq-quality-gate` validates raw data — valid records go to `cleansed_daily/YYYY-MM-DD/`, invalid records go to `quarantine/YYYY-MM-DD/` with rejection metadata.
5. `nasdaq-daily-etl` reads cleansed data, performs **Feature Engineering** using **Polars**, merges into the yearly Parquet dataset (`processed/YYYY.parquet`), and deletes the temporary buffer.

All Lambda functions are containerized using **Docker** and pushed to **Amazon ECR**.

```text
[EventBridge] ──trigger──> [nasdaq-daily-collector]
                                  │
                          Reads tickers.json from S3
                                  │
                          Splits into chunks of 100
                                  │
                          Pushes to SQS Queue
                                  │
                                  ▼
                    ┌── SQS: daily-collector-queue ──┐
                    │           (Fan-Out)             │
                    ▼                                 ▼
          [nasdaq-collector-producer]    [nasdaq-collector-producer]
                    │          (parallel consumers)         │
                    └────────── Yahoo Finance ──────────────┘
                                      │
                                      ▼
                               S3 Raw Zone
                                      │
                            (S3 Event Notification)
                                      ▼
                          [nasdaq-quality-gate]
                                      │
                         ┌────────────┴────────────┐
                         ▼                         ▼
               cleansed_daily/                quarantine/
                         │
                         ▼
                    [nasdaq-daily-etl]
                         │
                    (Feature Engineering - Polars)
                         │
                         ▼
              S3 Processed Zone (processed/YYYY.parquet)
```

#### Pipeline C – Prediction & Serving
* **Model Training:** XGBoost model is trained offline on the processed feature set and stored in `S3 Model Registry`.
* **Batch Inference:** EventBridge daily trigger launches `nasdaq-stock-predictor` Lambda to load the latest model, fetch recent features, run directional inference for all NASDAQ symbols, and persist predictions into **Amazon DynamoDB**.
* **API Serving:** Users interact with the Streamlit Dashboard → Requests sent to **AWS API Gateway** → `nasdaq-stock-predictor` fetches predictions from DynamoDB for sub-second latency response.

```text
            [EventBridge] ──────trigger──────> [nasdaq-stock-predictor]
                                                      │
                                          Load model from S3 Model Registry
                                                      │
                                          Fetch features from Feature Store
                                                      │
                                          Run inference (XGBoost)
                                                      │
                                                  [DynamoDB]
                                                      ▲
                                                      │
                                          [Lambda API Handler]
                                                      ▲
                                                      │
                                                [API Gateway]
                                                      ▲
                                                      │
                                          [Streamlit Dashboard]
```

#### Containerization
All Lambda functions are containerized using **Docker** and pushed to **Amazon ECR** to accommodate heavy dependencies:
* `Polars` & `PyArrow`
* `Pandera` & `Scikit-Learn` / `XGBoost`

---

### 3. Detailed Technical Components

#### 3.1. Data & Feature Engineering
* **Raw Input (OHLCV):** Open, High, Low, Close, Adj_Close, Volume.
* **Engineered Feature Set (16 indicators):**
  * **Trend Indicators:** SMA_5, SMA_20, EMA_12, EMA_26
  * **Momentum Indicators:** MACD, MACD_Signal, MACD_Hist, RSI_14
  * **Volatility Indicators:** BB_Upper, BB_Lower, BB_Width, Intraday_Volatility
  * **Lag & Return Features:** Lag_Close_1, Lag_Close_2, Lag_Close_3, Daily_Return
  * **Prediction Label:** Label (1 = Up, 0 = Down)

#### 3.2. Parquet Schema After Feature Engineering
Each `processed/YYYY.parquet` file contains the following columns:

| Feature Group | Columns |
|:---|:---|
| **Raw OHLCV** | `Date`, `Symbol`, `Open`, `High`, `Low`, `Close`, `Adj_Close`, `Volume` |
| **Trend Indicators** | `SMA_5`, `SMA_20`, `EMA_12`, `EMA_26` |
| **Momentum Indicators** | `MACD`, `MACD_Signal`, `MACD_Hist`, `RSI_14` |
| **Volatility Indicators** | `BB_Upper`, `BB_Lower`, `BB_Width`, `Intraday_Volatility` |
| **Lag & Return Features** | `Lag_Close_1`, `Lag_Close_2`, `Lag_Close_3`, `Daily_Return` |
| **Prediction Label** | `Label` (`1` = Up, `0` = Down) |

#### 3.3. Lambda Functions Summary

| Lambda Function | Handler | Memory | Timeout | Trigger |
|:---|:---|:---:|:---:|:---|
| `nasdaq-daily-collector` | `src.lambda_daily_collector.lambda_handler` | 512 MB | 5 minutes | Amazon EventBridge (Daily) |
| `nasdaq-collector-producer` | `src.lambda_collector_producer.lambda_handler` | 1024 MB | 15 minutes | Amazon SQS Trigger |
| `nasdaq-quality-gate` | `src.lambda_quality_gate.lambda_handler` | 512 MB | 5 minutes | Amazon S3 Event |
| `nasdaq-daily-etl` | `src.lambda_daily_etl.lambda_handler` | 3008 MB | 15 minutes | Amazon S3 Event |
| `nasdaq-stock-predictor` | `src.lambda_stock_predictor.lambda_handler` | 2048 MB | 30 seconds | Amazon API Gateway |

#### 3.4. Financial Backtesting Engine
Simulates real-world financial performance based on model prediction signals:
* **Buy / Hold Signal:** Model predicts Label 1 (Bullish trend).
* **Sell / Cash Signal:** Model predicts Label 0 (Bearish / Sideways trend).
* **Evaluation Metrics:**
  * **Total Profit:** Net cumulative portfolio return.
  * **Win Rate:** Percentage of profitable trades out of total executed trades.
  * **Sharpe Ratio:** Risk-adjusted return metric.
  * **Max Drawdown:** Maximum peak-to-trough decline in portfolio equity (systemic risk assessment).
  * **Benchmark:** Direct performance comparison against a passive **Buy & Hold** strategy.
