---
title : "Model Training & Deployment"
date: ""
weight : 2
chapter : false
pre : " <b> 5.4.2. </b> "
---

#### XGBoost Model Training & Deployment

---

#### Step 1: Train the Model Locally

The `train_model.py` script performs the complete model training pipeline:

```bash
python train_model.py
```

The script performs the following tasks:

1. Downloads all `processed/YYYY.parquet` files from the Amazon S3 Processed Bucket.
2. Removes rows containing null values (insufficient historical data for rolling-window calculations).
3. Splits the dataset chronologically into **70% Training / 30% Testing** (without shuffling to prevent data leakage).
4. Trains an `XGBoostClassifier`.
5. Evaluates the model and generates a `training_report.md` report.
6. Uploads the trained model (`xgboost_model.json`) to the Amazon S3 Model Bucket.

---

#### Algorithm & Hyperparameters

```python
XGBClassifier(
    n_estimators=200,
    max_depth=6,
    learning_rate=0.1,
    subsample=0.8,
    colsample_bytree=0.8,
    use_label_encoder=False,
    eval_metric='logloss',
    random_state=42
)
```

---

#### Training Results

![Confusion Matrix](images/4.2/confusion-matrix.png)

*Figure 4.2. Confusion Matrix of the XGBoost classification model on the test dataset, illustrating the number of correctly and incorrectly classified samples for each prediction class.*

![Feature Importance](images/4.2/feature-importance.png)

*Figure 4.3. Feature Importance scores produced by the XGBoost model, highlighting the relative contribution of each engineered feature to the stock movement prediction task.*

![ROC Curve](images/4.2/roc-curve.png)

*Figure 4.6. Receiver Operating Characteristic (ROC) Curve of the XGBoost classifier, illustrating the trade-off between the True Positive Rate and False Positive Rate across different decision thresholds.*

| Metric | Value |
|:---|:---|
| **Accuracy** | 53.12% |
| **Precision** | 0.5311 |
| **Recall** | 0.4828 |
| **F1 Score** | 0.5058 |
| **AUC-ROC** | 0.5487 |

{{%notice info%}}
**Why is the accuracy only around 53%?**

Predicting stock market movements is an extremely challenging task because financial markets are often modeled as a **Random Walk**. Achieving an accuracy above **52%** on an unseen, time-ordered test set is generally considered meaningful and can provide practical value for quantitative trading strategies.
{{%/notice%}}

---

#### Step 2: Deploy the Lambda Predictor

- **Function name:** `nasdaq-stock-predictor`
- **CMD override:** `src.lambda_stock_predictor.lambda_handler`
- **Memory:** `2048 MB`
- **Timeout:** `30 seconds`

Environment variables:

| Key | Value |
|:---|:---|
| `MODEL_BUCKET` | `my-nasdaq-stock-models-2026-ap-southeast-1` |
| `PROCESSED_BUCKET` | `my-nasdaq-stock-processed-2026-ap-southeast-1` |

The Lambda Predictor performs the following tasks:

1. Loads the trained XGBoost model from Amazon S3.
2. Retrieves the latest processed data for the requested stock ticker.
3. Performs feature engineering.
4. Returns the predicted probability `P(UP)` together with the predicted label (`1` or `0`).

![Lambda Predictor Configuration](images/4.2/lambda-predictor-config.png)

*Figure 4.5. Configuration of the Lambda Predictor function responsible for loading the trained XGBoost model and serving real-time prediction requests received from Amazon API Gateway.*

---

#### Step 3: Configure Amazon API Gateway

Create a REST API endpoint to invoke the Lambda Predictor:

```json
POST /predict
Body:
{
    "ticker": "AAPL"
}

Response:
{
    "ticker": "AAPL",
    "prediction": 1,
    "probability": 0.623,
    "label": "UP",
    "date": "2026-07-29"
}
```

![Amazon API Gateway Endpoint Configuration](images/4.2/api-gateway-endpoint.png)

*Figure 4.4. Amazon API Gateway endpoint configured to expose the stock prediction service as a RESTful API, enabling external applications to request predictions through HTTPS.*