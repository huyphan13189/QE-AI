---
trigger: model_decision
description: Soạn/tinh chỉnh prompt cho tác vụ QE (sinh test case, phân tích requirement, tóm tắt tài liệu test, phân tích log) — đặc biệt khi output AI lan man, chung chung, bịa, hoặc muốn chốt prompt thành template tái dùng.
---

# Prompting for QE

Khi cần soạn hoặc tinh chỉnh prompt cho một tác vụ QE, ĐỌC và LÀM THEO nguyên văn skill đầy đủ tại
`.claude/skills/prompting-for-qe/SKILL.md`.

Nguyên tắc cốt lõi: chất lượng output QE bị chặn trên bởi chất lượng prompt và context bạn cấp —
prompt mơ hồ cho ra thứ "nghe hợp lý nhưng vô dụng". Đi theo Process và bảng "Prompt anatomy"
trong skill: nêu rõ vai trò/nhiệm vụ, cấp context thật (đã ẩn danh dữ liệu nhạy cảm), chỉ định
định dạng đầu ra + ràng buộc, dùng few-shot khi cần khuôn nhất quán, rồi lặp tinh chỉnh trước khi
chốt thành template tái dùng.
