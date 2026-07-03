# Lộ trình AI cho Tester — Giai đoạn 1 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Dựng khung `curriculum/` trong repo QE-AI + toàn bộ nội dung học (15 module, 3 checklist, trang chủ) + sample project OSS có bug cài sẵn, để lộ trình dùng được ngay mà chưa cần viết xong các skill mới.

**Architecture:** Lộ trình theo hướng A+C: **Level** (Foundation → Practitioner → Builder) là xương sống; mỗi level chứa **module theo công đoạn QE**; mỗi module là 1 file Markdown 6 mục, trỏ tới một skill (skill chưa có thì ghi "đang phát triển"). Mọi hands-on chạy trên một sample project chung.

**Tech Stack:** Markdown thuần; sample project dùng `testsmith-io/practice-software-testing` (Toolshop: Laravel API + Angular UI, có Swagger). Không LMS, không hệ chấm tự động.

**Spec:** `docs/superpowers/specs/2026-07-03-lo-trinh-ai-cho-tester-design.md`

**Quy ước chung khi thực thi:**
- Tất cả file curriculum viết bằng **tiếng Việt**, giọng gần gũi, thực dụng.
- Mỗi module bám đúng **khuôn 6 mục** (xem Task 2). Trước khi commit một module, tự soát: đủ 6 mục? có ô "Skill liên quan"? có ≥1 prompt mẫu ở phần hands-on? có checklist tick được?
- Đường dẫn file dùng slug không dấu như trong plan.
- "Verify" cho task nội dung = đọc lại file, đối chiếu checklist của task; không có test runner.

---

## File Structure

```
curriculum/
├─ README.md                              # trang chủ: bảng tổng level/module/skill
├─ MODULE-TEMPLATE.md                     # khuôn 6 mục để copy khi viết module
├─ sample-project/
│  ├─ README.md                           # cách clone/chạy Toolshop + phạm vi dùng
│  └─ BUGS.md                             # danh mục bug/điểm yếu cài sẵn cho bài tập
├─ level-1-foundation/
│  ├─ M1.1-tu-duy-ai-cho-tester.md
│  ├─ M1.2-prompting-cho-qe.md
│  ├─ M1.3-dung-ai-coding-assistant.md
│  ├─ M1.4-danh-gia-output-ai.md
│  └─ LEVEL-CHECKLIST.md
├─ level-2-practitioner/
│  ├─ M2.1-phan-tich-requirement-sinh-test-case.md
│  ├─ M2.2-thiet-ke-test-charter.md
│  ├─ M2.3-automation-co-ai-ho-tro.md
│  ├─ M2.4-sinh-quan-ly-test-data.md
│  ├─ M2.5-phan-tich-bug-log-triage.md
│  ├─ M2.6-audit-chat-luong-test-suite.md
│  └─ LEVEL-CHECKLIST.md
└─ level-3-builder/
   ├─ M3.1-dung-ai-test-tool-chuyen-dung.md
   ├─ M3.2-viet-skill-qe-rieng.md
   ├─ M3.3-goi-claude-api-build-cong-cu.md
   ├─ M3.4-xay-agent-pipeline-qe.md
   └─ LEVEL-CHECKLIST.md
```

---

## Task 1: Khung thư mục + trang chủ curriculum

**Files:**
- Create: `curriculum/README.md`

- [ ] **Step 1: Tạo trang chủ `curriculum/README.md`**

Nội dung gồm: giới thiệu 3 dòng về lộ trình; cách dùng (manual bắt đầu Level 1, SDET test-out Level 1); bảng tổng dưới đây; link tới spec.

```markdown
# Lộ trình học & ứng dụng AI cho Tester

Lộ trình dài hạn, tự tiến độ, theo **level năng lực**. Ai cũng bắt đầu ở Level 1;
người đã có automation có thể test-out Level 1 (hoàn thành LEVEL-CHECKLIST là được
vào Level 2). Mọi hands-on chạy trên [sample-project](sample-project/README.md).

## Bản đồ lộ trình

| Level | Module | Skill liên quan | Trạng thái skill |
|-------|--------|-----------------|------------------|
| **1 · Foundation** | [M1.1 Tư duy AI cho tester](level-1-foundation/M1.1-tu-duy-ai-cho-tester.md) | — | — |
| | [M1.2 Prompting cho QE](level-1-foundation/M1.2-prompting-cho-qe.md) | `prompting-for-qe` | đang phát triển |
| | [M1.3 Dùng AI coding assistant](level-1-foundation/M1.3-dung-ai-coding-assistant.md) | — | — |
| | [M1.4 Đánh giá output AI](level-1-foundation/M1.4-danh-gia-output-ai.md) | — | — |
| **2 · Practitioner** | [M2.1 Requirement → test case](level-2-practitioner/M2.1-phan-tich-requirement-sinh-test-case.md) | `generating-test-cases` | đang phát triển |
| | [M2.2 Thiết kế test / charter](level-2-practitioner/M2.2-thiet-ke-test-charter.md) | `designing-test-strategy` | đang phát triển |
| | [M2.3 Automation có AI hỗ trợ](level-2-practitioner/M2.3-automation-co-ai-ho-tro.md) | `writing-automation-tests` | đang phát triển |
| | [M2.4 Sinh & quản lý test data](level-2-practitioner/M2.4-sinh-quan-ly-test-data.md) | `generating-test-data` | đang phát triển |
| | [M2.5 Phân tích bug / log / triage](level-2-practitioner/M2.5-phan-tich-bug-log-triage.md) | `analyzing-bugs-and-logs` | đang phát triển |
| | [M2.6 Audit chất lượng test suite](level-2-practitioner/M2.6-audit-chat-luong-test-suite.md) | `auditing-test-quality` | **đã có** |
| **3 · Builder** | [M3.1 Dùng AI test tool chuyên dụng](level-3-builder/M3.1-dung-ai-test-tool-chuyen-dung.md) | — | — |
| | [M3.2 Viết skill QE riêng](level-3-builder/M3.2-viet-skill-qe-rieng.md) | `writing-qe-skills` | đang phát triển |
| | [M3.3 Gọi Claude API build công cụ](level-3-builder/M3.3-goi-claude-api-build-cong-cu.md) | `building-test-tools-with-api` | đang phát triển |
| | [M3.4 Xây agent/pipeline QE](level-3-builder/M3.4-xay-agent-pipeline-qe.md) | — | — |

## Cách lên level
Hoàn thành checklist năng lực của mọi module bắt buộc trong level + 1 capstone tổng
(xem `LEVEL-CHECKLIST.md` mỗi level).

Thiết kế đầy đủ: [spec](../docs/superpowers/specs/2026-07-03-lo-trinh-ai-cho-tester-design.md).
```

- [ ] **Step 2: Verify** — mở file, xác nhận bảng đủ 15 module, mọi link tương đối trỏ đúng đường dẫn ở File Structure.

- [ ] **Step 3: Commit**

```bash
git add curriculum/README.md
git commit -m "docs(curriculum): add roadmap homepage"
```

---

## Task 2: Khuôn module (MODULE-TEMPLATE)

**Files:**
- Create: `curriculum/MODULE-TEMPLATE.md`

- [ ] **Step 1: Tạo `curriculum/MODULE-TEMPLATE.md`** — khuôn 6 mục để mọi module copy theo.

```markdown
# [Mã module] — [Tên module]

> **Level:** [1/2/3] · **Bắt buộc:** [có / tùy chọn nâng cao] · **Skill liên quan:** [`ten-skill`](../../.claude/skills/ten-skill/SKILL.md) *(đang phát triển — tạm dùng chatbot/coding assistant theo hướng dẫn ở mục 4)*

## 1. Mục tiêu năng lực
Sau module này, "Tôi có thể…" (viết 2–4 gạch đầu dòng, đo được).

## 2. Vì sao & khi nào dùng AI ở công đoạn này
Lợi ích thật + cạm bẫy (ảo giác, lộ dữ liệu, phụ thuộc quá mức). Khi nào KHÔNG nên.

## 3. Lý thuyết ngắn
1–2 trang, đủ hiểu. Có ví dụ.

## 4. Hands-on trên sample project
Bài từng bước trên [sample-project](../sample-project/README.md).
- Bối cảnh / input
- Prompt gợi ý (≥1 prompt cụ thể copy được)
- Kết quả kỳ vọng / đáp án tham chiếu

## 5. Checklist năng lực
| Tiêu chí | Tự đánh giá | Reviewer |
|----------|:-----------:|:--------:|
| … | ☐ | ☐ |

## 6. Capstone — áp vào dự án thật
Làm lại kỹ năng trên product đang làm; nộp output gì cho reviewer duyệt.
```

- [ ] **Step 2: Verify** — đủ đúng 6 mục; ô "Skill liên quan" ở header.

- [ ] **Step 3: Commit**

```bash
git add curriculum/MODULE-TEMPLATE.md
git commit -m "docs(curriculum): add module template"
```

---

## Task 3: Sample project — chọn app OSS + cài bug

**Files:**
- Create: `curriculum/sample-project/README.md`
- Create: `curriculum/sample-project/BUGS.md`

- [ ] **Step 1: Viết `curriculum/sample-project/README.md`**

Chốt app: **Toolshop** (`testsmith-io/practice-software-testing`). Không vendor toàn bộ source vào repo (nặng, khó bảo trì) — hướng dẫn clone + chạy bằng Docker, và curriculum chỉ sở hữu tài liệu bài tập (`BUGS.md`).

```markdown
# Sample Project — Toolshop

Mọi hands-on trong lộ trình chạy trên **Toolshop** — app OSS chuyên để luyện test
(Angular UI + Laravel REST API + Swagger).

## Cài & chạy
```bash
git clone https://github.com/testsmith-io/practice-software-testing.git
cd practice-software-testing
docker compose up -d      # UI: http://localhost:4200 · API: http://localhost:8091
```
Swagger/API docs: xem `docs/` trong repo Toolshop hoặc `http://localhost:8091/api/documentation`.

## Vì sao dùng app này
- Có sẵn UI + API + dữ liệu mẫu → luyện được mọi công đoạn QE.
- Đủ nhỏ để chạy local, đủ thật để không "đồ chơi".

## Điểm yếu & bug dùng cho bài tập
Xem [BUGS.md](BUGS.md) — danh mục các điểm yêu cầu mơ hồ, bug hành vi, và chỗ test
suite mỏng, gắn với từng module.

> Ta KHÔNG sửa source Toolshop. Bài tập khai thác hành vi/điểm yếu sẵn có và các
> "kịch bản requirement" mô tả trong BUGS.md để học viên phân tích, thiết kế test,
> tìm bug.
```

- [ ] **Step 2: Viết `curriculum/sample-project/BUGS.md`** — danh mục kịch bản/điểm yếu, mỗi mục gắn module sẽ dùng. Viết tối thiểu 6 mục thật, ví dụ:

```markdown
# Kịch bản & điểm yếu cho bài tập

Mỗi mục là chất liệu cho hands-on. "Dùng ở" trỏ tới module khai thác nó.

| # | Khu vực | Mô tả kịch bản / điểm yếu | Dùng ở |
|---|---------|---------------------------|--------|
| S1 | Checkout | User story "áp mã giảm giá" mô tả thiếu case mã hết hạn / mã sai → chất liệu phân tích requirement | M2.1 |
| S2 | Search sản phẩm | Yêu cầu mơ hồ về tìm kiếm rỗng / ký tự đặc biệt → thiết kế test charter | M2.2 |
| S3 | API `POST /products` | Thiếu validate giá âm / trường bắt buộc → viết automation kiểm chứng | M2.3 |
| S4 | Dữ liệu user/brand | Cần bộ test data đa dạng (biên, unicode, khối lượng lớn) | M2.4 |
| S5 | Log lỗi 500 khi đặt hàng thiếu field | Chuỗi log/stacktrace để luyện triage bằng AI | M2.5 |
| S6 | Test suite kèm theo | Vài test tautological / thiếu assert → luyện audit | M2.6 |
```

- [ ] **Step 3: Verify** — lệnh clone/chạy đúng repo Toolshop; BUGS.md có ≥6 mục và mỗi mục gắn đúng module có trong bản đồ.

- [ ] **Step 4: Commit**

```bash
git add curriculum/sample-project/
git commit -m "docs(curriculum): add sample project (Toolshop) + exercise catalog"
```

---

## Task 4: Module M1.1 — Tư duy AI cho tester

**Files:**
- Create: `curriculum/level-1-foundation/M1.1-tu-duy-ai-cho-tester.md`

- [ ] **Step 1: Viết module** theo MODULE-TEMPLATE, nội dung cụ thể:
  - **Mục tiêu:** phân biệt được việc LLM làm tốt vs dễ sai cho QE; nhận ra ảo giác; biết khi nào cần kiểm chứng.
  - **Mục 2:** lợi ích (tăng tốc soạn thảo, gợi ý case) vs cạm bẫy (bịa số liệu, tự tin sai, rò rỉ dữ liệu công ty).
  - **Lý thuyết:** LLM là bộ dự đoán văn bản, không phải nguồn chân lý; non-deterministic; context quyết định chất lượng.
  - **Hands-on:** yêu cầu AI mô tả tính năng checkout của Toolshop rồi đối chiếu app thật → chỉ ra ít nhất 1 điểm AI bịa/không chắc.
  - **Checklist:** liệt kê ≥3 tiêu chí (VD "chỉ ra được 1 chỗ AI ảo giác").
  - **Capstone:** viết 1 đoạn ngắn "3 việc tôi sẽ / sẽ không giao cho AI trong dự án của tôi + vì sao".
  - **Skill liên quan:** — (không có).

- [ ] **Step 2: Verify** — đủ 6 mục, hands-on trỏ Toolshop, checklist tick được.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-1-foundation/M1.1-tu-duy-ai-cho-tester.md
git commit -m "docs(curriculum): add module M1.1"
```

---

## Task 5: Module M1.2 — Prompting cho QE

**Files:**
- Create: `curriculum/level-1-foundation/M1.2-prompting-cho-qe.md`

- [ ] **Step 1: Viết module** (skill liên quan: `prompting-for-qe` — đang phát triển):
  - **Mục tiêu:** viết được prompt có cấu trúc (vai trò + context + yêu cầu + định dạng đầu ra); dùng few-shot; lặp cải tiến prompt.
  - **Lý thuyết:** cấu trúc prompt; cấp context (requirement, code, dữ liệu) an toàn; ví dụ prompt tệ → tốt.
  - **Hands-on:** cùng một yêu cầu "sinh test case cho form đăng ký Toolshop", viết prompt v1 sơ sài → v2 có cấu trúc, so sánh output. Có ≥2 prompt mẫu copy được.
  - **Checklist:** ≥3 tiêu chí (VD "prompt có nêu định dạng đầu ra mong muốn").
  - **Capstone:** chuẩn hóa 1 prompt hay dùng cho công việc thật thành template tái dùng.

- [ ] **Step 2: Verify** — có ≥2 prompt mẫu; ô skill ghi "đang phát triển".

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-1-foundation/M1.2-prompting-cho-qe.md
git commit -m "docs(curriculum): add module M1.2"
```

---

## Task 6: Module M1.3 — Dùng AI coding assistant an toàn

**Files:**
- Create: `curriculum/level-1-foundation/M1.3-dung-ai-coding-assistant.md`

- [ ] **Step 1: Viết module:**
  - **Mục tiêu:** dùng được Claude Code / Cursor / Copilot ở mức cơ bản (hỏi codebase, sinh/sửa file, chạy lệnh) một cách an toàn (review trước khi apply, không commit mù).
  - **Lý thuyết:** khác biệt chatbot vs coding assistant tích hợp repo; quyền hạn & rủi ro; luồng review-diff.
  - **Hands-on:** clone Toolshop, dùng coding assistant hỏi "giải thích luồng checkout" và yêu cầu thêm 1 test đơn giản; review diff trước khi giữ.
  - **Checklist:** ≥3 tiêu chí (VD "review được diff trước khi apply").
  - **Capstone:** dùng assistant làm 1 việc nhỏ có thật trên dự án, ghi lại đã review gì.
  - **Skill liên quan:** — .

- [ ] **Step 2: Verify** — hands-on chạy trên Toolshop; nhấn mạnh review diff.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-1-foundation/M1.3-dung-ai-coding-assistant.md
git commit -m "docs(curriculum): add module M1.3"
```

---

## Task 7: Module M1.4 — Đánh giá & kiểm chứng output AI

**Files:**
- Create: `curriculum/level-1-foundation/M1.4-danh-gia-output-ai.md`

- [ ] **Step 1: Viết module:**
  - **Mục tiêu:** có checklist kiểm chứng output AI trước khi dùng; phát hiện case bịa/thiếu/sai logic.
  - **Lý thuyết:** vì sao không tin mù; kỹ thuật đối chiếu (chạy thử, cross-check tài liệu, hỏi lại AI phản biện).
  - **Hands-on:** lấy bộ test case AI sinh ở M1.2, rà từng case trên Toolshop, đánh dấu case sai/thừa/thiếu.
  - **Checklist:** ≥3 tiêu chí (VD "loại được ≥1 case sai").
  - **Capstone:** áp checklist kiểm chứng vào 1 output AI dùng thật trong dự án.
  - **Skill liên quan:** — .

- [ ] **Step 2: Verify** — nối tiếp được với output của M1.2.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-1-foundation/M1.4-danh-gia-output-ai.md
git commit -m "docs(curriculum): add module M1.4"
```

---

## Task 8: Level 1 — LEVEL-CHECKLIST

**Files:**
- Create: `curriculum/level-1-foundation/LEVEL-CHECKLIST.md`

- [ ] **Step 1: Viết checklist lên Level 1 → 2**, tổng hợp năng lực M1.1–M1.4 + 1 capstone tổng.

```markdown
# Level 1 — Foundation · Checklist lên level

Đạt hết các mục bắt buộc (cột Reviewer tick) là đủ vào Level 2.
Người đã có automation có thể dùng bảng này để **test-out** Level 1.

| # | Tiêu chí | Tự đánh giá | Reviewer |
|---|----------|:-----------:|:--------:|
| M1.1 | Chỉ ra được việc AI dễ sai + 1 ví dụ ảo giác | ☐ | ☐ |
| M1.2 | Viết được prompt có cấu trúc cho 1 việc QE | ☐ | ☐ |
| M1.3 | Dùng AI coding assistant + review diff trước khi apply | ☐ | ☐ |
| M1.4 | Áp checklist kiểm chứng, loại được case AI sai | ☐ | ☐ |
| Capstone | Tài liệu ngắn: cách tôi sẽ dùng AI an toàn trong dự án | ☐ | ☐ |

**Reviewer:** QE senior/lead. Không có người kèm → dùng AI chấm sơ bộ bằng prompt:
"Đóng vai QE lead, chấm bài này theo từng tiêu chí trên, chỉ ra chỗ chưa đạt."
```

- [ ] **Step 2: Verify** — mỗi module M1.x có ≥1 dòng; có ghi chú reviewer + prompt chấm sơ bộ.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-1-foundation/LEVEL-CHECKLIST.md
git commit -m "docs(curriculum): add level 1 checklist"
```

---

## Task 9: Module M2.1 — Requirement → test case

**Files:**
- Create: `curriculum/level-2-practitioner/M2.1-phan-tich-requirement-sinh-test-case.md`

- [ ] **Step 1: Viết module** (skill: `generating-test-cases` — đang phát triển):
  - **Mục tiêu:** dùng AI phân tích 1 user story và sinh bộ test case phủ biên/negative; loại case sai/thừa.
  - **Hands-on:** dùng kịch bản **S1 (áp mã giảm giá)** trong `sample-project/BUGS.md`; prompt AI phân tích story → sinh test case → đối chiếu app. ≥1 prompt mẫu.
  - **Checklist:** ≥3 tiêu chí (VD "phủ được case mã hết hạn & mã sai").
  - **Capstone:** làm lại trên 1 user story thật của dự án.

- [ ] **Step 2: Verify** — hands-on trỏ đúng S1 trong BUGS.md; ô skill "đang phát triển".

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-2-practitioner/M2.1-phan-tich-requirement-sinh-test-case.md
git commit -m "docs(curriculum): add module M2.1"
```

---

## Task 10: Module M2.2 — Thiết kế test / charter

**Files:**
- Create: `curriculum/level-2-practitioner/M2.2-thiet-ke-test-charter.md`

- [ ] **Step 1: Viết module** (skill: `designing-test-strategy` — đang phát triển):
  - **Mục tiêu:** dùng AI phác chiến lược test / test charter cho 1 tính năng (rủi ro, phạm vi, loại test).
  - **Hands-on:** kịch bản **S2 (search sản phẩm)**; prompt AI đề xuất charter exploratory + ma trận rủi ro; học viên tinh chỉnh.
  - **Checklist:** ≥3 tiêu chí (VD "charter nêu được rủi ro chính + phạm vi").
  - **Capstone:** viết charter cho 1 tính năng sắp test thật.

- [ ] **Step 2: Verify** — trỏ S2; đủ 6 mục.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-2-practitioner/M2.2-thiet-ke-test-charter.md
git commit -m "docs(curriculum): add module M2.2"
```

---

## Task 11: Module M2.3 — Automation có AI hỗ trợ (nhánh nâng cao)

**Files:**
- Create: `curriculum/level-2-practitioner/M2.3-automation-co-ai-ho-tro.md`

- [ ] **Step 1: Viết module** (skill: `writing-automation-tests` — đang phát triển; đánh dấu **tùy chọn nâng cao**, cần chút code):
  - **Mục tiêu:** dùng AI viết & bảo trì test tự động (API hoặc UI) nhanh hơn, kèm review chất lượng test.
  - **Hands-on:** kịch bản **S3 (`POST /products` thiếu validate)**; dùng coding assistant viết 1 API test (Playwright/REST) khẳng định giá âm bị từ chối; chạy thử.
  - **Checklist:** ≥3 tiêu chí (VD "test chạy được và fail đúng khi bug tồn tại").
  - **Capstone:** thêm 1 automation test thật vào dự án với AI hỗ trợ.

- [ ] **Step 2: Verify** — header ghi "tùy chọn nâng cao"; trỏ S3.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-2-practitioner/M2.3-automation-co-ai-ho-tro.md
git commit -m "docs(curriculum): add module M2.3"
```

---

## Task 12: Module M2.4 — Sinh & quản lý test data

**Files:**
- Create: `curriculum/level-2-practitioner/M2.4-sinh-quan-ly-test-data.md`

- [ ] **Step 1: Viết module** (skill: `generating-test-data` — đang phát triển):
  - **Mục tiêu:** dùng AI sinh test data đa dạng (biên, unicode, khối lượng) + lưu ý dữ liệu nhạy cảm.
  - **Hands-on:** kịch bản **S4**; prompt AI sinh bộ dữ liệu user/brand gồm case biên; nạp/thử trên Toolshop.
  - **Checklist:** ≥3 tiêu chí (VD "bộ data có case biên + unicode").
  - **Capstone:** sinh test data cho 1 nhu cầu thật, ẩn danh dữ liệu nhạy cảm.

- [ ] **Step 2: Verify** — trỏ S4; nhắc dữ liệu nhạy cảm.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-2-practitioner/M2.4-sinh-quan-ly-test-data.md
git commit -m "docs(curriculum): add module M2.4"
```

---

## Task 13: Module M2.5 — Phân tích bug / log / triage

**Files:**
- Create: `curriculum/level-2-practitioner/M2.5-phan-tich-bug-log-triage.md`

- [ ] **Step 1: Viết module** (skill: `analyzing-bugs-and-logs` — đang phát triển):
  - **Mục tiêu:** dùng AI đọc log/stacktrace để khoanh vùng nguyên nhân + viết bug report rõ ràng.
  - **Hands-on:** kịch bản **S5 (log 500 khi đặt hàng thiếu field)**; prompt AI phân tích log → giả thuyết nguyên nhân → khung bug report.
  - **Checklist:** ≥3 tiêu chí (VD "bug report có bước tái hiện + kết quả mong đợi/thực tế").
  - **Capstone:** phân tích 1 log lỗi thật + viết bug report với AI hỗ trợ.

- [ ] **Step 2: Verify** — trỏ S5; đủ 6 mục.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-2-practitioner/M2.5-phan-tich-bug-log-triage.md
git commit -m "docs(curriculum): add module M2.5"
```

---

## Task 14: Module M2.6 — Audit chất lượng test suite (skill đã có)

**Files:**
- Create: `curriculum/level-2-practitioner/M2.6-audit-chat-luong-test-suite.md`

- [ ] **Step 1: Viết module**, ô "Skill liên quan" trỏ **`auditing-test-quality` (đã có)**: `../../.claude/skills/auditing-test-quality/SKILL.md`:
  - **Mục tiêu:** dùng skill `auditing-test-quality` để tìm fake/tautological test và khoảng trống rủi ro trong 1 suite.
  - **Hands-on:** kịch bản **S6 (test suite mỏng của Toolshop)**; chạy skill, đọc báo cáo, xác nhận ≥1 fake test.
  - **Checklist:** ≥3 tiêu chí (VD "chỉ ra ≥1 test không thực sự kiểm tra gì").
  - **Capstone:** audit 1 phần test suite thật của dự án bằng skill.

- [ ] **Step 2: Verify** — link skill trỏ đúng file `SKILL.md` tồn tại; trạng thái ghi "đã có".

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-2-practitioner/M2.6-audit-chat-luong-test-suite.md
git commit -m "docs(curriculum): add module M2.6"
```

---

## Task 15: Level 2 — LEVEL-CHECKLIST

**Files:**
- Create: `curriculum/level-2-practitioner/LEVEL-CHECKLIST.md`

- [ ] **Step 1: Viết checklist Level 2 → 3**, mọi module bắt buộc (M2.1, M2.2, M2.4, M2.5, M2.6) + M2.3 đánh dấu tùy chọn, + 1 capstone tổng. Theo đúng khuôn bảng của Task 8 (cột Tự đánh giá / Reviewer + ghi chú reviewer & prompt chấm sơ bộ). Mỗi module 1 dòng; M2.3 ghi "(nâng cao, khuyến khích)".

- [ ] **Step 2: Verify** — 6 module đều có dòng; M2.3 đánh dấu tùy chọn.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-2-practitioner/LEVEL-CHECKLIST.md
git commit -m "docs(curriculum): add level 2 checklist"
```

---

## Task 16: Module M3.1 — Dùng AI test tool chuyên dụng

**Files:**
- Create: `curriculum/level-3-builder/M3.1-dung-ai-test-tool-chuyen-dung.md`

- [ ] **Step 1: Viết module** (skill: — ):
  - **Mục tiêu:** đánh giá & dùng được 1 AI test tool chuyên dụng (test generation / self-healing / visual) cho Toolshop; biết điểm mạnh/yếu.
  - **Lý thuyết:** các nhóm AI test tool + tiêu chí chọn; cảnh báo lock-in & chi phí.
  - **Hands-on:** chọn 1 tool (gợi ý vài lựa chọn phổ biến), áp lên 1 luồng Toolshop, ghi nhận kết quả + hạn chế.
  - **Checklist:** ≥3 tiêu chí (VD "nêu được 2 hạn chế của tool đã thử").
  - **Capstone:** đề xuất 1 AI test tool phù hợp cho dự án + lý do.

- [ ] **Step 2: Verify** — hands-on áp lên Toolshop; nêu tiêu chí chọn tool.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-3-builder/M3.1-dung-ai-test-tool-chuyen-dung.md
git commit -m "docs(curriculum): add module M3.1"
```

---

## Task 17: Module M3.2 — Viết skill QE riêng

**Files:**
- Create: `curriculum/level-3-builder/M3.2-viet-skill-qe-rieng.md`

- [ ] **Step 1: Viết module** (skill: `writing-qe-skills` — đang phát triển):
  - **Mục tiêu:** viết được 1 skill QE mới theo chuẩn repo (bản gốc `.claude/skills/<tên>/SKILL.md` + adapter các tool).
  - **Lý thuyết:** cấu trúc SKILL.md (frontmatter name/description); mô hình bản gốc + adapter mỏng (tham chiếu README repo).
  - **Hands-on:** viết 1 skill nhỏ (VD `writing-bug-reports`) cho Toolshop, sinh adapter cho ≥1 tool, thử gọi.
  - **Checklist:** ≥3 tiêu chí (VD "skill có frontmatter hợp lệ + ≥1 adapter trỏ đúng").
  - **Capstone:** viết 1 skill giải quyết việc lặp lại thật trong dự án.

- [ ] **Step 2: Verify** — tham chiếu đúng mô hình bản gốc+adapter của repo (xem README.md gốc).

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-3-builder/M3.2-viet-skill-qe-rieng.md
git commit -m "docs(curriculum): add module M3.2"
```

---

## Task 18: Module M3.3 — Gọi Claude API build công cụ test

**Files:**
- Create: `curriculum/level-3-builder/M3.3-goi-claude-api-build-cong-cu.md`

- [ ] **Step 1: Viết module** (skill: `building-test-tools-with-api` — đang phát triển):
  - **Mục tiêu:** gọi được Claude API/SDK từ script để tự động 1 việc QE (VD sinh test case từ file requirement).
  - **Lý thuyết:** khái niệm API key/model/prompt trong code; bảo mật key; chi phí token. (Không nhúng giá trị model cụ thể vào plan — khi viết công cụ thật, tra skill `claude-api`.)
  - **Hands-on:** script nhỏ đọc 1 requirement của Toolshop → gọi API → in ra bộ test case; chạy được.
  - **Checklist:** ≥3 tiêu chí (VD "script chạy ra output; key không hard-code").
  - **Capstone:** viết 1 script nhỏ tự động 1 việc QE lặp lại thật.

- [ ] **Step 2: Verify** — nhấn mạnh bảo mật key; không hard-code model id trong tài liệu (trỏ tra cứu skill `claude-api`).

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-3-builder/M3.3-goi-claude-api-build-cong-cu.md
git commit -m "docs(curriculum): add module M3.3"
```

---

## Task 19: Module M3.4 — Xây agent/pipeline QE (capstone lớn)

**Files:**
- Create: `curriculum/level-3-builder/M3.4-xay-agent-pipeline-qe.md`

- [ ] **Step 1: Viết module** (skill: — ):
  - **Mục tiêu:** ghép các kỹ năng thành 1 pipeline/agent QE nhỏ (VD: đọc PR → sinh test → chạy → tóm tắt), là capstone của cả Level 3.
  - **Lý thuyết:** khái niệm pipeline/agent; ghép API + tool + skill; giới hạn & giám sát con người.
  - **Hands-on:** phác 1 pipeline tối giản chạy trên Toolshop (có thể là script tuần tự), mô tả từng chặng.
  - **Checklist:** ≥3 tiêu chí (VD "pipeline chạy end-to-end trên 1 input mẫu").
  - **Capstone:** đề xuất & (nếu được) dựng thử pipeline QE cho dự án thật.

- [ ] **Step 2: Verify** — là capstone tổng của Level 3; chạy trên Toolshop.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-3-builder/M3.4-xay-agent-pipeline-qe.md
git commit -m "docs(curriculum): add module M3.4"
```

---

## Task 20: Level 3 — LEVEL-CHECKLIST

**Files:**
- Create: `curriculum/level-3-builder/LEVEL-CHECKLIST.md`

- [ ] **Step 1: Viết checklist Level 3 (hoàn tất lộ trình)**, gồm M3.1–M3.4 + capstone tổng là chính M3.4. Theo đúng khuôn bảng Task 8. Ghi rõ Level 3 là nhánh nâng cao, phù hợp người có code.

- [ ] **Step 2: Verify** — 4 module có dòng; nêu Level 3 nâng cao.

- [ ] **Step 3: Commit**

```bash
git add curriculum/level-3-builder/LEVEL-CHECKLIST.md
git commit -m "docs(curriculum): add level 3 checklist"
```

---

## Task 21: Cập nhật README repo + kiểm tra liên kết

**Files:**
- Modify: `README.md` (root repo)

- [ ] **Step 1: Thêm mục "Lộ trình học" vào `README.md` gốc**, ngay sau bảng "Skill hiện có", trỏ tới `curriculum/README.md`:

```markdown
## Lộ trình học AI cho Tester

Ngoài bộ skill, repo có **lộ trình đào tạo** giúp tester học & ứng dụng AI vào công
việc, tổ chức theo level năng lực: [curriculum/README.md](curriculum/README.md).
```

- [ ] **Step 2: Verify toàn bộ liên kết** — rà thủ công mọi link tương đối trong `curriculum/` (trang chủ → 15 module + 3 checklist + sample-project; các module → sample-project & skill; M2.6 → `auditing-test-quality/SKILL.md`). Chạy kiểm tra file tồn tại:

```bash
cd /Users/phanhuy/Work/projects/QE-AI
ls curriculum/level-1-foundation/*.md curriculum/level-2-practitioner/*.md \
   curriculum/level-3-builder/*.md curriculum/sample-project/*.md \
   curriculum/README.md curriculum/MODULE-TEMPLATE.md
```
Expected: liệt kê đủ 22 file (15 module + 3 checklist + README + template + 2 sample-project).

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "docs: link learning curriculum from repo README"
```

---

## Self-Review (đã chạy khi viết plan)

- **Spec coverage:** Level/module map (Task 1,4–20) ↔ spec §2; khuôn 6 mục (Task 2) ↔ §3; sample project + cách lên level (Task 3,8,15,20) ↔ §4; bản đồ skill + "đang phát triển" (mọi module + README) ↔ §5; tách giai đoạn (plan này chỉ giai đoạn 1) ↔ §6; cấu trúc thư mục (File Structure) ↔ §7. Không có mục spec nào thiếu task.
- **Placeholder scan:** không dùng "TBD/TODO"; ô "đang phát triển" là trạng thái skill hợp lệ theo spec §6, không phải placeholder nội dung.
- **Type/tên nhất quán:** slug file trong File Structure = trong từng task = trong bảng README (Task 1). Kịch bản S1–S6 (Task 3) được tham chiếu đúng ở M2.1–M2.6.

## Ghi chú giai đoạn 2 (ngoài plan này)
8 skill "đang phát triển" (`prompting-for-qe`, `generating-test-cases`, `designing-test-strategy`, `writing-automation-tests`, `generating-test-data`, `analyzing-bugs-and-logs`, `writing-qe-skills`, `building-test-tools-with-api`) — mỗi skill một vòng brainstorm→plan→implement riêng, cập nhật lại ô "Skill liên quan" + cột trạng thái ở `curriculum/README.md` khi xong.
