# Blog Hugo + Blowfish

Đọc **HUONG-DAN-VIET-BAI.md** để hiểu cấu trúc thư mục và cách viết bài.

## Chạy thử
Cần Hugo bản **extended** (>= 0.169.0). Trong thư mục này:

    hugo server

Mở http://localhost:1313

## Đưa lên GitHub Pages (một lần)
1. Tạo repo trống trên GitHub.
2. Trong thư mục này:

       git init -b main
       git add .
       git commit -m "First commit"
       git remote add origin https://github.com/USERNAME/TEN-REPO.git
       git push -u origin main

3. GitHub: Settings -> Pages -> Source: **GitHub Actions**.
4. Tab Actions chờ workflow xanh.

## Lưu ý
- Theme Blowfish đã copy sẵn vào `themes/blowfish` (không dùng submodule).
- File video `static/video/background.mp4` nặng khoảng 10 MB, GitHub vẫn nhận bình thường.
- Che IP, token, thông tin công ty trong ảnh trước khi đăng.
