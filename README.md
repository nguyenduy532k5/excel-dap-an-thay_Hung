# Excel 365 – Trích xuất đáp án (PWA)

Web app chạy trong trình duyệt, cài được lên màn hình chính điện thoại như app thật.
Mọi xử lý đều diễn ra ngay trên máy: file Excel **không** bị tải lên đâu cả.

## Cấu trúc
| File | Vai trò |
|---|---|
| `index.html` | Toàn bộ giao diện + logic (sửa code ở đây) |
| `sw.js` | Service worker: lấy bản mới khi có mạng, dùng cache khi mất mạng |
| `manifest.webmanifest`, `icons/` | Thông tin để cài thành app |
| `tailwind.css` | Sinh tự động từ `index.html` (không cần sửa tay) |
| `vendor/jszip.min.js` | Thư viện đọc/ghi .xlsx (sinh từ npm) |
| `.github/workflows/pages.yml` | Tự build + xuất bản lên GitHub Pages mỗi lần push |

## Cài đặt lần đầu (làm 1 lần)
1. Tạo repo trên GitHub (ví dụ `excel-dap-an`), đẩy toàn bộ thư mục này lên nhánh `main`:
   ```bash
   git init
   git add .
   git commit -m "first commit"
   git branch -M main
   git remote add origin https://github.com/<ten-ban>/excel-dap-an.git
   git push -u origin main
   ```
2. Vào repo → **Settings → Pages → Build and deployment → Source: GitHub Actions**.
3. Mở tab **Actions**, đợi workflow "Deploy to GitHub Pages" chạy xong (khoảng 1 phút).
4. Địa chỉ app: `https://<ten-ban>.github.io/excel-dap-an/`

## Cài lên điện thoại
- **Android (Chrome):** mở địa chỉ trên → menu ⋮ → **Cài đặt ứng dụng** (hoặc *Thêm vào màn hình chính*).
- **iPhone (Safari):** mở địa chỉ trên → nút Chia sẻ → **Thêm vào Màn hình chính**.

## Sửa code → tự đồng bộ điện thoại
```bash
# sửa index.html
git add .
git commit -m "mô tả thay đổi"
git push
```
GitHub tự build và xuất bản. Lần tới mở app trên điện thoại (có mạng), app lấy bản mới:
nếu chưa mở file Excel nào thì tự tải lại, nếu đang làm việc thì hiện thông báo để bạn mở lại app.
Có thể mất tới ~10 phút để bản mới xuất hiện do bộ nhớ đệm của GitHub Pages.

## Chạy thử trên máy tính
```bash
npm install
npm run build
npx serve .        # hoặc: python -m http.server 8080
```
Service worker chỉ hoạt động trên `https://` hoặc `http://localhost`, không chạy khi mở file trực tiếp (`file://`).
