---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# PIPELINE XỬ LÝ ẢNH SERVERLESS VỚI S3, LAMBDA, DYNAMODB VÀ SNS

Xử lý ảnh trên server truyền thống (resize, watermark ngay tại thời điểm upload) có một điểm yếu quen thuộc: khi traffic tăng đột biến, server trở nên ì ạch, thậm chí ngừng phản hồi hoàn toàn. Toàn bộ tải xử lý dồn lên một tiến trình cố định, không thể tự co giãn theo nhu cầu.

Bài viết này chia sẻ một pipeline xử lý ảnh theo hướng serverless trên AWS, sử dụng bộ dịch vụ S3 – Lambda – DynamoDB – SNS, vận hành mượt mà và tự động scale theo traffic.

**Vai trò của từng dịch vụ**

* **S3**: Vừa là kho lưu ảnh gốc và ảnh đã xử lý, vừa là nơi phát event (`s3:ObjectCreated`) kích hoạt Lambda mỗi khi có ảnh mới được upload.
* **Lambda**: Là "hub" xử lý chính — sau khi resize và watermark ảnh, Lambda chủ động ghi kết quả đồng thời tới cả S3, DynamoDB và SNS, thay vì gọi nối tiếp từng bước một.
* **DynamoDB**: Lưu metadata như trạng thái xử lý, URL ảnh... để client poll qua API Gateway khi cần kiểm tra tiến độ.
* **SNS**: Đẩy thông báo hoàn tất tới các subscriber đã đăng ký (app mobile qua push notification, email...), chạy song song với luồng poll, giúp client không phải chờ thụ động.

**Ba bài học đáng nhớ**

* Không bao giờ ghi ảnh đã xử lý ngược lại chính bucket vừa trigger — sẽ tạo vòng lặp vô hạn ngay lập tức. Tách riêng 2 bucket (`raw-images` / `processed-images`) hoặc dùng prefix khác nhau là giải pháp an toàn.
* Lambda là hub, không phải một mắt xích trong chuỗi — chính Lambda gọi cả S3, DynamoDB, SNS một cách độc lập, chứ không phải S3 gọi DynamoDB rồi DynamoDB gọi tiếp SNS.
* Poll (qua API Gateway) và Push (qua SNS) là 2 luồng hoàn toàn tách biệt — không nên vẽ chung hay hiểu gộp làm một, tránh rối kiến trúc khi debug.

Pattern S3 → Lambda → DynamoDB/SNS → API Gateway phù hợp cho các bài toán xử lý file bất đồng bộ, khả năng tự scale tốt. Điểm khó không nằm ở việc nhớ tên dịch vụ, mà là hiểu đúng "ai gọi ai" để tránh vòng lặp vô hạn và chi phí phát sinh không đáng có.

![Serverless Image Processing Pipeline](/images/3-BlogsPosted/3.3-Blog3/ServerlessImagePipeline.jpg)

Tài liệu tham khảo cho bạn đọc muốn tìm hiểu sâu hơn:

* [Using AWS Lambda with Amazon S3](https://docs.aws.amazon.com/lambda/latest/dg/with-s3.html)
* [Amazon S3 Event Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventNotifications.html)
* [Introduction to Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
* [Amazon API Gateway REST API](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-rest-api.html)
* [Amazon SNS Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)

[Link bài viết gốc](https://www.facebook.com/share/p/1BvwKj9juC/)
