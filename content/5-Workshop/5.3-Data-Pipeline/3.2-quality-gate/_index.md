---
title : "Lambda Quality Gate"
date: ""
weight : 2
chapter : false
pre : " <b> 5.3.2. </b> "
---

#### Lambda Quality Gate — Data Validation & Quarantine

`lambda_quality_gate.py` validates raw market data stored in Amazon S3 before allowing it to proceed to the next stage of the data pipeline.

![Quality Gate Flow](images/3.2/quality-gate-flow.png)

---

#### Workflow

```
S3 Raw Data
     │
     ▼
lambda_quality_gate
     │
     ├── ✅ Valid Data   → S3 cleansed_daily/YYYY-MM-DD/
     │
     └── ❌ Invalid Data → S3 quarantine/YYYY-MM-DD/
```

---

#### Validation Rules

The validation logic is implemented in `src/validator.py`, which performs the following checks:

| Validation Type | Rule |
|:---|:---|
| **Schema Validation** | Verify that all required columns exist: `Date`, `Ticker`, `Open`, `High`, `Low`, `Close`, and `Volume` |
| **Negative Value Check** | Reject records where `Close < 0` or `Volume < 0` |
| **OHLC Validation** | Verify that `High >= Low`, `High >= Open`, and `High >= Close` |
| **Null / NaN Check** | Filter out tickers whose null-value ratio exceeds the configured threshold |

---

#### Step 1: Create the Lambda Function

Create a new Lambda function with the following configuration:

- **Function name:** `nasdaq-quality-gate`
- **CMD override:** `src.lambda_quality_gate.lambda_handler`
- **Memory:** `512 MB`
- **Timeout:** `5 minutes`

Configure the following environment variables:

| Key | Value |
|:---|:---|
| `RAW_BUCKET` | `my-nasdaq-stock-market-raw-2026-ap-southeast-1` |
| `PROCESSED_BUCKET` | `my-nasdaq-stock-processed-2026-ap-southeast-1` |
| `CLEANSED_DAILY_PREFIX` | `cleansed_daily/` |
| `QUARANTINE_PREFIX` | `quarantine/` |

![Lambda Quality Gate Config](images/3.2/lambda-quality-gate-config.png)

---

#### Step 2: Verify the Quarantine Folder

After the pipeline has completed, verify the quarantined data by listing the contents of the quarantine folder:

```bash
aws s3 ls s3://my-nasdaq-stock-processed-2026-ap-southeast-1/quarantine/ --recursive
```

Each file stored under `quarantine/` contains metadata describing why the record was rejected.

Example:

```json
{
    "ticker": "XYZ",
    "date": "2026-07-29",
    "reason": "OHLC_INVALID: High < Low detected",
    "row_count": 1
}
```

{{% notice tip %}}
Data stored in the `quarantine/` folder is **not deleted**. You can inspect these files later for debugging purposes or to refine the validation rules.
{{% /notice %}}

![S3 Quarantine Files](images/3.2/s3-quarantine-files.png)