# Workshop — Mổ App AI Thật

**Thời gian:** 35-45 phút  
**Hình thức:** cá nhân trước, chia sẻ theo nhóm sau  
**Output:** finding note + sketch `as-is / to-be`

Mục tiêu không phải chấm "UI đẹp hay xấu". Mục tiêu là dùng sản phẩm thật như một bài needfinding: tìm chỗ product gãy trong workflow thật, rồi viết finding đó thành quyết định product.

---

## 1. Sản phẩm được chọn

**V-App — V-AI** (Trợ lý voice/text, gợi ý theo ngữ cảnh)

---

## 2. Dùng thử: Promise vs Reality

### Product hứa gì?
- Trợ lý AI tích hợp trong hệ sinh thái Vingroup, có khả năng trả lời câu hỏi theo ngữ cảnh, hỗ trợ tìm kiếm thông tin, gợi ý dịch vụ liên quan.
- Có tính năng lưu lịch sử chat ("Lịch sử tra cứu") để user xem lại các cuộc hội thoại.
- Dẫn nguồn tham khảo (badge nguồn như `vinuni`, `moovitapp`, `onehousing`...) để tăng độ tin cậy.

### User nào được hứa sẽ được giúp?
- Sinh viên, cư dân Vinhomes cần tra cứu thông tin nội khu (ăn uống, di chuyển, tiện ích).
- Người dùng muốn hỏi về hệ thống giáo dục Vin, tìm đường, tra thông tin học tập.
- Developer / user kỹ thuật muốn nhờ AI check code.

### Kỳ vọng AI làm được task nào?
1. Trả lời câu hỏi về địa điểm ăn uống gần trường với giá cụ thể.
2. Vẽ sơ đồ/chỉ đường từ VinUni đến cổng chính Vinhomes Ocean Park.
3. Check và debug đoạn code Python.
4. Lưu lại lịch sử chat để tra cứu về sau.

### Khi dùng thật — Điểm gãy xuất hiện ở đâu?

| Test | Input thực tế | Hành vi quan sát được | Điểm gãy |
|---|---|---|---|
| TC1 | Paste đoạn code Python có template literal `{rubric}`, `{query}`, `{answer}` + yêu cầu "Check lỗi đoạn code này" | AI không nhận ra đây là code thật, đọc `{query}` như biến trống, trả lời "bạn chưa cung cấp đoạn mã Python cụ thể" | AI mất context — không phân biệt được code snippet vs prompt rỗng |
| TC2 | "Gợi ý các món ăn trưa gần trường đại học vin giá dưới 40 nghìn" | Trả về danh sách khá tốt (Bún chà Khuyên, Bếp 2 Cô Tấm, Canteen VinUni), có badge nguồn | Happy path — hoạt động tốt |
| TC3 | "Bạn có thể vẽ sơ đồ từ đại học vin đến cổng chính ocean park không" | Trả về văn bản mô tả lộ trình dạng ASCII sơ đồ, không có hình ảnh thật | AI không có khả năng render hình, nhưng vẫn nhận task "vẽ" mà không từ chối rõ ràng |
| TC4 | "Tôi muốn bạn vẽ sơ đồ cụ thể cho tôi" (follow-up) | Tự giải thích "V-AI là trợ lý dạng văn bản, không hỗ trợ vẽ hình ảnh trực tiếp" rồi vẫn tiếp tục vẽ ASCII | Correction đến muộn — lần 1 không nói giới hạn, lần 2 mới clarify |
| TC5 | Kiểm tra màn hình "Lịch sử tra cứu" sau nhiều lần chat | Hiển thị "Chưa có lịch sử chat" dù đã chat trước đó (test lúc 12:39 và 12:40) | Bug lưu lịch sử — tính năng được hứa nhưng không hoạt động |

---

## 3. Bốn Paths

### Happy Path — Khi AI đúng và tự tin
> **Query:** "Gợi ý các món ăn trưa gần trường đại học vin giá dưới 40 nghìn"

- AI trả về danh sách cụ thể, có tên quán, địa chỉ, mức giá, badge nguồn tham khảo.
- Có thêm context hữu ích: lưu ý giờ cao điểm buổi trưa thứ Tư, gợi ý đặt trước.
- User nhận được đủ thông tin để hành động ngay.

📸 *Evidence: screenshot ảnh 7 (12:10) — danh sách quán ăn dưới 40k gần VinUni*

---

### Low-confidence Path — Khi AI không chắc
> **Query:** "Hiện tại tôi có ý định học tại vin cho tôi các thông tin cụ thể"

- AI trả lời chung chung về hệ thống giáo dục Vin (VinUni + Vinschool), có badge nguồn.
- **Không hỏi lại** để clarify: học cấp nào? Tuyển sinh? Học phí? Chương trình?
- Intent quá rộng nhưng AI không nhận ra, tự chọn hướng trả lời thay vì hỏi lại.

📸 *Evidence: screenshot ảnh 6 (12:10) — câu hỏi về học tại Vin*

---

### Failure Path — Khi AI sai / không làm được
> **Query:** Paste đoạn code Python + "Check lỗi đoạn code này"

- Đoạn code chứa template literal `{rubric}`, `{query}`, `{answer}` — AI đọc các `{}` như placeholder trống.
- AI trả lời: *"hiện tại bạn chưa cung cấp đoạn mã Python cụ thể nào"* — sai hoàn toàn, code đã được gửi.
- AI không nhận diện được code trong message, không có fallback hỏi lại hay confirm.
- User không biết lỗi do đâu — do app render sai hay AI không hiểu?

📸 *Evidence: screenshot ảnh 3 & 4 (12:09) — code bị đọc như prompt rỗng*

---

### Correction Path — Khi user sửa, AI có học lại không?
> **Query:** "Tôi muốn bạn vẽ sơ đồ cụ thể cho tôi" (sau khi AI đã vẽ ASCII lần 1)

- Lần 1: AI nhận task "vẽ sơ đồ", vẽ ASCII mà không nói giới hạn.
- Lần 2 (user yêu cầu lại): AI mới clarify *"V-AI là trợ lý văn bản, không hỗ trợ vẽ hình ảnh"* — nhưng vẫn tiếp tục vẽ ASCII thêm lần nữa.
- Correction không được học lại: session mới vẫn sẽ mắc lỗi tương tự.
- Lịch sử chat không lưu → mọi correction biến mất sau session.

📸 *Evidence: screenshot ảnh 11, 12, 13, 16 (12:44–12:45) — flow vẽ sơ đồ*

---

## 4. Findings → Product Decisions

### Finding 1 — Code context bị mất

```text
Khi user gửi đoạn code Python có chứa template literal dạng {variable},
AI hiểu các {} như placeholder trống thay vì nhận ra đây là code thật,
hậu quả là AI từ chối giúp dù code đã được gửi đầy đủ, user không biết phải làm gì.
Lỗi thuộc layer: Intent + Data-tool (input parsing).
Nên sửa bằng: pre-process input để detect code block trước khi parse intent;
hoặc show lại đoạn text AI đã nhận được để user confirm.
```

### Finding 2 — Intent mơ hồ không được hỏi lại

```text
Khi user hỏi "cho tôi thông tin về học tại Vin" (intent rất rộng),
AI tự chọn một hướng trả lời mà không hỏi lại để clarify,
hậu quả là câu trả lời chung chung, không actionable với nhu cầu cụ thể của user.
Lỗi thuộc layer: Intent + UX Recovery.
Nên sửa bằng: low-confidence path — khi query ambiguous, show 2-3 options
("Bạn muốn hỏi về tuyển sinh, học phí, hay chương trình đào tạo?").
```

### Finding 3 — Capability mismatch: "vẽ sơ đồ"

```text
Khi user yêu cầu "vẽ sơ đồ",
AI nhận task và vẽ ASCII thay vì clarify ngay từ đầu rằng không hỗ trợ hình ảnh,
hậu quả là user kỳ vọng nhận được sơ đồ thật, nhận ASCII text, phải hỏi lại lần 2.
Lỗi thuộc layer: Promise + UX Recovery.
Nên sửa bằng: khi detect intent "vẽ/draw/sơ đồ/map", AI proactively clarify giới hạn
trước khi respond, đồng thời gợi ý tool phù hợp hơn (Google Maps).
```

### Finding 4 — Lịch sử chat không lưu

```text
Khi user kết thúc session và quay lại "Lịch sử tra cứu",
hệ thống hiển thị "Chưa có lịch sử chat" dù đã có nhiều cuộc hội thoại,
hậu quả là user mất toàn bộ context, không tra lại được thông tin đã hỏi.
Lỗi thuộc layer: UX Recovery + Data persistence (có thể là bug kỹ thuật).
Nên sửa bằng: fix bug lưu session; minimum viable: show 5 câu hỏi gần nhất
kể cả khi không đăng nhập.
```

---

## 5. Sketch As-is / To-be

### As-is Flow (hiện tại)

```
User nhập query
        |
        v
[V-AI nhận input] ──── input có {} ────> AI đọc như placeholder trống
        |                                         |
        | intent rõ                               v
        v                               "Bạn chưa cung cấp đoạn mã"  ← ĐIỂM GÃY 1
[AI tự chọn hướng trả lời]
        |
        | intent mơ hồ ──────────────> AI vẫn tự trả lời, không hỏi lại  ← ĐIỂM GÃY 2
        |
        v
[AI trả lời] ──── task = "vẽ sơ đồ" ──> AI vẽ ASCII, không clarify  ← ĐIỂM GÃY 3
        |
        v
[Session kết thúc]
        |
        v
[Lịch sử tra cứu] ──────────────────> "Chưa có lịch sử chat"  ← ĐIỂM GÃY 4
```

### To-be Flow (đề xuất)

```
User nhập query
        |
        v
[Pre-process input]
  - Detect code block → preserve as-is, không parse {} như placeholder
  - Detect intent ambiguity → trigger clarification flow
  - Detect unsupported task (vẽ/image) → clarify capability trước
        |
        v
[Intent rõ] ──────────────────────> AI trả lời có nguồn, có confidence indicator
        |
[Intent mơ hồ] ────────────────────> Show 2-3 option để user chọn hướng
        |
[Task ngoài capability] ───────────> Clarify ngay + gợi ý tool thay thế
        |
        v
[Session lưu tự động]
        |
        v
[Lịch sử tra cứu] ──────────────────> Hiển thị đúng, có thể tìm lại
```

---

## 6. Tự kiểm trước khi nộp

- [x] Có ít nhất 1 screenshot hoặc observation cụ thể — **16 screenshots từ session thực tế**
- [x] Có đủ 4 paths — Happy, Low-confidence, Failure, Correction
- [x] Finding được viết thành product decision, không chỉ là nhận xét
- [x] Sketch có as-is và to-be
- [x] Một câu nói rõ finding này sẽ đổi gì trong SPEC:

> **Finding quan trọng nhất:** V-AI nhận task vượt capability (vẽ sơ đồ, check code với template literal) mà không clarify sớm — cần bổ sung vào SPEC một **capability declaration layer**: AI phải detect và communicate giới hạn *trước* khi respond, không phải sau khi đã fail.
