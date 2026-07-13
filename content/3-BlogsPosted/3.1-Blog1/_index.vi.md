---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Hướng dẫn migration sang Amazon Aurora MySQL với "quyền năng" từ Kiro

Bài blog giới thiệu tính năng mới Amazon Aurora MySQL power cho Kiro — một công cụ AI tích hợp vào IDE giúp tự động hóa và đơn giản hóa toàn bộ quy trình dịch chuyển database từ Amazon RDS for MySQL sang Amazon Aurora MySQL qua 4 giai đoạn (Đánh giá, Đồng bộ, Nâng cấp, Chuyển đổi) bằng ngôn ngữ tự nhiên, giúp giảm tối đa thời gian chuẩn bị và đưa downtime khi cutover xuống chỉ còn vài chục giây.

Các điểm chính cần nắm:

* Khái niệm về Kiro Powers: là công cụ mở rộng giúp AI của Kiro IDE có kiến thức chuyên sâu về một công nghệ cụ thể (best-practices, APIs, cấu hình chuẩn). 
* Gồm 3 thành phần: MCP servers (kết nối trực tiếp để đọc trạng thái AWS/DB), Steering files (nạp quy chuẩn của chuyên gia), và Validation hooks (kiểm tra lỗi trước khi thực thi).
* Quy trình Migration 4 giai đoạn (Near-Zero Downtime): Assess (Đánh giá), Migrate (Dịch chuyển), Promote (Nâng cấp), Switch (Chuyển đổi)
* Phiên bản nguồn: RDS MySQL phải từ bản 5.7.44+ hoặc 8.0.28+.
* Storage Engine: Chỉ hỗ trợ InnoDB. Nếu DB nguồn có bảng dùng MyISAM thì phải chuyển sang InnoDB trước.
* Backup & Binlog: Instance nguồn bắt buộc phải bật sao lưu tự động (automated backups) với thời gian lưu ít nhất 1 ngày (để kích hoạt binary logging).
* Phạm vi: Hiện tại chỉ hỗ trợ migration trong cùng một tài khoản AWS và cùng một Region.
* Cơ chế an toàn: AI chỉ đề xuất và tạo câu lệnh, hệ thống không tự ý thay đổi tài nguyên AWS nếu không có sự xác nhận (approve) từng bước từ con người.
* Khả năng sau Migration: Sau khi chuyển lên Aurora thành công, AI có thể tiếp tục hỗ trợ: tự động thêm read replica theo workload, cấu hình Aurora Global Database (đa vùng để dự phòng thảm họa), thiết kế schema và tối ưu hóa các câu lệnh SQL.

Tính năng này đặc biệt hữu ích ì nó giới thiệu một giải pháp thực chiến giúp biến quy trình dịch chuyển database (migration) phức tạp và rủi ro từ Amazon RDS sang Aurora thành các bước tự động qua ngôn ngữ tự nhiên; công cụ AI này không chỉ giúp tối thiểu hóa downtime xuống còn vài chục giây mà còn đảm bảo an toàn kỹ thuật nhờ khả năng tự động check lỗi tương thích (binlog, InnoDB) dựa trên các bộ quy chuẩn (best-practices) của chuyên gia AWS.

![](/images/blog1.png)

Link: https://www.facebook.com/groups/awsstudygroupfcj/permalink/2208778813220412/

