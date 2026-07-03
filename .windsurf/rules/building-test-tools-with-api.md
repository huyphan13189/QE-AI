---
trigger: model_decision
description: Viết script/công cụ QE tự động gọi Claude API/SDK trong code — ví dụ đọc requirement sinh test case, đọc spec API sinh checklist, triage log hàng loạt. Không dùng cho AI coding assistant tương tác, cũng không phải để chọn AI test tool có sẵn.
---

# Building Test Tools with the Claude API

Khi cần viết công cụ QE tự động gọi Claude API/SDK trong code/script/pipeline, ĐỌC và LÀM THEO nguyên văn skill đầy đủ tại
`.claude/skills/building-test-tools-with-api/SKILL.md`.

Nguyên tắc cốt lõi: gọi LLM trong code cho sức mạnh tự động hóa, nhưng ba thứ giết chết một công cụ thật
là API key bị lộ, model ID hard-code lỗi thời, và chi phí/độ tin cậy không kiểm soát. Key luôn đọc từ
biến môi trường, model/tham số tra tài liệu hiện hành thay vì hard-code, và luôn có xử lý lỗi + ước
lượng token trước khi chạy hàng loạt.
