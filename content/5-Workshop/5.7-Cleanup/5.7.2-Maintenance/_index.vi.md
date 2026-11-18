---
title: "Chuyển CloudFront sang maintenance"
date: 2026-11-18
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---

## Chuyển CloudFront sang maintenance

`edge.yaml` sử dụng `EdgeMode` để điều khiển backend origin và hành vi API. Chuyển sang maintenance sẽ gỡ backend origin khỏi distribution và cho CloudFront trả phản hồi bảo trì trước khi tháo VPC Origin của Runtime.

1. Trong **tindy-edge**, chọn **Stack actions → Create change set for current stack**.
2. Chọn **Standard change set**, **Use existing template**, rồi **Next**.
3. Đặt tên `tindy-edge-maintenance`.
4. Đổi `EdgeMode = maintenance`. Giữ các giá trị hiện tại của `FrontendBucketName`, `AlbDnsName` và `VpcOriginId`. Điều kiện trong template quyết định việc gỡ backend origin.

**📸 Chụp tham số trước khi tạo Change Set.**

![Tham số Edge maintenance](/images/5-Workshop/cleanup/cleanup-03a-edge-maintenance-parameters.png)

5. Giữ **Use existing role**, Express mode bỏ chọn, **Roll back all resources** và deployment validations bật. Tạo Change Set.

![Tùy chọn Change Set Edge](/images/5-Workshop/cleanup/cleanup-03b-edge-maintenance-options.png)

6. Chờ `CREATE_COMPLETE` và execution status `AVAILABLE`.
7. Trong Resource changes, xác nhận duy nhất `EdgeDistribution → Modify → Replacement = False`.

**📸 Chụp Resource changes trước khi Execute.**

![Change Set Edge](/images/5-Workshop/cleanup/cleanup-03-edge-maintenance-changeset.png)

8. Bấm **Execute change set**, giữ **Use deletion policy** và xác nhận.

![Xác nhận Execute Edge](/images/5-Workshop/cleanup/cleanup-03c-edge-maintenance-execute.png)

9. Mở **Events**, Refresh đến khi `tindy-edge — UPDATE_COMPLETE`.

![Edge cập nhật hoàn tất](/images/5-Workshop/cleanup/cleanup-04-edge-maintenance-complete.png)

10. Mở **Outputs**, xác nhận `EdgeMode = maintenance`.

![Outputs xác nhận maintenance](/images/5-Workshop/cleanup/cleanup-04a-edge-maintenance-outputs.png)

11. Trong PowerShell, thay `<cloudfront-hostname>` bằng `DistributionDomainName` đã lưu và chạy lệnh sau.

```powershell
curl.exe -i "https://<cloudfront-hostname>/api/health/ready"
```

Kết quả đã xác minh gồm HTTP `503 Service Unavailable`, `Server: CloudFront`, `Cache-Control: no-store` và JSON `{"status":"maintenance"}`.

**📸 Chụp lệnh, mã HTTP và JSON phản hồi trong cùng khung hình.**

![API trả HTTP 503 maintenance](/images/5-Workshop/cleanup/cleanup-05-api-maintenance.png)
