---
title: "Event 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch “Agent Forge - Deep Dive (Ngày 2)”: Bộ nhớ, Đánh giá và Giám sát AI Agent

### Mục Đích Của Sự Kiện

Phiên thứ hai của chuỗi workshop chuyên sâu **AWS FCAJ Agent Forge – Deep Dive** do cộng đồng First Cloud AI Journey (FCAJ) phối hợp cùng các kỹ sư AWS tổ chức. Ở mức độ nâng cao (L300), buổi học tập trung vào việc xây dựng AI Agent có thể vận hành trong môi trường doanh nghiệp thực tế.

Buổi học được thiết kế nhằm giúp người tham gia:

- Củng cố hiểu biết về quản lý bộ nhớ, đánh giá chất lượng phản hồi, giám sát hệ thống và tối ưu hiệu suất.
- Thực hành biến một AI Agent cơ bản thành một hệ thống Agentic AI sẵn sàng cho production.

### Danh Sách Diễn Giả

- Anh **Hiếu** - Đồng trưởng cộng đồng FCAJ, Solution Architect tại AWS Việt Nam.
- Anh **Hải Anh** - Cloud Consultant tại Chiase Pacific, phụ trách phần thực hành lab.
- **Nghia Tran** - Agentic AI Solution Architect.
- **Anh Pham** - Cloud Consultant tại G-AsiaPacific Vietnam.

### Định Dạng Workshop

Đây là **chuỗi workshop kéo dài 3 ngày**, được xây dựng theo lộ trình từ nền tảng đến triển khai AI Agent trong môi trường production bằng Amazon Bedrock AgentCore.

- **Ngày 1 (01/08): AgentCore Foundations** — tổng quan kiến trúc AgentCore (Runtime, Gateway, Identity).
- **Ngày 2 (08/08): Memory, Evaluations, Observability & Optimization** — quản lý bộ nhớ, đánh giá chất lượng agent, giám sát hệ thống và tinh chỉnh hiệu suất.
- **Ngày 3 (15/08): DevOps, Policies & Production Best Practices** — DevOps cho agent, xây dựng policies, bảo mật và các thực hành tốt khi vận hành production.

## Nội Dung Nổi Bật

### Agent Memory

Agent Memory giúp Agent vượt qua giới hạn của Context Window, duy trì ngữ cảnh cuộc trò chuyện và cá nhân hóa trải nghiệm người dùng.

**Short-term Memory** lưu toàn bộ lịch sử hội thoại dưới dạng tin nhắn thô, giúp Agent bám sát mạch trao đổi hiện tại và phản hồi nhất quán. Hệ thống cũng hỗ trợ cơ chế rẽ nhánh, tương tự cách Git tạo nhánh trong phát triển phần mềm.

**Long-term Memory** hoạt động bất đồng bộ, trích xuất thông tin quan trọng từ hội thoại và lưu dưới dạng vector để truy xuất trong các phiên sau. Bốn chiến lược lưu trữ chính:

- **Summary:** tóm tắt và nén nội dung hội thoại.
- **User Preference:** lưu trữ sở thích của người dùng.
- **Semantic:** lưu trữ tri thức chuyên ngành.
- **Episodic:** lưu lại các quyết định hoặc sự kiện đã diễn ra.

**Namespace** được dùng như một cấu trúc thư mục phân cấp để cô lập dữ liệu theo strategy, actor hoặc session. Kết hợp semantic search và similarity ranking, Agent có thể tìm đúng thông tin cần thiết, giảm lượng token và cải thiện thời gian phản hồi.

### Khả Năng Quan Sát Hệ Thống

Workshop nhấn mạnh nguyên tắc: *“You cannot fix what you cannot see”* — không thể khắc phục vấn đề nếu không quan sát được vấn đề. Hệ thống Observability sử dụng chuẩn OpenTelemetry để thu thập ba nhóm dữ liệu chính:

- **Logs:** ghi lại chi tiết request, lỗi kết nối, lỗi hệ thống hoặc log từ terminal.
- **Traces:** theo dõi toàn bộ hành trình của một request, từ lúc gửi prompt đến khi Agent trả phản hồi, bao gồm các tool call.
- **Metrics:** đo các chỉ số như mức tiêu thụ token, tỷ lệ lỗi và độ trễ phản hồi.

Những dữ liệu này giúp đội phát triển xác định nguyên nhân chậm trễ, tối ưu chi phí token và cải thiện trải nghiệm người dùng.

### Hệ Thống Đánh Giá Agent

Một rủi ro phổ biến của AI Agent là hiện tượng **hallucination**, tức đưa ra thông tin không chính xác nhưng thể hiện như sự thật. Để hạn chế rủi ro này, hệ thống cung cấp 13 evaluator tích hợp sẵn, chẳng hạn như **correctness** và **helpfulness**.

Các evaluator được áp dụng ở ba cấp độ:

- **Session level:** đánh giá kết quả của toàn bộ phiên làm việc.
- **Trace level:** đánh giá độ chính xác của phản hồi.
- **Span level:** đánh giá từng bước xử lý, chẳng hạn như gọi tool hoặc truyền tham số.

Hệ thống hỗ trợ hai hình thức đánh giá. **On-demand** phù hợp với giai đoạn phát triển và thử nghiệm; **Online** dùng để theo dõi chất lượng Agent theo thời gian thực trong production. Kết quả đánh giá tự động vẫn cần được chuyên gia lĩnh vực kiểm chứng để bảo đảm tính chính xác.

## Những Gì Học Được

### Kiến Thức Chuyên Môn

- Hiểu rõ sự khác biệt giữa Short-term Memory và Long-term Memory, đặc biệt là cơ chế xử lý đồng bộ và bất đồng bộ.
- Nắm được ba trụ cột của Observability là Logs, Traces và Metrics, cùng vai trò của chuẩn OpenTelemetry trong việc theo dõi sức khỏe hệ thống.
- Hiểu cách các evaluator tự động đánh giá phản hồi của Agent theo tiêu chí chuẩn hóa thay vì dựa hoàn toàn vào cảm nhận chủ quan.
- Biết thêm về Cedar Policy và cơ chế sandbox, qua đó nhận thức rõ vai trò của bảo mật khi Agent thực hiện tác vụ hoặc thử nghiệm mã nguồn.

### Bài Học Kinh Nghiệm

- Thiết kế AI Agent theo từng chức năng nhỏ trước khi xây dựng hệ thống phức tạp.
- Luôn ưu tiên bảo mật và phân quyền khi AI Agent truy cập tài nguyên.
- Theo dõi, đánh giá và tối ưu AI Agent dựa trên kết quả thực tế.
- Xây dựng AI Agent theo hướng dễ mở rộng và dễ bảo trì.

## Trải Nghiệm Trong Workshop

Tham gia **Ngày 2 của AWS FCAJ Agent Forge – Deep Dive** giúp em có cái nhìn tổng quan về cách xây dựng và vận hành AI Agent trong môi trường doanh nghiệp.

Qua phần trình bày của diễn giả và các nội dung thực hành, em hiểu rõ hơn cách tạo ra một AI Agent hiệu quả bằng việc cung cấp cho hệ thống cơ chế lưu trữ tri thức, giám sát, đánh giá chất lượng và bảo mật chặt chẽ.

### Một số hình ảnh khi tham gia sự kiện

![Event Photo 1](/images/4-EventParticipated/image001.jpg)

> **Đánh giá tổng thể:** Ngày 2 của **AWS FCAJ Agent Forge – Deep Dive** đã cung cấp nền tảng vững chắc về **Agentic AI** và **Amazon Bedrock AgentCore**, giúp người tham gia hiểu rõ từ các khái niệm cơ bản đến kiến trúc và cách triển khai AI Agent trong môi trường production. Workshop kết hợp giữa lý thuyết, ví dụ minh họa và các nội dung thực hành, đồng thời nhấn mạnh các yếu tố quan trọng như bảo mật, khả năng mở rộng, quản lý vòng đời và tích hợp công cụ. Đây là một chương trình hữu ích cho những ai muốn xây dựng các hệ thống AI Agent đáp ứng yêu cầu của môi trường doanh nghiệp.
