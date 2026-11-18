---
title: "Database và Integrations"
date: 2026-11-18
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Phần này chuẩn bị nơi lưu dữ liệu ứng dụng và luồng quét PDF trước khi chạy backend. Database dùng mạng đã tạo ở mục 5.3. Integrations sử dụng media bucket của Foundation và cung cấp kết quả quét cho worker trong API.

{{% notice info %}}
Lượt thực hành chọn Database Fresh. Bootstrap, migration và seed được thực hiện ở giai đoạn Runtime sau khi RDS đã tạo hoàn tất.
{{% /notice %}}

## 5.4.1 — Tạo Database Fresh

`database.yaml` tạo ba tài nguyên chính để chạy PostgreSQL và giới hạn cách truy cập database.

| Tài nguyên hoặc cấu hình | Vai trò |
| --- | --- |
| DB subnet group | Đặt RDS trong nhóm private DB subnet từ Network. |
| PostgreSQL parameter group | Bật `rds.force_ssl` để yêu cầu TLS. |
| RDS instance | Lưu dữ liệu Tindy, dùng PostgreSQL 17, lớp `db.t4g.micro`, cấu hình Single-AZ. |
| RDS-managed master secret | Cung cấp credential quản trị cho task bootstrap. |
| Deletion protection và snapshot policy | Bảo vệ thao tác xóa DB và tạo snapshot theo chính sách template. |

Chọn `DBProvisionMode = Fresh`, để snapshot identifier trống và giữ deletion protection bật. RDS tạo database `tindy` và master user `tindy_admin`. API sẽ kết nối bằng DB credential riêng được chuẩn bị sau đó.

**Kết quả cần thấy** là `CREATE_COMPLETE` với năm Outputs, gồm endpoint, port, DB identifier, master secret ARN và provision mode. Ở thời điểm này, cần tiếp tục tạo schema và dữ liệu demo trong mục 5.5.

## 5.4.2 — Triển khai Integrations

`integrations.yaml` thiết lập chuỗi xử lý kết quả quét PDF và cảnh báo vận hành.

| Thành phần | Vai trò trong luồng xử lý |
| --- | --- |
| GuardDuty Malware Protection plan | Quét object mới thuộc `certificates/pdf/` và `ai-input/pdf/` trong media bucket. |
| Tag scanned objects | Gắn kết quả quét lên object để hỗ trợ kiểm soát truy cập. |
| EventBridge rule | Chuyển sự kiện kết quả quét đến queue. |
| SQS queue và queue policy | Nhận message để worker của API xử lý. |
| Dead-letter queue và alarm | Giữ message xử lý lỗi theo redrive policy và phát hiện tồn đọng. |
| SNS topic và email subscription | Gửi cảnh báo đến email đã xác nhận. |
| Alarm ALB/API có điều kiện | Sử dụng ALB/target group mới khi cập nhật sau Runtime. |

Trong lượt tạo đầu, để trống hai ALB metric dimension vì Runtime chưa có ALB và target group mới. Sau khi stack hoàn tất, xác nhận email SNS và kiểm tra protection plan Active, tagging bật, đúng hai prefix PDF.

## Đầu vào và đầu ra

| Nguồn | Tham chiếu cần dùng |
| --- | --- |
| Network → Database | `PrivateDbSubnetIds`, `DatabaseSecurityGroupId`. |
| Foundation → Integrations | `MediaBucketName`. |
| Database → Chuẩn bị Runtime | `DBEndpointAddress`, `DBMasterSecretArn`, `DBProvisionMode`. |
| Integrations → Runtime | `GuardDutyProtectionPlanArn`, `GuardDutyQueueUrl`. |
| Runtime → Cập nhật Integrations | ALB/target group metric dimension để bật alarm tương ứng. |

## Kết quả đã xác minh

| Thành phần | Trạng thái |
| --- | --- |
| Database | `CREATE_COMPLETE`, provision mode Fresh và đủ năm Outputs. |
| Integrations | `CREATE_COMPLETE`, có bảy Outputs. |
| SNS | Đã nhận kết quả Subscription confirmed. |
| GuardDuty | Plan Active, tagging bật và hai prefix đã đối chiếu. |

Kiểm thử upload PDF qua ứng dụng sẽ được thực hiện sau khi API và schema sẵn sàng.

## Hướng dẫn thực hành

| Trang | Nội dung và bằng chứng |
| --- | --- |
| [5.4.1 Tạo Database Fresh](5.4.1-Database/) | Tham số Fresh, subnet/security group, Options và Outputs RDS. |
| [5.4.2 Triển khai Integrations](5.4.2-Integrations/) | Stack quét PDF, xác nhận SNS và kiểm tra GuardDuty plan. |
