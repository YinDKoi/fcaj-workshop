---
title: "Worklog Tuần 6"
date: 2026-07-13
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Triển khai mô hình Machine Learning lên môi trường AWS.
* Xây dựng API phục vụ dự đoán giá cổ phiếu.
* Tích hợp các thành phần của hệ thống Serverless.
* Kiểm thử toàn bộ quy trình từ thu thập dữ liệu đến trả kết quả dự đoán.

### Công việc thực hiện trong tuần:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------ |
| 1 | Xây dựng AWS Lambda phục vụ suy luận (Inference) bằng cách tải mô hình đã huấn luyện từ Amazon S3. | 13/07/2026 | 13/07/2026 | AWS Lambda Documentation |
| 2 | Cấu hình Amazon API Gateway để cung cấp REST API cho chức năng dự đoán giá cổ phiếu. | 14/07/2026 | 14/07/2026 | Amazon API Gateway Documentation |
| 3 | Tích hợp API Gateway với Lambda Function và kiểm tra luồng xử lý yêu cầu từ người dùng. | 15/07/2026 | 15/07/2026 | AWS Documentation |
| 4 | Kiểm thử API bằng nhiều bộ dữ liệu đầu vào, đánh giá tính chính xác của kết quả dự đoán và xử lý các trường hợp ngoại lệ. | 16/07/2026 | 16/07/2026 | API Testing Guide |
| 5 | Theo dõi hoạt động của hệ thống bằng Amazon CloudWatch, kiểm tra log và xử lý các lỗi phát sinh trong quá trình triển khai. | 17/07/2026 | 17/07/2026 | Amazon CloudWatch Documentation |
| 6 | Tối ưu thời gian phản hồi của API và hiệu suất thực thi của Lambda Function. | 18/07/2026 | 18/07/2026 | Performance Testing |
| 7 | Thực hiện kiểm thử toàn bộ hệ thống từ thu thập dữ liệu, ETL, dự đoán đến trả kết quả thông qua API. | 19/07/2026 | 19/07/2026 | System Testing Report |

---

### Kết quả đạt được:

* Triển khai thành công mô hình Machine Learning lên môi trường AWS.

* Xây dựng REST API cho phép người dùng gửi yêu cầu dự đoán và nhận kết quả thông qua Amazon API Gateway.

* Tích hợp hoàn chỉnh API Gateway với AWS Lambda và mô hình Machine Learning.

* Sử dụng Amazon CloudWatch để giám sát hoạt động của hệ thống và hỗ trợ xử lý lỗi trong quá trình triển khai.

* Kiểm thử thành công toàn bộ quy trình xử lý từ thu thập dữ liệu, ETL, dự đoán đến trả kết quả cho người dùng.

* Tối ưu hiệu suất của Lambda Function và cải thiện thời gian phản hồi của API.

* Hoàn thành giai đoạn triển khai hệ thống và chuẩn bị cho quá trình kiểm thử tổng thể, hoàn thiện tài liệu trong tuần cuối.