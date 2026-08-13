---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu Tuần 6:

* Học AWS Step Functions và mẫu Wait for Task Token.
* Xây dựng quy trình phê duyệt tài liệu.
* Thêm thông báo email SNS.

### Nhiệm vụ trong tuần:

| STT | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Học AWS Step Functions và khái niệm state machine | 27/07/2026 | 28/07/2026 | |
| 2 | - Tạo SNS topic và email subscription | 28/07/2026 | 28/07/2026 | |
| 3 | - Thiết kế state machine phê duyệt (CaptureToken → Decision → Mark → Notify) | 29/07/2026 | 29/07/2026 | |
| 4 | - Tích hợp Step Functions vào backend (startExecution, SendTaskSuccess) | 30/07/2026 | 31/07/2026 | |
| 5 | - Xử lý task token callback và cập nhật trạng thái tài liệu | 31/07/2026 | 01/08/2026 | |
| 6 | - Test toàn bộ luồng phê duyệt end-to-end | 01/08/2026 | 02/08/2026 | |

### Kết quả Tuần 6:

* Hiểu AWS Step Functions và mẫu **Wait for Task Token** cho phê duyệt con người.
* Tạo SNS topic và xác nhận email subscription.
* Thiết kế và định nghĩa state machine phê duyệt (ASL).
* Tích hợp Step Functions vào backend: `startExecution` khi submit, `SendTaskSuccess` khi approve/reject.
* Lưu task token và xử lý callback để cập nhật trạng thái tài liệu.
* Test toàn bộ luồng phê duyệt end-to-end và xác minh email SNS.
