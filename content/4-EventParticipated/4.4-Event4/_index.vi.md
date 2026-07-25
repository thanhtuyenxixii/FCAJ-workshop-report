---
title: "Event 4"
date: 2026-07-25
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

## Bài thu hoạch AWS Agentic AI Buildweek Showcase & Sharing – Multi-Agent Systems & Enterprise Solutions

| Thông tin sự kiện | Chi tiết |
| :--- | :--- |
| **Tên sự kiện** | AWS Agentic AI Buildweek 2026 Showcase & Sharing |
| **Thời gian** | 25/07/2026 (08:30 - 12:00) |
| **Địa điểm** | Tầng 26, Bitexco Financial Tower, TP. Hồ Chí Minh |
| **Hình thức tham gia** | Trực tuyến (YouTube Livestream) |
| **Vai trò** | Người tham dự |

### Mục Đích Của Sự Kiện

Buổi **AWS Agentic AI Buildweek Showcase & Sharing** là sự kiện tổng kết cuộc thi Hackathon *Agentic AI Buildweek 2026* do **AWS**, quỹ đầu tư **JI Fund** và cộng đồng **FCAJ** phối hợp tổ chức. 

Nội dung buổi chia sẻ tập trung vào kiến trúc hạ tầng Multi-Agent và quy trình triển khai AI Agent thực tế trong các mảng F&B, nghiên cứu thị trường và tài chính - ngân hàng. Bên cạnh đó, các đại diện từ AWS và các đội thi cũng chia sẻ nhiều bài học thực tế về quản lý chi phí đám mây (FinOps), cách xử lý dữ liệu tiếng Việt và kinh nghiệm gói gọn scope khi làm sản phẩm.

### Danh Sách Diễn Giả & Các Đội Thi

| STT | Diễn giả / Đội thi | Chức vụ / Giải thưởng | Chủ đề trình bày |
| :-: | :--- | :--- | :--- |
| 1 | **Joseph Marazota** | Head of Technology @ *AWS ASEAN* | *Keynote: Sự chuyển dịch sang kỷ nguyên AI Agent, tư duy bứt phá rào cản và vai trò Human-in-the-loop* |
| 2 | **Nguyễn Gia Hưng** | Head of Solution Architect @ *AWS Vietnam* | *Đại diện AWS Việt Nam chào mừng & trao chứng nhận cho các đội thi* |
| 3 | **One Team** | Giải Nhất @ *AWS Track* | *Dự án KFC Chatbot: Tác nhân đặt món KFC qua Zalo/WhatsApp dùng AWS Agent Core Memory, TinyFish scraper và cơ chế Last Verify* |
| 4 | **Final Scale (Signal C)** | Giải Nhì @ *AWS Track* | *Multi-Agent Market Intelligence: Phân tích tín hiệu đối thủ & thị trường dựa trên Value Creation Canvas, LangFuse & Bedrock Guardrails* |
| 5 | **Team Plan V** | Đội thi Hackathon | *SA Professional AI Native App: Trợ lý AI phân tích requirement, tự động gen kiến trúc Draw.io, tính chi phí & xuất Terraform IaC* |
| 6 | **Team 3KA** | Đội thi Hackathon | *Dự án Sheper & Trải nghiệm Hackathon 24h: Quản trị tâm lý, bài học lập trình dưới áp lực và quản lý rủi ro* |
| 7 | **Six Pillars** | Đội thi xuất sắc @ *FinTech Track* | *Adaptive Workflow Engine for AML: Trợ lý tự động hóa điều tra rửa tiền cho ngân hàng, giảm 90-95% cảnh báo giả (False Positive)* |

---

### Nội Dung Nổi Bật

#### 1. Định hướng sự nghiệp & Tư duy bứt phá từ diễn giả AWS

Phần chia sẻ của ông **Joseph Marazota** (Head of Technology, AWS ASEAN) và anh **Nguyễn Gia Hưng** (Head of Solution Architect, AWS Vietnam) mang lại một số góc nhìn đáng chú ý:

- **Sự chuyển dịch tốc độ phát triển phần mềm:** 20 năm trước các ngân hàng thường 3 tháng mới release code 1 lần, sau đó rút ngắn xuống 2 tuần/lần với Agile/DevOps. Đến kỷ nguyên AI Agent, hệ thống có thể hỗ trợ tự động hóa và release liên tục.
- **Tư duy không gò bó rào cản:** Lập trình viên trẻ không nên bị giới hạn bởi tư duy cũ hay tâm lý thiếu kinh nghiệm. Các góc nhìn mới (New Mental Models) chính là điểm cộng để tạo ra cách giải quyết bài toán khác biệt.
- **Triết lý "Human-in-the-loop":** Amazon vận hành hơn 1 triệu robot trong các kho hàng Fulfillment, nhưng robot vẫn cần con người thiết lập logic và định hướng. Con người luôn là mắt xích kiểm soát chính (Human-in-the-loop) trong mọi hệ thống AI.

#### 2. Dự án KFC Chatbot (One Team - Giải Nhất)

Đội **One Team** giải quyết bài toán đặt đồ ăn thực tế trong ngành F&B với các điểm kỹ thuật chính:

- **Vấn đề thực tế:** Nhìn từ bài học thử nghiệm AI Drive-thru của McDonald's (AI bị nhầm ngữ cảnh dẫn tới đặt lộn 100 miếng gà), nhóm nhận thấy việc bắt người dùng cài thêm app mới để đặt hàng có tỷ lệ chuyển đổi rất thấp. Do đó, đưa chatbot về ứng dụng nhắn tin sẵn có như Zalo hay WhatsApp (trọng tâm là Zalo tại Việt Nam) là hướng đi thực tế hơn.
- **Kiến trúc Kỹ thuật:**
  - *Thu thập dữ liệu thực đơn:* Vì không có API trực tiếp từ KFC, đội dùng **TinyFish** cào (scrape) menu động từ website KFC và lưu vào CSDL trên AWS.
  - *Bộ nhớ ngữ cảnh cá nhân hóa:* Dùng **AWS Agent Core Memory** tạo bộ nhớ riêng cho từng user, tự ghi nhớ món khách từng đặt tuần trước để gợi ý nhanh.
  - *Chi phí & Tốc độ:* Tốc độ phản hồi đạt 3 - 5 giây, chi phí hạ tầng khoảng **0.006 USD/đơn hàng** (rẻ hơn khoảng 75% so với mô hình serverless truyền thống).
  - *Xác thực đơn hàng (Last Verify):* Đội thêm bước confirm lại toàn bộ chi tiết đơn hàng trước khi chốt thanh toán để chặn lỗi hallucination của AI.

#### 3. Phân tích tín hiệu đối thủ bằng Multi-Agent (Final Scale - Giải Nhì)

Đội **Final Scale** (sinh viên FPT) tập trung vào bài toán phân tích thông tin thị trường với cách tiếp cận bài bản:

- **Nghiệp vụ dẫn dắt công nghệ:** 70% thành bại của dự án nằm ở bài toán nghiệp vụ chứ không phải độ phức tạp của model. Nhóm dùng **Value Creation & Delivery Canvas** (tinh chỉnh từ Business Model Canvas) để tập trung vào giá trị đầu ra thay vì sa đà vào các chỉ số dòng tiền không cần thiết cho demo hackathon.
- **Kiến trúc Multi-Agent & Bảo mật:**
  - *Crawler Subagent:* Dùng Apify cho web tĩnh/dữ liệu lớn và TinyFish cho web động cần quét sâu.
  - *Lọc dữ liệu thô & Tối ưu Token:* Dùng code thuần xử lý bớt dữ liệu rác trước khi đẩy vào LLM. Cách này vừa đỡ tốn chi phí token vừa giảm nguy cơ bị Prompt Injection từ dữ liệu web bên ngoài.
  - *Đánh giá chất lượng bằng LangFuse:* Chạy qua **LangFuse** để chấm điểm output. Nếu điểm thấp, hệ thống tự retry tối đa 2 lần; nếu vẫn chưa đạt thì lưu vào DynamoDB để gắn tag cho người kiểm tra lại.
  - *Bảo mật:* Kết hợp **Bedrock Guardrails**, AWS Cognito, WAF và Amplify.

#### 4. Trợ lý AI cho Solution Architect (Team Plan V)

Đội **Team Plan V** mang đến giải pháp **SA Professional AI Native App** nhằm tự động hóa quy trình thiết kế hệ thống và báo giá chi phí cho các Solution Architect:

- **Bài toán thực tế:** Khi làm việc với khách hàng, các SA thường nhận yêu cầu gấp phải tạo sơ đồ kiến trúc và ước tính chi phí trong vòng 2-3 ngày, hoặc thậm chí ngay trong đêm. Việc vẽ tay sơ đồ, tra cứu bảng giá dịch vụ và viết script hạ tầng (IaC) tốn rất nhiều thời gian.
- **Giải pháp & Luồng xử lý:**
  - *Xử lý ngôn ngữ tự nhiên & Tài liệu:* Cho phép người dùng nhập yêu cầu bằng câu lệnh tự nhiên (Free text) hoặc tải lên tài liệu policy/quy chuẩn kỹ thuật của công ty.
  - *Tự động sinh sơ đồ Draw.io:* AI phân tích yêu cầu và tự động dựng sơ đồ kiến trúc chuẩn bộ icon AWS trên giao diện Draw.io. Người dùng có thể kéo thả và tinh chỉnh trực tiếp.
  - *Báo giá & Sinh mã IaC:* Tự động xuất bảng tính chi phí dự án và tạo file cấu hình Terraform / CloudFormation theo chuẩn mã nguồn mở (Terraform Modules).
  - *Kiểm soát Policy & Blacklist:* Đội thiết lập bộ lọc output validation để ngăn AI dùng các dịch vụ không mong muốn (ví dụ: chặn các service thiếu khả năng quản lý chi tiết như App Runner để ưu tiên ECS/Lambda trong môi trường doanh nghiệp).
  - *Triển khai tự động (Auto-Deploy):* Khi người dùng bấm confirm, hệ thống có thể kích hoạt luồng chạy script IaC để khởi tạo toàn bộ hạ tầng thực tế trên AWS.

#### 5. Giám sát đám đông thời gian thực & Bài học FinOps từ YOLO Demo (Team 3KA)

Đội **Team 3KA** (gồm 5 sinh viên FPT/chung trường) mang đến dự án **Sheper** (Hệ thống camera AI phát hiện và điều phối đám đông ùn tắc tại sân bay, siêu thị, sự kiện) cùng các bài học thực chiến 24h:

- **Kiến trúc Kỹ thuật Dự án Sheper:**
  - *Luồng Video Stream:* Dùng **Kinesis Video Streams** kết nối trực tiếp camera giám sát đẩy dữ liệu vào ECS Fargate Cluster.
  - *Nhận diện & Tracking đám đông:* Sử dụng mô hình **YOLO** kết hợp **ByteTrack** để detect người, gán ID theo dõi di chuyển và vẽ zone cảnh báo ùn tắc theo thời gian thực.
  - *Agent & Operator Co-pilot:* Dùng Amazon Bedrock tích hợp CSDL DynamoDB/S3 để theo dõi tự động (Autonomous Monitor) và hỗ trợ điều phối viên (Operator Co-pilot).
- **Bài học FinOps từ sự cố SageMaker ($48):**
  - Trong phần Q&A, đội chia sẻ trải nghiệm thực tế: ban đầu đội host mô hình AI lớn trên **Amazon SageMaker** để demo trong 3 tiếng, khiến chi phí vọt lên **48 USD**.
  - Đội sau đó linh hoạt chuyển sang **YOLOv26 Small** (gọn nhẹ hơn hẳn), vừa đảm bảo confident score đạt 90% - 97% khi tracking vừa tối ưu chi phí hạ tầng.
- **Bài học quản trị rủi ro & Tâm lý 24h Hackathon:**
  - *Sự cố lộ file bí mật:* Do mệt mỏi trong đêm, thành viên nhóm lỡ tay push file chứa biến môi trường (`.env`) lên Git, để lại bài học xương máu về bảo mật.
  - *Quản lý thời gian & Scope:* Mất 3 tiếng chỉ để sửa giao diện/chữ viết, giúp nhóm nhận ra tầm quan trọng của việc phân công vai trò rõ ràng và giữ scope vừa sức.

#### 6. Giải pháp chống rửa tiền AML (Six Pillars - Đội Xuất Sắc FinTech Track)

Đội **Six Pillars** giải bài toán nghiệp vụ trong lĩnh vực Tài chính - Ngân hàng (BFSI):

- **Vấn đề AML ở ngân hàng:** Quy tắc lọc truyền thống tạo ra tới **90% - 95% cảnh báo giả (False Positive)**. Chuyên viên ngân hàng mất khoảng 20 - 25 USD và 3 giờ đồng hồ để rà soát thủ công từng ca, gây quá tải công việc.
- **Luồng xử lý 3 tầng:**
  - *Layer 1 (Fast Detection):* Kinesis Data Stream nhận giao dịch real-time, Lambda trích xuất đặc trưng và XGBoost trên Bedrock lọc nhanh (chỉ chuyển 5-10% ca thực sự nghi vấn lên tầng trên).
  - *Layer 2 (Agentic Investigation):* Dùng 3 sub-agent chuyên biệt: **KYC Profile Check**, **Money Flow Check** (phát hiện hành vi chia nhỏ dòng tiền smurfing) và **Sanction Check** (đối soát danh sách cấm vận). Hệ thống tra cứu tri thức pháp lý lưu trên **Vector OpenSearch**.
  - *Layer 3 (Decision & Human-in-the-loop):* Dùng 2 tầng LLM (con thứ nhất đề xuất Dismiss/Hold/Escalate, con thứ hai làm LLM-as-a-Judge soát lại). Các ca phức tạp hoặc vi phạm cấm vận sẽ được đẩy thẳng về Dashboard để chuyên viên ngân hàng ra quyết định cuối.

---

### Những Gì Học Được

#### Tư duy thiết kế & Nghiệp vụ
- **Tập trung vào bài toán thực tế:** Kiến trúc AI có phức tạp đến đâu cũng không có giá trị nếu không giải quyết đúng bài toán nghiệp vụ. Cần bắt đầu từ vướng mắc của người dùng trước khi chọn công nghệ.
- **Quản lý Scope khi làm Hackathon:** Giữ scope vừa sức, làm sản phẩm MVP chạy mượt và tập trung truyền tải rõ ràng giải pháp khi pitching.
- **Vai trò Human-in-the-loop:** Với các mảng nhạy cảm như Tài chính hay F&B, AI đóng vai trò xử lý dữ liệu và gợi ý, còn con người vẫn giữ quyền duyệt cuối cùng.

#### Kiến trúc kỹ thuật & FinOps
- **Chia nhỏ Agent để thu hẹp Context:** Phân tách hệ thống thành các Sub-Agent chuyên biệt (Crawler, KYC, Money Flow...) giúp thu hẹp Context Window, dễ chọn model nhỏ cho task đơn giản để tiết kiệm chi phí.
- **Ý thức kiểm soát chi phí (FinOps):** Dùng code thuần tiền xử lý dữ liệu trước khi gọi LLM và cân nhắc chọn model gọn nhẹ (như bài học chuyển từ SageMaker sang YOLO Small).
- **Phối hợp dịch vụ AWS:** Nắm được cách liên kết các dịch vụ Kinesis, Step Functions, Lambda, Bedrock Agent Core Memory, DynamoDB và Vector OpenSearch vào sơ đồ kiến trúc hoàn chỉnh.

---

### Ứng Dụng Vào Công Việc & Học Tập

- **Áp dụng vào đồ án môn học:** Vận dụng mô hình phân tách Multi-Agent và cách thu hẹp context window vào các đồ án phần mềm tại trường.
- **Tối ưu chi phí Cloud:** Ưu tiên mô hình Serverless và chọn cấu hình/model đủ dùng khi làm việc với AWS để tránh phát sinh chi phí thừa.
- **Kỹ năng làm việc nhóm & Pitching:** Học hỏi cách vẽ sơ đồ kiến trúc rõ ràng và thiết kế slide đúc kết giải pháp gãy gọn từ các đội quán quân.

---

### Trải Nghiệm Khi Xem Sự Kiện Trực Tuyến

Xem qua YouTube tuy không trực tiếp ở hội trường nhưng mấy đoạn các đội kể chuyện thức 3h sáng debug hay bấm nhầm SageMaker bay mất $48 coi cuốn thật. Buổi này giúp mình vỡ ra nhiều thứ về cách chia bớt việc cho Agent để đỡ tốn tiền token, cũng như cách chọn model phù hợp với ngân sách.

Dù chỉ theo dõi lại qua video và tài liệu ghi chép, mình vẫn học được rất nhiều bài học thực tế từ cách các đội thi đóng gói sản phẩm và giải quyết vấn đề.

---

### Bài Học Rút Ra & Đóng Góp Cá Nhân

- **Bài học rút ra:** Cần giữ tinh thần chủ động học hỏi, hiểu rõ bài toán nghiệp vụ trước khi cắm đầu vào làm công nghệ và luôn linh hoạt khi chọn giải pháp kỹ thuật.
- **Đóng góp cá nhân:** Tổng hợp và ghi chép lại toàn bộ nội dung từ video/phụ đề sự kiện thành bài thu hoạch hệ thống, chia sẻ kiến trúc Multi-Agent và kinh nghiệm FinOps cho cộng đồng AWS FCJ.

---

### Một số hình ảnh khi tham gia sự kiện

Dưới đây là một số hình ảnh ghi lại slide kiến trúc và bài trình bày của các đội thi tại sự kiện **AWS Agentic AI Buildweek Showcase & Sharing**:

![Banner chính thức giới thiệu chuỗi sự kiện FCAJ - Agentic AI Build Week 2026 tại Bitexco Financial Tower](/images/4-EventParticipated/4.4-Event4/1.png)

![Diễn giả Mr. Joseph Marazota (Head of Technology, AWS ASEAN) phát biểu mở màn cùng sự xuất hiện của anh Nguyễn Gia Hưng (Head of SA, AWS Vietnam) bên bục sân khấu](/images/4-EventParticipated/4.4-Event4/2.jpg)

![Slide mở màn bài trình bày dự án AI-Powered Conversation Ordering của đội quán quân One Team](/images/4-EventParticipated/4.4-Event4/3.png)

![Đội Plan V trình bày slide mở màn dự án Solution Architect Professional Native App](/images/4-EventParticipated/4.4-Event4/5.png)

![Diễn giả đại diện Team Plan V trình bày sơ đồ kiến trúc hạ tầng AWS dự án Solution Architect Native App (bao gồm CloudFront, ECS Fargate, Amazon Bedrock, PostgreSQL và Draw.io)](/images/4-EventParticipated/4.4-Event4/4.png)

![Đội 3KA chia sẻ slide 4 giai đoạn chinh phục cuộc thi Hackathon "The Journey Ahead: Four stages of our hackathon"](/images/4-EventParticipated/4.4-Event4/6.png)

![Sơ đồ kiến trúc xử lý luồng Video Stream thời gian thực của dự án Sheper (đội 3KA) sử dụng Kinesis Video Stream, ECS Stream Processor, SageMaker Endpoint và Bedrock AgentCore Runtime](/images/4-EventParticipated/4.4-Event4/7.png)

![Đội Six Pillars trình bày giải pháp Adaptive AML Workflow Engine ứng dụng AI tự động hóa điều tra rửa tiền cho ngân hàng](/images/4-EventParticipated/4.4-Event4/8.png)

![Giao diện báo cáo và quy trình tự động hóa làm giàu dữ liệu điều tra tuân thủ pháp lý của dự án Adaptive AML Workflow Engine](/images/4-EventParticipated/4.4-Event4/9.png)

![Slide giới thiệu danh sách thành viên đội Six Pillars tham gia tranh tài tại cuộc thi Hackathon Agentic AI Build Week](/images/4-EventParticipated/4.4-Event4/10.png)

> *Buổi chia sẻ mang lại nhiều bài học kiến trúc thực tế, giúp mình củng cố tư duy thiết kế hệ thống Multi-Agent và quản lý chi phí khi làm việc trên AWS.*
