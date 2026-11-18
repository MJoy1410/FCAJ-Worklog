---
title: "Chuẩn bị triển khai"
date: 2026-11-18
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

Chuẩn bị quyền truy cập AWS, repo hạ tầng, image backend và các tham chiếu tài nguyên để thực hiện phần triển khai Tindy. Region của lượt thực hành là **N. Virginia (`us-east-1`)**, khớp các ràng buộc trong template.

## 1. Công cụ và quyền truy cập

- AWS Console để upload template, review Change Set, xem Events/Outputs và chụp ảnh.
- AWS CLI đã đăng nhập với profile vận hành. Lượt này dùng `tindy-admin`.
- PowerShell để gọi CLI và kiểm tra HTTP bằng `curl.exe`.
- Quyền tạo/cập nhật/xóa các tài nguyên được định nghĩa trong sáu template, gồm CloudFormation, IAM, EC2 networking, S3, ECR, Secrets Manager, RDS, ECS, ELB, CloudFront, GuardDuty, EventBridge, SQS, SNS và CloudWatch. Khi template yêu cầu, xác nhận IAM capability trên Console.
- Hộp thư nhận cảnh báo để xác nhận SNS subscription. Khi kiểm thử OAuth, có quyền cập nhật callback trên cấu hình ứng dụng Google/GitHub theo hostname mới.

Kiểm tra phiên CLI trên máy thao tác.

```powershell
aws sts get-caller-identity --profile tindy-admin --region us-east-1 --no-cli-pager
```

**📸 Nếu chụp kết quả identity, rà các định danh tài khoản hiển thị trước khi xuất bản.**

## 2. Source và image triển khai

Sử dụng repo `tindy-infrastructure`, thư mục `cloudformation`, với đủ sáu file YAML được mô tả trong mục 5.1. Chuẩn bị image backend trong ECR `tindy-api`, pin bằng digest để API và các tác vụ DB sử dụng đúng release.

Định dạng `ApiImage` của Runtime là `<account-id>.dkr.ecr.us-east-1.amazonaws.com/tindy-api@sha256:<digest>`. Trong lượt thực hành, đã đối chiếu image digest với ECR và tham số Runtime trước cleanup. Frontend sẽ được phát hành từ repo frontend theo quy trình deploy của Tindy.

## 3. Mapping cho lượt dựng lại

Lượt thực hành bắt đầu từ hệ thống đã có sẵn. Trước khi xóa Foundation, đã xuất mapping logical ID với physical ID, tham số, Outputs và template đang triển khai vào `backup/tindy-foundation-before-cleanup` trong Worklog.

Chín tài nguyên Foundation giữ lại gồm hai S3 bucket, media bucket policy, ECR repository, bốn application secret và GitHub OIDC provider. File `foundation-import.yaml` phục vụ việc gắn lại các tài nguyên này vào stack mới, sau đó cập nhật bằng `foundation.yaml`. Chi tiết ở mục 5.3.1.

## 4. Lựa chọn và quy ước cho lượt thực hành

| Nội dung | Lựa chọn |
| --- | --- |
| Database | `Fresh`, để trống snapshot identifier, chạy bootstrap/migration/seed. |
| Region | `us-east-1`. |
| CloudFront | Tạo distribution mới, cập nhật các cấu hình phụ thuộc hostname mới. |
| API lúc tạo Runtime | `DesiredCount = 0` đến khi DB và secrets sẵn sàng. |
| Traffic | Edge maintenance đến khi API và target group healthy. |
| Tags | `Project=Tindy`, `Environment=tindy`, `Owner=TindyTeam`. |

Sử dụng Outputs từ các stack mới để điền tham số. Application secret kết nối DB được giữ lại cần cập nhật cho database Fresh. Mật khẩu, JWT signing key và OAuth client secret được quản lý trong AWS.

## 5. Chuẩn bị ảnh và video

Ảnh thực hành được lưu trong Worklog ở `static/images/5-Workshop/cleanup` và `static/images/5-Workshop/deploy`. Hướng dẫn chỉ rõ các mốc chụp tham số, Change Set, trạng thái hoàn tất và Outputs.

Video được quay sau khi seed dữ liệu và xác minh ứng dụng. Luồng dự kiến gồm đăng nhập, tìm dự án, ứng tuyển và dùng tài khoản leader để xử lý đơn. Trạng thái đã hoàn tất và các phần đang triển khai được ghi riêng trên từng trang theo bằng chứng thực tế.
