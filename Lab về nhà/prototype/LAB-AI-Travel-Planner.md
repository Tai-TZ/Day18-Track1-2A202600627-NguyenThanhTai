# LAB Day 18 — TripMind AI Travel Planner

**Học viên:** Nguyễn Thành Tài — 2A202600627  
**Lát cắt:** AI tạo và điều chỉnh lịch trình chuyến đi Đà Nẵng 3N2Đ

---

## 1. Vấn đề & người dùng

**Người dùng:** Du khách Việt Nam (25–35 tuổi) muốn lập kế hoạch chuyến đi nhanh nhưng vẫn kiểm soát được quyết định.

**Vấn đề:** Các app du lịch AI thường:
- Tạo lịch trình mà không hỏi đủ ngữ cảnh
- Không giải thích vì sao gợi ý A hơn B
- Khi sai, chỉ có nút "không hữu ích" mà không giúp user tiếp tục

**Mục tiêu thiết kế:** Giúp người dùng hiểu đúng, tin đúng mức, giữ quyền kiểm soát khi AI không hoàn hảo.

---

## 2. Flow Map — Vòng đời trải nghiệm

```
Onboarding (T0)
    ↓  Quyền dữ liệu · Bỏ qua · Rút quyền
During — Hỏi & Lập kế hoạch (T2)
    ↓  AI Ask + Act sắp route
During — Nhiều phương án (T4)
    ↓  3 lịch trình hợp lý — user chọn
After — Review lịch trình
    ↓  📊 Dữ liệu vs 💡 AI · Nháp · Hoàn tác
After — Chọn khách sạn (T3)
Failure — Lịch quá dày (T6) ──→ Recovery loop
Failure — Info lỗi thời (T7) ──→ Recovery loop
Agency — Đặt dịch vụ (T9) ──→ Act/Ask/Don't Act
    ↺ Feedback → Kết quả tốt hơn / lần sau
```

---

## 3. Kịch bản chi tiết & Design Rationale

### T0 — Onboarding [Bắt buộc]

**Bối cảnh:** User lần đầu mở TripMind, chưa biết AI có thể đặt khách sạn hay chỉ gợi ý.

| Câu hỏi rationale | Trả lời |
|-------------------|---------|
| AI biết gì? | Chưa biết gì về user |
| AI chưa biết gì? | Sở thích, ngân sách, người đi cùng |
| Giả định? | User muốn lập lịch nhanh |
| Rủi ro nếu sai? | User kỳ vọng AI tự đặt → thất vọng |
| Act/Ask/Don't? | **Don't Act** — không tự đặt gì trong onboarding |
| Feedback? | User chọn mục tiêu chuyến đi (Explicit User) |

**Thiết kế:** 3 bước ngắn — Khả năng AI → Quyền kiểm soát → Bắt đầu (chọn style chuyến đi, không form dài).

---

### T2 — Hỏi thêm trước khi lập kế hoạch [During]

**Bối cảnh:** User nói "Muốn đi Đà Nẵng" nhưng chưa nói số ngày, ngân sách, đi với ai.

| Câu hỏi rationale | Trả lời |
|-------------------|---------|
| AI biết gì? | Điểm đến = Đà Nẵng |
| AI chưa biết? | Thời gian, ngân sách, style, người đi cùng |
| Nếu Act ngay? | Lịch trình lệch nhu cầu → user reject |
| Nếu hỏi quá nhiều? | User mệt, bỏ dở |
| Act/Ask/Don't? | **Ask** — hỏi 1 câu/lần, giải thích vì sao cần |
| Explicit System? | Progress "2/4 thông tin" + "Tôi cần biết để..." |
| Implicit System? | Câu hỏi dạng chip, không form dài |

**Thiết kế:** Progressive disclosure — 4 câu hỏi quan trọng nhất, mỗi câu có lý do ngắn.

---

### T4 — Nhiều lịch trình đều hợp lý [During]

**Bối cảnh:** Cùng chuyến 3N2Đ có thể nghỉ dưỡng, khám phá, hoặc tiết kiệm.

| Câu hỏi rationale | Trả lời |
|-------------------|---------|
| AI biết gì? | Nhiều combo điểm hợp lý |
| Có thể sai? | Chỉ show 1 lịch → user tưởng tối ưu duy nhất |
| Ask | User chọn 1 trong 3 — AI không auto-pick |
| Implicit System | 3 card visual weight bằng nhau |

---

### Review lịch trình [Giai đoạn C]

**Bối cảnh:** Sau chọn phương án, user xem chi tiết trước khi khóa.

| Yếu tố | Thiết kế |
|--------|----------|
| Dữ liệu vs đề xuất | 📊 giá KS từ Booking · 💡 điểm tham quan AI gợi ý |
| Nháp vs xác nhận | Badge BẢN NHÁP → ĐÃ XÁC NHẬN |
| Kiểm soát | Chấp nhận / Từ chối / Hoàn tác |

---

### T3 — Tiêu chí khách sạn xung đột [Explainability]

**Bối cảnh:** 3 khách sạn: rẻ nhất / gần biển nhất / đánh giá cao nhất. AI ưu tiên gần biển.

| Câu hỏi rationale | Trả lời |
|-------------------|---------|
| AI biết gì? | 3 KS + giá, khoảng cách biển, rating |
| Giả định? | User ưu tiên gần biển (từ onboarding "biển") |
| Có thể sai? | User thực ra ưu tiên giá |
| Hậu quả? | Chọn KS đắt hơn dự kiến |
| Act/Ask/Don't? | **Ask** — đề xuất, user chọn |
| Bằng chứng? | Slider tiêu chí + nguồn giá (Booking.com, cập nhật 2 ngày trước) |
| Explicit User? | Điều chỉnh slider ưu tiên + thumbs + tag "Sai ngân sách" |

---

### T6 — Lịch trình quá dày [Failure & Recovery]

**Bối cảnh:** Ngày 2 có 6 điểm, di chuyển 4h, không có giờ nghỉ.

| Câu hỏi rationale | Trả lời |
|-------------------|---------|
| AI sai ở đâu? | Quá tham vọng, không tính thời gian di chuyển |
| User phát hiện? | Nhìn timeline thấy dày + cảnh báo màu vàng |
| Hậu quả? | Chuyến đi mệt, bỏ lỡ điểm |
| Recovery? | User chọn "Quá dày" → tag cụ thể → AI đề xuất 3 cách sửa |
| Feedback loop? | Explicit: chọn vấn đề + Accept đề xuất sửa |
| Implicit? | User scroll nhiều lần qua ngày 2 (dwell time) |

**Thiết kế:** Không dừng ở thumbs down — popup tag + recovery options.

---

### T7 — Thông tin lỗi thời [Failure & Recovery + Explainability]

**Bối cảnh:** AI gợi ý nhà hàng nhưng giờ mở cửa đã thay đổi (cập nhật 8 tháng trước).

| Câu hỏi rationale | Trả lời |
|-------------------|---------|
| AI biết gì? | Dữ liệu cũ từ nguồn tổng hợp |
| Không chắc? | Giờ mở cửa hiện tại |
| Hậu quả? | Đến nơi đóng cửa |
| Recovery? | Cảnh báo confidence thấp → gợi ý 2 nhà hàng thay thế gần đó |
| Explicit System? | Badge "Cập nhật 8 tháng trước · Độ tin cậy: Thấp" |

---

### T9 — Đặt dịch vụ [Agency]

**Bối cảnh:** User chọn KS Mường Thanh, muốn đặt phòng.

| Hành động | Mức | Lý do |
|-----------|-----|-------|
| Tóm tắt lựa chọn | **Act** | Rủi ro thấp, dễ kiểm tra |
| Điền form đặt phòng | **Ask** | Cần xác nhận thông tin cá nhân |
| Thanh toán | **Don't Act** | Rủi ro cao, không hoàn tác dễ |
| Hủy booking | **Don't Act** | Cần user tự làm trên nền tảng |

---

## 4. Ma trận Feedback 2×2

### Explicit User → System

| Ví dụ | Thời điểm | Mục tiêu | Phản ứng hệ thống |
|-------|-----------|----------|-------------------|
| 👍/👎 + tag "Sai ngân sách" | Sau gợi ý KS | AI biết tiêu chí sai | Điều chỉnh slider, gợi ý lại |
| Chọn "Lịch quá dày" | Sau xem itinerary | AI biết loại lỗi | Đề xuất 3 cách sửa |
| Từ chối đặt phòng | Trước thanh toán | User chưa sẵn sàng | Lưu draft, không charge |

### Implicit User → System

| Tín hiệu | Ý nghĩa có thể | Không đồng nghĩa |
|----------|----------------|------------------|
| Scroll nhiều ngày 2 | Có thể thấy dày | Cũng có thể chỉ đang xem |
| Đổi slider tiêu chí KS | Ưu tiên thay đổi | Không chắc AI sai |
| Bỏ dở form đặt | Chưa sẵn sàng | Không phải KS xấu |

**Minh bạch:** "Hành vi giúp cải thiện gợi ý cho bạn — không chia sẻ bên ngoài."

### Explicit System → User

| Ví dụ | Thời điểm |
|-------|-----------|
| "Đang tạo lịch trình... 65%" | Khi AI xử lý |
| "Vì sao gợi ý này?" + nguồn | Sau gợi ý KS |
| "Cập nhật 8 tháng trước" | Khi confidence thấp |

### Implicit System → User

| Cue UI | Mental model |
|--------|--------------|
| Timeline màu đỏ/vàng | Ngày quá dày |
| Nút "Đặt phòng" mờ hơn "Lưu" | Đặt = bước quan trọng hơn |
| Chip câu hỏi thay vì form | AI hỏi từng bước, không ép |

---

## 5. Nguyên tắc feedback áp dụng

1. **Make it easy:** 👍/👎 + tag inline, không rời màn hình
2. **Tạo động lực:** "Đánh giá giúp gợi ý phù hợp style chuyến đi của bạn"
3. **Minh bạch:** Ghi rõ feedback dùng để cải thiện gợi ý cá nhân, không train công khai

---

## 6. Rủi ro còn lại

- Implicit signal (scroll) có thể hiểu sai → chỉ dùng làm gợi ý, không tự sửa
- Dữ liệu địa điểm phụ thuộc nguồn bên thứ ba → cần hiển thị độ tin cậy
- User có thể tin AI quá mức dù đã cảnh báo → onboarding nhấn mạnh "AI gợi ý, bạn quyết định"

---

*Prototype tương tác: `prototype/index.html`*
