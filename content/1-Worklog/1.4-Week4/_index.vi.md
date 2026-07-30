---
title: "Worklog Tuần 4"
date: 2026-06-29
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Xây dựng quy trình ETL để xử lý dữ liệu chứng khoán.
* Thực hiện làm sạch, chuyển đổi và chuẩn hóa dữ liệu.
* Thiết kế các đặc trưng (Feature Engineering) phục vụ mô hình Machine Learning.
* Kiểm thử chất lượng dữ liệu sau khi xử lý.

### Công việc thực hiện trong tuần:

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------ |
| 1 | Phân tích cấu trúc dữ liệu thu thập từ Yahoo Finance và xác định các trường dữ liệu cần xử lý. | 29/06/2026 | 29/06/2026 | Yahoo Finance Documentation |
| 2 | Xây dựng Lambda Function thực hiện quy trình ETL, bao gồm làm sạch dữ liệu và xử lý các giá trị thiếu. | 30/06/2026 | 30/06/2026 | AWS Lambda Documentation |
| 3 | Chuẩn hóa định dạng dữ liệu, chuyển đổi kiểu dữ liệu và loại bỏ các bản ghi không hợp lệ trước khi lưu trữ. | 01/07/2026 | 01/07/2026 | ETL Design Notes |
| 4 | Thực hiện Feature Engineering bằng cách tạo các đặc trưng kỹ thuật phục vụ cho quá trình huấn luyện mô hình dự đoán. | 02/07/2026 | 02/07/2026 | Project Documentation |
| 5 | Lưu dữ liệu đã xử lý vào Amazon S3 theo cấu trúc dữ liệu Processed và kiểm tra tính toàn vẹn của dữ liệu. | 03/07/2026 | 03/07/2026 | Amazon S3 Documentation |
| 6 | Kiểm thử toàn bộ quy trình ETL với nhiều bộ dữ liệu khác nhau và đánh giá kết quả xử lý. | 04/07/2026 | 04/07/2026 | Internal Testing |
| 7 | Tối ưu hiệu suất xử lý dữ liệu, sửa các lỗi phát sinh và hoàn thiện quy trình ETL. | 05/07/2026 | 05/07/2026 | Testing Report |

---

### Kết quả đạt được:

* Hoàn thành quy trình ETL phục vụ xử lý dữ liệu chứng khoán.

* Xây dựng thành công Lambda Function thực hiện các bước làm sạch, chuẩn hóa và chuyển đổi dữ liệu.

* Thiết kế và tạo các đặc trưng (Feature Engineering) nhằm nâng cao chất lượng dữ liệu đầu vào cho mô hình Machine Learning.

* Lưu trữ dữ liệu đã xử lý vào Amazon S3 theo đúng cấu trúc của hệ thống.

* Kiểm thử thành công quy trình ETL với nhiều bộ dữ liệu khác nhau và đảm bảo dữ liệu sau xử lý đáp ứng yêu cầu của mô hình.

* Tối ưu hiệu suất xử lý dữ liệu và khắc phục các lỗi phát sinh trong quá trình triển khai.

* Hoàn thành giai đoạn xây dựng ETL Pipeline và chuẩn bị dữ liệu cho quá trình huấn luyện mô hình Machine Learning.