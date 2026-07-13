---
title : "Truy cập S3 từ môi trường truyền thống"
date : 2024-01-01 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---

#### Tổng quan

ng phần này, chúng ta sẽ đóng gói ứng dụng React frontend bằng Vite, cấu hình các biến môi trường thực tế từ AWS, rồi tải toàn bộ tài sản tĩnh lên Amazon S3 để phân phối qua Amazon CloudFront CDN.

#### 1. Quy trình triển khai thủ công (Tham khảo)

Thông thường, việc triển khai ứng dụng Single Page Application (SPA) lên AWS bao gồm các bước:
1. Đọc các kết quả đầu ra (Outputs) từ stack CloudFormation.
2. Tạo file cấu hình `frontend/.env` cục bộ.
3. Chạy lệnh `npm install` và `npm run build` để sinh ra thư mục chứa tài nguyên tĩnh đã biên dịch `frontend/dist/`.
4. Upload các tài nguyên lên S3 Bucket Hosting:
   ```bash
   aws s3 sync frontend/dist/ s3://<Ten-Frontend-S3-Bucket> --delete
   ```
5. Xoá cache (Invalidate) CloudFront để trình duyệt nhận diện bản cập nhật ngay lập tức:
   ```bash
   aws cloudfront create-invalidation --distribution-id <ID-CloudFront> --paths "/*"
   ```

---

#### 2. Tự động hóa triển khai bằng Script PowerShell

Để đơn giản hóa quy trình và tránh sai sót khi copy dữ liệu thủ công, chúng tôi đã chuẩn bị sẵn một script tự động hóa **`deploy-frontend.ps1`** nằm trong thư mục gốc của dự án. 

Script này sẽ tự động truy vấn CloudFormation Stack để lấy các giá trị outputs, ghi cấu hình .env, chạy build và đồng bộ lên S3 đồng thời invalidate CDN cache.

##### Các bước thực hiện:

1. Mở PowerShell dưới quyền Admin hoặc Bypass Execution Policy.
2. Chạy lệnh triển khai tự động:

```powershell
powershell -ExecutionPolicy Bypass -File .\deploy-frontend.ps1
```
*(Lưu ý: Chúng ta cần thêm đường dẫn của Node.js vào biến môi trường $env:Path để PowerShell có thể nhận diện lệnh npm)*

![Màn hình chạy Deploy Script](/images/5-Workshop/5.4-S3-onprem/Deploy-frontend.png)
![Deploy script execution console](/images/5-Workshop/5.4-S3-onprem/Deploy-frontend-2.png)

##### Kết quả đầu ra của Script:

```text
=== Step 1: Fetching AWS CloudFormation Stack Outputs ===
API URL: https://ki73ac3b15.execute-api.ap-southeast-1.amazonaws.com/Prod/
Cognito UserPool: ap-southeast-1_FMpyd6sHf
Cognito Client ID: 7tlipc9j4vdt7lvd5rh3f5pkq0
Target S3 Bucket: essay-scoring-frontend-919968175298-ap-southeast-1
CloudFront URL: https://d1bzf841axvekw.cloudfront.net/

=== Step 2: Creating Frontend Production Environment Config ===
Config saved to frontend\.env

=== Step 3: Building React Application ===
vite v8.1.3 building client environment for production...
built in 713ms

=== Step 4: Deploying Assets to S3 Bucket ===
upload: frontend\dist\assets\index-Bc35BIHX.js to s3://...

=== Step 5: Invalidating CloudFront Cache ===
Cache invalidation request submitted for Distribution: E1CXVT41RVE5J8

=== Deployment Completed Successfully! ===
Your application is live at: https://d1bzf841axvekw.cloudfront.net/
```

Khi nhìn thấy dòng chữ màu xanh lá **Deployment Completed Successfully!**, trang web của bạn đã chính thức được đưa lên cloud! Bạn hãy truy cập vào đường dẫn **CloudFront URL** kết xuất ở màn hình để bắt đầu trải nghiệm!



