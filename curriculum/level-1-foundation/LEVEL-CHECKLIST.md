# Level 1 — Foundation · Checklist lên level

Đạt hết các mục bắt buộc (cột **Reviewer** tick) là đủ vào Level 2.
Người đã có automation có thể dùng bảng này để **test-out** Level 1.

## Học xong Level 1 bạn làm được gì

Nền tảng để **dùng AI đúng và an toàn** — mọi tester (kể cả manual, ít code) đều cần:

- **Tư duy đúng về AI trong QE** — biết AI mạnh/yếu ở đâu, khi nào nên dùng và khi nào không; coi AI là trợ lý cần kiểm chứng, không phải nguồn chân lý.
- **Viết prompt cho công việc QE** — cấu trúc vai trò · context · nhiệm vụ · định dạng · ràng buộc; biến prompt tốt thành template tái dùng.
- **Dùng AI coding assistant cơ bản** — thao tác được với Claude Code / Cursor / Copilot và luôn review diff trước khi apply.
- **Không tin mù output AI** — phát hiện ảo giác, bịa requirement, sai sót; ẩn dữ liệu nhạy cảm (PII / credential) trước khi đưa vào prompt.

> **Ranh giới:** dùng AI như công cụ hỗ trợ hằng ngày một cách có kỷ luật — nhưng chưa tạo ra sản phẩm test hoàn chỉnh bằng AI (đó là Level 2).

| # | Tiêu chí | Tự đánh giá | Reviewer |
|---|----------|:-----------:|:--------:|
| M1.1 | Chỉ ra được việc AI dễ sai trong QE + 1 ví dụ ảo giác cụ thể | ☐ | ☐ |
| M1.2 | Viết được prompt có cấu trúc (vai trò + context + định dạng) cho 1 việc QE | ☐ | ☐ |
| M1.3 | Dùng AI coding assistant + review diff trước khi apply | ☐ | ☐ |
| M1.4 | Áp checklist kiểm chứng, loại được ≥1 case AI sai/thừa | ☐ | ☐ |
| Capstone | Tài liệu ngắn: cách tôi sẽ dùng AI an toàn trong dự án (3 việc nên / 3 việc không) | ☐ | ☐ |

**Reviewer:** một QE senior / lead tick cột Reviewer.
Không có người kèm → dùng AI chấm sơ bộ trước bằng prompt:

> "Đóng vai QE lead, chấm bài này theo từng tiêu chí trong checklist Level 1, chỉ ra
> chỗ chưa đạt và cần bổ sung gì. Đừng khen chung chung."

Sau đó tự sửa rồi mới nhờ người thật duyệt.
