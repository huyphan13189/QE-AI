# GEMINI.md — Hướng dẫn cho Gemini CLI

Repo này phân phối các **skill Quality Engineering (QE)** dùng chung cho mọi AI coding assistant.
File này được **Gemini CLI** nạp làm context tự động (project-level và có thể đặt global ở
`~/.gemini/GEMINI.md`).

## Skill: auditing-test-quality

**Khi nào áp dụng:** khi người dùng yêu cầu audit / đánh giá / review chất lượng hoặc độ phủ
của một test suite hiện có, hoặc trước khi tin vào một suite đang xanh / coverage cao.
**Không** dùng để viết test mới.

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/auditing-test-quality/SKILL.md`.

Nguyên tắc cốt lõi: suite xanh và coverage cao chỉ chứng minh test **CHẠY**, không chứng minh
test **KIỂM TRA** được gì. Đi theo Process, 7 lenses và red-flag trong skill.

## Skill: generating-test-cases

**Khi nào áp dụng:** khi cần sinh / thiết kế test case từ requirement, user story, ticket, hoặc
spec API/UI bằng AI, hoặc review một danh sách case AI vừa tạo. **Không** dùng để audit test suite
tự động sẵn có (dùng `auditing-test-quality`).

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/generating-test-cases/SKILL.md`.

Nguyên tắc cốt lõi: AI sinh case rất nhanh nhưng mặc định lệch happy-path, bịa hành vi không có
trong spec, và bỏ sót case biên/âm/lỗi. Đi theo Process, bảng coverage dimensions và red-flag trong
skill; ground trước khi sinh, ép đủ chiều phủ, cắt case bịa/trùng, kiểm chứng lại với spec/app thật.
