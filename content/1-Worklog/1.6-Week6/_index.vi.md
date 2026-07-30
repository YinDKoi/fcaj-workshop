---
title: "Worklog Tuần 6"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Triển khai dịch vụ dự đoán giá cổ phiếu.
* Xây dựng API phục vụ truy xuất kết quả dự đoán.
* Lưu trữ kết quả dự đoán trên Amazon DynamoDB.
* Phát triển Dashboard trực quan hóa dữ liệu và kết quả dự đoán.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | -------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ---------------------- |
| 2 | - Triển khai Lambda Function phục vụ dự đoán giá cổ phiếu.<br>- Tích hợp mô hình XGBoost đã huấn luyện vào Lambda. | 20/07/2026 | 20/07/2026 | AWS Lambda Documentation |
| 3 | - Thiết kế và cấu hình Amazon DynamoDB để lưu trữ kết quả dự đoán.<br>- Kiểm tra khả năng ghi và đọc dữ liệu. | 21/07/2026 | 21/07/2026 | Amazon DynamoDB Documentation |
| 4 | - Xây dựng REST API bằng Amazon API Gateway.<br>- Kết nối API Gateway với Lambda Predictor. | 22/07/2026 | 22/07/2026 | Amazon API Gateway Documentation |
| 5 | - Phát triển Dashboard bằng Streamlit.<br>- Hiển thị dữ liệu lịch sử và kết quả dự đoán theo thời gian. | 23/07/2026 | 24/07/2026 | Streamlit Documentation |
| 6 | - Kiểm thử toàn bộ quy trình dự đoán từ API đến Dashboard.<br>- Kiểm tra hiệu năng và xử lý các lỗi phát sinh. | 25/07/2026 | 26/07/2026 | Project Documentation |

### Kết quả đạt được tuần 6:

* Triển khai thành công Lambda Function phục vụ dự đoán giá cổ phiếu bằng mô hình XGBoost.

* Xây dựng API thông qua Amazon API Gateway để cung cấp kết quả dự đoán cho các ứng dụng bên ngoài.

* Hoàn thành cấu hình Amazon DynamoDB để lưu trữ và truy xuất dữ liệu dự đoán.

* Phát triển Dashboard bằng Streamlit giúp trực quan hóa dữ liệu lịch sử và kết quả dự đoán.

* Kiểm thử thành công luồng xử lý từ yêu cầu dự đoán, thực thi mô hình, lưu trữ kết quả đến hiển thị trên Dashboard.

* Hoàn thiện dịch vụ dự đoán và sẵn sàng cho giai đoạn kiểm thử tổng thể và hoàn thiện hệ thống.