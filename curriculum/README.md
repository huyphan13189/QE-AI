# Lộ trình học & ứng dụng AI cho Tester

Lộ trình dài hạn, tự tiến độ, theo **level năng lực**. Ai cũng bắt đầu ở Level 1;
người đã có automation có thể **test-out** Level 1 (hoàn thành `LEVEL-CHECKLIST` là
được vào Level 2). Mọi hands-on chạy trên [sample-project](sample-project/README.md).

> 🧭 **Mới bắt đầu?** Đọc [Hướng dẫn học](HUONG-DAN-HOC.md) trước — cách đi qua lộ trình,
> dùng skill khi học, và lên level.

Mỗi module theo [khuôn 6 mục](MODULE-TEMPLATE.md): mục tiêu năng lực → vì sao/khi nào
dùng AI → lý thuyết ngắn → hands-on trên sample → checklist năng lực → capstone áp
vào dự án thật.

## Bản đồ lộ trình

| Level | Module | Skill liên quan | Trạng thái skill |
|-------|--------|-----------------|------------------|
| **1 · Foundation** | [M1.1 Tư duy AI cho tester](level-1-foundation/M1.1-tu-duy-ai-cho-tester.md) | — | — |
| | [M1.2 Prompting cho QE](level-1-foundation/M1.2-prompting-cho-qe.md) | `prompting-for-qe` | **đã có** |
| | [M1.3 Dùng AI coding assistant](level-1-foundation/M1.3-dung-ai-coding-assistant.md) | — | — |
| | [M1.4 Đánh giá output AI](level-1-foundation/M1.4-danh-gia-output-ai.md) | — | — |
| | [→ Checklist lên Level 2](level-1-foundation/LEVEL-CHECKLIST.md) | | |
| **2 · Practitioner** | [M2.1 Requirement → test case](level-2-practitioner/M2.1-phan-tich-requirement-sinh-test-case.md) | `generating-test-cases` | **đã có** |
| | [M2.2 Thiết kế test / charter](level-2-practitioner/M2.2-thiet-ke-test-charter.md) | `designing-test-strategy` | **đã có** |
| | [M2.3 Automation có AI hỗ trợ](level-2-practitioner/M2.3-automation-co-ai-ho-tro.md) *(nâng cao)* | `writing-automation-tests` | **đã có** |
| | [M2.4 Sinh & quản lý test data](level-2-practitioner/M2.4-sinh-quan-ly-test-data.md) | `generating-test-data` | **đã có** |
| | [M2.5 Phân tích bug / log / triage](level-2-practitioner/M2.5-phan-tich-bug-log-triage.md) | `analyzing-bugs-and-logs` | **đã có** |
| | [M2.6 Audit chất lượng test suite](level-2-practitioner/M2.6-audit-chat-luong-test-suite.md) | `auditing-test-quality` | **đã có** |
| | [→ Checklist lên Level 3](level-2-practitioner/LEVEL-CHECKLIST.md) | | |
| **3 · Builder** | [M3.1 Dùng AI test tool chuyên dụng](level-3-builder/M3.1-dung-ai-test-tool-chuyen-dung.md) | — | — |
| | [M3.2 Viết skill QE riêng](level-3-builder/M3.2-viet-skill-qe-rieng.md) | `writing-qe-skills` | **đã có** |
| | [M3.3 Gọi Claude API build công cụ](level-3-builder/M3.3-goi-claude-api-build-cong-cu.md) | `building-test-tools-with-api` | **đã có** |
| | [M3.4 Xây agent/pipeline QE](level-3-builder/M3.4-xay-agent-pipeline-qe.md) | — | — |
| | [→ Checklist hoàn tất lộ trình](level-3-builder/LEVEL-CHECKLIST.md) | | |

## Cách lên level

Hoàn thành checklist năng lực của **mọi module bắt buộc** trong level + **1 capstone
tổng** cuối level (xem `LEVEL-CHECKLIST.md` mỗi level). Level 3 là nhánh nâng cao,
phù hợp người đã vững code.

## Tham khảo

- [Skill pack cộng đồng cho QE](tham-khao/skill-packs-cong-dong.md) — danh sách tuyển
  chọn các bộ skill mã nguồn mở đáng tái dùng + checklist đánh giá & áp dụng an toàn
  (dùng ở [M3.2](level-3-builder/M3.2-viet-skill-qe-rieng.md)).

## Trạng thái skill

Toàn bộ 8 skill trong lộ trình đã được viết xong (ô "Trạng thái skill" = **đã có**).
Mỗi skill có một bản gốc ở `.claude/skills/<tên>/SKILL.md` + adapter cho các công cụ
AI khác — xem [README repo](../README.md) để biết cách cài cho từng tool.

Thiết kế đầy đủ:
[spec](../docs/superpowers/specs/2026-07-03-lo-trinh-ai-cho-tester-design.md).
