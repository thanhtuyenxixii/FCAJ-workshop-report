---
title: "Event 2"
date: 2026-05-23
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “FCAJ Community Day – 23/05/2026”

### Mục Đích Của Sự Kiện

- Làm chủ ngữ cảnh AI và tư duy xây dựng bộ não AI thứ hai (Second AI Brain).
- Giới thiệu về Amazon Quick, trợ lý thân thiện cho mọi người dùng.
- Giới thiệu BMAD Method hỗ trợ kiểm soát chất lượng mã nguồn, hạn chế tình trạng AI ảo tưởng.

### Danh Sách Diễn Giả

- **Anh Tịnh** – Platform Engineer tại GoTymeX
- **Anh Hải Anh** – G-AsiaPacific Vietnam, AWS Community Builder (Security)
- **Nguyễn Tuấn Thịnh** – DevOps/Cloud Engineer tại AWS
- **Team dự án UTM Morpho** (Chị Uyển, Thảo, Mai)
- **Anh Đào Đức** – Solution Architect tại Cloud Kinetics
- **Chị Vy** – Senior Business Systems Analyst tại VPBank

### Nội Dung Nổi Bật

#### Quản lý ngữ cảnh (Context) và tư duy xây dựng não bộ thứ hai (Second AI Brain)

- **Tối ưu hóa ngữ cảnh (Context Engineering):** Chất lượng hơn Số lượng, vì vậy nên cung cấp mục tiêu, ràng buộc, và tiêu chuẩn cụ thể rõ ràng thay vì nhồi nhét dữ liệu thô hay lặp lại những thông tin dư thừa.
- **Tư duy hệ thống:** Chuyển dịch từ các câu lệnh đơn lẻ sang xây dựng hệ thống có bộ nhớ để AI cá nhân hóa và hỗ trợ tốt hơn theo thời gian.
- Áp dụng bộ khung Context chuẩn 4 yếu tố (Goal, Role, Format, Evidence) và tận dụng các công cụ ghi chú hệ thống (Obsidian) để xây dựng khái niệm Second AI Brain.

#### Kiến trúc Trợ lý AI thân thiện & Hệ sinh thái tự động hóa dữ liệu không code với Amazon Quick

- **Mô hình Agent hoàn chỉnh:** Kết hợp LLM với giao thức kết nối công cụ, AI đóng vai trò một trợ lý thân thiện, giúp mọi người dùng khai thác và tự động hóa dữ liệu chỉ bằng các câu lệnh chat.
- **Quick Chat/ Quick Sight:** Cho phép người dùng đặt câu hỏi để phân tích dữ liệu thô và tự động xuất Dashboard báo cáo.
- **Quick Flows / Quick Spaces:** Tự sinh workflow thông minh và xây dựng không gian chia sẻ tri thức chung cho team mà không cần chạm một dòng code nào.

#### Tối ưu chi phí Cloud và Bảo mật hạ tầng

- **Cơ chế mới Flat-Rate Pricing:** Giải pháp bẻ gãy hoàn toàn rủi ro vượt hóa đơn sau một đêm DDoS theo cơ chế Pay-as-you-go. Doanh nghiệp sẽ mua theo gói chi phí cố định mà không phát sinh thêm tiền (nếu dùng vượt quá thì hệ thống chỉ bị bóp băng thông).
- **Mutual TLS:** Bắt tay xác thực 2 chiều giữa Client và Server cho các ứng dụng tài chính bảo mật.
- **VPC Origin:** Tạo đường hầm kết nối thẳng CloudFront vào Private Subnet, ẩn hoàn toàn hạ tầng máy chủ khỏi Internet Public.

#### Quy trình phát triển sản phẩm từ trải nghiệm Hackathon thực chiến

- **Tìm kiếm ý tưởng:** Khắc phục tình trạng AI luôn phải regenerate lại từ đầu toàn bộ UI chỉ vì chỉnh sửa nhỏ về màu sắc hay khoảng cách, gây tốn token và thời gian.
- **Giải pháp UTM Morpho ra đời:** Tạo ra app dùng AI để generate những UI theo các template khác nhau và có thể chỉnh sửa trực tiếp trên UI đó.
- **Kinh nghiệm dưới áp lực 36 tiếng:** Cách quản lý token limit, xử lý lỗi AI over-generation; chọn ra các tính năng chính, cắt bỏ những tính năng thừa khi bị quá nhiều ý tưởng để dồn lực tối ưu trải nghiệm cốt lõi, phân chia công việc theo thế mạnh thành viên và quản lý tốt sức bền không bị kiệt sức.

#### Bản chất toán học & Chiến lược chế ngự tính bất định của LLM

- Ở Temperature = 0, LLM Cloud vẫn lệch kết quả do sai số làm tròn số thập phân của GPU và cơ chế gộp prompt thương mại (Inference Batching). Tự host Local mới giống nhau 100%.
- **Giải pháp:** Set Temp = 0.1, bật JSON Mode, thiết kế downstream chịu lỗi và liên tục thực hiện testing.

#### Ứng dụng Multi-Agent và tính tuân thủ Enterprise

- **Bài toán Nghiệp vụ:** Xây dựng mô hình Virtual Credit Committee để chấm điểm tín dụng startup bằng dữ liệu thay thế multidimensional.
- **Kiến trúc hệ thống:** Agent điều phối (Manager) phân rã nhiệm vụ cho các Sub-Agent chuyên gia (Financial Analyst, Market Analyst, Risk Assessor) để challenge chéo lẫn nhau.
- **Tính tuân thủ:** Quản lý nghiêm ranh giới tự hành của AI, chống tấn công giao thức MCP, bắt buộc lưu vết (Audit trail) rõ ràng.

### Những Gì Học Được

#### Kiến trúc & Công nghệ

- Cách phân rã bài toán lớn thành các Agent chuyên biệt để vượt giới hạn context window.
- Hiểu cấu trúc bảo mật mạng biên và bản chất bài toán gây lệch kết quả của LLM Cloud.

#### Tư duy hệ thống

- Hiểu sâu cấu trúc bảo mật mạng biên (VPC Origin, mTLS, Caching đa tầng) để bảo vệ hệ thống cấp Enterprise.

### Ứng Dụng Vào Công Việc

- **Kỹ nghệ Prompt chặt chẽ:** Áp dụng bộ khung Context 4 yếu tố (Goal, Role, Format, Evidence). Luôn phân rã bài toán thành các bài toán nhỏ rõ ràng trước khi giao cho AI.
- **Tối ưu hóa quy trình làm việc:** Sử dụng các công cụ dạng ngôn ngữ tự nhiên để tự động hóa nhanh các workflow dữ liệu thô và xuất Dashboard báo cáo mà không cần code.
- **Vận hành hạ tầng:** Chú ý cấu hình thủ công gói Flat-Rate Pricing trên CloudFront Console để chủ động quản lý rủi ro chi phí cho các dự án.

### Trải nghiệm trong event

Tham gia buổi Meetup mang lại nhiều góc nhìn thực tế về định hướng sự nghiệp thời đại GenAI, đồng thời cung cấp các giải pháp và tư duy làm chủ công nghệ thiết thực. Một số trải nghiệm nổi bật bao gồm:

#### Ứng dụng công cụ trực quan

- Trực tiếp xem demo hệ sinh thái **Amazon Quick** (bao gồm Quick Chat, Quick Flows, Quick Sight) - giải pháp trực quan sử dụng hoàn toàn bằng ngôn ngữ tự nhiên để tự động hóa nhanh các workflow dữ liệu thô và kết xuất Dashboard báo cáo tức thì trên nền tảng AWS.
- Tiếp cận giải pháp chuỗi Multi-Agent Serverless qua dự án thực tế **UTM Morpho** - giải quyết triệt để bài toán chuyển đổi hình phác thảo thành code HTML/CSS mà không bị lỗi AI phải regenerate lại từ đầu toàn bộ giao diện mỗi khi cần chỉnh sửa một chi tiết nhỏ.

#### Bài học rút ra

- Chủ động kiểm soát và quản lý tri thức thông qua tư duy thiết kế hệ thống vững chắc (Second AI Brain) phối hợp với việc nắm vững an ninh hạ tầng biên (mTLS, VPC Origin, Flat-Rate Pricing).
- Thực tế Enterprise luôn đi đầu: Hệ thống lớn trong doanh nghiệp không làm AI wrapper bề nổi mà phải đặt tính an toàn hạ tầng biên, ranh giới tuân thủ pháp lý và khả năng lưu vết dữ liệu giải trình (Audit trail) lên hàng đầu.

#### Một số hình ảnh khi tham gia sự kiện

![Hình ảnh minh chứng tham gia Event 2](/images/4-EventParticipated/4.2-Event2/event2-photo1.jpg)
