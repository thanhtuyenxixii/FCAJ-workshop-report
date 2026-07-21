---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# AWS LAMBDA DURABLE FUNCTIONS

Kiến trúc Serverless trên AWS Lambda tồn tại một hạn chế đã được biết đến rộng rãi: tính chất stateless. Mỗi lần Invoke là một vòng đời thực thi độc lập, giới hạn tối đa 15 phút; nếu hàm gặp lỗi giữa chừng, toàn bộ tiến trình sẽ bị mất và phải chạy lại từ đầu.

Hạn chế này gây khó khăn cho việc xây dựng các workflow gồm nhiều bước (ví dụ: xử lý đơn hàng theo trình tự Validate ➔ Charge tiền ➔ Chờ Webhook ➔ Gửi Email) hoặc các quy trình cần chờ phê duyệt thủ công trong nhiều giờ, nhiều ngày. Để giải quyết, các đội phát triển thường phải kết hợp thêm Step Functions, SQS, Cron Jobs, DynamoDB..., khiến hệ thống trở nên cồng kềnh hơn mức cần thiết.

Tại AWS re:Invent 2025, AWS đã giới thiệu một tính năng mới nhằm giải quyết vấn đề này: AWS Lambda Durable Functions (Session CNS380, trình bày bởi Eric Johnson và Michael Gasch).

**AWS Lambda Durable Functions là gì?**

Đây là tính năng cho phép một execution của Lambda tự động "checkpoint" tiến trình và tạm dừng (wait) mà không phát sinh chi phí compute, sau đó tự động resume đúng tại vị trí đã dừng khi nhận được tín hiệu. Thời gian sống của một Execution hiện có thể kéo dài tới 1 năm, trong khi giới hạn của mỗi lần Invoke đơn lẻ vẫn giữ nguyên ở mức 15 phút. Khi function bị tạm dừng hoặc gặp lỗi, Lambda sẽ gọi lại hàm từ đầu, nhưng sẽ replay các bước đã hoàn thành dựa trên kết quả đã lưu trong log, và chỉ thực thi tiếp phần việc chưa hoàn tất.

![AWS Lambda Durable Functions](/images/3-BlogsPosted/3.1-Blog1/AWSLambdaDurableFunctions.drawio.png)

[Link bài viết gốc](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2206391430125817/?rdid=ygFv7ftHySKZbKkF#)

...Hướng dẫn...
