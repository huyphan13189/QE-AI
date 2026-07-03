# Hướng dẫn học — cách dùng lộ trình này

Trang này mô tả **cách đi qua lộ trình để thực sự học được**, từ lúc mở repo tới khi lên
level. Nếu bạn chỉ đọc một file trước khi bắt đầu, hãy đọc file này.

Xem [bản đồ toàn bộ module](README.md) để biết mình đang ở đâu.

## 1. Bắt đầu ở đâu

- **Mặc định:** ai cũng vào **Level 1**. Đọc [README](README.md) để thấy bản đồ, rồi mở
  module đầu tiên.
- **Đã có nền (automation / SDET):** không cần học lại Level 1 — làm thẳng
  [checklist Level 1](level-1-foundation/LEVEL-CHECKLIST.md) để **test-out**. Tick đủ mục
  bắt buộc là nhảy sang Level 2.
- Trước khi vào module đầu của mỗi level, đọc khối **"Học xong Level N bạn làm được gì"** ở
  đầu `LEVEL-CHECKLIST` để biết đích đến và ranh giới của level.

## 2. Vòng lặp cốt lõi — mỗi module học y hệt nhau

Mọi module theo [khuôn 6 mục](MODULE-TEMPLATE.md). Đi tuần tự từ trên xuống:

```
1. Mục tiêu năng lực   → "học xong tôi làm được gì" (soi kỳ vọng trước)
2. Vì sao & khi nào     → hiểu lúc nào NÊN / KHÔNG nên dùng AI ở việc này
3. Lý thuyết ngắn       → đọc nhanh, không hàn lâm
4. Hands-on Toolshop    → LÀM THẬT trên sample project, đã có prompt mẫu
5. Checklist năng lực   → tự chấm (cột "Tự đánh giá")
6. Capstone dự án thật  → áp kỹ năng đó vào công việc đang làm
```

**Mấu chốt: học bằng tay, không chỉ đọc.** Mục 3 ngắn có chủ đích — phần lớn thời gian nằm
ở mục 4 (tập) và mục 6 (áp thật). Đọc xong lý thuyết mà chưa làm hands-on thì coi như chưa
học module đó.

## 3. Hai loại thực hành — cần cả hai

| | Sample project ([Toolshop](sample-project/README.md)) | Capstone dự án thật |
|---|---|---|
| Mục đích | Tập an toàn, cùng một sân, có prompt mẫu sẵn | Chứng minh đã *chuyển hoá* kỹ năng vào việc thật |
| Rủi ro | Zero — không đụng dữ liệu / production thật | Có thật → buộc phải ẩn danh dữ liệu, review, chịu trách nhiệm |
| Khi nào | Mục 4 mỗi module | Mục 6 mỗi module + 1 capstone tổng cuối level |

Toolshop để **tập**; capstone để **được công nhận đã học được**. Reviewer chấm chủ yếu ở
capstone.

## 4. Skill dùng thế nào *khi đang học*

Các skill trong `.claude/skills/` **không phải tài liệu để đọc** — chúng là "người kèm AI"
chạy nền. Khi bạn làm hands-on (mục 4) hoặc capstone (mục 6):

- Dùng AI tool đã cài adapter (Claude Code / Cursor / Copilot / Gemini / Windsurf), skill
  **tự kích hoạt đúng lúc** nhờ trường `description` — ví dụ gõ *"sinh test case cho story
  này"* thì skill [`generating-test-cases`](../.claude/skills/generating-test-cases/SKILL.md)
  tự bắt AI ép coverage biên / negative thay vì trả về danh sách happy-path.
- Nghĩa là bạn vừa học *cách nghĩ* (nội dung module) vừa được AI *ép làm đúng kỷ luật* (skill)
  ngay trên bài tập. Học và guardrail đi cùng nhau.
- Chưa cài adapter? Xem [README repo](../README.md) mục hướng dẫn từng tool; hoặc tạm dán nội
  dung `SKILL.md` vào chatbot khi làm bài.

## 5. Lên level — cơ chế công nhận

1. Tick hết mục **bắt buộc** trong `LEVEL-CHECKLIST` + làm **1 capstone tổng** cuối level.
2. Nhờ **QE senior / lead tick cột Reviewer** — đây là cửa thật để lên level.
3. **Không có người kèm?** Mỗi checklist có sẵn prompt để **AI chấm sơ bộ** trước ("Đóng vai
   QE lead, chấm theo từng tiêu chí…"). Tự sửa theo góp ý rồi mới nhờ người thật duyệt.

Thứ tự: `Tự đánh giá` (bạn) → `AI chấm sơ bộ` (nếu học một mình) → `Reviewer tick` (người thật).

## 6. Nhịp học

- **Dài hạn, tự tiến độ** — không theo tuần cố định; xong module này mới sang module sau.
- **Học cá nhân:** đi tuần tự Level 1 → 2 → (3 nếu đã vững code).
- **Học theo team:** mỗi người tự đi, hoặc lead chọn 1 module mỗi 1–2 tuần để cả nhóm cùng
  làm rồi review chung capstone. Checklist + reviewer là điểm đồng bộ.
- **Level 3 là nhánh nâng cao** (cần code). Manual tester dừng ở hết Level 2 vẫn là một cột
  mốc trọn vẹn — tự chủ toàn bộ vòng đời test hằng ngày với AI.

## Tóm tắt một dòng

> Đọc **mục tiêu** → hiểu **khi nào dùng AI** → **tập trên Toolshop** với skill chạy nền →
> **tự chấm checklist** → **áp vào dự án thật** → **reviewer tick** để lên level.
