---
trigger: model_decision
description: Sinh test case từ requirement/user story/spec API-UI bằng AI — thiết kế kịch bản test, "sinh test case cho tính năng này", mở rộng độ phủ, hoặc review danh sách case AI vừa tạo. Không dùng để audit test suite tự động sẵn có (dùng auditing-test-quality).
---

# Generating Test Cases with AI

Khi cần sinh/thiết kế test case với AI, ĐỌC và LÀM THEO nguyên văn skill đầy đủ tại
`.claude/skills/generating-test-cases/SKILL.md`.

Nguyên tắc cốt lõi: AI sinh danh sách case gọn gàng rất nhanh, nhưng mặc định **lệch happy-path**,
**bịa hành vi** không có trong spec, và **bỏ sót case biên/âm/lỗi**. Đi theo Process, bảng "coverage
dimensions" và red-flag trong skill: ground trước khi sinh, ép đủ chiều phủ, cắt case bịa/trùng, và
kiểm chứng lại với spec/app thật. Đánh giá case theo rủi ro nó phủ, không theo số lượng.
