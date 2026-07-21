---
title : "Deploy frontend với AWS Amplify"
date : 2024-01-01
weight : 9
chapter : false
pre : " <b> 5.9 </b> "
---

### Mục tiêu

Sau khi backend đã chạy ổn ở bước trước, giờ chúng ta sẽ deploy `phim-fe` (Next.js 15) lên **Amplify Hosting**, kết nối trực tiếp GitHub để có CI/CD tự động: mỗi lần push code, Amplify tự build và deploy mà chúng ta không cần thao tác gì thêm.

### 9.1. Chuẩn bị repo

Đảm bảo `phim-fe/` đã push lên GitHub (repo riêng hoặc thư mục trong monorepo) và file `amplify.yml` nằm ở gốc thư mục frontend.

### 9.2. Tạo app Amplify

1. Console → **AWS Amplify** → **Create new app** (Host web app).
2. Chọn **GitHub** → **Next** → cửa sổ GitHub hiện ra → **Authorize AWS Amplify** → chọn repo + branch (vd `main`).
   - Nếu là monorepo: tick **My app is a monorepo** và điền `phim-fe` vào ô monorepo root.

![deploy từ git provider](/images/5-Workshop/5.9-Amplify-Frontend/01-deploy-from-git.png)

3. **App settings:** Amplify tự nhận framework **Next.js - SSR** và đọc `amplify.yml` sẵn có — giữ nguyên.
4. Mở phần **Advanced settings → Environment variables**, thêm:

| Key | Value | Ghi chú |
|---|---|---|
| `NEXT_PUBLIC_API_URL` | `https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/api` | ⚠️ **Bắt buộc có hậu tố `/api`** — code FE đọc biến này tại `src/config/API.js` |

![advanced settings env](/images/5-Workshop/5.9-Amplify-Frontend/02-advanced-settings-env.png)

{{% notice tip %}}
Cách tự kiểm tra tên biến: mở `phim-fe/src/config/API.js`, dòng 1: `export const API_URL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:5000/api'` — thấy rõ biến cần đặt và việc URL phải gồm `/api`.
{{% /notice %}}

5. → **Next** → **Save and deploy**.

### 9.3. Chờ build & kiểm tra

1. Theo dõi pipeline: Provision → Build → Deploy. Lần đầu mất ~5–10 phút.
2. Mở domain Amplify cấp: `https://<branch>.<app-id>.amplifyapp.com` — trang chủ web xem phim hiển thị, danh sách phim load được (chứng tỏ FE gọi API Gateway thành công).

![deploy thành công](/images/5-Workshop/5.9-Amplify-Frontend/03-deploy-success.png)

### 9.4. Cập nhật FRONTEND_URL cho backend

Backend dùng `FRONTEND_URL` để cấu hình CORS. Quay lại **SSM Parameter Store** → sửa `/phim/prod/FRONTEND_URL` = `https://<branch>.<app-id>.amplifyapp.com`.

Sau đó vào Lambda `phim-backend` → tab Code → **Deploy** lại (hoặc đổi bất kỳ env var nào rồi Save) để function khởi động lại và đọc giá trị mới.

### ✅ Kết quả mong đợi

- Build Amplify xanh; web truy cập được qua domain `*.amplifyapp.com`; trang chủ load danh sách phim từ API.

### 🛠 Troubleshooting

| Lỗi | Xử lý |
|---|---|
| Build fail ở `npm ci` | Xem log build; thường do Node version — Amplify dùng Node 20 mặc định cho Next.js 15 |
| Web hiện nhưng không có dữ liệu phim | `NEXT_PUBLIC_API_URL` thiếu `/api` hoặc sai Invoke URL; sửa env var rồi **Redeploy this version** |
| Lỗi CORS trên DevTools Console | `FRONTEND_URL` trong SSM chưa đúng domain Amplify (bước 9.4) |
