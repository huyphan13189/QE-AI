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
