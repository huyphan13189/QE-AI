# Level 2 — Practitioner · Checklist lên level

Đạt hết các mục **bắt buộc** (cột **Reviewer** tick) + capstone tổng là đủ vào Level 3.
M2.3 là nhánh nâng cao (cần chút code) — khuyến khích nhưng không chặn lên level với
bạn manual.

## Học xong Level 2 bạn làm được gì

Thực chiến: **dùng AI tạo ra sản phẩm test** ở từng công đoạn QE thật:

- **Sinh test case theo rủi ro** — không nhận list happy-path; ép AI phủ biên / negative / error, đánh dấu "assumption" cần xác nhận, truy vết về requirement.
- **Thiết kế chiến lược / charter test** — ưu tiên theo rủi ro thay vì liệt kê tràn lan.
- **Sinh test data** — đúng schema, phủ biên / bất hợp lệ / unicode / bulk, tuyệt đối ẩn danh.
- **Phân tích bug & log với AI** — coi nguyên nhân AI đưa là giả thuyết phải tái hiện, viết bug report chuẩn, làm sạch PII.
- **Audit chất lượng test** — nhận ra test "luôn xanh" / giả mạo coverage.
- *(Nâng cao, tùy chọn)* **Viết automation test** — chỉ tin khi đã thấy test FAIL đúng lúc có bug.

> **Ranh giới:** tự chủ toàn bộ vòng đời test hằng ngày với AI — nhưng vẫn là *người dùng* công cụ, chưa *tự xây* công cụ (đó là Level 3 — Builder).

| # | Tiêu chí | Tự đánh giá | Reviewer |
|---|----------|:-----------:|:--------:|
| M2.1 | Dùng AI phân tích 1 story: chỉ ra điểm mơ hồ + sinh test case phủ biên/âm, loại case AI bịa | ☐ | ☐ |
| M2.2 | Viết được test charter + ma trận rủi ro có ưu tiên cho 1 tính năng | ☐ | ☐ |
| M2.3 | *(nâng cao, khuyến khích)* Viết automation test có AI hỗ trợ, test FAIL đúng khi có bug | ☐ | ☐ |
| M2.4 | Sinh được bộ test data đa dạng (biên + unicode), đúng định dạng, không chứa dữ liệu thật | ☐ | ☐ |
| M2.5 | Phân tích log lỗi + viết bug report tái hiện được, có giả thuyết nguyên nhân đã kiểm chứng | ☐ | ☐ |
| M2.6 | Audit 1 test suite bằng skill `auditing-test-quality`: chỉ ra ≥1 fake test + ≥1 khoảng trống | ☐ | ☐ |
| Capstone | Áp ≥1 kỹ năng Level 2 vào dự án thật, có bằng chứng output (test case / charter / bug report / data / báo cáo audit) | ☐ | ☐ |

**Reviewer:** một QE senior / lead tick cột Reviewer.
Không có người kèm → dùng AI chấm sơ bộ trước bằng prompt:

> "Đóng vai QE lead, chấm bài này theo từng tiêu chí trong checklist Level 2, chỉ ra
> chỗ chưa đạt và cần bổ sung gì. Đừng khen chung chung."

Sau đó tự sửa rồi mới nhờ người thật duyệt.
