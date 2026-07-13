---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# Automated English Essay Scoring System  

### 1. Tóm tắt điều hành  
Dự án Automated English Essay Scoring System là hệ thống chấm điểm bài luận tiếng Anh tự động, được xây dựng trên nền tảng AWS Cloud kết hợp với Google Gemini API nhằm hỗ trợ đánh giá bài luận nhanh chóng, chính xác và nhất quán. Hệ thống cho phép người dùng tải lên bài luận, tự động trích xuất nội dung, phân tích bằng trí tuệ nhân tạo và trả về điểm số cùng nhận xét chi tiết.

### 2. Mô tả Bài toán (Problem Statement)
#### Thách thức Hiện tại
* Việc chấm điểm bài luận tiếng Anh chủ yếu được thực hiện thủ công, tốn nhiều thời gian, chi phí và phụ thuộc vào người chấm, dẫn đến kết quả có thể chưa nhất quán. Bên cạnh đó, người học thường phải chờ lâu để nhận được điểm số và phản hồi chi tiết.

#### Giải pháp Đề xuất
* Xây dựng hệ thống chấm điểm bài luận tiếng Anh tự động trên nền tảng AWS, tích hợp Google Gemini API để phân tích nội dung, đánh giá bài luận và cung cấp điểm số cùng phản hồi nhanh chóng, chính xác.

#### Lợi ích & Hiệu quả Đầu tư (ROI)
* Hệ thống giúp giảm thời gian và chi phí chấm điểm, nâng cao tính nhất quán trong đánh giá và tăng hiệu quả hỗ trợ người học. Việc sử dụng kiến trúc serverless trên AWS cũng giúp tối ưu chi phí vận hành, dễ mở rộng theo nhu cầu và mang lại hiệu quả đầu tư lâu dài.


### 3. Kiến trúc giải pháp  
Hệ thống được xây dựng theo mô hình Serverless trên nền tảng AWS. Người dùng truy cập ứng dụng thông qua AWS Amplify và Amazon CloudFront, sau đó gửi yêu cầu đến Amazon API Gateway. Hệ thống sử dụng Amazon Cognito để xác thực người dùng, AWS Lambda để xử lý nghiệp vụ, Amazon S3 để lưu trữ bài luận và kết quả, Amazon SQS cùng AWS Step Functions để điều phối quy trình xử lý. Amazon Textract trích xuất văn bản từ tài liệu và Google Gemini API thực hiện chấm điểm, đánh giá bài luận. Kết quả được lưu trên Amazon DynamoDB và Amazon S3, đồng thời Amazon SNS gửi thông báo đến người dùng. Toàn bộ hệ thống được giám sát bằng Amazon CloudWatch, AWS X-Ray và quản lý quyền truy cập bằng AWS IAM.  

![](/images/2-Proposal/AWS.jpeg.png)


#### Các Dịch vụ AWS Sử dụng
| Phân vùng chức năng | Dịch vụ AWS sử dụng | Vai trò kỹ thuật chi tiết |
| :--- | :--- | :--- |
| **Biên / Giao diện** | **AWS Amplify + CloudFront** | Host và phân phối ứng dụng frontend toàn cầu với độ trễ thấp. |
| **Xác thực & API** | **Amazon Cognito + API Gateway** | Xác thực mã token JWT và cung cấp endpoint bảo mật để lấy S3 Presigned URL. |
| **Tiếp nhận Tài liệu** | **Amazon S3 + Amazon SQS** | Lưu bài luận thô vào bucket bảo mật và đưa sự kiện vào hàng đợi để giảm tải cho tầng tính toán. |
| **Điều phối AI Lõi** | **AWS Step Functions** | Điều phối luồng công việc của trạng thái máy, quản lý rẽ nhánh, theo dõi tiến độ và xử lý lỗi. |
| **Phân tích Tài liệu** | **Amazon Textract** | Thực thi OCR nâng cao để trích xuất các khối chữ từ ảnh chụp hoặc bản quét PDF. |
| **Đánh giá AI** | **AWS Lambda + Gemini API** | Gọi `Gemini 1.5 Flash` để chấm điểm cấu trúc ngữ pháp, chất lượng bài viết và trả về JSON kết quả. |
| **Lưu trữ Dữ liệu** | **Amazon DynamoDB + Amazon S3** | Lưu trạng thái xử lý & tổng điểm vào NoSQL; lưu file báo cáo JSON chi tiết vào bucket S3 kết quả. |
| **Hệ thống Thông báo** | **Amazon SNS** | Phát đi thông báo cập nhật trạng thái và điểm số cuối cùng (Email/SMS) trực tiếp cho User/Admin. |


### 4. Triển khai kỹ thuật  
*Các giai đoạn triển khai*  

1. *Phân tích và thiết kế hệ thống*: Phân tích chức năng, thiết kế kiến trúc hệ thống, thiết kế cơ sở dữ liệu, thiết kế giao diện và lựa chọn các dịch vụ AWS phù hợp.
2. *Phát triển hệ thống*: Xây dựng giao diện người dùng, phát triển Backend, tích hợp các dịch vụ AWS (Amplify, API Gateway, Lambda, Cognito, S3, DynamoDB...), tích hợp Google Gemini API để chấm điểm bài luận.  
3. *Kiểm thử*: Kiểm thử các chức năng, sửa lỗi, tối ưu hiệu năng, kiểm tra bảo mật, đánh giá kết quả chấm điểm và hoàn thiện hệ thống. 
4. *Triển khai*: Triển khai hệ thống lên AWS, kiểm tra vận hành thực tế, đánh giá kết quả, hoàn thiện tài liệu và chuẩn bị báo cáo, thuyết trình.


### 5. Quản trị Hạ tầng & Vận hành (Infrastructure & Operations)

#### Bảo mật, Quản trị & Giám sát Hệ thống (Cross-cutting)
- **Từ khóa**: *AWS IAM, Amazon X-Ray, Amazon CloudWatch, AWS Systems Manager*.
- **Giải thích**: 
    * **AWS IAM**: Áp dụng nguyên tắc đặc quyền tối thiểu (Least Privilege) cho các vai trò thực thi giữa các dịch vụ serverless.
    * **Amazon CloudWatch**: Tập trung hóa hệ thống log, giám sát các chỉ số hiệu năng và kích hoạt cảnh báo tự động khi tỷ lệ lỗi vượt ngưỡng.
    * **Amazon X-Ray**: Cung cấp khả năng truy vết phân tán (Distributed Tracing) xuyên suốt từ API Gateway, Lambda đến Step Functions để khoanh vùng các điểm nghẽn độ trễ.
    * **AWS Systems Manager**: Lưu trữ an toàn các biến môi trường và thông tin cấu hình nhạy cảm (như Google Gemini API Key) bên trong Parameter Store.

### 6. Ước tính ngân sách  

*Chi phí hạ tầng*  
- AWS Amplify: 2,50 USD/tháng (120 build minutes, 2 GB lưu trữ, 30 GB dữ liệu truyền tải).
- Amazon CloudFront: 1,20 USD/tháng (30 GB dữ liệu truyền ra Internet, 100.000 HTTPS requests).
- Amazon API Gateway: 0,10 USD/tháng (15.000 REST API requests).
- Amazon Cognito: 0,00 USD/tháng (300 Monthly Active Users – nằm trong Free Tier).
- AWS Lambda: 0,25 USD/tháng (12.000 requests, 512 MB bộ nhớ, thời gian thực thi trung bình 2 giây).
- Amazon S3 Standard: 0,60 USD/tháng (20 GB lưu trữ, 6.000 PUT requests, 10.000 GET requests).
- Amazon SQS: 0,01 USD/tháng (6.000 message requests).
- Amazon Textract: 9,00 USD/tháng (6.000 trang tài liệu được xử lý).
- Amazon DynamoDB: 0,80 USD/tháng (2 GB lưu trữ, 20.000 Read requests, 10.000 Write requests).
- Amazon SNS: 0,01 USD/tháng (3.000 thông báo được gửi).
- Amazon CloudWatch: 1,20 USD/tháng (5 GB log, 20 metrics, 1 dashboard).

*Tổng*: 15,67 USD/tháng,  188,04 USD/12 tháng   

### 7. Đánh giá rủi ro  
*Ma trận rủi ro*  
- Mất mạng
- Chậm tiến độ do khối lượng công việc lớn.
- Chi phí sử dụng dịch vụ AWS vượt dự kiến.
- Lỗi khi tích hợp các dịch vụ AWS và Google Gemini API.
- AI đánh giá bài luận chưa chính xác hoặc kết quả không ổn định.
- Thiếu kinh nghiệm với một số dịch vụ .

*Chiến lược giảm thiểu*  
- Mạng: Lưu trữ cục bộ trên Raspberry Pi với Docker.  
- Chậm tiến độ: Lập kế hoạch chi tiết theo từng tuần, ưu tiên hoàn thành các chức năng cốt lõi trước.  
- Chi phí AWS vượt dự kiến: Sử dụng AWS Pricing Calculator để ước tính chi phí, theo dõi Billing Dashboard và tận dụng AWS Free Tier khi có thể.
- Lỗi tích hợp AWS và Gemini API: Kiểm thử từng thành phần riêng biệt trước khi tích hợp toàn bộ hệ thống.
- AI đánh giá chưa chính xác: Xây dựng prompt rõ ràng, kiểm thử với nhiều bộ dữ liệu và điều chỉnh kết quả đánh giá.
- Thiếu kinh nghiệm AWS: Nghiên cứu tài liệu AWS, thực hành trên môi trường thử nghiệm và tham khảo các hướng dẫn chính thức.

*Kế hoạch dự phòng*  
- Ưu tiên hoàn thành các chức năng cốt lõi, theo dõi chi phí và sao lưu dữ liệu định kỳ để giảm thiểu rủi ro. Khi xảy ra sự cố, hệ thống sẽ thực hiện khôi phục, thử lại yêu cầu hoặc quay về phiên bản ổn định để đảm bảo hoạt động liên tục.

### 8. Kết quả kỳ vọng  

#### Cải tiến Kỹ thuật
* **Khả năng Mở rộng Tự động**: Thay thế hoàn toàn quy trình chấm điểm thủ công bằng một công cụ tự động vận hành 24/7, có khả năng co giãn linh hoạt theo lượng bài nộp tăng đột biến mà không gây quá tải hệ thống.
* **Xử lý Đa Định dạng**: Xây dựng một kênh tiếp nhận tài liệu bền vững, tự động chuyển đổi ảnh chụp chữ viết tay, file PDF và văn bản thô thành một ngữ cảnh dữ liệu chuẩn hóa, đồng nhất.
* **Dữ liệu Cấu trúc Định hình**: Chuẩn hóa các bài luận tự do thành các lược đồ dữ liệu JSON nghiêm ngặt, bóc tách chi tiết từng tiêu chí điểm số thành phần phục vụ cho các ứng dụng tiêu thụ dữ liệu phía sau.

#### Giá trị Lâu dài
* **Kho Dữ liệu Nghiên cứu Học thuật**: Thu thập và xây dựng nên một cơ sở dữ liệu lịch sử bài luận sạch và có cấu trúc, tạo nền tảng tài nguyên giá trị cho các hoạt động nghiên cứu NLP hoặc fine-tune các mô hình ngôn ngữ lớn (LLM) chuyên biệt của phòng Lab trong tương lai.
* **Tài liệu & Khuôn mẫu Chuyển giao**: Đóng vai trò như một dự án mẫu (blueprint) chuẩn chỉnh về kiến trúc serverless thuần túy để các nhóm phát triển khác học hỏi phương pháp đệm dữ liệu bất đồng bộ, tích hợp biên bảo mật và điều phối luồng tối ưu chi phí.