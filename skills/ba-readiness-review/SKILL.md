---
name: ba-readiness-review
description: "Đánh giá mức độ sẵn sàng của tài liệu BA/requirement đầu vào từ document ID hoặc folder tài liệu và xuất ra 1 file markdown review có nhận xét theo readiness. Dùng khi user muốn review tài liệu, đánh giá đầu vào, xem đã đủ lên technical design chưa, có thể code end-to-end chưa, hoặc đưa 1 doc ID / folder BA để nhận kết luận có cấu trúc. Trigger khi thấy các ý như 'đánh giá tài liệu', 'review BA', 'readiness', 'đủ để code chưa', 'phân tích folder tài liệu', 'doc ID này review giúp tôi'. Luôn dùng skill này thay vì nhận xét ad-hoc."
metadata:
  author: bbs-team
  version: "1.0"
  language: vi/en
compatibility: Works with Claude Code, Cursor, Gemini CLI, Codex CLI.
---

# BA Readiness Review — Document / Folder Assessment

**Mục tiêu:** đánh giá tài liệu đầu vào theo góc nhìn BA → Tech → Delivery, rồi xuất ra **1 file `.md`** giúp trả lời nhanh:

- tài liệu đã đủ để lên **technical design** chưa?
- tài liệu đã đủ để **code ngay end-to-end** chưa?
- điểm mạnh, điểm mơ hồ, mâu thuẫn, rủi ro lớn nhất là gì?

> Skill này là **review skill**, không phải skill viết spec chi tiết. Nếu user muốn sinh tech spec, dùng `gen-technical-spec`.

---

## Khi nào dùng

Kích hoạt skill này khi user:

- muốn **đánh giá tài liệu BA / requirement / spec**
- hỏi: *"tài liệu này đã đủ rõ chưa?"*, *"có thể lên tech design chưa?"*, *"có thể code luôn chưa?"*
- cung cấp **1 document ID**, **wiki page ID**, **Jira ticket**, **wiki title**, **1 file `.md`**, hoặc **1 folder tài liệu**
- cần một bản review có cấu trúc để gửi lại cho BA / PO / Tech Lead

### Khi KHÔNG nên dùng

- User muốn **viết mới BA spec** → dùng `ba-spec-writer`
- User muốn **sinh technical spec** → dùng `gen-technical-spec`
- User muốn **review code / PR** → dùng `code-review`

---

## Input được nhận

Skill chấp nhận một trong các dạng sau:

### A. Document ID / Remote source
- `Jira ticket key` (ví dụ `BBS-14767`)
- `Confluence page ID` / wiki URL
- `Wiki page title`

**Cách xử lý:**
- nếu là Jira / Confluence source → dùng `fetch-ba-source` để lấy tài liệu trước
- sau khi fetch xong mới bắt đầu review

### B. Local documents
- 1 file `.md`
- 1 folder chứa nhiều file `.md`
- 1 feature folder có `BA/`

---

## Output bắt buộc

Skill phải tạo **1 file markdown duy nhất**.

### Vị trí output khuyến nghị

| Input | Output file |
|---|---|
| `features/<feature>/BA/` | `features/<feature>/BA_READINESS_REVIEW.md` |
| `features/<feature>/BA/<file>.md` | `features/<feature>/<slug>_READINESS_REVIEW.md` |
| remote doc ID only | `review/BA_READINESS_REVIEW_<doc-id>.md` hoặc file cùng feature nếu xác định được |

> Nếu chưa rõ feature root, ưu tiên tạo file trong folder gần nhất của input hoặc một thư mục `review/` riêng.

---

## Framework đánh giá

### 1. Business clarity
Đánh giá mức độ rõ ràng của:
- mục tiêu nghiệp vụ
- actor / role
- scope / out-of-scope
- business rules
- acceptance criteria

### 2. UI clarity
Đánh giá mức độ rõ ràng của:
- màn hình / flow / navigation
- fields / filter / actions
- trạng thái `loading / empty / error / success`
- design resource (Figma / attachments)

### 3. Technical readiness
Đánh giá mức độ sẵn sàng để dev thiết kế và code:
- data model / entity hint
- API intent
- status/state machine
- validation rules
- permissions
- integration / third-party contract
- edge cases / failure paths

### 4. Risk level
Phân loại rủi ro:
- `Low`
- `Medium`
- `High`
- `Critical`

---

## Thang điểm gợi ý

### Business clarity
- **9–10**: scope, role, rules, AC rất rõ
- **6–8**: khá rõ nhưng còn vài điểm cần confirm
- **0–5**: thiếu nhiều rule hoặc nhiều chỗ mơ hồ

### UI clarity
- **9–10**: màn hình, field, action, state rõ + có Figma
- **6–8**: có flow cơ bản nhưng chưa đủ state/edge case
- **0–5**: UI intent chưa rõ, thiếu navigation / state

### Technical readiness
- **9–10**: đủ để lên design và giao dev rõ ràng
- **6–8**: làm design được nhưng code ngay có rủi ro
- **0–5**: chưa đủ để chốt API / DB / integration

### Readiness Labels
- `Ready` = 8–10
- `Partial` = 5–7.9
- `Not Ready` = dưới 5

---

## Quy trình thực hiện

### Step 0 — Resolve source

Xác định input là:
- `doc ID / Jira / wiki` → fetch trước
- `file local` → đọc trực tiếp
- `folder tài liệu` → đọc toàn bộ `.md` liên quan

---

### Step 1 — Nhóm tài liệu

Nếu input là folder, phân loại thành 3 nhóm:

1. **Overview / context docs**
   - roadmap, hiện trạng, định hướng, epic summary

2. **Implementation docs**
   - màn hình, flow, report, integration, mobile/app

3. **Reference docs**
   - glossary, context map, phụ lục, hình ảnh

> Không được trộn lẫn overview doc với implementation doc khi kết luận readiness.

---

### Step 2 — Đọc design resources

Với mỗi tài liệu:
- tìm `Figma link`
- nếu không có Figma → đọc ảnh attachments nếu có
- ghi nhận UI states và luồng điều hướng quan trọng

Nếu không có thiết kế, phải ghi rõ ở phần đánh giá UI clarity.

---

### Step 3 — Extract evidence

Từ tài liệu, extract các fact sau:
- actors / roles
- mục tiêu nghiệp vụ
- màn hình / flow / steps
- validation rules
- business rules
- API intent (nếu có)
- entity / data hints (nếu có)
- permissions / approval flow
- trạng thái / lifecycle
- edge cases / exception flow

Mọi nhận định phải bám vào evidence. Không được tự bịa cho “đủ ý”.

---

### Step 4 — Tìm mơ hồ và mâu thuẫn

Bắt buộc đánh dấu:
- rule chưa rõ
- field / flow / state chưa mô tả đủ
- chỗ BA nói mâu thuẫn giữa 2 tài liệu
- chỗ có Figma nhưng text BA không khớp
- chỗ chưa thể chốt design hoặc code nếu thiếu confirm

Nếu không chắc, dùng nhãn:
- `[UNCLEAR]`
- `[CONFLICT]`
- `[MISSING]`

---

### Step 5 — Chấm readiness

Đánh giá tối thiểu 5 khía cạnh:
- `Business clarity`
- `UI clarity`
- `Technical readiness`
- `Risk level`
- `Readiness for technical design`
- `Readiness for end-to-end coding`

> Khi chấm điểm, luôn kèm 1–3 lý do chính, không chỉ cho số điểm.

---

### Step 6 — Ghi ra 1 file `.md`

Output phải là **1 file markdown** với đúng cấu trúc bên dưới.

---

## Mandatory Output Template

```md
# BA Readiness Review — [Tên tài liệu / folder]

**Input:** [doc ID hoặc folder path]
**Reviewed on:** [YYYY-MM-DD]
**Scope:** [single document / document folder / feature BA folder]

## Các thành phần chính
| Thành phần | Có / Thiếu / Chưa rõ | Ghi chú |
|---|---|---|
| Business overview |  |  |
| User roles / actors |  |  |
| User flow / workflow |  |  |
| UI / màn hình |  |  |
| Business rules |  |  |
| Validation rules |  |  |
| API / integration intent |  |  |
| Data model hint |  |  |
| Permissions / approval flow |  |  |
| Edge cases / exception flow |  |  |

## Mức sẵn sàng để lên technical design
**Rating:** `Ready | Partial | Not Ready` (`x/10`)

**Lý do chính:**
- ...
- ...

## Mức sẵn sàng để code ngay end-to-end
**Rating:** `Ready | Partial | Not Ready` (`x/10`)

**Lý do chính:**
- ...
- ...

## ✅ Điểm mạnh của tài liệu
- ...
- ...
- ...

## ⚠️ Các điểm chưa rõ hoặc đang mâu thuẫn
- ...
- ...
- ...

## 🔎 Đánh giá theo góc nhìn triển khai
- **Business clarity:** `x/10` — ...
- **UI clarity:** `x/10` — ...
- **Technical readiness:** `x/10` — ...
- **Risk level:** `Low | Medium | High | Critical` — ...

## 🚨 Rủi ro lớn nhất cần chốt trước
1. ...
2. ...
3. ...

## 💡 Kết luận
[1 đoạn ngắn, thẳng, evidence-based]

## Đề xuất bước tiếp theo
1. ...
2. ...
3. ...
```

---

## Nguyên tắc viết nhận xét

### Bắt buộc
- viết ngắn, rõ, có cấu trúc
- ưu tiên **evidence-based review**
- nêu rõ cái gì **đã đủ**, cái gì **chưa đủ**, cái gì **rủi ro**
- nếu tài liệu chỉ đủ để làm **tech design** nhưng chưa đủ để code E2E, phải nói rõ điều đó

### Không được làm
- không khen/chê chung chung kiểu “ổn”, “khá tốt” mà không có lý do
- không bịa technical detail khi tài liệu không có
- không đánh đồng `có UI` = `đủ code`
- không kết luận “ready” nếu còn blocker lớn về rule / state / integration

---

## Quick Decision Logic

### Có thể lên technical design khi:
- business flow đủ rõ
- scope và actor rõ
- phần lớn rules chính đã được mô tả
- mâu thuẫn còn ít và có thể handle bằng assumptions

### Có thể code ngay end-to-end khi:
- đã rõ flow + rules + states + validations
- ít hoặc không có blocker về integration
- data / API / status lifecycle đủ rõ
- QA có thể viết test cases từ tài liệu

Nếu thiếu một trong các phần trên → thường chỉ được đánh giá là `Partial`.

---

## Ví dụ trigger phrases

Skill này nên được chọn khi user nói:
- "đánh giá tài liệu BA này"
- "review độ sẵn sàng của requirement"
- "folder docs này đủ để code chưa"
- "doc ID này có thể lên tech design chưa"
- "phân tích đầu vào và cho nhận xét"

---

## One-line Principle

**Review the input like a delivery gate: enough for design? enough for coding? what blocks the team right now?**
