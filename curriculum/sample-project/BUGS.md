# Kịch bản & điểm yếu cho bài tập

Mỗi mục là chất liệu cho hands-on. "Dùng ở" trỏ tới module khai thác nó.

| # | Khu vực | Mô tả kịch bản / điểm yếu | Dùng ở |
|---|---------|---------------------------|--------|
| S1 | Checkout | User story "áp mã giảm giá" mô tả thiếu case mã hết hạn / mã sai → chất liệu phân tích requirement | M2.1 |
| S2 | Search sản phẩm | Yêu cầu mơ hồ về tìm kiếm rỗng / ký tự đặc biệt → thiết kế test charter | M2.2 |
| S3 | API `POST /products` | Thiếu validate giá âm / trường bắt buộc → viết automation kiểm chứng | M2.3 |
| S4 | Dữ liệu user/brand | Cần bộ test data đa dạng (biên, unicode, khối lượng lớn) | M2.4 |
| S5 | Đặt hàng thiếu field | Log lỗi 500 / stacktrace để luyện triage bằng AI | M2.5 |
| S6 | Test suite kèm theo | Vài test tautological / thiếu assert → luyện audit | M2.6 |

> Ghi chú: hành vi thật của Toolshop có thể đổi theo phiên bản. Khi làm bài, xác nhận
> lại điểm yếu trên bản bạn clone; nếu app đã sửa, xem đây là "kịch bản requirement"
> để phân tích thay vì bug sống.
