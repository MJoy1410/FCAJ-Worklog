---
title: "Dọn dẹp tài nguyên"
date: 2026-11-18
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

Phần này ghi lại quá trình tháo hạ tầng Tindy bằng CloudFormation tại **N. Virginia (`us-east-1`)**. Hệ thống đã có sẵn nên lượt thực hành bắt đầu bằng cleanup. Sau đó dựng lại sáu stack, tạo database **Fresh**, chạy bootstrap, migration và seed dữ liệu demo để quay video sử dụng web trên hostname CloudFront mới.

## Tiến độ đã xác minh

| Thành phần | Trạng thái có bằng chứng |
| --- | --- |
| Edge | Đã xác minh maintenance, tắt termination protection và xóa stack thành công với `DELETE_COMPLETE`. |
| Runtime | Đã chuyển ingress ALB sang prefix list và xóa stack thành công. |
| Database | Đã tắt deletion protection và xóa stack thành công, xác nhận `DELETE_COMPLETE`. |
| Integrations | Đã xóa thành công, stack và các tài nguyên hiển thị trong ảnh đạt `DELETE_COMPLETE`. |
| Network | Đã tắt termination protection và xóa stack thành công với `DELETE_COMPLETE`. |
| Foundation | Đã lưu mapping và xóa stack thành công với `DELETE_COMPLETE`. Tài nguyên giữ lại có trạng thái `DELETE_SKIPPED`. |

## Các bước cleanup

| Trang hướng dẫn |
| --- |
| [5.7.1 Kiểm tra hạ tầng trước cleanup](5.7.1-Inventory/) |
| [5.7.2 Chuyển CloudFront sang maintenance](5.7.2-Maintenance/) |
| [5.7.3 Chuẩn bị và xóa Runtime](5.7.3-Runtime/) |
| [5.7.4 Tắt bảo vệ và xóa Database](5.7.4-Database/) |
| [5.7.5 Kiểm tra và xóa Integrations](5.7.5-Integrations/) |
| [5.7.6 Tắt bảo vệ và xóa Edge](5.7.6-Edge/) |
| [5.7.7 Tắt bảo vệ và xóa Network](5.7.7-Network/) |
| [5.7.8 Lưu mapping và xóa Foundation](5.7.8-Foundation/) |
| [5.7.9 Kết quả cleanup và chuẩn bị dựng lại](5.7.9-Results/) |
