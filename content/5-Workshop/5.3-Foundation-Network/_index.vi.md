---
title: "Foundation và Network"
date: 2026-11-18
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

Phần này xây dựng nền tảng lưu trữ, cấu hình ứng dụng và mạng cho Tindy. Foundation cung cấp bucket, image registry và các tham chiếu secrets. Network tạo VPC, subnet và security group để Database và Runtime có thể được triển khai ở các phần sau.

{{% notice info %}}
Lượt thực hành bắt đầu sau cleanup. Chín tài nguyên Foundation đã được giữ lại, vì vậy cần import chúng vào stack mới trước khi cập nhật về template chính.
{{% /notice %}}

## 5.3.1 — Triển khai Foundation

`foundation.yaml` quản lý các tài nguyên dùng xuyên suốt quá trình vận hành và các IAM role có điều kiện cho GitHub Actions.

| Nhóm tài nguyên | Vai trò trong Tindy |
| --- | --- |
| Frontend bucket | Lưu bản build frontend, sau đó được CloudFront truy cập qua Origin Access Control. |
| Media bucket và bucket policy | Lưu file upload, áp dụng chính sách truy cập và các điều kiện phục vụ luồng quét PDF. |
| ECR repository | Lưu image API, sử dụng URI pin theo digest khi tạo Runtime. |
| Application secrets | Cung cấp tham chiếu cấu hình DB, JWT và OAuth cho task ECS. |
| GitHub OIDC provider | Cho phép role triển khai xác thực GitHub Actions qua OIDC. |
| IAM role triển khai/vận hành | Cấp quyền cho các workflow theo tham số bật tương ứng. |

Thực hành gồm hai lượt. Đầu tiên dùng `foundation-import.yaml` để đưa chín tài nguyên được giữ vào stack. Sau khi `IMPORT_COMPLETE`, cập nhật bằng `foundation.yaml` để khôi phục Outputs và tạo lại `InfraPlannerRole`. Các role phụ thuộc vào CloudFront và Runtime được cấu hình khi đã có đầu vào mới.

**Kết quả cần thấy** là stack `UPDATE_COMPLETE`, có Outputs để lấy tên bucket, ECR URI, ARN secrets, OIDC provider và role hiện được bật.

## 5.3.2 — Triển khai Network

`network.yaml` tạo VPC `10.20.0.0/16` và phân chia subnet theo nhiệm vụ. Hai subnet của mỗi nhóm nằm ở hai Availability Zone để cung cấp lựa chọn mạng cho các dịch vụ.

| Thành phần | Nơi sử dụng |
| --- | --- |
| Public task subnets | Chạy Fargate API và các one-off task với public IP được gán theo cấu hình ECS. |
| Private app subnets | Đặt internal ALB, kết nối với CloudFront qua VPC Origin. |
| Private DB subnets | Tạo subnet group cho RDS PostgreSQL. |
| Internet Gateway và route tables | Định tuyến theo từng nhóm subnet. |
| Security groups và ingress rules | Kiểm soát kết nối giữa ALB, API, migration task và database. |

Template có 26 tài nguyên và chín Outputs. Dùng các ID subnet/security group mới ở mục 5.4 và 5.5. Nhóm subnet ở hai Availability Zone cung cấp hạ tầng mạng, còn số lượng API task và chế độ Multi-AZ của DB phụ thuộc cấu hình Runtime/Database.

## Quan hệ giữa các stack

| Đầu ra của mục này | Đầu vào của phần tiếp theo |
| --- | --- |
| `MediaBucketName` | GuardDuty plan trong Integrations và media của Runtime. |
| `FrontendBucketName` | S3 origin và bucket policy của Edge. |
| `ApiRepositoryUri`, ARN secrets | Image và cấu hình ứng dụng của Runtime. |
| `PrivateDbSubnetIds`, `DatabaseSecurityGroupId` | RDS trong mục 5.4.1. |
| VPC, subnet và security group của API/ALB | Runtime trong mục 5.5.2. |

## Kết quả đã xác minh

| Thành phần | Trạng thái |
| --- | --- |
| Foundation | Import thành công, cập nhật template chính đạt `UPDATE_COMPLETE`, có 14 Outputs ở giai đoạn hiện tại. |
| Network | `CREATE_COMPLETE`, có đủ chín Outputs. |

## Hướng dẫn thực hành

| Trang | Nội dung và bằng chứng |
| --- | --- |
| [5.3.1 Triển khai Foundation](5.3.1-Foundation/) | Import tài nguyên giữ lại, khôi phục template chính, review role và Outputs. |
| [5.3.2 Triển khai Network](5.3.2-Network/) | Tạo stack mạng, gắn tag và đối chiếu Outputs mới. |
