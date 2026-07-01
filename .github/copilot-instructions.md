# GitHub Copilot — Hướng dẫn cho repo QE-AI

Repo này phân phối các **skill Quality Engineering (QE)** dùng chung cho mọi AI coding assistant.
File này được **GitHub Copilot** đọc tự động làm chỉ dẫn ở mức repo.

## Skill: auditing-test-quality

**Khi nào áp dụng:** khi người dùng yêu cầu audit / đánh giá / review chất lượng hoặc độ phủ
của một test suite hiện có (ví dụ "test của mình tốt không", "thiếu test gì", "audit coverage"),
hoặc trước khi tin vào một suite đang xanh / coverage cao. **Không** dùng để viết test mới.

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/auditing-test-quality/SKILL.md`.

Nguyên tắc cốt lõi: một suite xanh và coverage cao chỉ chứng minh test **CHẠY**, không chứng minh
test **KIỂM TRA** được gì. Đi theo Process, 7 lenses và danh sách red-flag trong skill; luôn chạy
suite, truy tìm cả fake/tautological test lẫn khoảng trống rủi ro chưa được test.
