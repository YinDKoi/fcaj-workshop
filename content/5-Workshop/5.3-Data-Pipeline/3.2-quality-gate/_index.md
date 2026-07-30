---
title : "Lambda Quality Gate"
date: ""
weight : 2
chapter : false
pre : " <b> 5.3.2. </b> "
---

#### Lambda Quality Gate — Data Validation & Isolation

`lambda_quality_gate.py` receives raw data from S3 and strictly validates it before passing it to the next processing stage.

![Quality Gate Validation Flow](images/3.2/quality-gate-flow.png)

*Figure 3.2. Quality Gate validation workflow. The Lambda Quality Gate validates incoming stock data against predefined business rules before routing valid records to the processing pipeline and isolating invalid records for further inspection.*
---

#### How It Works

```
S3 Raw Data
     │
     ▼
lambda_quality_gate
     │
     ├── ✅ Valid   → S3 cleansed_daily/YYYY-MM-DD/
     │
     └── ❌ Invalid → S3 quarantine/YYYY-MM-DD/
```

---

#### Validation Rules

`src/validator.py` applies the following checks:

| Check Type | Rule |
|:---|:---|
| **Schema Check** | Ensures all required columns exist: `Date`, `Ticker`, `Open`, `High`, `Low`, `Close`, `Volume` |
| **Negative Values** | Rejects records with `Close < 0` or `Volume < 0` |
| **OHLC Validity** | Validates `High >= Low`, `High >= Open`, `High >= Close` |
| **Null/NaN Check** | Filters tickers with null ratio exceeding the allowed threshold |

---

#### Step 1: Create Lambda Function

- **Function name:** `nasdaq-quality-gate`
- **CMD override:** `src.lambda_quality_gate.lambda_handler`
- **Memory:** `512 MB`
- **Timeout:** `5 minutes`

> *[TODO: Add screenshot of Lambda Quality Gate configuration — `images/3.2/lambda-quality-gate-config.png`]*
