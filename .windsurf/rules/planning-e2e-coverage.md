---
trigger: model_decision
description: Lập kế hoạch e2e cho codebase — chọn flow/màn hình nào cần e2e và theo thứ tự nào, phân tầng coverage low/medium/high, map flow đã có e2e vs chưa, dựng roadmap mở rộng suite.
---

# Planning E2E Coverage

Khi cần quyết định codebase cần e2e những gì và theo thứ tự nào, ĐỌC và LÀM THEO nguyên văn skill đầy
đủ tại `.claude/skills/planning-e2e-coverage/SKILL.md`.

Nguyên tắc cốt lõi: coverage e2e là ROADMAP ưu tiên theo rủi ro, KHÔNG phải "test hết" — e2e là tầng
đắt nhất, nhiều spec là gánh nặng. Sweep codebase (route/nav/controller) liệt kê JOURNEY (không phải
từng màn) → xếp hạng theo blast-radius × traffic (auth/checkout/core loop trước) → baseline map
journey ↔ e2e đã có → phân tầng LOW (smoke gate làm trước) / MEDIUM / HIGH (hoãn tới khi đáng) → roadmap
có điều kiện dừng. Implement từng spec thì dùng writing-e2e-tests.
