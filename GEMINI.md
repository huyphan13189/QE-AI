# GEMINI.md — Hướng dẫn cho Gemini CLI

Repo này phân phối các **skill Quality Engineering (QE)** dùng chung cho mọi AI coding assistant.
File này được **Gemini CLI** nạp làm context tự động (project-level và có thể đặt global ở
`~/.gemini/GEMINI.md`).

## Skill: auditing-test-quality

**Khi nào áp dụng:** khi người dùng yêu cầu audit / đánh giá / review chất lượng hoặc độ phủ
của một test suite hiện có, hoặc trước khi tin vào một suite đang xanh / coverage cao.
**Không** dùng để viết test mới.

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/auditing-test-quality/SKILL.md`.

Nguyên tắc cốt lõi: suite xanh và coverage cao chỉ chứng minh test **CHẠY**, không chứng minh
test **KIỂM TRA** được gì. Đi theo Process, 7 lenses và red-flag trong skill.

## Skill: generating-test-cases

**Khi nào áp dụng:** khi cần sinh / thiết kế test case từ requirement, user story, ticket, hoặc
spec API/UI bằng AI, hoặc review một danh sách case AI vừa tạo. **Không** dùng để audit test suite
tự động sẵn có (dùng `auditing-test-quality`).

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/generating-test-cases/SKILL.md`.

Nguyên tắc cốt lõi: AI sinh case rất nhanh nhưng mặc định lệch happy-path, bịa hành vi không có
trong spec, và bỏ sót case biên/âm/lỗi. Đi theo Process, bảng coverage dimensions và red-flag trong
skill; ground trước khi sinh, ép đủ chiều phủ, cắt case bịa/trùng, kiểm chứng lại với spec/app thật.

## Skill: prompting-for-qe

**Khi nào áp dụng:** khi soạn / tinh chỉnh prompt cho một tác vụ QE, đặc biệt khi output lan man /
chung chung / bịa / không dùng được, hoặc muốn chuẩn hóa prompt thành template. **Không** dùng riêng
cho việc sinh test case (dùng `generating-test-cases`).

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/prompting-for-qe/SKILL.md`.

Nguyên tắc cốt lõi: chất lượng output QE bị chặn trên bởi chất lượng prompt & context; prompt tốt =
vai trò + context thật + nhiệm vụ + định dạng + ràng buộc, không dán PII/secret thật.

## Skill: designing-test-strategy

**Khi nào áp dụng:** khi lập kế hoạch test cho một tính năng/epic/release, viết test charter, hoặc
quyết định test gì trước và bỏ gì. **Không** dùng để sinh test case chi tiết (dùng
`generating-test-cases`).

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/designing-test-strategy/SKILL.md`.

Nguyên tắc cốt lõi: chiến lược tốt là ưu tiên theo rủi ro, không phải liệt kê mọi thứ; AI giỏi liệt
kê, giá trị nằm ở việc cắt theo rủi ro và nêu out-of-scope.

## Skill: writing-automation-tests

**Khi nào áp dụng:** khi viết / mở rộng / bảo trì automation test (API/UI) có AI hỗ trợ, sinh khung
test, chuyển test case thủ công sang script. **Không** dùng để audit suite sẵn có (dùng
`auditing-test-quality`) hay thiết kế test case (dùng `generating-test-cases`).

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/writing-automation-tests/SKILL.md`.

Nguyên tắc cốt lõi: AI hay sinh test luôn-xanh và selector giòn; test chỉ có giá trị khi FAIL đúng
lúc có bug — luôn chạy cả chiều pass lẫn fail.

## Skill: generating-test-data

**Khi nào áp dụng:** khi cần bộ test data đa dạng (biên, unicode, bulk, nhiều biến thể) cho
UI/API/DB, seed data, fixtures. **Không** dùng để thiết kế test case (dùng `generating-test-cases`).

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/generating-test-data/SKILL.md`.

Nguyên tắc cốt lõi: AI mặc định chỉ ra giá trị đẹp, dễ sai schema, cám dỗ dùng dữ liệu thật; phải
phủ biên/bất hợp lệ, đúng schema, và tuyệt đối ẩn danh — không dùng dữ liệu prod.

## Skill: analyzing-bugs-and-logs

**Khi nào áp dụng:** khi có log lỗi / stacktrace / lỗi 500 cần khoanh nguyên nhân, triage, hoặc viết
bug report tái hiện được; khi log quá dài cần tóm tắt. **Không** dùng để viết automation test (dùng
`writing-automation-tests`).

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/analyzing-bugs-and-logs/SKILL.md`.

Nguyên tắc cốt lõi: AI đoán nguyên nhân rất tự tin kể cả khi sai — coi output là giả thuyết phải tái
hiện để kiểm chứng; làm sạch secret/PII trong log trước khi gửi.

## Skill: writing-qe-skills

**Khi nào áp dụng:** khi đóng gói một quy trình QE lặp lại thành skill dùng chung trong repo này,
thêm skill mới, hoặc sửa skill sẵn có. **Không** dùng để áp dụng một skill vào việc QE thật (dùng
chính skill đó).

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/writing-qe-skills/SKILL.md`.

Nguyên tắc cốt lõi: skill tốt là reference tái dùng, không phải câu chuyện một lần; `description`
phải nói KHI NÀO dùng; nhớ sinh đủ adapter cho mọi tool.

## Skill: building-test-tools-with-api

**Khi nào áp dụng:** khi tự động hóa một việc QE lặp lại bằng cách gọi Claude API/SDK trong
code/script/pipeline. **Không** dùng cho AI coding assistant tương tác hay chọn AI test tool có sẵn.

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/building-test-tools-with-api/SKILL.md`.

Nguyên tắc cốt lõi: key đọc từ ENV, model/tham số tra tài liệu Claude API hiện hành (không hard-code
ID), luôn có xử lý lỗi + ước lượng token, và validate output như mọi output AI.

## Skill: writing-e2e-tests

**Khi nào áp dụng:** khi viết hoặc sửa e2e test golden-path chạy trình duyệt thật qua backend + DB
thật — một hành trình người dùng đầy đủ (login, checkout, luồng chính), khi spec e2e sẵn có bị
flaky / chậm ở khâu setup, hoặc khi code đổi cần thêm/sửa e2e. **Không** dùng để assert một đơn vị
API/UI lẻ (dùng `writing-automation-tests`) hay để lập kế hoạch/chọn journey nào cần phủ và theo thứ
tự nào (dùng `planning-e2e-coverage`).

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/writing-e2e-tests/SKILL.md`.

Nguyên tắc cốt lõi: độ tin cậy của e2e quyết định TRƯỚC assertion đầu tiên — bởi cách đưa app vào
đúng trạng thái; đau nhất là khâu setup (state flaky, OAuth thật, entry bị gate). Theo spine reset →
seed → auth-bypass → enter (deep-link qua gate rồi drive UI) → assert chính xác trên data tất định;
không automate OAuth/payment/email thật (dùng sandbox/test-mode/stub), không seed bằng cách bấm xuyên
UI, không `waitForTimeout`. Endpoint test (reset/seed/dev-login) cần backend hỗ trợ. Khi code đổi:
grep spec theo route/label/chuỗi assert để sửa spec cũ. Reuse scale theo quy mô suite (helper/data-
builder → fixtures/`storageState` → POM khi 1 màn nhiều spec).

## Skill: planning-e2e-coverage

**Khi nào áp dụng:** khi quyết định codebase cần e2e những gì — lập kế hoạch coverage lần đầu, chọn
flow/màn hình nào cần phủ và theo thứ tự nào, phân tầng LOW/MEDIUM/HIGH, map flow đã có e2e vs chưa,
dựng roadmap mở rộng suite. **Không** dùng để viết 1 spec cụ thể (dùng `writing-e2e-tests`) hay đánh
giá test sẵn có có thực sự test gì không (dùng `auditing-test-quality`).

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/planning-e2e-coverage/SKILL.md`.

Nguyên tắc cốt lõi: coverage e2e là roadmap ưu tiên theo rủi ro, không phải "test hết"; sweep codebase
liệt kê journey → xếp hạng blast-radius × traffic (main flow trước) → baseline map journey ↔ e2e đã
có → phân tầng LOW (smoke gate làm trước)/MEDIUM/HIGH (hoãn tới khi đáng) → roadmap có điều kiện dừng.
Tie-breaker: "đánh giá e2e phủ flow/màn hình nào (covered vs chưa)" → dùng bước baseline của skill này;
"test sẵn có có thực sự bắt lỗi không (fake/tautological)" → dùng `auditing-test-quality`.

## Skill: capturing-ui-sweep

**Khi nào áp dụng:** khi muốn chụp ảnh toàn bộ UI của app để tự review layout lệch/vỡ, thiếu data,
empty-state xấu, UX cần cải thiện — một lượt visual QA sweep gom ảnh theo tính năng rồi chạy MỘT lượt
AI soi lỗi. Trigger "chụp UI toàn app", "visual QA sweep", "quét màn hình review UI", "screenshot gom
nhóm tính năng". **Không** dùng để đánh giá test có bắt lỗi không (dùng `auditing-test-quality`) hay
lập kế hoạch journey nào cần e2e (dùng `planning-e2e-coverage`) — đây là lượt báo cáo, KHÔNG sửa UI.

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/capturing-ui-sweep/SKILL.md`.

Nguyên tắc cốt lõi: sweep chụp mọi feature/state đã curated thành ảnh, gom theo tính năng vào một trang
index, rồi chạy MỘT lượt AI soi lỗi — và KHÔNG bao giờ đụng code UI. Ba mảnh tách biệt: registry
(FeatureBlock[], thứ duy nhất phải bảo trì) → runner generic (sinh PNG + manifest.json) → build-index
(manifest → index.html, nhúng findings). Chụp trên DB test đã reset+seed, không đụng DB dev thật. Ba
bước tách rời capture → index → review; review là việc của AI (đọc PNG cả 2 viewport, soi tràn/lệch/
thiếu data/empty-state/spacing/UX), ghi review.json rồi rebuild index; báo người dùng, không tự sửa.

## Skill: managing-seed-data

**Khi nào áp dụng:** khi dựng/mở rộng seed data LOCAL thành thư viện fixtures tái dùng cho cả CLI seed
dev lẫn e2e — thêm builder/scenario cho dev data của tính năng mới, wire HTTP seed slice cho e2e, hay
sửa seed bị cộng dồn row mỗi lần chạy. Trigger "thêm seed data", "seed cho màn X", "seed để test UI",
"làm seed idempotent". **Không** dùng để sinh giá trị data đa dạng (biên/unicode/bulk — dùng
`generating-test-data`), không cho prod reference data, không để viết e2e spec tiêu thụ seed (dùng
`writing-e2e-tests`) — đây là hạ tầng seed và vòng đời của nó.

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/managing-seed-data/SKILL.md`.

Nguyên tắc cốt lõi: feature data nằm ở MỘT thư viện fixtures dùng chung (builder = 1 slice, scenario =
gộp builder), sau PROD guard fail-closed, và PHẢI idempotent (chạy lại = cùng state, không cộng dồn).
Bẫy chính: row gắn vào user "POV" (un-prefixed, không bị xoá) sẽ cộng dồn +N mỗi lần chạy nếu không
mang seed marker hoặc cascade theo parent seed bị xoá — sửa bằng cách gắn marker + thêm 1 dòng
deleteMany vào clean(). Hai tầng seed đừng trộn: prod reference (ship khi deploy) vs fixtures local.
Edge data lấy từ palette dùng chung (long/emoji/số biên/empty-state), deterministic. Verify: typecheck
+ smoke test idempotency (chạy 2 lần, đếm bằng nhau).
