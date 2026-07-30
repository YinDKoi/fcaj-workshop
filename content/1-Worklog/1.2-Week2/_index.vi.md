---
title: "Worklog Tuần 2"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Thiết lập môi trường AWS cho dự án.
* Cấu hình các dịch vụ nền tảng của hệ thống ETL Serverless.
* Chuẩn bị hạ tầng lưu trữ và cơ chế xử lý theo mô hình hướng sự kiện.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | -------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ---------------------- |
| 2 | - Thiết lập AWS CLI và kiểm tra thông tin tài khoản.<br>- Tạo IAM User và IAM Role phục vụ phát triển dự án. | 22/06/2026 | 22/06/2026 | AWS IAM Documentation |
| 3 | - Tạo Amazon S3 Bucket để lưu trữ dữ liệu.<br>- Thiết kế cấu trúc thư mục cho dữ liệu Raw, Processed và Model. | 23/06/2026 | 23/06/2026 | Amazon S3 Documentation |
| 4 | - Cấu hình Amazon EventBridge để tự động kích hoạt quá trình thu thập dữ liệu hằng ngày.<br>- Thiết lập lịch thực thi cho Lambda Function. | 24/06/2026 | 24/06/2026 | Amazon EventBridge Documentation |
| 5 | - Tạo và cấu hình Amazon SQS.<br>- Kiểm tra cơ chế truyền nhận Message giữa các Lambda Function. | 25/06/2026 | 25/06/2026 | Amazon SQS Documentation |
| 6 | - Kiểm tra quyền truy cập giữa các dịch vụ AWS.<br>- Kiểm thử hoạt động của toàn bộ hạ tầng Cloud và hoàn thiện tài liệu cấu hình. | 26/06/2026 | 28/06/2026 | AWS Documentation |

### Kết quả đạt được tuần 2:

* Hoàn thành việc thiết lập môi trường AWS phục vụ cho dự án.

* Tạo và cấu hình thành công các Amazon S3 Bucket để lưu trữ dữ liệu theo từng giai đoạn của hệ thống.

* Thiết lập Amazon EventBridge để tự động hóa quá trình thu thập dữ liệu theo lịch.

* Cấu hình Amazon SQS phục vụ giao tiếp bất đồng bộ giữa các Lambda Function.

* Hoàn thành cấu hình IAM Role và quyền truy cập giữa các dịch vụ AWS theo nguyên tắc phân quyền tối thiểu.

* Kiểm tra thành công kết nối giữa EventBridge, Lambda, Amazon SQS và Amazon S3, đảm bảo kiến trúc Serverless hoạt động đúng như thiết kế.

* Hoàn thiện hạ tầng AWS ban đầu, sẵn sàng triển khai các chức năng ETL và Machine Learning trong các tuần tiếp theo.