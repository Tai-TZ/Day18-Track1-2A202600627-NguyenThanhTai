# BÀI PHÂN TÍCH: HỆ THỐNG FEEDBACK TRONG SẢN PHẨM CURSOR

**Môn:** Day 18 - Human centered AI Design  
**Sản phẩm phân tích:** [Cursor](https://cursor.com) — AI code editor  
**Góc nhìn:** Developer sử dụng hàng ngày  
**Khung phân tích:** User Feedback ↔ System Feedback × Explicit ↔ Implicit

---

## Tóm tắt

Cursor là IDE tích hợp AI, nơi feedback không chỉ ảnh hưởng trải nghiệm mà còn quyết định **code có được ghi vào source hay không**. Bài phân tích này áp dụng khung 4 ô feedback từ slide buổi học, kết hợp đánh giá theo 3 nguyên tắc thu thập feedback hiệu quả.

---

## 1. User Feedback (Giúp sản phẩm học về người dùng)

### 1a. Explicit User Feedback — Phản hồi tường minh

Người dùng **chủ động, có ý thức** gửi tín hiệu cho Cursor biết kết quả tốt hay chưa đúng thông qua các thành phần UI được thiết kế riêng.

#### Đánh giá nhanh (Quick Ratings)


| Loại                      | Ví dụ trong Cursor                                                       |
| ------------------------- | ------------------------------------------------------------------------ |
| **Binary Ratings**        | Icon **Thumbs Up / Thumbs Down** dưới mỗi câu trả lời Chat hoặc Composer |
| **Single-action Ratings** | Nút **Regenerate** — tạo lại câu trả lời khi bản hiện tại chưa đạt       |
| **Scale / Score**         | Cursor chưa có hệ thống 5 sao; chủ yếu dùng binary (👍/👎)               |


#### Lựa chọn & gợi ý hành động (Choices & Suggested Actions)


| Loại                         | Ví dụ trong Cursor                                                    |
| ---------------------------- | --------------------------------------------------------------------- |
| **Comparisons / Selections** | Chọn model (GPT, Claude, Gemini…), chọn mode **Agent / Ask / Manual** |
| **Suggested Actions**        | Nút *Apply*, *Add to Composer*, *Run in terminal*                     |
| **Follow-up Requests**       | Câu hỏi gợi ý sẵn dựa trên ngữ cảnh code hiện tại                     |


#### Làm rõ & phản hồi sâu (Clarification & Deep Feedback)


| Loại                     | Ví dụ trong Cursor                                                             |
| ------------------------ | ------------------------------------------------------------------------------ |
| **Clarifying Questions** | Agent hỏi lại bằng câu hỏi trắc nghiệm khi yêu cầu chưa rõ                     |
| **Open Text Fields**     | Nhập prompt chi tiết; thêm **Rules** (`.cursor/rules`), **Memories**           |
| **Edits / Corrections**  | **Accept / Reject** diff khi dùng `Ctrl+K`; sửa lại prompt sau khi kết quả sai |


**Ví dụ thực tế:**

```
User: "Fix lỗi login"
AI:   [sinh code sai]
User: Reject diff → gõ lại: "Dùng JWT, không dùng session"
      ↑ Explicit Feedback dạng Correction + Open Text
```

**Điểm mạnh:** Accept/Reject diff là feedback rất rõ ràng — AI biết chính xác đoạn code nào bị từ chối trước khi ghi vào repo.

---

### 1b. Implicit User Feedback — Phản hồi ngầm định

Cursor quan sát **hành vi tự nhiên** của người dùng mà không cần bấm nút đánh giá.


| Hành vi                                  | Tín hiệu ngầm                                 |
| ---------------------------------------- | --------------------------------------------- |
| **Tab chấp nhận autocomplete**           | Gợi ý inline phù hợp → positive signal        |
| **Esc / gõ đè autocomplete**             | Gợi ý không phù hợp → negative signal         |
| **Copy code từ Chat** (không bấm Apply)  | Code có giá trị nhưng flow Apply chưa tối ưu  |
| **Accept rồi sửa tay ngay**              | Code gần đúng nhưng chưa khớp style/thói quen |
| **Accept rồi chạy test → fail**          | Code hợp lệ cú pháp nhưng logic sai           |
| **Bỏ qua kết quả, gõ prompt mới**        | Câu trả lời trước không hữu ích               |
| **Đóng tab Chat, chuyển file khác**      | Task hiện tại không đáp ứng nhu cầu           |
| **Dùng `@file` nhiều lần cho cùng file** | AI chưa tự lấy đủ context cần thiết           |


**Ví dụ thực tế:**

```
AI gợi ý: const userId = req.params.id
User: Tab (accept) → đổi thành const { userId } = req.params
      ↑ Implicit: "đúng hướng, sai convention của tôi"
```

---

## 2. System Feedback (Giúp người dùng học về sản phẩm)

### 2a. Explicit System Feedback — Phản hồi hệ thống tường minh

Hệ thống **chủ động hiển thị** trạng thái, giới hạn và lý do đưa ra gợi ý.


| Loại                       | Ví dụ trong Cursor                                                                          |
| -------------------------- | ------------------------------------------------------------------------------------------- |
| **Progress Indicator**     | *"Thinking..."*, *"Searching codebase..."*, *"Reading package.json..."*, số file đang xử lý |
| **Contextual Explanation** | Dòng `@main.ts`, `@package.json` phía trên câu trả lời — minh bạch nguồn context            |
| **Giới hạn & trạng thái**  | Cảnh báo hết quota, model đang dùng, Privacy Mode bật/tắt                                   |
| **Minh bạch thay đổi**     | Diff view (đỏ/xanh) trước khi ghi vào source code                                           |


**Ví dụ:**

```
┌─────────────────────────────────────┐
│ 🔍 Searching codebase...            │
│ 📄 Read: auth.service.ts            │
│ 📄 Read: user.controller.ts         │
│ ✍️  Generating fix...               │
└─────────────────────────────────────┘
→ User biết AI không "bịa" mà đang đọc code thật
```

---

### 2b. Implicit System Feedback — Phản hồi qua thiết kế UI

Giúp user xây dựng **mental model** về cách Cursor hoạt động mà không cần giải thích bằng chữ.


| Thiết kế                                   | Mental model hình thành                     |
| ------------------------------------------ | ------------------------------------------- |
| **Streaming text nhanh**                   | AI đang "viết real-time"                    |
| **Delay lâu trước khi stream**             | Task phức tạp, cần suy luận nhiều           |
| **Inline edit che mờ code xung quanh**     | `Ctrl+K` chỉ tác động vùng được chọn        |
| **Composer vs Chat layout khác nhau**      | Composer = multi-file; Chat = hỏi đáp nhanh |
| **Autocomplete xuất hiện mờ, Tab để nhận** | Gợi ý nhẹ, user vẫn kiểm soát               |
| **Gợi ý `@file` khi gõ `@`**               | Hệ thống muốn user chỉ rõ context           |


---

## 3. Đánh giá theo 3 nguyên tắc thu thập feedback hiệu quả

> *"Thu thập feedback đúng cách → AI học nhanh hơn, user cảm thấy được lắng nghe"*


| Nguyên tắc                         | Cursor làm tốt                                                     | Cursor còn thiếu                                                      |
| ---------------------------------- | ------------------------------------------------------------------ | --------------------------------------------------------------------- |
| **Make it easy** (1 click, inline) | Thumbs up/down ngay dưới câu trả lời; Accept/Reject ngay trên diff | Thumbs down không luôn mở hộp "vì sao?" — mất cơ hội thu feedback sâu |
| **Tạo động lực cho user**          | Rules/Memories giúp AI "nhớ" preference lâu dài                    | Không giải thích rõ feedback ảnh hưởng gì đến trải nghiệm cá nhân     |
| **Giải thích cách dùng feedback**  | Hiển thị `@context` — minh bạch nguồn dữ liệu                      | Không nói feedback có được dùng để train không, ai đọc, lưu bao lâu   |


### So sánh Do / Don't (theo slide)

```
❌ Don't:  [👍] [👎]
           → User không biết bấm để làm gì, có ai đọc không

✅ Do:     [👍 Hữu ích] [👎 Sai logic]
           "Phản hồi giúp cải thiện gợi ý code cho project này"
           → Cursor đạt mức này ở Accept/Reject diff,
             chưa đạt ở thumbs up/down
```

---

## 4. Tổng kết & đề xuất cải tiến

### Điểm mạnh

- **Explicit System Feedback** xuất sắc: `@context`, progress indicator, diff preview
- **Explicit User Feedback** mạnh ở Accept/Reject — kiểm soát chặt trước khi ghi code
- **Implicit feedback loop** phong phú: autocomplete, copy, manual edit, test fail

### Điểm cần cải thiện

1. **Nối Implicit → Explicit:** Test fail sau Accept → Cursor chủ động hỏi *"Test vừa fail, bạn muốn tôi sửa không?"*
2. **Thumbs Down sâu hơn:** Popup tag nhanh — *"Sai logic"*, *"Sai convention"*, *"Thiếu context"*
3. **Transparency:** Ghi rõ feedback được dùng như thế nào (cải thiện session vs train model)
4. **Motivation copy:** *"Đánh giá giúp Cursor gợi ý code đúng style project của bạn hơn"*

### Trade-off

Cursor ưu tiên **kiểm soát & an toàn** (diff preview, reject dễ) hơn **thu feedback nhanh** (1-click rating). Hợp lý với developer — sai code tốn kém hơn sai gợi ý du lịch — nhưng bỏ lỡ cơ hội học từ hành vi ngầm.

---

## 5. Ma trận Feedback (Tổng quan)

```
                    EXPLICIT                          IMPLICIT
              ┌─────────────────────┐         ┌─────────────────────┐
 USER         │ 👍/👎 Thumbs        │         │ Tab/Esc autocomplete│
 FEEDBACK     │ Accept/Reject diff  │         │ Copy code, sửa tay  │
              │ Rules, Memories     │         │ Bỏ qua, prompt mới  │
              └─────────────────────┘         └─────────────────────┘

              ┌─────────────────────┐         ┌─────────────────────┐
 SYSTEM       │ Progress indicator  │         │ Streaming speed     │
 FEEDBACK     │ @context files      │         │ UI layout, defaults │
              │ Diff preview        │         │ Autocomplete UX     │
              └─────────────────────┘         └─────────────────────┘
```

---

## 6. Kết luận

Cursor có hệ thống feedback **mạnh ở kiểm soát** (Accept/Reject, diff preview, `@context`) nhưng **yếu ở động lực và minh bạch** (thumbs up/down thiếu giải thích). Cải tiến lớn nhất nên là **đóng vòng Implicit → Explicit** — biến tín hiệu ngầm (test fail, sửa tay) thành prompt chủ động từ hệ thống.

---

*Nguyễn Thành Tài — Day 18 Track 1 — 2A202600627*