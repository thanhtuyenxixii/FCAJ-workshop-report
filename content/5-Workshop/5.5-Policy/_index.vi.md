---
title : "VPC Endpoint Policies"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

Trong phần này, chúng ta sẽ thực hiện các bước đăng ký tài khoản, xác thực email từ dịch vụ SNS và tiến hành tải lên bài luận dưới các định dạng khác nhau để kiểm thử tính năng OCR của Amazon Textract và khả năng chấm điểm của Gemini AI.

---

#### Bước 1: Kích hoạt email nhận thông báo từ AWS SNS

Trước khi thực hiện chấm điểm, bạn cần đồng ý đăng ký nhận thông báo từ SNS để hệ thống có thể gửi email về hộp thư của bạn:

1. Đăng nhập vào hộp thư email bạn đã khai báo trong quá trình deploy (ví dụ: testuser@gmail.com).
2. Tìm một email được gửi từ no-reply@sns.amazonaws.com với tiêu đề **"AWS Notification - Subscription Confirmation"**.
3. Nhấp vào liên kết **Confirm Subscription** trong email. Trình duyệt hiển thị thông báo **Subscription confirmed!** nghĩa là bạn đã kích hoạt thành công.

![Xác nhận đăng ký email SNS](/images/5-Workshop/5.5-Policy/confirm-notification.jpeg)

---

#### Bước 2: Đăng nhập vào ứng dụng Web

1. Mở trình duyệt và truy cập vào **CloudFront URL** của bạn.
2. Trang web hiển thị giao diện đăng nhập (Sign In).
3. Đăng nhập bằng tài khoản thử nghiệm của bạn:
   * **Email:** testuser@gmail.com
   * **Mật khẩu:** MatKhauChauAu123!

![Giao diện đăng nhập website](/images/5-Workshop/5.5-Policy/test-web.png)

4. Sau khi đăng nhập thành công, bạn sẽ được chuyển hướng đến trang **Dashboard** của hệ thống.

---

#### Bước 3: Kiểm thử tải lên bài luận (Upload & Evaluation)

Hệ thống hỗ trợ 3 loại định dạng đầu vào. Bạn có thể tiến hành kiểm thử lần lượt:

##### Trường hợp A: Upload file văn bản thuần (.txt)
1. Tạo một file .txt chứa nội dung bài luận tiếng Anh của bạn.
2. Kéo thả hoặc chọn file này vào khung **Upload Essay** ở bên trái màn hình.
3. Nhấn nút **Submit for Assessment**. Trạng thái bài luận sẽ chuyển thành Analyzing.
4. Sau khoảng 30-50 giây (thời gian Gemini xử lý), trạng thái sẽ cập nhật thành Scored kèm theo điểm số hiển thị trên bảng.

##### Trường hợp B: Upload file hình ảnh chứa bài luận (.png, .jpg, .jpeg)
1. Chuẩn bị một bức ảnh chụp bài viết tiếng Anh (có thể viết tay rõ ràng hoặc in ấn).
2. Kéo thả file ảnh này vào khung upload của ứng dụng.
3. Nhấn **Submit for Assessment**. 
4. Hệ thống sẽ tự động kích hoạt luồng **Choice State** trong Step Functions, điều hướng file ảnh sang dịch vụ **Amazon Textract** để trích xuất chữ, rồi mới chuyển cho Gemini đánh giá.

##### Trường hợp C: Upload file tài liệu chứa bài luận (.pdf)
1. Tương tự như trường hợp ảnh, upload một file tài liệu .pdf.
2. Textract sẽ quét qua các trang tài liệu PDF để lấy chữ thô và gửi chấm điểm.

![Màn hình giao diện Dashboard quản lý](/images/5-Workshop/5.5-Policy/thuc-hien-cham-diem.png)

---

#### Bước 4: Kiểm tra kết quả chấm điểm & Email báo điểm

##### 1. Xem chi tiết phân tích trên Website:
* Nhấn nút **View Analysis** bên cạnh bài luận đã được chấm điểm trên bảng.
* Hệ thống sẽ hiển thị một trang báo cáo chi tiết trực quan bao gồm:
  * **Tổng điểm (Overall Score):** Điểm số từ 0 - 100.
  * **Đánh giá chung (Overall Feedback):** Lời khuyên tổng quan từ AI.
  * **Phân tích chi tiết từng tiêu chí:** Đánh giá về Ngữ pháp (Grammar), Từ vựng (Vocabulary), Bố cục cấu trúc (Structure), và Sự mạch lạc (Coherence).

![Giao diện báo cáo kết quả chi tiết](/images/5-Workshop/5.5-Policy/ket-qua-cham-diem.png)

##### 2. Kiểm tra Hộp thư Email:
* Mở hộp thư email của bạn. Bạn sẽ nhận được một thư báo điểm mới từ hệ thống với tiêu đề:
  * **Essay Scored! File: [Ten_File_Cua_Ban]**
* Nội dung email sẽ hiển thị chi tiết điểm số cùng toàn bộ bài đánh giá chấm điểm của AI, giúp bạn theo dõi kết quả ngay lập tức mà không cần mở website.

![Thư báo điểm tự động từ SNS](/images/5-Workshop/5.5-Policy/mail-thong-bao.png)