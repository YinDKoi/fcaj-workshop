---
title: "Worklog Tuần 3"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Xây dựng quy trình thu thập dữ liệu chứng khoán tự động.
* Triển khai AWS Lambda để lấy dữ liệu từ Yahoo Finance.
* Lưu trữ dữ liệu thô lên Amazon S3.
* Kiểm thử luồng thu thập dữ liệu theo mô hình hướng sự kiện.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | -------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ---------------------- |
| 2 | - Xây dựng Lambda Function thu thập dữ liệu giá cổ phiếu từ Yahoo Finance.<br>- Tìm hiểu cấu trúc dữ liệu trả về từ API. | 29/06/2026 | 29/06/2026 | Yahoo Finance Documentation |
| 3 | - Xử lý dữ liệu đầu vào và chuyển đổi sang định dạng JSON.<br>- Thiết kế cấu trúc lưu trữ dữ liệu trên Amazon S3. | 30/06/2026 | 30/06/2026 | Amazon S3 Documentation |
| 4 | - Tích hợp Lambda với Amazon EventBridge để tự động thực hiện thu thập dữ liệu theo lịch.<br>- Kiểm tra quá trình ghi dữ liệu lên S3. | 01/07/2026 | 01/07/2026 | AWS Lambda Documentation |
| 5 | - Tích hợp Amazon SQS để truyền dữ liệu sang bước xử lý tiếp theo.<br>- Kiểm tra cơ chế gửi và nhận Message. | 02/07/2026 | 03/07/2026 | Amazon SQS Documentation |
| 6 | - Thực hiện kiểm thử toàn bộ quy trình thu thập dữ liệu.<br>- Theo dõi log bằng Amazon CloudWatch và xử lý các lỗi phát sinh. | 04/07/2026 | 05/07/2026 | Amazon CloudWatch Documentation |

### Kết quả đạt được tuần 3:

* Hoàn thành Lambda Function thu thập dữ liệu chứng khoán từ Yahoo Finance.

* Xây dựng thành công quy trình tự động thu thập dữ liệu theo lịch thông qua Amazon EventBridge.

* Lưu trữ dữ liệu thô lên Amazon S3 theo cấu trúc đã thiết kế.

* Tích hợp Amazon SQS để chuyển dữ liệu giữa các thành phần của hệ thống.

* Theo dõi và kiểm tra hoạt động của Lambda thông qua Amazon CloudWatch nhằm đảm bảo quá trình thu thập dữ liệu diễn ra ổn định.

* Hoàn thiện giai đoạn Data Ingestion, tạo nền tảng cho bước kiểm tra chất lượng dữ liệu và xử lý ETL trong tuần tiếp theo.