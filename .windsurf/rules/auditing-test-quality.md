---
trigger: model_decision
description: Audit chất lượng/độ phủ của một test suite hiện có — truy tìm fake/tautological test và rủi ro chưa được test. Dùng khi được yêu cầu "test tốt không", "audit coverage", "thiếu test gì", hoặc trước khi tin một suite đang xanh.
---

# Auditing Test Quality

Khi cần audit chất lượng test, ĐỌC và LÀM THEO nguyên văn skill đầy đủ tại
`.claude/skills/auditing-test-quality/SKILL.md`.

Nguyên tắc cốt lõi: một suite xanh và coverage cao chỉ chứng minh test **CHẠY**, không chứng minh
test **KIỂM TRA** được gì. Truy tìm cả fake/tautological/false-safety test lẫn khoảng trống rủi ro
chưa được test — đừng chỉ trông vào con số coverage. Đi theo Process, 7 lenses và red-flag trong skill.
