# Sample Project — Toolshop

Mọi hands-on trong lộ trình chạy trên **Toolshop** — app OSS chuyên để luyện test
(Angular UI + Laravel REST API + Swagger).

## Cài & chạy

```bash
git clone https://github.com/testsmith-io/practice-software-testing.git
cd practice-software-testing
docker compose up -d      # UI: http://localhost:4200 · API: http://localhost:8091
```

Swagger/API docs: xem thư mục `docs/` trong repo Toolshop, hoặc
`http://localhost:8091/api/documentation`.

## Vì sao dùng app này

- Có sẵn UI + API + dữ liệu mẫu → luyện được mọi công đoạn QE.
- Đủ nhỏ để chạy local, đủ thật để không "đồ chơi".

## Điểm yếu & bug dùng cho bài tập

Xem [BUGS.md](BUGS.md) — danh mục các điểm yêu cầu mơ hồ, bug hành vi, và chỗ test
suite mỏng, gắn với từng module.

> Ta **không** sửa source Toolshop. Bài tập khai thác hành vi / điểm yếu sẵn có và
> các "kịch bản requirement" mô tả trong BUGS.md để học viên phân tích, thiết kế
> test, tìm bug.
