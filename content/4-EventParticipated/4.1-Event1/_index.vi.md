---
title: "Event 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---


# Bài thu hoạch “AWS COMMUNITY DAY”

### Mục Đích Của Sự Kiện

- Cập nhật công nghệ và xu hướng mới
- Chia sẻ kinh nghiệm thực chiến
- Kết nối cộng đồng


### Danh Sách Diễn Giả

- **1. Anh Tịnh - Build second brain** 
- **2. Hải Anh - Friendly AI Assistant with Amazon Quick** 
- **3. Thịnh - From Edge To Origin: CloudFront as Your Foundation** 
- **4. Team VIB - 36 hrs with LotusHacks - Building UTMorpho from Idea to Reality** 
- **5. Đào Đức - Deep dive talk: How LLM actually works?**
- **6. Cát Vy - Enterprise-Grade Multi-Agent System: The Case of startup Scredit Scoring**

### Nội Dung Nổi Bật

#### Context Is Everything trong GenAIOps

- Phân tích sâu lý do các mô hình AI lớn (LLM) thường thất bại do thiếu ngữ cảnh đầu vào hơn là lỗi từ bản thân mô hình. Bài nghiên cứu chỉ ra sự dịch chuyển từ kỹ thuật prompt đơn thuần sang xây dựng "Hệ thống ngữ cảnh" (Context systems) hoàn chỉnh

#### Friendly AI Assistant với Amazon Quick Suite

- Giới thiệu giải pháp Agentic AI toàn diện cho doanh nghiệp. Công cụ này tự động hóa quy trình kinh doanh phức tạp, tổng hợp dữ liệu đa nguồn và thực hiện các tác vụ quản lý dự án

#### Hạ tầng CloudFront & Mô hình Giá Flat-Rate mới

- Mô hình giá cố định này tích hợp sẵn CDN, WAF, Anti-DDoS, Route 53 và S3, giúp doanh nghiệp triệt tiêu hoàn toàn rủi ro "vỡ nợ" vì hóa đơn đám mây tăng đột biến khi bị tấn công DDoS

#### Hành trình 36 giờ tại LotusHacks

- Câu chuyện thực tế đầy áp lực từ việc tìm kiếm ý tưởng đến hoàn thiện sản phẩm

#### Deep dive talk

- các mô hình LLM chạy trên các endpoint dùng chung (Shared Endpoints) vẫn sinh ra kết quả khác nhau (không đồng nhất)

#### Enterprise-Grade Multi-Agent System

- Giải quyết bài toán hóc búa của ngân hàng truyền thống khi đánh giá startup. Kiến trúc Single Agent gặp giới hạn về ngữ cảnh và dễ sai sót, thay vào đó, một "Hội đồng tín dụng ảo" gồm nhiều Agent chuyên biệt (Tài chính, Thị trường, Nhân sự, Rủi ro) cùng phản biện để đưa ra điểm số chính xác và minh bạch nhất

### Những Gì Học Được

#### Hiểu về cơ chế LLM

- Hiểu rõ bản chất sinh văn bản theo xác suất của LLM. Tỷ lệ không đồng nhất (Non-determinism) ảnh hưởng trực tiếp đến việc kiểm thử prompt (A/B testing) và độ tin cậy khi xuất dữ liệu cấu trúc (JSON/YAML)

#### Tư Duy Thiết Kế

- Đưa "Context" vào AI một cách chọn lọc. Việc nhồi nhét quá nhiều tài liệu thô (nạn "Internet Puller") chỉ làm tăng chi phí token và làm giảm độ chính xác của mô hình

#### Sức mạnh của Multi-Agent

- Học được cách chia nhỏ một bài toán lớn, phức tạp và có tính rủi ro cao thành một mạng lưới các tác nhân AI chuyên biệt có cơ chế kiểm tra chéo lẫn nhau

#### Quản trị chi phí và bảo mật biên

- Nắm bắt cách thức hoạt động của mạng lưới phân tán AWS Edge Network trong việc chặn đứng tấn công DDoS ngay tại nguồn, kết hợp cơ chế gom request để giảm tải cho máy chủ gốc 

### Ứng Dụng Vào Công Việc

- **Ứng dụng trong Phát triển Phần mềm & AI:** Áp dụng kiến trúc Multi-Agent khi thiết kế các module hệ thống phức tạp
- **Ứng dụng trong Vận hành DevOps & Hạ tầng:** Cân nhắc chuyển dịch sang các gói giá Flat-rate của CloudFront khi triển khai ứng dụng cho khách hàng doanh nghiệp vừa và nhỏ để dễ dàng ước tính chi phí hạ tầng cố định hàng tháng
- **Ứng dụng trong Quản trị & Tự động hóa:** Agentic AI của Amazon Quick Suite để tự động hóa các tác vụ lặp đi lặp lại hàng ngày như làm biên bản họp hay tối ưu hóa quy trình tương tác nội bộ


### Trải nghiệm trong event

Nội dung không xa rời thực tiễn, đi thẳng vào các bài toán nhức nhối như hóa đơn AWS tăng phi mã hay bài toán chấm điểm tín dụng khắt khe của ngành ngân hàng, có được tư duy tổng quan về mặt kinh doanh, vừa có thể "nhúng tay" vào các bài thực hành chi tiết (Hands-on exercise) về phân quyền, bảo mật, và Terraform

#### Bài học rút ra
- Công nghệ tốt cần tư duy đúng: Một mô hình AI dù mạnh đến đâu cũng sẽ trở nên vô dụng nếu người vận hành cung cấp ngữ cảnh nghèo nàn. Đầu vào quyết định đầu ra
- Biết người biết ta khi chọn kiến trúc: Single-Agent rất tốt cho việc làm prototype nhanh hoặc tác vụ tuyến tính đơn giản; nhưng để chạy sản phẩm Enterprise thực tế, bắt buộc phải nâng cấp lên Multi-Agent để kiểm soát rủi ro và tăng tính chính xác.


#### Một số hình ảnh khi tham gia sự kiện
![](/images/event1.png)

