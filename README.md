# QE-AI

Bộ **skill Quality Engineering (QE)** dùng chung cho mọi AI coding assistant:
Claude Code, OpenAI Codex, Gemini CLI, Cursor, và các tool khác.

Mỗi skill có **một bản gốc duy nhất** ở `.claude/skills/<tên-skill>/SKILL.md`. Các file cấu hình
riêng của từng tool chỉ là *adapter mỏng* trỏ về bản gốc đó — sửa skill một lần, tất cả tool dùng chung.

## Skill hiện có

| Skill | Dùng khi nào |
|-------|--------------|
| [`auditing-test-quality`](.claude/skills/auditing-test-quality/SKILL.md) | Audit / đánh giá chất lượng & độ phủ của một test suite hiện có — truy tìm fake test và rủi ro chưa được test. Không dùng để viết test mới. |
| [`generating-test-cases`](.claude/skills/generating-test-cases/SKILL.md) | Sinh / thiết kế test case từ requirement, user story hoặc spec API/UI bằng AI — ép đủ chiều phủ, cắt case bịa/trùng, kiểm chứng với spec/app thật. |
| [`prompting-for-qe`](.claude/skills/prompting-for-qe/SKILL.md) | Soạn / tinh chỉnh prompt cho tác vụ QE khi output lan man / bịa / không dùng được, hoặc chuẩn hóa prompt thành template. |
| [`designing-test-strategy`](.claude/skills/designing-test-strategy/SKILL.md) | Lập kế hoạch test / test charter cho một tính năng — ưu tiên theo rủi ro, nêu out-of-scope, không liệt kê tràn lan. |
| [`writing-automation-tests`](.claude/skills/writing-automation-tests/SKILL.md) | Viết / bảo trì automation test (API/UI) có AI hỗ trợ — bảo đảm test FAIL đúng lúc có bug, selector bền. |
| [`generating-test-data`](.claude/skills/generating-test-data/SKILL.md) | Sinh test data đa dạng (biên, unicode, bulk) đúng schema và an toàn — không dùng dữ liệu prod. |
| [`analyzing-bugs-and-logs`](.claude/skills/analyzing-bugs-and-logs/SKILL.md) | Phân tích log / stacktrace để khoanh nguyên nhân, triage, và viết bug report tái hiện được — coi nguyên nhân AI đưa là giả thuyết. |
| [`writing-qe-skills`](.claude/skills/writing-qe-skills/SKILL.md) | Đóng gói một quy trình QE lặp lại thành skill dùng chung của repo (bản gốc + adapter). |
| [`building-test-tools-with-api`](.claude/skills/building-test-tools-with-api/SKILL.md) | Xây công cụ QE tự động bằng cách gọi Claude API/SDK — key từ ENV, không hard-code model, có xử lý lỗi. |

## Lộ trình học AI cho Tester

Ngoài bộ skill, repo có **lộ trình đào tạo** giúp tester học & ứng dụng AI vào công
việc, tổ chức theo level năng lực (Foundation → Practitioner → Builder):
[curriculum/README.md](curriculum/README.md).

## Cấu trúc repo

```
.
├── .claude/skills/<tên-skill>/SKILL.md   # BẢN GỐC của skill (nguồn chân lý)
├── AGENTS.md                             # Adapter cho Codex (và chuẩn AGENTS.md chung)
├── GEMINI.md                             # Adapter cho Gemini CLI
├── .cursor/rules/*.mdc                   # Adapter cho Cursor
├── .windsurf/rules/*.md                  # Adapter cho Windsurf
├── .github/copilot-instructions.md       # Adapter cho GitHub Copilot
└── README.md
```

## Cách dùng cho từng AI

> Có 2 cách cài: **(A) Clone repo này** rồi làm việc trực tiếp trong đó — mọi adapter đã sẵn sàng.
> Hoặc **(B) Copy skill vào project của bạn** theo hướng dẫn dưới đây.

### Claude Code
- **Trong repo này:** không cần làm gì — skill tự được nhận diện, gọi qua Skill tool.
- **Vào project của bạn:** copy cả thư mục `.claude/skills/auditing-test-quality/` vào project,
  hoặc vào global `~/.claude/skills/` để dùng ở mọi nơi.

```bash
cp -r .claude/skills/auditing-test-quality ~/.claude/skills/
```

### OpenAI Codex CLI
Codex đọc file `AGENTS.md` tự động (project root, và global `~/.codex/AGENTS.md`).
- **Trong repo này:** đã có sẵn [`AGENTS.md`](AGENTS.md).
- **Vào project của bạn:** copy `AGENTS.md` + thư mục `.claude/skills/` vào project. Nếu không muốn
  giữ thư mục skill, dán thẳng nội dung `SKILL.md` vào `AGENTS.md`.

### Gemini CLI
Gemini nạp `GEMINI.md` làm context (project root, và global `~/.gemini/GEMINI.md`).
- **Trong repo này:** đã có sẵn [`GEMINI.md`](GEMINI.md).
- **Vào project của bạn:** copy `GEMINI.md` + thư mục `.claude/skills/`, hoặc dán nội dung
  `SKILL.md` thẳng vào `GEMINI.md`.

### Cursor
Cursor dùng rule dạng `.cursor/rules/*.mdc` (kích hoạt theo `description`).
- **Trong repo này:** đã có sẵn [`.cursor/rules/auditing-test-quality.mdc`](.cursor/rules/auditing-test-quality.mdc).
- **Vào project của bạn:** copy thư mục `.cursor/rules/` + `.claude/skills/`, hoặc dán nội dung
  `SKILL.md` thẳng vào file `.mdc`.

### Windsurf
Windsurf dùng rule dạng `.windsurf/rules/*.md` (kích hoạt theo `trigger: model_decision` + `description`).
- **Trong repo này:** đã có sẵn [`.windsurf/rules/auditing-test-quality.md`](.windsurf/rules/auditing-test-quality.md).
- **Vào project của bạn:** copy thư mục `.windsurf/rules/` + `.claude/skills/`, hoặc dán nội dung
  `SKILL.md` thẳng vào file `.md`.

### GitHub Copilot
Copilot đọc `.github/copilot-instructions.md` làm chỉ dẫn mức repo.
- **Trong repo này:** đã có sẵn [`.github/copilot-instructions.md`](.github/copilot-instructions.md).
- **Vào project của bạn:** copy file này + thư mục `.claude/skills/`, hoặc dán nội dung `SKILL.md`
  thẳng vào file.

### Tool khác
Cùng một khuôn mẫu — tạo file "instruction/rule" của tool đó rồi trỏ về, hoặc dán thẳng nội dung
`.claude/skills/auditing-test-quality/SKILL.md`.

## Nguồn chân lý & cách cập nhật

Chỉ sửa nội dung skill ở **`.claude/skills/<tên-skill>/SKILL.md`**. Các adapter chỉ trỏ tới file này
nên không cần sửa theo. Nếu ở project của bạn bạn *dán thẳng* nội dung skill vào file config của tool
(thay vì trỏ), nhớ đồng bộ lại khi bản gốc thay đổi.

## Thêm skill mới

1. Tạo `.claude/skills/<tên-skill>/SKILL.md` (có frontmatter `name` + `description`).
2. Thêm dòng trỏ tới nó trong `AGENTS.md`, `GEMINI.md`, `.github/copilot-instructions.md`,
   và một file rule cho `.cursor/rules/<tên>.mdc` + `.windsurf/rules/<tên>.md`.
3. Thêm một dòng vào bảng "Skill hiện có" ở trên.
