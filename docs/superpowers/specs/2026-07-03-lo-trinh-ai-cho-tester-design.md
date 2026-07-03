# Thiết kế: Lộ trình học & ứng dụng AI cho Tester

**Ngày:** 2026-07-03
**Repo:** QE-AI
**Trạng thái:** Đã duyệt thiết kế, chờ review spec

## 1. Mục tiêu

Xây một **chương trình đào tạo** giúp tester học và ứng dụng AI vào công việc test
thực tế. Chương trình tổ chức theo **level năng lực**, dài hạn / tự tiến độ, sống
ngay trong repo QE-AI và tận dụng / mở rộng các skill sẵn có.

### Ràng buộc & bối cảnh
- **Đối tượng:** hỗn hợp nhiều trình độ (manual tester ít/không code → SDET).
  Cần nền chung cho mọi người + nhánh nâng cao cho người có code.
- **Công cụ AI học viên dùng:** AI coding assistant (Claude Code, Cursor, Copilot),
  AI test tool chuyên dụng, và tự build công cụ qua Claude API/SDK.
- **Thời lượng:** dài hạn / liên tục, học viên tự tiến theo tốc độ riêng, mốc theo
  level năng lực (không gò tuần cố định).
- **Nơi sống:** thư mục `curriculum/` trong repo QE-AI, song song `.claude/skills/`.

### Tiêu chí thành công
- Manual tester chưa biết code vẫn bắt đầu và có giá trị được từ Level 1–2.
- Mỗi kỹ năng đều có bài thực hành đo được (checklist) + bằng chứng áp dụng vào
  dự án thật.
- Lộ trình dùng được ngay ở giai đoạn 1 mà không phải chờ viết xong toàn bộ skill.

## 2. Kiến trúc tổng thể

Hướng thiết kế **A + C**: **Level** là xương sống; bên trong mỗi level là các
**module theo công đoạn QE**; mỗi module trỏ tới một **skill** để học viên có công
cụ AI dùng thật.

```
Level 1 — FOUNDATION  (ai cũng bắt đầu ở đây, kể cả manual)
  "Dùng AI đúng cách & đánh giá được output"
  ├─ M1.1 Tư duy AI cho tester (LLM làm được/không được gì, ảo giác, khi nào tin)
  ├─ M1.2 Prompting cho công việc QE (viết prompt, context, few-shot)
  ├─ M1.3 Dùng AI coding assistant an toàn (Claude Code/Cursor/Copilot cơ bản)
  └─ M1.4 Đánh giá & kiểm chứng output của AI (không tin mù)

Level 2 — PRACTITIONER  (áp AI vào từng công đoạn QE thật)
  "Tăng tốc & nâng chất công việc test hàng ngày bằng AI"
  ├─ M2.1 Phân tích requirement & sinh test case bằng AI
  ├─ M2.2 Thiết kế test / test charter với AI
  ├─ M2.3 Viết & bảo trì automation có AI hỗ trợ
  ├─ M2.4 Sinh & quản lý test data bằng AI
  ├─ M2.5 Phân tích bug, log, triage với AI
  └─ M2.6 Audit chất lượng test suite  → skill `auditing-test-quality` (đã có)

Level 3 — BUILDER  (tự xây công cụ, không chỉ dùng)
  "Tự động hóa & xây công cụ QE riêng bằng AI"
  ├─ M3.1 Dùng & tùy biến AI test tool chuyên dụng
  ├─ M3.2 Viết skill QE của riêng mình (chuẩn repo này)
  ├─ M3.3 Gọi Claude API/SDK để build công cụ test
  └─ M3.4 Xây agent/pipeline QE (capstone lớn)
```

### Nguyên tắc phân tầng
- **Manual tester:** bắt buộc qua Level 1; làm được Level 2 tới M2.2 đã có giá trị;
  M2.3 và Level 3 là nhánh nâng cao (tùy chọn, cần chút code).
- **Có automation / SDET:** có thể test-out Level 1, vào thẳng Level 2–3.
- Mỗi module gắn 1 skill; skill chưa có nằm trong backlog (mục 5), viết dần.

## 3. Cấu trúc chuẩn của một Module

Mỗi module là một file `curriculum/level-X/MX.Y-<ten>.md` gồm 6 mục cố định:

1. **Mục tiêu năng lực** — học xong *làm được gì*, viết dạng "Tôi có thể…", đo được.
   VD: "Tôi có thể dùng AI sinh bộ test case từ một user story và loại được case
   sai/thừa."
2. **Vì sao & khi nào dùng AI ở công đoạn này** — lợi ích thật + cạm bẫy (ảo giác,
   lộ dữ liệu, phụ thuộc quá mức).
3. **Lý thuyết ngắn** — 1–2 trang, đủ hiểu, không hàn lâm.
4. **Hands-on trên sample project** — bài thực hành từng bước trên repo mẫu, có
   prompt gợi ý + kết quả kỳ vọng.
5. **Checklist năng lực (self-check + reviewer-check)** — danh sách tiêu chí tick
   để xác nhận "đạt"; là căn cứ lên level.
6. **Capstone — áp vào dự án thật** — làm lại kỹ năng đó trên product đang làm,
   nộp output cho reviewer duyệt.

Mỗi module có ô **"Skill liên quan"** trỏ tới `.claude/skills/<skill>/SKILL.md`.
Module nào skill chưa có: ghi *"đang phát triển"* + hướng dẫn dùng chatbot / coding
assistant tạm.

## 4. Mô hình thực hành & cách lên level

### Sample project chuẩn
- Thư mục `curriculum/sample-project/`: chọn **một demo app mã nguồn mở phổ biến**
  (dạng buggy shop / todo app) rồi **thêm bug + tài liệu**, thay vì tự viết app từ
  đầu. Có UI + API + vài bug + test suite yếu cố ý.
- Mọi hands-on ở mọi module dùng chung repo mẫu này → cùng bối cảnh, reviewer dễ
  chấm và so sánh.

### Hai lớp thực hành mỗi module
- **Hands-on (trên sample):** luyện kỹ năng, không rủi ro, có đáp án tham chiếu.
- **Capstone (trên dự án thật):** chứng minh áp dụng được vào việc thật, reviewer
  duyệt.

### Cách lên level
Hoàn thành checklist năng lực của **tất cả module bắt buộc** trong level + **1
capstone tổng** cuối level. Mỗi level có `curriculum/level-X/LEVEL-CHECKLIST.md`:

| Tiêu chí | Tự đánh giá | Reviewer |
|----------|-------------|----------|
| Làm được [năng lực M2.1] trên sample | ☐ | ☐ |
| Capstone: áp dụng [M2.1] vào dự án thật, có bằng chứng | ☐ | ☐ |
| … | | |

### Reviewer
- Một QE senior / lead tick cột reviewer.
- Nhóm không có người kèm: thêm hướng dẫn *"dùng chính AI làm reviewer sơ bộ"*
  (kèm prompt chấm điểm) trước khi người thật duyệt.

### Theo dõi tiến độ
`curriculum/README.md` — "trang chủ" của lộ trình: bảng tổng level + module +
trạng thái skill.

## 5. Bản đồ Skill & tích hợp repo

Skill mới theo đúng mô hình repo: bản gốc ở `.claude/skills/<tên>/SKILL.md`, rồi
sinh adapter cho Codex (`AGENTS.md`), Gemini (`GEMINI.md`), Cursor
(`.cursor/rules/`), Windsurf (`.windsurf/rules/`), Copilot
(`.github/copilot-instructions.md`).

| Module | Skill | Trạng thái |
|--------|-------|-----------|
| M1.2 Prompting cho QE | `prompting-for-qe` | cần viết (nhẹ) |
| M2.1 Phân tích requirement & sinh test case | `generating-test-cases` | cần viết |
| M2.2 Thiết kế test / charter | `designing-test-strategy` | cần viết |
| M2.3 Automation có AI hỗ trợ | `writing-automation-tests` | cần viết |
| M2.4 Sinh & quản lý test data | `generating-test-data` | cần viết |
| M2.5 Phân tích bug / log / triage | `analyzing-bugs-and-logs` | cần viết |
| M2.6 Audit chất lượng test suite | `auditing-test-quality` | **đã có** |
| M3.2 Viết skill QE riêng | `writing-qe-skills` | cần viết |
| M3.3 Gọi Claude API build công cụ test | `building-test-tools-with-api` | cần viết |

Level 1 (M1.1, M1.3, M1.4) chủ yếu là kiến thức nền + prompt → để dạng tài liệu
thuần; chỉ M1.2 có một skill nhẹ `prompting-for-qe`.

## 6. Kế hoạch triển khai 2 giai đoạn

Curriculum và skill là hai lớp tách biệt, triển khai theo 2 giai đoạn để lộ trình
dùng được ngay:

- **Giai đoạn 1 — Khung + nội dung học:** dựng cấu trúc `curriculum/`, viết tất cả
  module (dùng được ngay), chọn & cài bug cho sample project, viết các checklist &
  trang chủ. Module nào chưa có skill thì ô "Skill liên quan" ghi *"đang phát
  triển"* + cách dùng chatbot/coding assistant tạm.
- **Giai đoạn 2 — Viết skill dần:** mỗi skill mới đi qua một vòng
  brainstorm → plan → implement riêng, cập nhật lại ô "Skill liên quan" của module
  tương ứng khi xong.

## 7. Cấu trúc thư mục dự kiến

```
QE-AI/
├─ .claude/skills/<skill>/SKILL.md      # bản gốc skill (đã có + viết dần)
├─ curriculum/
│  ├─ README.md                         # trang chủ lộ trình (bảng tổng)
│  ├─ level-1-foundation/
│  │  ├─ M1.1-tu-duy-ai-cho-tester.md
│  │  ├─ M1.2-prompting-cho-qe.md
│  │  ├─ M1.3-dung-ai-coding-assistant.md
│  │  ├─ M1.4-danh-gia-output-ai.md
│  │  └─ LEVEL-CHECKLIST.md
│  ├─ level-2-practitioner/
│  │  ├─ M2.1 … M2.6
│  │  └─ LEVEL-CHECKLIST.md
│  ├─ level-3-builder/
│  │  ├─ M3.1 … M3.4
│  │  └─ LEVEL-CHECKLIST.md
│  └─ sample-project/                   # demo app OSS + bug cài sẵn + tài liệu
└─ docs/superpowers/specs/…             # spec này
```

## 8. Ngoài phạm vi (YAGNI)

- Không xây LMS / hệ thống chấm điểm tự động — dùng file Markdown + checklist tick tay.
- Không tự viết sample app từ đầu — tái sử dụng demo app OSS.
- Không viết trước toàn bộ 8 skill ở giai đoạn 1 — viết dần ở giai đoạn 2.
- Không làm nhánh riêng biệt hoàn toàn cho manual vs automation — dùng chung lộ
  trình, đánh dấu module nâng cao là tùy chọn.
