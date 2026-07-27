---
title: "Event 2"
date: 2026-06-06
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

## Bài thu hoạch AWS First Cloud Journey AI

| Thông tin sự kiện | Chi tiết |
| :--- | :--- |
| **Tên sự kiện** | AWS First Cloud Journey AI |
| **Thời gian** | 06/06/2026 (09:00 - 12:00) |
| **Địa điểm** | Tầng 26, Bitexco Financial Tower, TP. Hồ Chí Minh |
| **Vai trò** | Người tham dự |

### Mục Đích Của Sự Kiện
Sự kiện **AWS First Cloud Journey AI** tổ chức tại văn phòng AWS Việt Nam (tầng 26 Bitexco). Buổi này tập trung chia sẻ về AI, ứng dụng Cloud-native, kỹ năng làm việc nhóm và định hướng nghề nghiệp cho sinh viên cũng như lập trình viên trẻ.

Nội dung bao quát từ thiết kế game multiplayer thời gian thực, tối ưu đóng gói container với Docker, xây dựng tìm kiếm GraphRAG, đến bảo mật Web bằng Machine Learning và lộ trình thăng tiến thực tế từ Helpdesk lên Sysadmin.

### Danh Sách Diễn Giả

| STT | Diễn giả | Chức vụ | Chủ đề |
| :-: | -------- | ------- | ------ |
| 1 | **Nguyễn Quốc Bảo** | Cloud Engineer / Game Developer | *Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets* |
| 2 | **Huỳnh Bảo** | Junior Cloud Native Developer @Endava Vietnam | *Docker — A containerization technology* |
| 3 | **Việt Phát** | AI Majoring @Swinburne University of Technology | *GraphRAG: Build GraphRAG applications using Amazon Bedrock and Amazon Neptune* |
| 4 | **Lê Hoàng Gia Đại** | Sinh viên năm cuối @HUTECH University | *WAF + ML for Cyber Attack Detection: Machine Learning-based Network Intrusion Detection System (NIDS) on AWS* |
| 5 | **Trần Trung Vinh** | System Administrator @Central Retail Group | *From IT Helpdesk to Senior Sysadmin* |
| 6 | **Trương Huy Phước** | Presenter / Teamwork Coach | *The Art of Effective Teamwork* |

---

### Nội Dung Nổi Bật

#### 1. Panel Thảo Luận Giám Đốc AWS AI

Mấy chia sẻ ở phiên Panel gãi đúng chỗ ngứa về thực trạng học và làm AI hiện nay:

- **Tư duy thiết kế quan trọng hơn gõ code:** Trong thời đại AI hỗ trợ viết code, kỹ năng quan trọng nhất của dev trẻ là tư duy giải quyết bài toán, thiết kế kiến trúc và biết cách ghép nối các dịch vụ Cloud. Code thủ công sẽ bớt dần, nhưng tư duy phân tích nghiệp vụ vẫn là chìa khóa.
- **Thu hẹp khoảng cách doanh nghiệp:** Doanh nghiệp muốn dùng AI ngay nhưng đội ngũ triển khai thường thiếu kỹ năng thực tế. Giải pháp tốt nhất là tận dụng các Managed Services có sẵn của AWS để rút ngắn thời gian và đỡ tốn chi phí vận hành.
- **Cơ hội cho sinh viên:** AI và Cloud đã hạ thấp rào cản kỹ thuật. Chỉ cần tư duy đúng và ý tưởng tốt, sinh viên hoàn toàn có thể tự dựng sản phẩm chạy toàn cầu.

#### 2. Game Multiplayer với Godot & AWS WebSockets

Phần chia sẻ này demo hệ thống game kéo-búa-bao nhiều người chơi thời gian thực với các ghi chép kỹ thuật chính:

- **Lựa chọn giao thức mạng:**
  - *UDP/ENet:* Nhẹ, độ trễ cực thấp (hợp game FPS, đua xe), nhưng phải tự viết logic đảm bảo truyền tin tin cậy.
  - *HTTP Polling:* Dễ làm nhưng trễ cao, lãng phí băng thông do client phải hỏi server liên tục.
  - *WebSocket:* Lựa chọn hợp lý cho game theo lượt hoặc sảnh chờ (lobby) nhờ truyền nhận 2 chiều full-duplex thời gian thực.
- **Kiến trúc trên AWS:** Client Godot dùng `WebSocketPeer` nối tới API Gateway WebSocket. API Gateway dựa vào `$request.body.action` để gọi AWS Lambda (Node.js 20) xử lý logic và lưu trạng thái vào DynamoDB.
- **Một số vướng mắc thực tế:**
  - *Stale Connections:* Khách thoát game đột ngột làm `connectionId` bị rác trong DynamoDB, gây lỗi `GoneException` khi server gửi data.
  - *Chi phí DynamoDB Scan:* Dùng `ScanCommand` để ghép cặp (matchmaking) sẽ làm quét toàn bộ table, tốn tiền và chậm khi user tăng.
  - *Stateless Lambda:* Lambda không lưu trạng thái nên mỗi request đều phải đọc/ghi lại data game vào DynamoDB.
- **Hướng nâng cấp:** Chuyển sang AWS GameLift nếu cần dedicated server chạy tính toán vật lý phức tạp.

#### 3. Ghi chép về Docker & Containerization

Anh Huỳnh Bảo thì chia sẻ cách đóng gói ứng dụng bằng Docker để xử lý triệt để lỗi "chạy trên máy tôi thì được nhưng lên server thì lỗi":

- **Hạn chế của VM truyền thống:** Mỗi VM phải gánh 1 OS riêng, nặng nề và ngốn CPU/RAM/ổ cứng.
- **Lợi ích của Docker:** Đóng gói app + thư viện phụ thuộc vào 1 container siêu nhẹ, dùng chung OS kernel với máy chủ.
- **Cơ chế Image Layers:** Mỗi dòng trong `Dockerfile` tạo ra 1 layer. Docker tận dụng cache layer cũ giúp thời gian build lại image cực nhanh.
- **Ứng dụng:** Là nền tảng để làm Microservices, dựng pipeline CI/CD tự động.

#### 4. Ứng dụng GraphRAG với Amazon Bedrock & Neptune

Bài chia sẻ của bạn Việt Phát tập trung vào cách giải bài toán tìm kiếm ngữ cảnh phức tạp cho LLM:

- **Điểm yếu của RAG thường:** RAG dựa trên vector search thông thường hay bị hạn chế khi gặp câu hỏi cần suy luận qua nhiều bước (multi-hop reasoning).
- **Giải pháp GraphRAG:** Dùng đồ thị tri thức (Knowledge Graph) lưu thực thể (nodes) và quan hệ (edges). Khi LLM nhận câu hỏi, hệ thống sẽ duyệt đồ thị (graph traversal) qua nhiều tài liệu để trích xuất ngữ cảnh liên kết.
- **Cách triển khai trên AWS:**
  - *Fully Managed:* Dùng Amazon Bedrock Knowledge Bases tự động chunking, trích xuất entity và lưu đồ thị trên Amazon Neptune Analytics.
  - *Custom Route:* Dùng LlamaIndex kết hợp Amazon Neptune, dùng ngôn ngữ Cypher Query để duyệt đồ thị theo ý muốn.

#### 5. Bảo mật Web: Kết hợp WAF + Machine Learning (NIDS)

Khá ấn tượng với bài WAF + ML của bạn Gia Đại (đồng môn HUTECH luôn). Cách bạn xử lý imbalanced dataset trên bộ CSE-CIC-IDS2018 rồi đẩy log qua Firehose về S3 nhìn rất mượt và thực tế:

- **Hạn chế của WAF truyền thống:** WAF chỉ chạy theo rule định sẵn nên dễ bỏ sót các đợt tấn công zero-day hoặc hành vi bất thường mới.
- **Mô hình WAF + Machine Learning:**
  - Huấn luyện model LightGBM trên bộ dữ liệu CSE-CIC-IDS2018 trên AWS để phát hiện xâm nhập mạng (NIDS).
  - Tiền xử lý dữ liệu: Gộp CSV, làm sạch dữ liệu nhiễu (NaN, âm, vô cực) và cân bằng lại các lớp nhãn tấn công thiểu số trước khi train.
- **Luồng hạ tầng AWS:** VPC chứa EC2 chạy model NIDS sau ALB. Traffic log gửi real-time qua Kinesis Data Firehose về S3 -> Lambda phân tích -> bắn cảnh báo qua SNS tới Security Hub, GuardDuty và CloudWatch.

#### 6. Lộ trình phát triển: Từ IT Helpdesk lên Senior Sysadmin

Bài chia sẻ từ anh Trần Trung Vinh (Central Retail) cho mình nhiều góc nhìn thực tế về tự học và thăng tiến nghề nghiệp:

- **Kỹ năng tích lũy từ Helpdesk:** Học cách chịu áp lực khi xử lý sự cố, kỹ năng giao tiếp với user và tư duy tìm nguyên nhân gốc rễ (troubleshooting).
- **Bước ngoặt:** Tự học sâu Linux và Networking, tự dựng phòng Lab thực hành ảo hóa tại nhà.
- **Triết lý Sysadmin:**
  - Nắm vững nguyên tắc: *"Không bao giờ test trực tiếp trên Production"*.
  - Dịch chuyển tư duy sang Cloud (AWS), hạ tầng dạng mã (IaC - Terraform) và DevOps tự động hóa.

#### 7. Kinh nghiệm làm việc nhóm (Teamwork & Digital Tools)

Anh Trương Huy Phước đúc kết 4 quy tắc làm việc nhóm:
1. *Mục tiêu rõ ràng (Clear Goals)*: Cùng hướng về một đích đến.
2. *Đúng người đúng việc (Right Place)*: Phân chia theo thế mạnh cá nhân.
3. *Giao tiếp cởi mở (Open Communication)*: Lắng nghe và phản hồi tôn trọng.
4. *Trách nhiệm cá nhân (Accountability)*: Chủ động chốt task đúng hạn.

Công cụ hỗ trợ: Trello/ClickUP quản lý task; Slack/Discord/Google Workspace để trao đổi và lưu trữ tài liệu.

---

### Những Gì Học Được

#### Tư duy kỹ thuật
- Kết hợp Graph Database (Amazon Neptune) với LLM giúp giải bài toán RAG phức tạp hiệu quả hơn hẳn vector search thuần túy.
- Thiết kế ứng dụng real-time phải chọn đúng giao thức (WebSocket vs UDP) và luôn có phương án xử lý ngắt kết nối đột ngột (stale connection).
- Làm bảo mật nên kết hợp Machine Learning với WAF để chủ động phát hiện hành vi bất thường thay vì chỉ phụ thuộc vào rule tĩnh.

#### Kỹ năng & Công cụ
- Nắm cách API Gateway WebSocket dùng Route Key và `connectionId` trong DynamoDB để quản lý session.
- Hiểu cơ chế layer cache của Dockerfile để tối ưu tốc độ build.
- Bài học tự học từ anh Vinh: tập trung học chắc nền tảng (Linux & Networking) trước khi chạy theo các công cụ Cloud đắt tiền.

---

### Ứng Dụng Vào Công Việc & Học Tập

- Viết Dockerfile đóng gói các bài tập web ở trường, tối ưu layer cache và thử push lên Amazon ECR.
- Dựng phòng lab Linux/Networking nhỏ trên máy cá nhân để tập viết script shell tự động hóa.
- Thử nghiệm làm game đơn giản bằng Godot nối với API Gateway WebSocket và DynamoDB.
- Áp dụng các công cụ Trello/Slack vào bài tập nhóm ở HUTECH để quản lý tiến độ rõ ràng hơn.

---

### Trải Nghiệm & Thảo Luận Tại Sự Kiện

- **Góc nhìn thực tế:** Ấn tượng nhất là bài WAF + ML của bạn Gia Đại (HUTECH) và câu chuyện tự học dựng lab từ Helpdesk lên Sysadmin của anh Vinh. Nghe xong thấy có thêm nhiều động lực để cày sâu lab cá nhân.
- **Demo thực tế:** Phần demo game kéo-búa-bao qua WebSocket của anh Bảo giúp mình thấy rõ cách nạp/nhả `connectionId` trong DynamoDB thực tế chạy như thế nào.
- **Networking:** Giờ nghỉ mình có trao đổi thêm với anh Huỳnh Bảo về môi trường làm việc tại Endava và các hoạt động tại ITea Lab.

---

### Bài Học Rút Ra & Đóng Góp Cá Nhân

- **Bài học rút ra:** Giải pháp tốt là giải pháp vừa đủ xài cho bài toán nghiệp vụ chứ không cần quá phức tạp. Nền tảng Linux & Networking vững sẽ giúp học Cloud/DevOps nhanh hơn rất nhiều.
- **Đóng góp cá nhân:** Đặt câu hỏi thảo luận về sự khác biệt chi phí/hiệu năng giữa Custom GraphRAG và Managed Bedrock Knowledge Bases; giao lưu kết nối với các bạn sinh viên cùng tham dự.

---

### Một số hình ảnh khi tham gia sự kiện 

Dưới đây là một số hình ảnh ghi lại slide kiến trúc và khoảnh khắc tại sự kiện AWS First Cloud Journey AI:

![Slide hiển thị 3 câu hỏi thảo luận lớn của Panel thảo luận với Giám đốc AWS AI về xu hướng AI, kỹ năng cần thiết cho lập trình viên trẻ và giải quyết khoảng cách năng lực doanh nghiệp](/images/4-EventParticipated/4.2-Event2/IMG20260606090407.jpg)

![Giám đốc AWS AI trình bày về việc điện toán đám mây và AI giúp hạ thấp rào cản gia nhập thị trường cho các nhà xây dựng công nghệ trẻ](/images/4-EventParticipated/4.2-Event2/IMG20260606091540.jpg)

![Slide tiêu đề phần thuyết trình "Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets" của diễn giả Nguyễn Quốc Bảo](/images/4-EventParticipated/4.2-Event2/IMG20260606092843.jpg)

![Slide mục lục (Table of Contents) của chủ đề game multiplayer kết nối qua WebSocket](/images/4-EventParticipated/4.2-Event2/IMG20260606092927.jpg)

![Slide mô tả chi tiết thiết kế Schema cơ sở dữ liệu DynamoDB (DynamoDB Schema) lưu trữ trạng thái người chơi và đối thủ](/images/4-EventParticipated/4.2-Event2/IMG20260606093539.jpg)

![Slide đúc kết các thách thức kỹ thuật (Challenges) như Stale Connections, chi phí DynamoDB Scan và tính stateless của Lambda](/images/4-EventParticipated/4.2-Event2/IMG20260606095009.jpg)

![Slide tiêu đề chủ đề "Docker — A containerization technology" do anh Huỳnh Bảo trình bày](/images/4-EventParticipated/4.2-Event2/IMG20260606095859.jpg)

![Slide chương trình giới thiệu (Agenda) các mục chính của phần chia sẻ Docker](/images/4-EventParticipated/4.2-Event2/IMG20260606100049.jpg)

![Slide phân tích lợi ích của ảo hóa và sự cần thiết của công nghệ container hóa đối với việc tối ưu hóa tài nguyên máy chủ](/images/4-EventParticipated/4.2-Event2/IMG20260606100430.jpg)

![Slide so sánh trực quan (VM vs Container) chi tiết về cấu trúc hệ thống, kích thước và tài nguyên tiêu thụ](/images/4-EventParticipated/4.2-Event2/IMG20260606101230.jpg)

![Slide cảm ơn (Thank You) kết thúc phần trình bày Docker của anh Huỳnh Bảo với hình ảnh nhân vật Kasane Teto](/images/4-EventParticipated/4.2-Event2/IMG20260606103701.jpg)

![Slide chương trình (Agenda) giới thiệu cấu trúc các nội dung của chuyên đề GraphRAG do bạn Việt Phát chia sẻ](/images/4-EventParticipated/4.2-Event2/IMG20260606103959.jpg)

![Slide tiêu đề chuyên đề "WAF + ML for Cyber Attack Detection: Machine Learning-based Network Intrusion Detection System (NIDS) on AWS" của bạn Lê Hoàng Gia Đại](/images/4-EventParticipated/4.2-Event2/IMG20260606105006.jpg)

![Slide giới thiệu chi tiết về chức năng bảo vệ ứng dụng web của AWS WAF (Web Application Firewall)](/images/4-EventParticipated/4.2-Event2/IMG20260606105132.jpg)

![Slide sơ đồ nguyên lý hoạt động của NIDS (How NIDS Works) kết nối giữa Web traffic, ML Model, DB và cảnh báo](/images/4-EventParticipated/4.2-Event2/IMG20260606105920.jpg)

![Sơ đồ kiến trúc hạ tầng đám mây tích hợp AWS WAF, Lambda, S3, Kinesis Firehose, CloudWatch và các dịch vụ bảo mật của AWS](/images/4-EventParticipated/4.2-Event2/IMG20260606110049.jpg)

![Slide tổng hợp các kết quả thực nghiệm, định hướng cải tiến sử dụng Amazon Bedrock và các bài học kinh nghiệm của bạn Lê Hoàng Gia Đại](/images/4-EventParticipated/4.2-Event2/IMG20260606110335.jpg)

![Slide tiêu đề bài chia sẻ "From IT Helpdesk to Senior Sysadmin" của anh Trần Trung Vinh tại Central Retail Group](/images/4-EventParticipated/4.2-Event2/IMG20260606111037.jpg)

![Slide mục lục chương trình (Agenda) chia sẻ lộ trình thăng tiến sự nghiệp từ kỹ sư Helpdesk lên Sysadmin và Cloud/DevOps](/images/4-EventParticipated/4.2-Event2/IMG20260606111212.jpg)

![Slide phân tích những kỹ năng học được từ Helpdesk và bước ngoặt (Turning Point) khi tự xây dựng phòng Lab nghiên cứu](/images/4-EventParticipated/4.2-Event2/IMG20260606111433.jpg)

![Slide đúc kết thực tế vai trò Sysadmin (Realistic View) cùng các bài học xương máu về việc không bao giờ được kiểm thử trên Production](/images/4-EventParticipated/4.2-Event2/IMG20260606111742.jpg)

![Slide lộ trình học tập DevOps hiện đại (Modern DevOps Roadmap) gồm 8 bước từ Linux đến Monitoring & Observability](/images/4-EventParticipated/4.2-Event2/IMG20260606112554.jpg)

![Slide hiển thị thông tin liên hệ và mã QR kết nối (Connect With Me) của diễn giả Trần Trung Vinh](/images/4-EventParticipated/4.2-Event2/IMG20260606113609.jpg)

![Ảnh chụp cuốn sổ tay ghi chú thứ tự trình bày và tóm tắt các diễn giả của sự kiện do tôi ghi lại](/images/4-EventParticipated/4.2-Event2/note.png)

> *Buổi chia sẻ mang lại nhiều kiến thức thực tế, giúp mình hiểu rõ hơn cách kết nối công nghệ trên AWS và có thêm động lực học tập.*
