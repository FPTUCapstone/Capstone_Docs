# Hướng dẫn sử dụng Documentation Framework (A-Z)

Tài liệu này hướng dẫn cách dùng bộ skill trong [.claude/skills/](.claude/skills/) để đi từ một
Use Case thô đến một Stitch Prompt sẵn sàng generate UI, theo đúng pipeline mô tả trong
[CLAUDE.md](CLAUDE.md) và [README.md](README.md).

Đối tượng đọc: người dùng Claude Code trong repo này, chưa từng chạy qua pipeline lần nào.

---

## 1. Tổng quan pipeline

```text
Use Case (bạn viết tay)
   ↓ /usecase-analysis
Requirement Analysis          → requirements/<feature>.md
   ↓ /mvp-planning
MVP Scope                     → requirements/mvp/<feature>-mvp.md
   ↓ /user-flow
User Flow                     → ux/user-flows/<feature>-user-flow.md
   ↓ /screen-specification
Screen Specification          → ux/screen-specifications/<feature>-screen-spec.md
   ↓ /stitch-prompt
Stitch Prompt                 → ux/stitch-prompts/<feature>-stitch-prompt.md
   ↓ (dán vào Google Stitch, ngoài repo)
UI/UX MVP (mockup)
   ↓ (chưa có skill — xem mục 8)
API Contract                  → api/contracts/
   ↓
Implementation (repo khác, không nằm trong repo này)
```

Nguyên tắc xuyên suốt (từ [CLAUDE.md](CLAUDE.md)):

- Mỗi bước chỉ được dùng **output đã duyệt** của bước trước làm nguồn sự thật — không bịa thêm requirement.
- Assumption / UX suggestion / Open Question phải được đánh dấu rõ, tách biệt với fact.
- Không sinh application source code trong repo này.
- Use Case gốc không được sửa nếu không có chỉ định rõ ràng.

**Gợi ý quy trình làm việc:** dừng lại đọc và duyệt (approve) output của mỗi bước trước khi
chạy skill kế tiếp. Các skill không tự động hỏi lại bạn — nếu bạn chạy `/user-flow` khi MVP
scope còn sai, lỗi đó sẽ lan xuống toàn bộ các bước sau.

---

## 2. Bước 0 — Viết Use Case gốc

Trước khi dùng bất kỳ skill nào, tạo file Use Case bằng tay trong `requirements/`, ví dụ:

```text
requirements/<feature-name>.md
```

File này là nguồn sự thật ban đầu, gồm mô tả nghiệp vụ (actors, main flow, business rules...)
viết theo ngôn ngữ tự nhiên. Không cần theo format cố định ở bước này.

---

## 3. Bước 1 — Use Case Analysis (`/usecase-analysis`)

**Input:** Use Case gốc ở bước 0.

**Việc skill làm:** đọc use case, giữ nguyên ý nghĩa nghiệp vụ, xác định Actors, Preconditions,
Main/Alternative/Exception Flows, Postconditions, Functional Requirements, Business Rules, Edge
Cases, Missing Information, MVP Scope sơ bộ. Tách rõ Explicit Requirements / Assumptions /
Suggestions / Questions.

**Cách chạy:**

```text
/usecase-analysis
Phân tích use case tại requirements/register-traveler-account.md
```

**Output:** `requirements/<feature-name>.md` (ghi đè/bổ sung ngay trên file use case, hoặc file
phân tích riêng tuỳ bạn tổ chức — quan trọng là giữ trong `requirements/`).

**Trước khi qua bước tiếp theo:** đọc kỹ mục "Missing Information" — trả lời càng nhiều câu hỏi
càng tốt (trực tiếp sửa vào file, ghi rõ "Decided <ngày>") trước khi chạy MVP Planning, vì mọi
quyết định còn bỏ ngỏ sẽ chặn các bước sau.

---

## 4. Bước 2 — MVP Planning (`/mvp-planning`)

**Input:** Requirement Analysis đã duyệt (bước 1).

**Việc skill làm:** xác định phạm vi sản phẩm nhỏ nhất để validate giá trị cốt lõi — primary
user goal, core user journey, phân loại feature theo Must/Should/Could/Out of Scope, dependency,
risk.

**Cách chạy:**

```text
/mvp-planning
Lập MVP scope từ requirements/register-traveler-account.md
```

**Output:** `requirements/mvp/<feature-name>-mvp.md`

Ví dụ: `requirements/mvp/register-traveler-account-mvp.md`

---

## 5. Bước 3 — User Flow (`/user-flow`)

**Input:** MVP Planning đã duyệt (bước 2).

**Việc skill làm:** chuyển MVP scope thành user journey đầu-cuối tối giản — entry point, main
flow (User Action / System Action / Next State cho từng bước), alternative flow, validation
flow, error flow, success state, navigation map, và một **Mermaid flowchart**. Chưa thiết kế UI.

**Cách chạy:**

```text
/user-flow
Tạo user flow từ requirements/mvp/register-traveler-account-mvp.md
```

**Output:** `ux/user-flows/<feature-name>-user-flow.md`

---

## 6. Bước 4 — Screen Specification (`/screen-specification`)

**Input:** User Flow đã duyệt (bước 3) + MVP Planning + Requirement Analysis.

**Việc skill làm:** xác định tập màn hình tối thiểu cần cho MVP journey. Với mỗi màn hình: mục
đích, user, entry conditions, input/thông tin hiển thị, action chính/phụ, validation, error
handling, states (Initial/Input/Validation Error/Business Error/Loading/Success), navigation,
business rules liên quan, open question, UX suggestion.

**Cách chạy:**

```text
/screen-specification
Tạo screen spec từ ux/user-flows/register-traveler-account-user-flow.md
```

**Output:** `ux/screen-specifications/<feature-name>-screen-spec.md`

Quy tắc quan trọng: chỉ tạo màn hình cho tính năng **Must Have**; không tạo state hoặc feature
ngoài phạm vi MVP đã chốt ở bước 2.

---

## 7. Bước 5 — Stitch Prompt (`/stitch-prompt`)

**Input:** Screen Specification đã duyệt (bước 4) + User Flow + MVP Planning.

**Việc skill làm:** chuyển screen spec thành một design brief súc tích cho Google Stitch —
Product Context, Screen Objective, Target User, Screen Content, Required Components/States,
Navigation Context, UX Constraints, Open Questions, và cuối cùng là **Stitch Prompt** (tiếng Anh,
mobile-first, không có implementation detail).

**Cách chạy:**

```text
/stitch-prompt
Tạo stitch prompt từ ux/screen-specifications/register-traveler-account-screen-spec.md
```

**Output:** `ux/stitch-prompts/<feature-name>-stitch-prompt.md`

**Bước thủ công tiếp theo (ngoài repo):** copy phần "Stitch Prompt" trong file trên, dán vào
Google Stitch để sinh UI mockup. Kết quả UI/UX MVP không lưu trong repo này.

---

## 8. Sau Stitch — API Contract & Implementation

Hai bước cuối trong pipeline ở [README.md](README.md) (API Contract → Implementation) **chưa có
skill tương ứng** trong `.claude/skills/` tại thời điểm viết tài liệu này. Khi cần:

- API Contract sẽ được lưu tại `api/contracts/` (đã khai báo trong CLAUDE.md) — tạo skill mới
  theo đúng pattern của 5 skill hiện có (Source of Truth rõ ràng, không bịa requirement, có
  Output Location + naming convention) khi bắt đầu cần.
- Implementation (Next.js / Flutter / .NET) nằm ở repo khác, **không** tạo source code trong
  repo tài liệu này.

---

## 9. Quy ước đặt tên & vị trí file (tóm tắt)

| Bước | Skill | Output | Naming |
|---|---|---|---|
| 1 | `/usecase-analysis` | `requirements/` | `<feature-name>.md` |
| 2 | `/mvp-planning` | `requirements/mvp/` | `<feature-name>-mvp.md` |
| 3 | `/user-flow` | `ux/user-flows/` | `<feature-name>-user-flow.md` |
| 4 | `/screen-specification` | `ux/screen-specifications/` | `<feature-name>-screen-spec.md` |
| 5 | `/stitch-prompt` | `ux/stitch-prompts/` | `<feature-name>-stitch-prompt.md` |

`<feature-name>` dùng kebab-case, giữ nguyên xuyên suốt cả 5 file của cùng một feature (ví dụ:
`register-traveler-account`).

---

## 10. Ví dụ end-to-end đã chạy trong repo

Feature `register-traveler-account` đã đi qua đủ 5 bước, dùng làm tham chiếu khi bạn chạy feature
mới:

1. [requirements/register-traveler-account.md](requirements/register-traveler-account.md)
2. [requirements/mvp/register-traveler-account-mvp.md](requirements/mvp/register-traveler-account-mvp.md)
3. [ux/user-flows/register-traveler-account-user-flow.md](ux/user-flows/register-traveler-account-user-flow.md)
4. [ux/screen-specifications/register-traveler-account-screen-spec.md](ux/screen-specifications/register-traveler-account-screen-spec.md)
5. [ux/stitch-prompts/register-traveler-account-stitch-prompt.md](ux/stitch-prompts/register-traveler-account-stitch-prompt.md)

---

## 11. Checklist nhanh khi bắt đầu một feature mới

1. Viết use case gốc vào `requirements/<feature-name>.md`.
2. Chạy `/usecase-analysis` → đọc, trả lời Missing Information → duyệt.
3. Chạy `/mvp-planning` → duyệt Must/Should/Could/Out of Scope.
4. Chạy `/user-flow` → duyệt flow + Mermaid diagram.
5. Chạy `/screen-specification` → duyệt danh sách màn hình + states.
6. Chạy `/stitch-prompt` → copy Stitch Prompt sang Google Stitch.
7. Commit sau mỗi bước đã duyệt để giữ lịch sử truy vết được (traceable).
