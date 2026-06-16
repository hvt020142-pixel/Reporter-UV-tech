# Hướng dẫn cho Claude — Repo Reporter-UV-tech

## Quy trình xuất bản bản tin hằng ngày

- **Nhánh xuất bản chính thức là `main`.** Mọi bản tin hằng ngày phải được **commit và push thẳng lên `main`**.
- Lý do: GitHub Action `.github/workflows/telegram-notify.yml` chỉ kích hoạt khi có `push` vào **`main`** với thay đổi trong `reports/**`, và sẽ gửi báo cáo theo ngày mới nhất (`reports/YYYY-MM-DD.md`) qua Telegram.
- Nếu cấu hình phiên (session config) chỉ định một nhánh phát triển khác, **vẫn ưu tiên `main`** cho bản tin theo yêu cầu của chủ repo. Có thể giữ nhánh phụ đồng bộ với `main`, nhưng `main` là nơi quyết định việc gửi Telegram.

### Các bước chuẩn
```bash
mkdir -p reports
# ghi báo cáo đầy đủ vào reports/$(date +%F).md  (KHÔNG đặt tên bắt đầu bằng "_")
git checkout main
git pull origin main
git add reports/$(date +%F).md
git commit -m "Bản tin UV $(date +%F)"
git push origin main
```

- Commit cuối cùng trong lần push phải là commit **thêm/sửa báo cáo theo ngày**, để Action nhận diện đúng file cần gửi.
- KHÔNG gọi Telegram hay bất kỳ API mạng nào ngoài web search và git — việc gửi Telegram do GitHub Action đảm nhận.

## Quy ước nội dung báo cáo
- Bám đúng khung trong `reports/_TEMPLATE.md`: 5 lĩnh vực (UAV / UGV+AV / USV / UUV / Không gian) × 5 vùng (Việt Nam–ĐNÁ / Mỹ / EU / Nga / Trung Quốc).
- Chỉ lấy diễn biến 24–48h; trích dẫn `[N]` gắn với danh mục Nguồn; đọc 2 báo cáo mới nhất trong `reports/` để tránh lặp tin.
