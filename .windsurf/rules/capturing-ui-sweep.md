---
trigger: model_decision
description: Chụp ảnh toàn bộ UI của app để tự review layout lệch/vỡ, thiếu data, empty-state xấu, UX cần cải thiện — visual QA sweep gom ảnh theo tính năng rồi chạy một lượt AI soi lỗi, không sửa UI.
---

# Capturing UI Sweep

Khi cần chụp toàn bộ UI để tự review (visual QA sweep), ĐỌC và LÀM THEO nguyên văn skill đầy đủ tại
`.claude/skills/capturing-ui-sweep/SKILL.md`.

Nguyên tắc cốt lõi: sweep chụp mọi feature/state đã curated thành ảnh, gom theo tính năng vào MỘT trang
index, rồi chạy MỘT lượt AI soi lỗi — và KHÔNG bao giờ đụng code UI (chỉ báo cáo). Ba mảnh tách biệt:
registry (FeatureBlock[], thứ duy nhất phải bảo trì khi UI đổi) → runner generic (sinh PNG +
manifest.json) → build-index (manifest → index.html, nhúng findings nếu có review.json). Chụp trên DB
test đã reset+seed, KHÔNG đụng DB dev thật (mất tái lập). Ba bước tách rời: capture → index → review.
Review là việc của AI: đọc PNG cả 2 viewport, soi tràn ngang/lệch/thiếu data/empty-state/spacing/UX,
ghi review.json rồi rebuild index, báo người dùng — không tự sửa. Đánh giá test bắt lỗi thì dùng
auditing-test-quality; lập kế hoạch e2e thì dùng planning-e2e-coverage.
