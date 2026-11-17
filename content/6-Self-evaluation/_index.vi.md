---
title: "Tự đánh giá"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

Trong chương trình **First Cloud AI Journey (FCAJ)**, em học AWS theo từng chủ đề, liên hệ kiến thức với dự án **Tindy** và tham gia các hoạt động cộng đồng. Báo cáo ghi nhận ba đầu ra chính là nhật ký học [AWS Cloud Practitioner Essentials](/vi/1-worklog/), [bản đề xuất và kiến trúc Tindy](/vi/2-proposal/), và phiên [Education tại FCAJ Community Day](/vi/4-eventparticipated/4.5-event5/) ngày 31/10/2026.

### Đánh giá theo tiêu chí

| STT | Tiêu chí | Căn cứ tự đánh giá | Tốt | Khá | Trung bình |
| --- | --- | --- | --- | --- | --- |
| 1 | **Kiến thức và kỹ năng chuyên môn** | Em liên hệ kiến thức AWS với Tindy qua CloudFormation, ECS, RDS và luồng xử lý PDF. Phần hạ tầng có cấu hình theo dõi tình trạng ALB và lỗi HTTP 5xx. | ✅ |  |  |
| 2 | **Khả năng học hỏi** | Em ghi lại nội dung AWS Cloud Practitioner Essentials theo từng tuần. Khi gặp một dịch vụ mới, em đối chiếu chức năng của nó với một tình huống trong dự án, như dùng CloudWatch để tìm bước gây lỗi khi API trả 5xx. | ✅ |  |  |
| 3 | **Tính chủ động** | Em chủ động tìm thêm bài kỹ thuật về Kiro, AgentCore và AWS Emulator để đọc và tổng hợp. Em cũng chuẩn bị phần chia sẻ về cách bắt đầu học AWS cho FCAJ Community Day. | ✅ |  |  |
| 4 | **Tinh thần trách nhiệm** | Khi chuẩn bị phát hành backend, em bổ sung yêu cầu có minh chứng sao lưu và kiểm tra migration cho thay đổi schema. Em cũng bổ sung workflow để nhóm diễn tập rollback. | ✅ |  |  |
| 5 | **Kỷ luật** | Em chia nội dung học theo tuần và theo dõi các mốc dự án bằng sprint. Em cần giữ nhịp cập nhật đều hơn khi việc học, tích hợp và viết báo cáo diễn ra song song. |  | ✅ |  |
| 6 | **Tinh thần cầu tiến** | Em có thói quen ghi lại điều cần tìm hiểu thêm. Em muốn xin nhận xét sớm hơn cho từng đầu ra và ghi rõ phần đã sửa sau mỗi lượt review. |  | ✅ |  |
| 7 | **Giao tiếp** | Em trình bày chủ đề New to AWS? Where Should You Start? trong phiên Education của FCAJ Community Day. Em tổ chức bài nói theo nhu cầu của người mới và giải thích vai trò của ba nguồn học. | ✅ |  |  |
| 8 | **Hợp tác nhóm** | Tindy có các pull request hợp nhất thay đổi ở backend và frontend, cùng các thay đổi hạ tầng liên quan. Em muốn trao đổi sớm hơn khi một thay đổi của mình ảnh hưởng đến API, dữ liệu hoặc thứ tự triển khai của phần khác. |  | ✅ |  |
| 9 | **Ứng xử chuyên nghiệp** | Em ghi nguồn cho nội dung kỹ thuật và dùng tài liệu mẫu khi mô tả kịch bản demo. Em có thể cải thiện cách bàn giao bằng một bản tóm tắt ngắn về quyết định, phần còn mở và người phụ trách. |  | ✅ |  |
| 10 | **Tư duy giải quyết vấn đề** | Với lỗi API, em có thể lần theo tình trạng ALB, sự kiện ECS và log ứng dụng. Em cần thực hành thêm các tình huống có chủ đích để so sánh dấu hiệu, giả thuyết và cách khôi phục. |  | ✅ |  |
| 11 | **Đóng góp vào dự án và cộng đồng** | Các đóng góp ghi nhận được gồm workflow phát hành backend ECS, giám sát ALB, cảnh báo hàng đợi lỗi cho luồng quét PDF và phiên Education tại FCAJ Community Day. | ✅ |  |  |
| 12 | **Tổng thể** | Em đánh giá tốt khả năng tự học, vận dụng vào Tindy và chia sẻ kiến thức. Hai ưu tiên tiếp theo của em là kiểm chứng vận hành và phối hợp sớm hơn giữa các phần của dự án. | ✅ |  |  |

### Cần cải thiện

1. **Lưu minh chứng ở từng mốc phát hành.** Với một bản triển khai hoặc diễn tập rollback của Tindy, em muốn ghi cùng nhau phiên bản, môi trường, bước kiểm tra, kết quả mong đợi và kết quả quan sát. Ví dụ, với API, em có thể lưu kết quả readiness sau khi phát hành và sau khi quay về phiên bản trước.
2. **Thực hành chẩn đoán và khôi phục có chủ đích.** Trên môi trường thử nghiệm, em có thể chọn một tình huống như target ALB không khỏe hoặc worker nhận lại thông điệp quét PDF. Mỗi lần thử cần có giả thuyết, dấu hiệu để kiểm tra và cách xác minh hệ thống đã ổn định.
3. **Chia sẻ tiến độ sớm và ngắn hơn.** Em muốn tóm tắt phần đã xong, ảnh hưởng tới phần việc khác và quyết định cần người hỗ trợ ngay khi phát hiện vướng mắc. Cuối sprint, em có thể ghi lại góp ý nào đã được xử lý và việc nào cần mang sang sprint sau.
