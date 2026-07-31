---
title : "Create S3 Buckets"
date: ""
weight : 2
chapter : false
pre : " <b> 5.2.2. </b> "
---

#### Create S3 Buckets

This project uses **four Amazon S3 buckets** to store different types of data throughout the data pipeline.

| Bucket | Purpose |
|:---|:---|
| `Raw Bucket` | Stores raw market data collected from Yahoo Finance (JSON/Parquet) under the `raw/` prefix |
| `Processed Bucket` | Stores feature-engineered datasets, validated data (`cleansed_daily/`), and reports (`reports/`) |
| `Model Bucket` | Stores trained XGBoost models (`models/xgboost_v1/`) |
| `Simulation Bucket` | Provides an isolated sandbox environment for the Replay Simulator |

{{% notice info %}}
**Note:** The `Quarantine` and `Cleansed` datasets are stored in the **Processed Bucket** by default using separate prefixes (`quarantine/` and `cleansed_daily/`). If preferred, you can store them in dedicated S3 buckets instead.
{{% /notice %}}

---

#### Step 1: Create the S3 Buckets Using the AWS CLI

Run the following commands in your terminal to create the required S3 buckets:

```bash
# Raw Bucket — stores raw market data collected from Yahoo Finance
aws s3 mb s3://my-nasdaq-stock-market-raw-2026-ap-southeast-1 \
    --region ap-southeast-1

# Processed Bucket — stores processed Parquet datasets,
# validated data, reports, and quarantined records
aws s3 mb s3://my-nasdaq-stock-processed-2026-ap-southeast-1 \
    --region ap-southeast-1

# Model Bucket — stores trained XGBoost models
aws s3 mb s3://my-nasdaq-stock-models-2026-ap-southeast-1 \
    --region ap-southeast-1

# Simulation Bucket — sandbox environment for the Replay Simulator
aws s3 mb s3://my-nasdaq-stock-simulation-2026-ap-southeast-1 \
    --region ap-southeast-1
```

![S3 Buckets Created](images/2.2/s3-buckets-created.png)

---

#### Step 2: Verify the Buckets

Run the following command to verify that all buckets were created successfully:

```bash
aws s3 ls | grep my-nasdaq
```

Expected output:

```text
2026-07-29 10:00:00 my-nasdaq-stock-market-raw-2026-ap-southeast-1
2026-07-29 10:00:00 my-nasdaq-stock-models-2026-ap-southeast-1
2026-07-29 10:00:00 my-nasdaq-stock-processed-2026-ap-southeast-1
2026-07-29 10:00:00 my-nasdaq-stock-simulation-2026-ap-southeast-1
```

---

#### Step 3: Upload the Ticker List to Amazon S3

The `tickers.json` file contains a list of more than **3,000 NASDAQ stock tickers** that will be monitored by the data pipeline.

Upload the file to Amazon S3:

```bash
python upload_tickers_config.py --file tickers.json
```

![S3 Tickers Uploaded](images/2.2/s3-tickers-uploaded.png)

{{% notice tip %}}
To refresh the ticker list with the latest NASDAQ listings, run:

```bash
python refresh_tickers_from_nasdaq.py
```

{{% /notice %}}

---

#### S3 Directory Structure After Setup

```
processed-bucket/
├── config/
│   └── tickers.json           ← List of monitored stock tickers
├── raw/                       ← Raw market data collected from Yahoo Finance
├── cleansed_daily/            ← Temporary buffer after Quality Gate validation
│   └── YYYY-MM-DD/
├── processed/                 ← Main yearly Parquet datasets
│   ├── 2024.parquet
│   └── 2025.parquet
├── quarantine/                ← Invalid or rejected records
│   └── YYYY-MM-DD/
├── reports/                   ← Data quality reports
└── models/                    ← Trained XGBoost models
    └── xgboost_v1/
        └── xgboost_model.json
```