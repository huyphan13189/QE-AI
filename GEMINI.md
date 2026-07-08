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
thật — một hành trình người dùng đầy đủ (login, checkout, luồng chính), hoặc khi spec e2e sẵn có bị
flaky / chậm ở khâu setup. **Không** dùng để assert một đơn vị API/UI lẻ (dùng
`writing-automation-tests`) hay để chọn hành trình nào cần phủ (dùng `designing-test-strategy`).

**Phải làm:** ĐỌC và LÀM THEO nguyên văn `.claude/skills/writing-e2e-tests/SKILL.md`.

Nguyên tắc cốt lõi: độ tin cậy của e2e quyết định TRƯỚC assertion đầu tiên — bởi cách đưa app vào
đúng trạng thái; đau nhất là khâu setup (state flaky, OAuth thật, entry bị gate). Theo spine reset →
seed → auth-bypass → enter (deep-link qua gate rồi drive UI) → assert chính xác trên data tất định;
không automate OAuth thật, không seed bằng cách bấm xuyên UI, không `waitForTimeout`.
