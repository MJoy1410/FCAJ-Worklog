---
title: "Runtime, seed và Edge"
date: 2026-11-18
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

Phần này kết nối các stack đã tạo thành môi trường chạy Tindy. Edge cung cấp hostname HTTPS. Runtime chạy API và tạo đường kết nối qua CloudFront VPC Origin đến internal ALB. Các task chuẩn bị database tạo quyền DB, schema và dữ liệu demo trước khi mở API.

{{% notice info %}}
Giữ Edge ở maintenance và Runtime với DesiredCount 0 trong lúc chuẩn bị secrets, bootstrap, migration và seed. Mở traffic sau khi các tác vụ DB hoàn tất và API đạt kiểm tra health.
{{% /notice %}}

## 5.5.1 — Tạo Edge ở chế độ maintenance

`edge.yaml` được tạo trước Runtime để lấy `DistributionDomainName`. Hostname này là đầu vào `CloudFrontPublicHost` của API và cơ sở cập nhật callback OAuth khi kiểm thử đăng nhập.

| Thành phần | Vai trò |
| --- | --- |
| CloudFront distribution | Cung cấp điểm truy cập HTTPS cho frontend và API. |
| Origin Access Control | Cho CloudFront truy cập frontend S3 origin theo chính sách bucket. |
| Frontend bucket policy | Giới hạn truy cập origin theo distribution được cấu hình. |
| Maintenance routing function | Trả HTTP 503 và JSON maintenance cho đường API trong lúc backend đang chuẩn bị. |

Ở lượt tạo đầu, `EdgeMode = maintenance`, để trống `AlbDnsName` và `VpcOriginId`. Khi Runtime có Outputs và API healthy, cập nhật Edge sang live với hai giá trị mới.

**Kết quả cần thấy** là stack CREATE_COMPLETE, ba Outputs và request HTTPS tới `/api/health/ready` trả HTTP 503 cùng JSON maintenance.

## 5.5.2 — Tạo Runtime với DesiredCount bằng 0

`runtime.yaml` nhận đầu vào từ cả Foundation, Network, Database, Integrations và Edge.

| Nhóm tài nguyên | Vai trò |
| --- | --- |
| ECS cluster, task definition và service | Xác định image API, biến môi trường, secret references, CPU/memory và số task. |
| IAM execution role và task role | Hỗ trợ khởi chạy container và quyền AWS của ứng dụng. |
| Internal ALB, listener và target group | Nhận traffic từ CloudFront VPC Origin, chuyển đến container và kiểm tra health. |
| CloudFront VPC Origin | Kết nối distribution với internal ALB. |
| Security group ingress | Giới hạn nguồn truy cập ALB theo cấu hình CloudFront. |
| CloudWatch log group | Ghi log API và các tác vụ DB. |
| One-off task definition có điều kiện | Chạy bootstrap, migration và seed sau khi cung cấp đủ tham số. |

Runtime ban đầu dùng image ECR pin theo digest, `DBProvisionMode = Fresh` và `DesiredCount = 0`. OAuth, media và Bedrock text theo cấu hình đã đối chiếu. AI PDF được giữ false trong lúc chuẩn bị schema. Sau khi service bằng 0 đã tồn tại, chuẩn bị secrets rồi cập nhật tham số để tạo các task DB.

## Các chặng sau khi Runtime tạo hoàn tất

| Thứ tự | Công việc | Kết quả cần kiểm tra |
| --- | --- | --- |
| 1 | Chuẩn bị application/migration/seed secrets, đối chiếu endpoint DB mới. | Các secret reference trỏ đúng cấu hình Fresh. |
| 2 | Bổ sung tham số one-off task qua Change Set. | Có task definition bootstrap, migration và seed trong Outputs. |
| 3 | Chạy bootstrap → migration → seed theo thứ tự. | Mỗi task kết thúc với exit code 0 và log hoàn tất. |
| 4 | Cập nhật ingress CloudFront service SG và tăng DesiredCount lên 1. | ECS ổn định và target group Healthy. |
| 5 | Gắn alarm theo ALB/target group mới, cập nhật Edge sang live. | API readiness trả 200 qua hostname mới. |
| 6 | Đối chiếu OAuth, phát hành frontend và nghiệm thu các luồng demo. | Frontend/API cùng hoạt động qua CloudFront. |

Các chặng này sẽ có hướng dẫn cùng ảnh tương ứng khi thực hiện. Trong cấu hình hiện tại, database API credential và migration credential được dùng theo vai trò riêng, còn master secret phục vụ bootstrap.

## Tiến độ thực hành

| Thành phần | Trạng thái đã xác minh |
| --- | --- |
| Edge | UPDATE_COMPLETE, EdgeMode live, readiness qua CloudFront trả HTTP 200 Healthy. |
| Runtime | UPDATE_COMPLETE, có chín Outputs. Secrets và ba task definition DB đã sẵn sàng. |
| Bootstrap, migration và seed | Cả ba task hoàn tất với exit code 0 và log xác nhận. API đã chạy, chờ kiểm thử web. |
| API live và kiểm thử web | Runtime Desired 1 / Running 1 / Pending 0, ALB 1 Healthy. Edge đã live, readiness qua CloudFront HTTP 200 Healthy. Đăng nhập demo và Discover Projects đã hiển thị dữ liệu. Các luồng khác chờ kiểm tra. |

## Hướng dẫn thực hành

| Trang | Nội dung và bằng chứng |
| --- | --- |
| [5.5.1 Tạo Edge ở chế độ maintenance](5.5.1-Edge-Maintenance/) | Tạo distribution, lấy hostname và kiểm tra HTTP 503. |
| [5.5.2 Tạo Runtime với DesiredCount bằng 0](5.5.2-Runtime/) | Giải thích 28 tham số, tạo ECS/ALB/VPC Origin và lấy Outputs. |
| [5.5.3 Chuẩn bị secrets cho Database Fresh](5.5.3-Secrets/) | Kiểm tra ECS bằng 0, chạy script và lấy tham chiếu cho task DB. |
| [5.5.4 Chạy bootstrap, migration và seed](5.5.4-Database-Tasks/) | Cấu hình Fargate, kiểm tra exit code và log theo thứ tự. |
| [5.5.5 Bật API và kiểm tra Runtime](5.5.5-Activate-Runtime/) | Lấy CloudFront service SG, cập nhật ingress và khởi chạy API. |
| [5.5.6 Chuyển Edge sang live](5.5.6-Edge-Live/) | Lấy Runtime Outputs và review Change Set routing API qua CloudFront. |
| [5.5.7 Gắn lại quyền CI/CD](5.5.7-CICD/) | Khôi phục role deploy và đối chiếu repository variables. |
