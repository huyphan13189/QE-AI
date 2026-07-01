# AGENTS.md — Hướng dẫn cho AI coding agent

Repo này phân phối các **skill Quality Engineering (QE)** dùng chung cho mọi AI coding assistant.
File này được đọc tự động bởi **OpenAI Codex CLI** và nhiều tool khác cũng đọc chuẩn `AGENTS.md`.

## Skill: auditing-test-quality

**Khi nào áp dụng:** khi người dùng yêu cầu audit / đánh giá / review / phán xét chất lượng
hoặc độ phủ của một test suite hiện có (ví dụ "test của mình tốt không", "thiếu test gì",
"audit coverage"), hoặc trước khi tin vào một suite đang xanh / coverage cao.
**Không** dùng để viết test mới.

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/auditing-test-quality/SKILL.md`.

Nguyên tắc cốt lõi: một suite xanh và coverage cao chỉ chứng minh test **CHẠY**, không chứng minh
test **KIỂM TRA** được gì. Đi theo Process, 7 lenses và danh sách red-flag trong skill; luôn chạy
suite, truy tìm cả fake/tautological test lẫn khoảng trống rủi ro chưa được test.
