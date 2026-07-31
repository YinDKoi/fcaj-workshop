---
title : "Lambda Daily ETL"
date: ""
weight : 3
chapter : false
pre : " <b> 5.3.3. </b> "
---

#### Lambda Daily ETL — Parquet Consolidation

`lambda_daily_etl.py` is the final component of Pipeline B. This Lambda function reads all validated data from the `cleansed_daily/YYYY-MM-DD/` buffer, performs **Feature Engineering**, and merges the data into the yearly Parquet dataset (`processed/YYYY.parquet`).

---

#### Workflow

```
S3 cleansed_daily/YYYY-MM-DD/
         │
         ▼
  lambda_daily_etl
         │
    1. Read validated data from the buffer
    2. Load the existing yearly dataset: processed/YYYY.parquet
    3. Merge and remove duplicate records (keeping the latest data)
    4. Recalculate Feature Engineering using Polars
    5. Write the updated dataset to processed/YYYY.parquet
    6. Delete the cleansed_daily/ buffer
         │
         ▼
  S3 processed/YYYY.parquet (updated)
```

---

#### Step 1: Create the Lambda Function

Create a new Lambda function with the following configuration:

- **Function name:** `nasdaq-daily-etl`
- **CMD override:** `src.lambda_daily_etl.lambda_handler`
- **Memory:** `3008 MB` (additional memory is required for processing large Parquet files with Polars)
- **Timeout:** `15 minutes`

Configure the following environment variables:

| Key | Value |
|:---|:---|
| `PROCESSED_BUCKET` | `my-nasdaq-stock-processed-2026-ap-southeast-1` |
| `CLEANSED_DAILY_PREFIX` | `cleansed_daily/` |
| `PROCESSED_PREFIX` | `processed/` |

![Lambda Daily Etl Config](images/3.3/lambda-daily-etl-config.png)

---

#### Step 2: Verify the Output

After the Lambda function completes successfully, verify that the yearly Parquet files have been updated:

```bash
aws s3 ls s3://my-nasdaq-stock-processed-2026-ap-southeast-1/processed/
```

Expected output:

```text
2026-07-29 01:30:00   1234567890 2026.parquet
2025-01-01 00:00:00    987654321 2025.parquet
...
```

{{% notice info %}}
**Why use Polars instead of Pandas?**

Polars is built on a native multi-threaded execution engine and the Apache Arrow memory format, making it **10–50× faster than Pandas** for large-scale data processing. When working with decades of historical market data from more than 3,000 stock tickers, processing performance becomes a critical factor.
{{% /notice %}}

---

#### Parquet Schema After Feature Engineering

Each `processed/YYYY.parquet` file contains the following feature groups:

| Feature Group | Columns |
|:---|:---|
| **Raw OHLCV** | `Date`, `Symbol`, `Open`, `High`, `Low`, `Close`, `Adj_Close`, `Volume` |
| **Trend Indicators** | `SMA_5`, `SMA_20`, `EMA_12`, `EMA_26` |
| **Momentum Indicators** | `MACD`, `MACD_Signal`, `MACD_Hist`, `RSI_14` |
| **Volatility Indicators** | `BB_Upper`, `BB_Lower`, `BB_Width`, `Intraday_Volatility` |
| **Lag & Return Features** | `Lag_Close_1`, `Lag_Close_2`, `Lag_Close_3`, `Daily_Return` |
| **Prediction Label** | `Label` (`1` = Up, `0` = Down) |