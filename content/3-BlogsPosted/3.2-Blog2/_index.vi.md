---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# TỐI ƯU CHI PHÍ AWS LAMBDA

Mô hình Serverless trên AWS Lambda thường được xem là tiết kiệm nhờ cơ chế trả tiền theo mức sử dụng (pay-per-use). Tuy nhiên, nếu không chủ động tối ưu, hóa đơn cuối tháng hoàn toàn có thể vượt xa dự kiến — chi phí phát sinh âm thầm từ việc chọn sai cấu hình memory, cold start xử lý chưa hợp lý, hay log lưu trữ không giới hạn.

Dưới đây là các hướng tối ưu chi phí AWS Lambda đáng chú ý, được tổng hợp từ bài viết của CloudKeeper:

**Các hướng tối ưu chính**

* **Chuyển sang AWS Graviton2 (ARM)**: Giải pháp đơn giản nhưng hiệu quả nhất. Chi phí thấp hơn khoảng 20% so với kiến trúc x86, trong khi hiệu năng ở nhiều workload còn tốt hơn.
* **Tối ưu Memory (Right-sizing)**: Không nên chọn dung lượng memory một cách cảm tính. Sử dụng công cụ như AWS Lambda Power Tuning để tìm điểm cân bằng giữa RAM và CPU — RAM tăng kéo theo CPU tăng, thời gian thực thi giảm, và tổng chi phí giảm.
* **Xử lý Cold Start & Concurrency**: Cân nhắc cấu hình Provisioned Concurrency ở mức hợp lý để không ảnh hưởng trải nghiệm người dùng, đồng thời tránh lãng phí tài nguyên dự phòng.
* **Dọn dẹp Log**: CloudWatch Logs lưu trữ không giới hạn là một "bẫy chi phí" thường bị bỏ qua. Cần thiết lập thời gian Retention phù hợp cho từng log group.

Kết quả chạy AWS Lambda Power Tuning dưới đây minh họa rõ điểm cân bằng giữa chi phí và thời gian thực thi: Best Cost tại 512MB, Best Time tại 2048MB, và Graviton luôn có chi phí thấp hơn x86:

![AWS Lambda Power Tuning Results](/images/3-BlogsPosted/3.2-Blog2/LambdaPowerTuningResults.jpg)

Bài viết chi tiết dành cho bạn đọc muốn tìm hiểu sâu hơn: [Reducing AWS Lambda Costs — Optimization Tips for Serverless Computing](https://www.cloudkeeper.com/insights/blog/reducing-aws-lambda-costs-optimization-tips-serverless-computing#toc-using-aws-graviton2)

[Link bài viết gốc](https://www.facebook.com/share/p/1cMX6QhMWE/)
