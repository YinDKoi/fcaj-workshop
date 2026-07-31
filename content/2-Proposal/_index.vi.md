---
title : "Đề xuất dự án"
date: ""
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

#### Đề xuất dự án

*Tài liệu đề xuất dự án chi tiết bao gồm tổng quan bài toán, mục tiêu cốt lõi, kiến trúc AWS Cloud, mô hình kho dữ liệu, bộ máy kiểm thử chiến lược Backtesting và kết quả mong đợi.*

---

#### Tóm tắt thông tin dự án

| Mục | Nội dung |
|:---|:---|
| **Tên dự án** | END-TO-END DATA PIPELINE & BACKTESTING ENGINE FOR STOCK MARKET DIRECTION |
| **Tác giả** | Vương Nhật Minh - 2212094, Phạm Đức Hoài Nam - 2212157, Doãn Anh Khôi - 2352601, Trần Quốc Khánh - 2311538, Nguyễn Huy Lượng - 2311997 |
| **Người hướng dẫn** | Lữ Hoàn Thiện |
| **Ngày bắt đầu** | 15/06/2026 |
| **Ngày kết thúc** | 15/08/2026 |

---

### 1. Tổng quan bài toán và Mục tiêu cốt lõi

Hệ thống được thiết kế nhằm tự động hóa toàn bộ chu trình từ thu thập dữ liệu chứng khoán, xử lý chuẩn hóa, huấn luyện mô hình học máy đến kiểm thử chiến lược (Backtesting) và đưa ra dự báo xu hướng giá cổ phiếu hằng ngày.

#### 1.1. Bài toán Core (Core Objective)
* **Mô tả:** Phân loại nhị phân xu hướng dịch chuyển giá cổ phiếu cho ngày giao dịch tiếp theo T+1.
* **Định nghĩa Nhãn (Target Label):**
  * **Label 1 (Tăng):** (T+1) > (T)
  * **Label 0 (Giảm hoặc Đi ngang):** (T+1) <= (T)

#### 1.2. Bài toán Mở rộng (Business Value Extension)
* **Top Stock Recommendation:** Xếp hạng và gợi ý danh mục cổ phiếu tiềm năng nhất trong ngày dựa trên xác suất chiến thắng (Confidence Score) đầu ra của mô hình Core.

#### 1.3. Mục tiêu Hệ thống
* Tự động hóa hoàn toàn Luồng dữ liệu (Data Pipeline) và chu trình MLOps trên nền tảng điện toán đám mây AWS.
* Cung cấp giao diện tương tác trực quan, độ trễ thấp cho người dùng cuối thông qua ứng dụng Streamlit Dashboard.

---

### 2. Kiến trúc Hệ thống (AWS Architecture)

Hệ thống vận hành dựa trên kiến trúc **Serverless**, chia làm **ba luồng xử lý chính**:

#### Luồng A – Backfill Dữ liệu Lịch sử
Thu thập dữ liệu cổ phiếu NASDAQ từ năm 1962 đến nay, thực hiện Feature Engineering và lưu dưới dạng file Apache Parquet theo từng năm (`processed/YYYY.parquet`) vào S3.

#### Luồng B – Cập nhật Hàng ngày (Data Pipeline)
Pipeline này chạy mỗi ngày giao dịch, gồm 5 Lambda Function với kiến trúc **Fan-Out**:

1. **EventBridge** kích hoạt `nasdaq-daily-collector` (Lambda Producer) hàng ngày.
2. `nasdaq-daily-collector` đọc `tickers.json` từ S3, chia tickers thành các chunk (CHUNK_SIZE=100), và đẩy từng chunk vào SQS queue `daily-collector-queue`.
3. **SQS Fan-Out**: `nasdaq-collector-producer` (Lambda Consumer) nhận SQS message song song và tải dữ liệu OHLCV từ Yahoo Finance, ghi Parquet vào `S3 Raw Zone` (`raw/{SYMBOL}.parquet`).
4. `nasdaq-quality-gate` kiểm duyệt dữ liệu — dữ liệu hợp lệ vào `cleansed_daily/YYYY-MM-DD/`, dữ liệu lỗi vào `quarantine/YYYY-MM-DD/` kèm metadata.
5. `nasdaq-daily-etl` đọc dữ liệu sạch, thực hiện **Feature Engineering** bằng **Polars**, gộp vào file Parquet chính (`processed/YYYY.parquet`), và xóa buffer tạm.

Tất cả Lambda Function được đóng gói bằng **Docker** và đẩy lên **Amazon ECR**.

```text
[EventBridge] ──trigger──> [nasdaq-daily-collector]
                                  │
                          Đọc tickers.json từ S3
                                  │
                          Chia thành các chunk 100
                                  │
                          Đẩy vào SQS Queue
                                  │
                                  ▼
                    ┌── SQS: daily-collector-queue ──┐
                    │           (Fan-Out)             │
                    ▼                                 ▼
          [nasdaq-collector-producer]    [nasdaq-collector-producer]
                    │          (xử lý song song)           │
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

#### Luồng C – Dự đoán & Phục vụ
* **Huấn luyện mô hình:** Mô hình XGBoost được huấn luyện offline trên bộ feature đã xử lý và lưu vào `S3 Model Registry`.
* **Batch Inference:** EventBridge kích hoạt `nasdaq-stock-predictor` hàng ngày để tải model mới nhất, lấy feature mới, dự đoán xu hướng cho tất cả mã NASDAQ, và lưu kết quả vào **Amazon DynamoDB**.
* **API Serving:** Người dùng tương tác trên Streamlit Dashboard → Gửi request qua **AWS API Gateway** → `nasdaq-stock-predictor` truy vấn từ DynamoDB trả về kết quả ngay lập tức.

```text
            [EventBridge] ──────trigger──────> [nasdaq-stock-predictor]
                                                      │
                                          Tải model từ S3 Model Registry
                                                      │
                                          Lấy features từ Feature Store
                                                      │
                                          Chạy inference (XGBoost)
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

#### Đóng gói Container
Tất cả Lambda Function được đóng gói bằng **Docker** và đẩy lên **Amazon ECR** để hỗ trợ các thư viện lớn:
* `Polars` & `PyArrow`
* `Pandera` & `Scikit-Learn` / `XGBoost`

---

### 3. Chi tiết các Thành phần Kỹ thuật

#### 3.1. Dữ liệu và Feature Engineering
* **Dữ liệu đầu vào (OHLCV):** Open, High, Low, Close, Adj_Close, Volume.
* **Danh sách Đặc trưng (16 chỉ báo):**
  * **Chỉ báo Xu hướng (Trend):** SMA_5, SMA_20, EMA_12, EMA_26
  * **Chỉ báo Động lượng (Momentum):** MACD, MACD_Signal, MACD_Hist, RSI_14
  * **Chỉ báo Biến động (Volatility):** BB_Upper, BB_Lower, BB_Width, Intraday_Volatility
  * **Đặc trưng Lag & Return:** Lag_Close_1, Lag_Close_2, Lag_Close_3, Daily_Return
  * **Nhãn dự đoán:** Label (1 = Tăng, 0 = Giảm)

#### 3.2. Cấu trúc Parquet sau Feature Engineering
Mỗi file `processed/YYYY.parquet` chứa các cột:

| Nhóm | Cột |
|:---|:---|
| **Raw OHLCV** | `Date`, `Symbol`, `Open`, `High`, `Low`, `Close`, `Adj_Close`, `Volume` |
| **Chỉ báo Xu hướng** | `SMA_5`, `SMA_20`, `EMA_12`, `EMA_26` |
| **Chỉ báo Động lượng** | `MACD`, `MACD_Signal`, `MACD_Hist`, `RSI_14` |
| **Chỉ báo Biến động** | `BB_Upper`, `BB_Lower`, `BB_Width`, `Intraday_Volatility` |
| **Lag & Return** | `Lag_Close_1`, `Lag_Close_2`, `Lag_Close_3`, `Daily_Return` |
| **Nhãn dự đoán** | `Label` (1 = Tăng, 0 = Giảm) |

#### 3.3. Bảng tổng hợp Lambda Functions

| Lambda Function | Handler | Memory | Timeout | Trigger |
|:---|:---|:---:|:---:|:---|
| `nasdaq-daily-collector` | `src.lambda_daily_collector.lambda_handler` | 512 MB | 5 phút | Amazon EventBridge (Hàng ngày) |
| `nasdaq-collector-producer` | `src.lambda_collector_producer.lambda_handler` | 1024 MB | 15 phút | Amazon SQS Trigger |
| `nasdaq-quality-gate` | `src.lambda_quality_gate.lambda_handler` | 512 MB | 5 phút | Amazon S3 Event |
| `nasdaq-daily-etl` | `src.lambda_daily_etl.lambda_handler` | 3008 MB | 15 phút | Amazon S3 Event |
| `nasdaq-stock-predictor` | `src.lambda_stock_predictor.lambda_handler` | 2048 MB | 30 giây | Amazon API Gateway |

#### 3.4. Bộ máy Kiểm thử (Backtesting Engine)
Mô phỏng hiệu năng tài chính của mô hình dựa trên quy tắc giao dịch:
* **Tín hiệu Mua/Nắm giữ:** Khi Model dự đoán nhãn 1 (Xu hướng tăng).
* **Tín hiệu Bán/Đứng ngoài:** Khi Model dự đoán nhãn 0 (Xu hướng giảm/đi ngang).
* **Chỉ số đánh giá chính (Financial Metrics):**
  * **Total Profit:** Lợi nhuận ròng cuối kỳ.
  * **Win Rate:** Tỷ lệ lệnh sinh lời trên tổng số lệnh cấu thành.
  * **Sharpe Ratio:** Đo lường hiệu suất sinh lời trên một đơn vị rủi ro.
  * **Max Drawdown:** Mức sụt giảm vốn lớn nhất tính từ đỉnh vốn (Đo lường rủi ro hệ thống).
  * **Đối chứng:** So sánh trực tiếp hiệu quả của chiến lược với phương pháp Mua và Nắm giữ thụ động (Buy & Hold).
