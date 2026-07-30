---
title: "Worklog Tuần 3"
date: 2026-06-22
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Phát triển các AWS Lambda Function phục vụ thu thập dữ liệu.
* Xây dựng luồng xử lý dữ liệu tự động theo kiến trúc serverless.
* Tích hợp EventBridge và Amazon SQS vào hệ thống.
* Kiểm thử quy trình thu thập dữ liệu từ Yahoo Finance.

### Công việc thực hiện trong tuần:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------ |
| 1 | Xây dựng Lambda Function thực hiện thu thập dữ liệu chứng khoán từ Yahoo Finance. | 22/06/2026 | 22/06/2026 | AWS Lambda Documentation |
| 2 | Phát triển Lambda Producer để gửi thông điệp vào Amazon SQS phục vụ xử lý song song. | 23/06/2026 | 23/06/2026 | Amazon SQS Documentation |
| 3 | Cấu hình Amazon EventBridge để tự động kích hoạt quy trình thu thập dữ liệu theo lịch định kỳ. | 24/06/2026 | 24/06/2026 | Amazon EventBridge Documentation |
| 4 | Xây dựng Lambda Consumer nhận thông điệp từ hàng đợi SQS và xử lý dữ liệu thu thập được. | 25/06/2026 | 25/06/2026 | AWS Lambda Documentation |
| 5 | Lưu dữ liệu thu thập thành công vào Amazon S3 theo cấu trúc đã thiết kế. | 26/06/2026 | 26/06/2026 | Amazon S3 Documentation |
| 6 | Thực hiện kiểm thử toàn bộ luồng EventBridge → Lambda → SQS → Lambda → Amazon S3. | 27/06/2026 | 27/06/2026 | Internal Testing |
| 7 | Khắc phục các lỗi phát sinh, tối ưu thời gian thực thi Lambda và kiểm tra dữ liệu lưu trữ trên Amazon S3. | 28/06/2026 | 28/06/2026 | Testing Report |

---

### Kết quả đạt được:

* Hoàn thành các AWS Lambda Function phục vụ quá trình thu thập dữ liệu chứng khoán.

* Thiết lập thành công cơ chế tự động kích hoạt bằng Amazon EventBridge theo lịch định kỳ.

* Xây dựng quy trình xử lý bất đồng bộ thông qua Amazon SQS nhằm nâng cao khả năng mở rộng của hệ thống.

* Thu thập thành công dữ liệu từ Yahoo Finance và lưu trữ dữ liệu vào Amazon S3 theo đúng cấu trúc đã thiết kế.

* Kiểm thử toàn bộ luồng xử lý dữ liệu từ EventBridge đến Lambda, Amazon SQS và Amazon S3, đảm bảo các thành phần hoạt động ổn định.

* Tối ưu thời gian thực thi của các Lambda Function và xử lý các lỗi phát sinh trong quá trình kiểm thử.

* Hoàn thành giai đoạn xây dựng Data Collection Pipeline và sẵn sàng triển khai quy trình ETL trong tuần tiếp theo.