---
title: "Event 3"
date: 2026-06-27
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Bài thu hoạch “FCAJ Community Day – 27/06/2026”

### Mục Đích Của Sự Kiện

- Cập nhật xu hướng ứng dụng AI Agent vào vận hành hạ tầng cloud (CloudOps, DevOps) trong doanh nghiệp.
- Tìm hiểu về Voice AI và các thách thức đặc thù khi xây dựng trợ lý giọng nói cho tiếng Việt.
- Giới thiệu Amazon Quick – công cụ AI Agentic của AWS – và cách ứng dụng vào bài toán tuyển dụng nhân sự (HR).
- Chia sẻ giải pháp thiết lập kết nối bảo mật (private) giữa AI Agent và MCP Server trong môi trường doanh nghiệp.

### Danh Sách Diễn Giả

- **Steve Tran** – CTO/Founder Cloud Thinker
- **Nghi Danh** – AI Engineering Renova Cloud
- **Kiet Tran** – AI Engineering AWS Student Builder Group
- **Trung Vu** – CEO, Revve AI
- **Bao Phan** – Cloud Engineer, Cloud Kinetics
- **Nguyen Nguyen** – Cloud Engineer, Cloud Kinetics
- **Truong Tran** – AI Solution Sales, Noventiq
- **Anh Dang** – Solution Sales, Noventiq
- **Toan Nguyen** – AWS Security Builder

### Nội Dung Nổi Bật

#### Cloud Thinker – Agentic Platform cho vận hành Cloud

- Cloud Thinker là nền tảng Agentic hỗ trợ 3 bài toán chính: xử lý incident nhanh, code review tự động và tối ưu chi phí (FinOps) bằng AI, cùng với kiểm thử bảo mật (pen-testing).
- **So sánh kiến trúc Single Agent và Multi-Agent:** Single Agent xử lý được phần lớn tác vụ, Multi-Agent tối ưu hơn về chi phí, ngữ cảnh và phân quyền (role-based access control) khi hệ thống phức tạp.
- Hành động nhanh, bám sát bài toán thực tế của khách hàng chiến lược “Champion customers” (doanh nghiệp lớn).

#### Voice AI cho tiếng Việt

- Giới thiệu hai kiến trúc Voice AI: speech-to-speech trực tiếp và kiến trúc 3 tầng (Speech-to-Text → LLM → Text-to-Speech).
- Tiếng Việt là ngôn ngữ ít tài nguyên huấn luyện (low-resource) nên các mô hình speech-to-speech hiện chưa đáp ứng tốt; giải pháp thực tế cho ngân hàng dùng kiến trúc 3 tầng để kiểm soát nội dung và tool-calling.
- Các thách thức riêng của tiếng Việt: nhận diện giới tính người nói để xưng hô đúng, xử lý ngắt lời tự nhiên, nhận diện giọng vùng miền, cùng yêu cầu vận hành thực tế như audit log, versioning, chuyển giao (handoff) cho con người.

#### DevOps AI Agent

- Có sáu trụ cột của DevOps Agent: Context Learning, Control, Integration (qua MCP), Collaboration, Convenience, Cost-effective (tính phí theo thời gian chạy).
- **Quy trình DevOps AI Agent thực hiện 4 bước:** Triage → Investigation (đưa giả thuyết, kiểm chứng) → Mitigation (đề xuất phương án, không tự thực thi) → Improvement.

#### Amazon Quick ứng dụng cho HR

- Sàng lọc CV thủ công dễ bỏ sót ứng viên tốt, đánh giá thiếu bộ tiêu chí chuẩn, rủi ro bảo mật khi dùng AI công cộng, thời gian tuyển dụng kéo dài.
- Amazon Quick là AI Agentic tự động hóa khâu sàng lọc CV, phân tích dữ liệu ứng viên dựa trên JD (Job Description), chấm điểm ứng viên, tối ưu quy trình phỏng vấn.

#### Kết nối bảo mật giữa Amazon Quick và MCP Server

- Vấn đề bảo mật khi kết nối AI Agent với MCP server bên thứ ba qua Internet công cộng (rủi ro DDoS, Man-in-the-middle).
- Giải quyết vấn đề bằng kỹ thuật thiết lập MCP server kết nối Amazon Quick với nguồn dữ liệu bên thứ ba một cách riêng tư mà không thông qua Internet public, nhằm tăng sự bảo mật trong doanh nghiệp.

### Những Gì Học Được

#### Tư duy phát triển

- Hiểu rằng AI khuếch đại năng lực con người chứ không thay thế hoàn toàn, đặc biệt ở các vị trí đòi hỏi kinh nghiệm vận hành thực chiến.
- Nhận thấy tầm quan trọng của việc trải nghiệm thực tế sớm (thực tập, dự án) để thích ứng với thị trường lao động đang thay đổi do AI.

#### Kiến trúc kỹ thuật

- Nắm được sự khác biệt và thời điểm nên dùng Single Agent hay Multi-Agent trong thiết kế hệ thống AI.
- Hiểu quy trình 4 bước điều tra sự cố (Triage – Investigation – Mitigation – Improvement) của một DevOps AI Agent.
- Nắm được nguyên tắc thiết lập kết nối private giữa AI Agent và MCP Server bằng VPC Endpoint, ALB và Route 53 Resolver.

### Ứng Dụng Vào Công Việc

- **Áp dụng mô hình Multi-Agent** chuyên biệt hóa theo từng vai trò để tối ưu chi phí và kiểm soát phạm vi truy cập.
- **Thiết kế Voice AI hoặc chatbot** có xét đến đặc thù ngôn ngữ tiếng Việt (giới tính người nói, ngắt lời, vùng miền) thay vì áp dụng nguyên mô hình quốc tế.
- **Tham khảo mô hình DevOps Agent** để đề xuất tự động hóa một phần công việc điều tra sự cố trong hệ thống của mình.
- **Ứng dụng nguyên tắc bảo mật private-first** (VPC Endpoint, không public endpoint) khi tích hợp AI Agent với hệ thống nội bộ doanh nghiệp.

### Trải nghiệm trong event

Tham gia FCAJ Community Day tháng 6/2026 là dịp để cập nhật các ứng dụng AI Agent mới nhất trong lĩnh vực Cloud, đồng thời quan sát trực tiếp nhiều phần demo thực tế từ các doanh nghiệp. Một số trải nghiệm nổi bật:

#### Học hỏi từ các diễn giả có kinh nghiệm thực chiến

- Các diễn giả đến từ nhiều doanh nghiệp khác nhau mang đến góc nhìn đa dạng, từ startup đến giải pháp cho ngân hàng và tập đoàn lớn.
- Có cơ hội đặt câu hỏi trực tiếp và nhận phản hồi cụ thể về cách triển khai AI Agent trong môi trường production.

#### Trải nghiệm demo trực quan

- Được xem trực tiếp nhiều phần demo: Voice Agent trả lời câu hỏi bằng giọng nói, DevOps Agent tự động điều tra sự cố DDoS giả lập, và Amazon Quick phân tích – chấm điểm CV ứng viên theo thời gian thực.
- Hiểu rõ hơn về chi phí vận hành thực tế của các giải pháp AI Agent trên AWS thông qua phần hỏi đáp về pricing.

#### Một số hình ảnh khi tham gia sự kiện

![Hình ảnh minh chứng tham gia Event 3](/images/4-EventParticipated/4.3-Event3/event3-photo1.jpg)
