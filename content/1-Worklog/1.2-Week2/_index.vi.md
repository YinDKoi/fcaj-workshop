---
title: "Worklog Tuần 2"
date: 2026-06-15
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Xây dựng hạ tầng AWS phục vụ dự án.
* Chuẩn bị các tài nguyên cần thiết cho Data Pipeline.
* Thiết kế kiến trúc lưu trữ dữ liệu trên Amazon S3.
* Thiết lập quyền truy cập và môi trường phát triển trên AWS.

### Công việc thực hiện trong tuần:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------ |
| 1 | Thiết lập tài khoản AWS, cấu hình IAM User và IAM Role phục vụ cho quá trình phát triển và triển khai hệ thống. | 15/06/2026 | 15/06/2026 | AWS IAM Documentation |
| 2 | Tạo các Amazon S3 Bucket phục vụ lưu trữ dữ liệu Raw, Cleansed, Processed và Machine Learning Model. | 16/06/2026 | 16/06/2026 | Amazon S3 Documentation |
| 3 | Thiết kế cấu trúc thư mục lưu trữ dữ liệu trên S3, thống nhất quy tắc đặt tên và tổ chức dữ liệu theo từng giai đoạn của pipeline. | 17/06/2026 | 17/06/2026 | Design Notes |
| 4 | Tìm hiểu Amazon EventBridge và xây dựng cơ chế tự động kích hoạt pipeline theo lịch hằng ngày. | 18/06/2026 | 18/06/2026 | EventBridge Documentation |
| 5 | Nghiên cứu Amazon SQS và mô hình Fan-Out để xử lý song song dữ liệu chứng khoán từ nhiều mã cổ phiếu. | 19/06/2026 | 19/06/2026 | Amazon SQS Documentation |
| 6 | Hoàn thiện sơ đồ kiến trúc AWS, xác định luồng tương tác giữa các dịch vụ trong hệ thống. | 20/06/2026 | 20/06/2026 | AWS Architecture Center |
| 7 | Kiểm tra toàn bộ hạ tầng đã triển khai, rà soát quyền truy cập và chuẩn bị cho giai đoạn phát triển Lambda Functions. | 21/06/2026 | 21/06/2026 | Internal Review |

---

### Kết quả đạt được:

* Hoàn thành việc thiết lập môi trường AWS phục vụ cho dự án.

* Cấu hình thành công IAM User, IAM Role và các quyền truy cập cần thiết cho quá trình phát triển.

* Tạo và tổ chức các Amazon S3 Bucket theo từng giai đoạn xử lý dữ liệu:
  * Raw Data
  * Cleansed Data
  * Processed Data
  * Model Storage

* Hiểu rõ cách EventBridge hoạt động để lập lịch tự động cho Data Pipeline.

* Nghiên cứu mô hình xử lý bất đồng bộ bằng Amazon SQS nhằm tăng khả năng mở rộng và xử lý song song.

* Hoàn thiện bản thiết kế kiến trúc AWS và xác định luồng dữ liệu giữa các thành phần của hệ thống.

* Hoàn thành toàn bộ công việc chuẩn bị hạ tầng, sẵn sàng bước sang giai đoạn phát triển các Lambda Function trong tuần tiếp theo.