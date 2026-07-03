---
trigger: model_decision
description: Viết/mở rộng/bảo trì automation test (API hoặc UI) với sự hỗ trợ của AI — sinh khung test, chuyển test case thủ công sang script, thêm test cho endpoint/luồng mới.
---

# Writing Automation Tests with AI

Khi cần viết automation test với AI, ĐỌC và LÀM THEO nguyên văn skill đầy đủ tại
`.claude/skills/writing-automation-tests/SKILL.md`.

Nguyên tắc cốt lõi: AI sinh test rất nhanh nhưng mặc định hay ra test LUÔN XANH — assertion yếu
(chỉ kiểm status/type), selector giòn, mock echo lại chính giá trị vừa set. Test chỉ có giá trị khi
nó FAIL đúng lúc có bug — luôn chứng minh cả chiều pass lẫn chiều fail trước khi tin nó.
