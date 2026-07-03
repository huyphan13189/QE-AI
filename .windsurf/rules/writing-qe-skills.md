---
trigger: model_decision
description: Đóng gói một quy trình/kỹ thuật QE lặp lại thành skill dùng chung cho nhiều AI tool trong repo này, hoặc sửa/thêm một skill sẵn có. Dùng khi thêm skill mới, tách skill, hoặc khi một skill không bao giờ được kích hoạt đúng lúc.
---

# Writing QE Skills

Khi cần viết hoặc sửa một skill QE, ĐỌC và LÀM THEO nguyên văn skill đầy đủ tại
`.claude/skills/writing-qe-skills/SKILL.md`.

Nguyên tắc cốt lõi: một skill tốt là REFERENCE tái dùng (kỹ thuật/quy trình đã kiểm chứng), không
phải kể lể một lần giải quyết vấn đề. `description` phải nói KHI NÀO dùng, không tóm tắt quy
trình — nói sai sẽ khiến skill không bao giờ được tìm thấy, hoặc bị AI đọc rồi làm tắt. Đi theo
Process và bảng "Touch points" trong skill.
