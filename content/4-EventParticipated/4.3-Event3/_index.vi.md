---
title: "Event 3"
date: 2026-06-27
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

## Bài thu hoạch AWS FC Community Day – Solutions for Enterprise AI Agents & Security

| Thông tin sự kiện | Chi tiết |
| :--- | :--- |
| **Tên sự kiện** | AWS FC Community Day 2026 |
| **Thời gian** | 27/06/2026 (09:00 - 12:00) |
| **Địa điểm** | Tầng 26 & Tầng 36, Bitexco Financial Tower, TP. Hồ Chí Minh |
| **Hình thức tham gia** | Trực tuyến (YouTube Livestream) |
| **Vai trò** | Người tham dự |

### Mục Đích Của Sự Kiện

Sự kiện **AWS FC Community Day** là chuỗi hội thảo công nghệ hàng tháng kết nối cộng đồng lập trình viên và sinh viên với các bài toán thực tế từ doanh nghiệp.

Nội dung số này tập trung vào các giải pháp triển khai AI Agent trong môi trường Enterprise: từ kiến trúc Multi-Agent cho hạ tầng đám mây và FinOps, xử lý Voice AI tiếng Việt thời gian thực cho tổng đài ngân hàng, tự động hóa xử lý sự cố DevOps, đến ứng dụng Amazon Q cho quy trình HR và bảo mật private qua VPC Endpoints kết nối máy chủ MCP.

### Danh Sách Diễn Giả

| STT | Diễn giả | Chức vụ | Chủ đề |
| :-: | :--- | :--- | :--- |
| 1 | **Steve Trần** | Founder @ *Cloud Thinker* (Ex-AWS Solution Architect) | *Agentic Platform for Cloud Infrastructure: Hành trình thăng tiến sự nghiệp đám mây, kiến trúc Single-Agent vs Multi-Agent và tối ưu chi phí FinOps* |
| 2 | **Hiếu Nghị, Kiệt & Trung Đỗ** | Cloud Engineers @ *Renova Cloud* & CEO @ *R AI* | *Voice AI Agents: Xây dựng hệ thống tổng đài thoại thông minh, xử lý dữ liệu tiếng Việt thời gian thực và kịch bản gọi công cụ (Tool calling) ngân hàng* |
| 3 | **Nguyên Nguyễn & Bảo** | Cloud Engineers @ *Cloud Kinetics* | *DevOps AI Agent: Tự động hóa quy trình phân loại, điều tra Root Cause và giảm thiểu chỉ số MTTR/MTTD hạ tầng thông qua Agent Space* |
| 4 | **Trường (Wren) & Minh Anh** | AI Solutions Team @ *Noventics* | *HR Intelligent Automation via Amazon Q: Giải pháp bóc tách CV chính xác 99%, chấm điểm ứng viên theo khung năng lực và xây dựng No-code Pipeline* |
| 5 | **Toàn Nguyễn & Hiếu Nghị** | AWS Security Builder @ *AWS Community* | *Private Security for AI Agents: Thiết lập kết nối mạng khép kín kết nối Amazon Q tới máy chủ MCP qua VPC Endpoints bảo mật* |

---

### Nội Dung Nổi Bật

#### 1. Ghi chép về Multi-Agent & FinOps (Anh Steve Trần)

Diễn giả Steve Trần chia sẻ câu chuyện phát triển sự nghiệp và kinh nghiệm xử lý bài toán hạ tầng khi doanh nghiệp mở rộng:

- **Câu chuyện định hướng sự nghiệp:** Anh kể về giai đoạn nghỉ học đại học năm 19 tuổi làm việc tại Contact Center (2018-2019), vất vả vận hành server vật lý và rủi ro sập phần cứng. Sau khi nhận ra mình hổng kiến thức nền tảng và thi trượt chứng chỉ Azure 3-4 lần, anh chuyển sang tự học kỹ hệ thống tài liệu của AWS, sau đó trở thành Solution Architect tại AWS trước khi thành lập Cloud Thinker.
- **Giải quyết nợ công nghệ (Tech Debt):** Trong các hệ thống ngân hàng/tài chính lớn, nợ công nghệ qua nhiều thế hệ làm việc điều tra sự cố bằng tay mất hàng giờ, trong khi AI có thể đọc log và phân tích trong vài phút.
- **Single-Agent vs Multi-Agent:** Mặc dù 1 con Single Super Agent thiết kế tốt có thể gánh ~95% tác vụ, nhưng kiến trúc Multi-Agent vượt trội hơn nhờ thu hẹp Context Window, cho phép gán model nhỏ cho việc dễ để tiết kiệm chi phí và hỗ trợ phân quyền RBAC khắt khe.
- **Cơ chế kiểm duyệt thay đổi (Approval Layers):** Tránh rủi ro agent tự ý chạy script làm hỏng DB, hệ thống của Cloud Thinker đặt nhiều tầng phê duyệt (Layer Approval) trước khi áp thay đổi lên Production. Hệ thống cũng chạy tự động hóa FinOps để liên tục tối ưu chi phí hạ tầng.

#### 2. Xây dựng Voice AI tiếng Việt cho tổng đài (Team R AI)

Phiên chia sẻ của team R AI và Renova Cloud giải bài toán làm trợ lý giọng nói tiếng Việt cho ngành ngân hàng (VPBank, VIB):

- **Thách thức của tiếng Việt:** Các mô hình Speech-to-Speech hiện tại chủ yếu tối ưu cho tiếng Anh, trong khi tiếng Việt là ngôn ngữ ít dữ liệu huấn luyện (low-resource).
- **Pipeline 3 bước thực tế:** Speech-to-Text (STT) -> LLM -> Text-to-Speech (TTS). STT chuyển giọng nói thành text đẩy vào LLM, LLM xử lý prompt theo ngữ cảnh ngân hàng rồi xuất text cho bộ TTS chuyển thành giọng nói phản hồi.
- **Xử lý đặc thù tiếng Việt:**
  - *Nhận diện giới tính:* Cần đoán giới tính từ giọng nói để xưng hô "anh/chị" chính xác.
  - *Thuật toán ngắt lời (Interruption):* Train model phụ để nhận biết khi nào khách dừng lại suy nghĩ (như đọc dở số điện thoại), tránh việc AI nhảy vào miệng khách hàng hoặc nói tràn lan.
  - *Giọng vùng miền (Accent):* Nạp 10%-20% data giọng miền Trung/Bắc để AI không bị lỗi nhận diện.
- **Tích hợp Tool Calling:** Cho phép AI thực hiện hành động thực tế (như kiểm tra CCCD và khóa thẻ ngân hàng thời gian thực). Nếu khách hàng bức xúc vượt khả năng xử lý, AI sẽ tự động chuyên giao cuộc gọi cho tổng đài viên.

#### 3. Tự động hóa xử lý sự cố hạ tầng với DevOps AI Agent (Cloud Kinetics)

Team Cloud Kinetics giới thiệu giải pháp tự động tìm nguyên nhân gốc rễ (Root Cause) khi hệ thống gặp sự cố, giảm tải cho đội SRE/DevOps:

- **Thách thức khi log bị phân tán:** Khi website chậm/lỗi, dev phải mò log thủ công trên CloudWatch, CloudTrail... làm mất ngữ cảnh và kéo dài thời gian xử lý (MTTR/MTTD).
- **6 trụ cột của DevOps AI Agent:**
  1. *Context Learning:* Dùng Agent Space (container logic định nghĩa tài nguyên qua tag) để AI tự vẽ sơ đồ Topology hệ thống.
  2. *Control:* Giới hạn quyền agent theo tag hoặc Private Connection.
  3. *Integration:* Mở rộng năng lực qua giao thức MCP (Model Context Protocol) để query trực tiếp vào DB lấy dữ liệu chứng cứ.
  4. *Collaboration:* Tương tác qua Web, Slack hoặc ServiceNow.
  5. *Convenience:* Kích hoạt nhanh trên AWS Console.
  6. *Cost-effective:* Tính tiền theo giây thực thi (~0.083 USD/giây).
- **Quy trình 4 bước:** Trigger Alert -> Đưa giả thuyết -> Kiểm chứng bằng log để ra Root Cause (RCA) -> Đề xuất bản vá (không tự ý chạy code sửa để đảm bảo Safety First).
- **Kết quả thực tế:** Trường WGU giảm thời gian xử lý sự cố từ 2 tiếng xuống 28 phút (giảm 77% MTTR). Zenchef giảm 75% thời gian tìm lỗi cấu hình xuống còn 20 phút.

#### 4. Tự động hóa quy trình HR với Amazon Q (Team Noventics)

Ứng dụng AI giải quyết bài toán nhân sự phi kỹ thuật từ team Noventics:

- **Thách thức làm HR thủ công:** Lọc CV bằng tay lâu, dễ bỏ sót người giỏi, đánh giá bị cảm tính và rủi ro lộ data nhân sự khi up lên các AI public.
- **Giải pháp Amazon Q:** Trợ lý Agentic hỗ trợ kết nối Google Workspace, SharePoint, OneDrive, Gmail, S3... Dữ liệu được bảo vệ trong Local Zone tại Việt Nam.
- **Quy trình tự động hóa:**
  - *Học kỹ năng:* Nạp file `.md`, Amazon Q tự học và tạo skill *HR Talent Review Assistant*.
  - *Sàng lọc & Chấm điểm CV:* AI quét thư mục CV (OCR chính xác 99%), so với JD để xếp loại (*Strong, Good, Low, Very Low*), xuất báo cáo HTML phân tích điểm mạnh/yếu và gợi ý khung lương.
  - *Tự động luồng việc:* Tự check calendar của Hiring Manager để hẹn lịch phỏng vấn và viết nháp email phản hồi.

#### 5. Bảo mật Private cho AI Agent qua VPC Endpoints (Toàn Nguyễn & Hiếu Nghị)

Phiên bảo mật chuyên sâu hướng dẫn cách bảo vệ dữ liệu khi AI kết nối với các hệ thống bên ngoài:

- **Rủi ro Public Endpoint:** Cho Amazon Q nối tới các MCP Server bên ngoài (Zalo, WhatsApp, Jira...) qua internet công cộng dễ bị tấn công Man-in-the-middle hoặc DDoS.
- **Kiến trúc mạng khép kín:** Áp dụng Zero Trust, đưa toàn bộ MCP Server vào Private Subnet.
- **Luồng kết nối nội bộ:** Amazon Q -> VPC Connection (dùng Interface Endpoint / AWS PrivateLink) -> Xác thực Cognito -> Application Load Balancer (mã hóa TLS qua ACM) -> Route 53 Resolver phân giải tên miền nội bộ -> MCP Server. Toàn bộ luồng đi hoàn toàn private trong hạ tầng AWS.

---

### Những Gì Học Được

#### Tư duy kỹ thuật & Kiến trúc
- Hệ thống Enterprise không thể dừng ở mức "chạy được là xong" mà phải bảo mật khép kín từ biên mạng (VPC Endpoints).
- Nguyên tắc Human-in-the-loop: Với các tác vụ đụng tới Production (sửa code hạ tầng) hay chi phí (FinOps), AI chỉ đề xuất, con người vẫn giữ quyền duyệt cuối cùng.
- Phân tách Multi-Agent giúp cô lập Context Window, gán đúng model nhỏ cho task dễ để tối ưu tiền cloud.
- Nắm được pipeline 3 bước Voice AI (STT -> LLM -> TTS) và cách dùng MCP Server mở rộng tri thức cho AI Agent.

#### Bài học sự nghiệp
- Bài học từ anh Steve Trần: Muốn đi xa trong mảng Cloud/DevOps bắt buộc phải học chắc nền tảng Linux và Networking trước khi chạy theo các công cụ AI thượng tầng.

---

### Ứng Dụng Vào Công Việc & Học Tập

- Tiếp tục dùng Docker đóng gói các module dịch vụ trong đồ án môn học, áp dụng gán thẻ (Tags) để phân quyền theo nguyên tắc Least Privilege.
- Trải nghiệm thử công cụ DevOps AI Agent trên AWS Console để xem cách hệ thống tự dựng sơ đồ Topology và phân tích log.
- Cấu hình thử Amazon Q chat agent cá nhân, nạp slide `.md` và tài liệu môn học để hỗ trợ tra cứu kiến thức ôn thi.

---

### Trải Nghiệm & Thảo Luận Tại Sự Kiện

- **Ấn tượng diễn giả:** Thích phong cách chia sẻ thẳng thắn của anh Steve Trần về những bài học thất bại thời đầu và các điểm trade-off thực tế khi chọn kiến trúc cho khách hàng.
- **Demo thực tế:** Phần demo Voice Agent trên Amazon Bedrock Agent Core của bạn Kiệt và phần giải thích stream text tiếng Việt của anh Trung Đỗ (CEO R AI) cho mình thấy rõ cách xử lý các case thực tế như khách ngắt lời hay đoán giới tính.
- **Ứng dụng thực tế:** Đoạn demo Amazon Q bóc tách CV xuất báo cáo HTML của team Noventics và phần phân tích chi phí vận hành mạng private ($250-$350/tháng cho ALB, Route 53 Resolver, EC2) từ anh Toàn Nguyễn mang lại góc nhìn rất thực tế về làm Solution Architect.
- **Networking:** Dù xem qua Livestream, mình vẫn tranh thủ kết nối LinkedIn với anh Toàn Nguyễn để hỏi thêm tài liệu về VPC Connection.

---

### Bài Học Rút Ra & Đóng Góp Cá Nhân

- **Bài học rút ra:** Hệ thống Enterprise thành công là giải quyết đúng bài toán nghiệp vụ (giảm MTTR, bảo mật data) với chi phí tối ưu. Nền tảng Linux & Networking vững vẫn là yếu tố quyết định.
- **Đóng góp cá nhân:** Theo dõi livestream và đặt câu hỏi cho diễn giả Toàn Nguyễn về chi phí vận hành hạ tầng private network trong thực tế; kết nối học hỏi với các bạn trong cộng đồng chứ giờ không "chat" được.

---

### Một số hình ảnh khi tham gia sự kiện

Dưới đây là hình ảnh thực tế ghi lại từ sự kiện AWS FC Community Day 2026:

![Slide mở màn giới thiệu chuỗi sự kiện định kỳ hàng tháng AWS FC Community Day tại Bitexco](/images/4-EventParticipated/4.3-Event3/IMG20260620130023.jpg)

> *Sự kiện mang lại nhiều góc nhìn thực tế về triển khai AI Agent và bảo mật hạ tầng trên AWS, giúp mình định hình rõ hơn lộ trình học tập.*