# Level 3 — Builder · Checklist hoàn tất lộ trình

Level 3 là **nhánh nâng cao**, phù hợp với người đã vững code. Đạt hết các mục
(cột **Reviewer** tick) + capstone tổng (chính là M3.4) là hoàn tất lộ trình.

## Học xong Level 3 bạn làm được gì

Từ *người dùng* AI trở thành **người xây công cụ QE bằng AI**:

- **Vận hành AI test tool chuyên dụng** — áp lên dự án thật và nêu được hạn chế thực tế của tool.
- **Viết skill QE mới đúng chuẩn repo** — SKILL.md hợp lệ + adapter, dùng chung được cho mọi AI tool trong team.
- **Gọi Claude API tự động hoá công việc QE** — key đọc từ ENV, không hard-code, có xử lý lỗi và validate output.
- **Dựng pipeline / agent QE end-to-end** — có điểm human-in-the-loop và log, chạy được trên input mẫu.

> **Ranh giới:** hoàn tất lộ trình — bạn không chỉ *dùng* AI mà đã *xây* được công cụ QE riêng cho team.

| # | Tiêu chí | Tự đánh giá | Reviewer |
|---|----------|:-----------:|:--------:|
| M3.1 | Áp được 1 AI test tool chuyên dụng lên Toolshop + nêu 2 hạn chế thực tế | ☐ | ☐ |
| M3.2 | Viết được 1 skill QE mới đúng chuẩn repo (SKILL.md hợp lệ + ≥1 adapter) và gọi được | ☐ | ☐ |
| M3.3 | Viết script gọi Claude API tự động 1 việc QE; key đọc từ ENV, không hard-code | ☐ | ☐ |
| Capstone (M3.4) | Dựng 1 pipeline/agent QE chạy end-to-end trên 1 input mẫu, có điểm human-in-the-loop + log | ☐ | ☐ |

**Reviewer:** một QE senior / lead / SDET tick cột Reviewer.
Không có người kèm → dùng AI chấm sơ bộ trước bằng prompt:

> "Đóng vai SDET lead, chấm bài này theo từng tiêu chí trong checklist Level 3, chỉ ra
> chỗ chưa đạt và cần bổ sung gì. Đừng khen chung chung."

Hoàn tất Level 3 nghĩa là bạn không chỉ *dùng* AI mà đã *xây* được công cụ QE riêng.
