---
title: "Tắt bảo vệ và xóa Edge"
date: 2026-11-18
weight: 6
chapter: false
pre: " <b> 5.7.6. </b> "
---

## Chuẩn bị xóa Edge và xử lý termination protection

Sau khi Integrations đã xóa, chọn **CloudFormation → Stacks → Active → tindy-edge → Resources**. Stack có bốn tài nguyên gồm `EdgeDistribution`, `FrontendBucketPolicy`, `FrontendOriginAccessControl` và `MaintenanceRoutingFunction`.

**📸 Chụp danh sách tài nguyên trước khi xóa.**

![Tài nguyên Edge trước khi xóa](/images/5-Workshop/cleanup/cleanup-21-edge-resources-before.png)

Khi chọn **Delete stack**, Console báo stack đang bật **termination protection** và yêu cầu tắt trước khi xóa.

![Edge đang bật termination protection](/images/5-Workshop/cleanup/cleanup-21a-edge-termination-protection.png)

1. Bấm **Edit termination protection** trong thông báo.
2. Kiểm tra tiêu đề hiển thị đúng `tindy-edge`, chọn **Deactivated**.

**📸 Chụp lựa chọn Deactivated trước khi bấm Save.**

![Chọn Deactivated cho Edge](/images/5-Workshop/cleanup/cleanup-21b-edge-disable-termination-protection.png)

3. Bấm **Save** để áp dụng, sau đó mở lại **Delete stack**.

4. Sau khi lưu, chọn **Delete stack**, nhập `tindy-edge` và để Express mode bỏ chọn.

**📸 Chụp hộp thoại trước khi xác nhận xóa.**

![Xác nhận xóa Edge](/images/5-Workshop/cleanup/cleanup-22-edge-delete-confirm.png)

5. Bấm **Delete stack**, theo dõi Events. Khi stack biến mất khỏi Active, đổi bộ lọc sang **Deleted** và chọn đúng lượt có thời gian tạo **11/11/2026 12:23:27 UTC+07**.
6. Xác nhận `tindy-edge — DELETE_COMPLETE`. Ảnh Events cũng ghi nhận `MaintenanceRoutingFunction` và `FrontendOriginAccessControl` xóa hoàn tất.

**📸 Chụp dòng stack xóa hoàn tất.**

![Edge DELETE_COMPLETE](/images/5-Workshop/cleanup/cleanup-23-edge-deleted.png)

Edge đã xóa thành công. Lượt triển khai tiếp theo sẽ tạo CloudFront distribution mới và cập nhật cấu hình ứng dụng theo hostname mới.
