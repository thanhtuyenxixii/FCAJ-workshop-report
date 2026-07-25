---
title: "Event 1"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

## Bài thu hoạch AWS Vietnam Community Day 2026

| Thông tin sự kiện | Chi tiết |
| :--- | :--- |
| **Tên sự kiện** | AWS Vietnam Community Day 2026 (Saturday Meetup) |
| **Thời gian** | 23/05/2026 (09:00 - 12:00) |
| **Địa điểm** | Tầng 26, Bitexco Financial Tower, TP. Hồ Chí Minh |
| **Vai trò** | Người tham dự |

### Mục Đích Của Sự Kiện
Sự kiện AWS Vietnam Community Day 2026 tổ chức tại tòa nhà Bitexco là buổi gặp gỡ trực tiếp nhằm kết nối cộng đồng học tập và làm việc trong hệ sinh thái đám mây AWS tại Việt Nam.

Nội dung chương trình tập trung vào các bài toán vận hành hệ thống lớn thực tế từ GoTymeX, VPBank và VIB. Buổi chia sẻ giúp sinh viên năm cuối như mình cập nhật thêm các hướng đi mới về tối ưu hạ tầng, ứng dụng Generative AI/Multi-Agent và học hỏi kinh nghiệm làm việc thực chiến.

### Danh Sách Diễn Giả

| STT | Diễn giả | Chức vụ | Chủ đề |
| :-: | -------- | ------- | ------ |
| 1 | **Tinh Truong** | Platform Engineer @GoTymeX | *Context Is Everything – Making AI Actually Work for You* |
| 2 | **Pham Ng Hai Anh** | AWS Community Builder @G-AsiaPacificVietnam | *Friendly AI Assistant w/ Amazon Quick* |
| 3 | **Nguyen Tuan Thinh** | AWS Champion Instructor, 12x AWS Certified | *From Edge To Origin: CloudFront as Your Foundation* |
| 4 | **Team VIB** | Quán quân AWS Track @LotusHacks2026 | *36 hours with LotusHacks – Building UTMorpho* |
| 5 | **Duc Dao** | Solution Architect @CloudKinetics | *Non-Determinism of "Deterministic" LLM Settings* |
| 6 | **Vy Lam** | Sr. Business Systems Analyst @VPBank | *Enterprise-Grade Multi-Agent System* |

---

### Nội Dung Nổi Bật

#### 1. Những "nỗi đau" từ hệ thống Monolith cũ & AI thô sơ

Qua phần chia sẻ của các diễn giả, nhiều doanh nghiệp hiện vẫn gặp vướng mắc với hệ thống Monolith hoặc phân phối nội dung trực tiếp từ một server gốc mà không qua CDN:

- **Độ trễ cao:** User ở Việt Nam gọi về origin server đặt ở Mỹ phải đợi >200ms, tốn băng thông quốc tế và trải nghiệm tải trang bị chậm.
- **Rủi ro bảo mật:** Mở kết nối công khai trực tiếp tới origin server khiến hệ thống dễ thành mục tiêu cho các cuộc tấn công DDoS hoặc botnet khi không có lớp chặn ở biên (Edge).
- **Hạn chế khi dùng AI thô sơ:** Gửi prompt đơn lẻ mà không quản lý context/bộ nhớ khiến AI hay bị hallucination và trả lời thiếu chính xác.
- **Lầm tưởng về `Temperature = 0`:** Đặt nấc này không đồng nghĩa LLM sẽ trả ra kết quả hoàn toàn giống nhau 100%, do cơ chế tối ưu GPU chạy song song bất đồng bộ sinh ra tính phi xác định (Non-Determinism).

#### 2. Xu hướng Microservices & Ứng dụng Multi-Agent vào Banking

Dịch chuyển từ Monolith sang Microservices là hướng đi bắt buộc để chia nhỏ hệ thống lớn thành các dịch vụ độc lập giao tiếp qua API/Event.

Một điểm hay từ bài nói của chị Vy Lam (VPBank) là áp dụng tư duy Microservices vào thiết kế AI dưới dạng hệ thống **Multi-Agent**:
- Thay vì bắt 1 con LLM ôm đồm tất cả, hệ thống tách thành các Agent chuyên biệt: thu thập dữ liệu doanh nghiệp, phân tích rủi ro tín dụng và kiểm tra tuân thủ.
- Tách nhỏ agent giúp cô lập lỗi tốt hơn và nâng cấp từng phần độc lập.

#### 3. Phân tách nghiệp vụ với Domain-Driven Design (DDD)

DDD đóng vai trò xác định ranh giới cho dịch vụ hoặc từng Agent. Kỹ sư cần làm việc với bộ phận nghiệp vụ để chốt ngôn ngữ chung (Ubiquitous Language) và chia hệ thống thành các Bounded Contexts.

- **Bài học từ nhóm VIB (LotusHacks 2026):** Trong 36 tiếng làm sản phẩm UTMorpho, việc dùng DDD để phân tách ranh giới nghiệp vụ giúp nhóm không bị rối scope và kịp hoàn thành MVP.
- **Ứng dụng tại VPBank:** Mô hình "Ủy ban tín dụng ảo" bằng Multi-Agent đòi hỏi ranh giới trách nhiệm của từng agent phải rõ ràng để đảm bảo tuân thủ quy trình ngân hàng.

#### 4. Tối ưu độ trễ với Event-Driven Architecture & CloudFront Edge

- **Event-Driven:** Các dịch vụ hoặc Agent phát/nhận sự kiện bất đồng bộ, giảm phụ thuộc trực tiếp.
- **Xử lý tại trạm biên (Edge):** Kết hợp Amazon CloudFront với CloudFront Functions và Lambda@Edge cho phép can thiệp request/response ngay tại Edge Location gần user. Rewrite URL hoặc sửa header tại Edge giúp đạt độ trễ <1ms mà không cần đẩy request ngược về server gốc ở xa.

#### 5. Sự tiến hóa của Compute: Từ EC2 đến Serverless

Nhìn lại hành trình tiến hóa compute: Máy chủ vật lý -> Máy ảo EC2 -> Container (ECS, Fargate) -> Serverless (AWS Lambda).

Dùng Serverless giúp bỏ qua khâu vận hành OS và vá lỗi máy chủ. Hệ thống tự scale linh hoạt theo traffic và chỉ tính tiền khi code chạy, giúp dev tập trung hoàn toàn vào logic ứng dụng.

#### 6. Trợ lý AI tăng tốc quy trình phát triển (Amazon Q & Quick Suite)

Amazon Q Developer cùng bộ công cụ Amazon Quick (Quick Chat, Quick Flow, Quick Spaces, Quick Sight) hỗ trợ đáng kể cho quy trình làm phần mềm:

- Hỗ trợ sinh code, refactor và viết unit test nhanh hơn.
- Công cụ Quick Sight cho phép nhân viên nghiệp vụ gõ câu lệnh tự nhiên để truy vấn data thô và dựng dashboard báo cáo trực quan mà không cần gõ SQL.

---

### Những Gì Học Được

#### Tư duy thiết kế & Kiến trúc
- CloudFront không chỉ để cache ảnh/file tĩnh, nó là lớp lá chắn bảo vệ origin server (Origin Cloaking) và giảm tải cho backend.
- Xây dựng ứng dụng AI cần chuẩn bị context và bộ nhớ dài hạn (qua RAG) thì AI mới trả lời đúng nghiệp vụ.
- Thiết kế hệ thống phải bắt đầu từ nghiệp vụ thực tế (Business-first) chứ không nên ép công nghệ phức tạp vào bài toán đơn giản.

#### Kỹ thuật CloudFront & LLM
- Phân biệt công cụ Edge:
  - *CloudFront Functions:* Chạy JS siêu nhẹ tại Edge, siêu nhanh (<1ms), thích hợp sửa header, redirect URL.
  - *Lambda@Edge:* Chạy full Node.js/Python cho các logic phức tạp hơn.
- Hiểu nguyên nhân LLM bị phi xác định (do phép tính dấu phẩy động trên GPU) và cách dùng Guardrails để kiểm soát output.

---

### Ứng Dụng Vào Công Việc & Học Tập

- Cấu hình CloudFront đứng trước backend trong các đồ án (như IoT Weather Platform) để tăng tốc tải data và bảo vệ origin bằng OAC / AWS Shield.
- Triển khai web tĩnh S3 + CloudFront với Origin Access Control (OAC) để chặn truy cập trực tiếp vào S3 bucket.
- Tích hợp Amazon Q Developer vào VS Code hỗ trợ viết code và unit test.
- Tự làm một bot hỗ trợ học AWS bằng RAG để sắp xếp ghi chú cá nhân.

---

### Trải Nghiệm & Thảo Luận Tại Sự Kiện

- **Học từ diễn giả:** Ấn tượng với bài chia sẻ CloudFront của anh Nguyễn Tuấn Thịnh (AWS Champion Instructor). Cách anh giải thích cơ chế CDN và Edge Locations tại VN bằng ví dụ thực tế rất dễ ngấm.
- **Demo thực tế:** Xem quy trình thẩm định tín dụng Multi-Agent của VPBank và nghe nhóm VIB kể lại các sự cố, cách xử lý bug phút chót tại LotusHacks cho mình nhiều góc nhìn thực chiến.
- **Networking:** Giờ giải lao mình có tranh thủ hỏi thêm các anh kỹ sư lâu năm và các AWS Community Builders về kinh nghiệm làm việc và cách chọn tài liệu ôn thi chứng chỉ AWS.

---

### Bài Học Rút Ra & Đóng Góp Cá Nhân

- **Bài học rút ra:** Cách truyền đạt kỹ thuật tốt nhất là quy đổi khái niệm phức tạp thành ví dụ gần gũi. Tối ưu ở biên (Edge) và chuẩn bị context cho AI là 2 yếu tố then chốt cho hệ thống đám mây hiện đại.
- **Đóng góp cá nhân:** Đặt câu hỏi thảo luận trong phiên CloudFront để làm rõ ranh giới áp dụng giữa CloudFront Functions và Lambda@Edge; giao lưu kết nối với các bạn trong cộng đồng AWS FCJ.

---

### Một số hình ảnh khi tham gia sự kiện

Dưới đây là một số hình ảnh ghi lại các slide kiến trúc và khoảnh khắc tại sự kiện AWS Vietnam Community Day 2026:

![Slide "What's Next" giới thiệu dự án UTMorpho của nhóm VIB với mã QR truy cập Devpost và GitHub](/images/4-EventParticipated/4.1-Event1/IMG20260523105858.jpg)

![Slide giới thiệu các tham số cấu hình mô hình ngôn ngữ lớn (LLM): Top-P và Top-K cùng mã QR trải nghiệm trực quan](/images/4-EventParticipated/4.1-Event1/IMG20260523111358.jpg)

![Slide phân tích thực tế nghiên cứu hiệu năng của các mô hình LLM trên nhiều tác vụ xử lý ngôn ngữ tự nhiên (NLP) khác nhau](/images/4-EventParticipated/4.1-Event1/IMG20260523111644.jpg)

![Slide giải thích nguyên nhân kỹ thuật gây ra tính phi xác định ở LLM từ phép tính dấu phẩy động IEEE 754 trên GPU và thứ tự thực thi thread song song](/images/4-EventParticipated/4.1-Event1/IMG20260523112629.jpg)
![Slide đề xuất các chiến lược giảm thiểu tính phi xác định của LLM như chạy nhiều lần, định dạng đầu ra cấu trúc và tự host model](/images/4-EventParticipated/4.1-Event1/IMG20260523112744.jpg)
![Slide tổng hợp các mẹo (Tips) cấu hình mô hình LLM: rủi ro lặp của greedy decoding, giá trị tối ưu nhiệt độ và repeat penalty](/images/4-EventParticipated/4.1-Event1/IMG20260523113044.jpg)
![Slide "Tips" cấu hình LLM có thêm ghi chú màu đỏ về việc chọn nhiệt độ 0.1 để tránh lặp nội dung](/images/4-EventParticipated/4.1-Event1/IMG_20260523_113141.jpg)

![Slide tổng kết các bài học cốt lõi (Key Takeaways) về việc thiết kế ứng dụng thích ứng với độ lệch và chú trọng kiểm thử](/images/4-EventParticipated/4.1-Event1/IMG20260523113240.jpg)

![Slide hiển thị mã QR liên kết đến bài báo nghiên cứu chuyên sâu về các thiết lập của mô hình ngôn ngữ lớn (LLM Settings)](/images/4-EventParticipated/4.1-Event1/IMG20260523113404.jpg)

![Slide chương trình (Agenda) giới thiệu cấu trúc đa tác nhân (Multi-Agent) cấp doanh nghiệp ứng dụng trong thẩm định tín dụng](/images/4-EventParticipated/4.1-Event1/IMG20260523114740.jpg)

![Slide phân tích lý do kiến trúc đa tác nhân hoạt động hiệu quả nhờ tính chuyên môn hóa, kiểm tra chéo và khả năng chịu lỗi](/images/4-EventParticipated/4.1-Event1/IMG20260523120206.jpg)

![Slide giới thiệu 6 trụ cột của hệ thống AI cấp doanh nghiệp: Bảo mật, Quản trị dữ liệu, Mạng, Vận hành, Con người và Tuân thủ](/images/4-EventParticipated/4.1-Event1/IMG20260523120612.jpg)

![Slide đề xuất quy trình triển khai ứng dụng Multi-Agent chi tiết qua từng bước từ local app đến hạ tầng AWS](/images/4-EventParticipated/4.1-Event1/IMG20260523121833.jpg)

![Sơ đồ kiến trúc mô tả luồng triển khai cơ bản (Basic Deployment Flow) từ môi trường local lên hạ tầng cloud AWS](/images/4-EventParticipated/4.1-Event1/IMG20260523121900.jpg)

![Slide danh sách các bài thực hành workshop từ cơ bản đến nâng cao bao gồm xác thực, Guardrails, MCP và Terraform](/images/4-EventParticipated/4.1-Event1/IMG20260523122352.jpg)

![Slide bài thực hành workshop có thêm ghi chú màu đỏ nhấn mạnh việc xây dựng hệ thống không chỉ cần chạy được mà phải an toàn](/images/4-EventParticipated/4.1-Event1/IMG_20260523_123205.jpg)

![Bức ảnh tập thể các thành viên chụp chung lưu niệm khi kết thúc sự kiện](/images/4-EventParticipated/4.1-Event1/event1.jpg)

> *Sự kiện mang lại nhiều góc nhìn kiến trúc thực tế, củng cố thêm định hướng học tập về Cloud và Generative AI cho bản thân.*
