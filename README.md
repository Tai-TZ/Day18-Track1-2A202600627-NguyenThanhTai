# Day 18 — Human-Centered AI Design Lab

**Học viên:** Nguyễn Thành Tài — `2A202600627`  
**Track:** 1  
**Đề bài:** AI Travel Planner — **TripMind AI**  
**Lát cắt:** AI tạo và điều chỉnh lịch trình chuyến đi **Đà Nẵng 3N2Đ**

---

## Dành cho giảng viên — mở prototype

Đầu ra chính của bài là **một file HTML tương tác** (mockup có thể bấm), không cần cài đặt hay API thật.

| Cách mở | Liên kết |
|---------|----------|
| **Prototype (khuyến nghị)** | [**Mở `index.html`**](Lab%20v%E1%BB%81%20nh%C3%A0/prototype/index.html) |
| Tài liệu thiết kế (tham khảo) | [`LAB-AI-Travel-Planner.md`](Lab%20v%E1%BB%81%20nh%C3%A0/prototype/LAB-AI-Travel-Planner.md) |

**Nếu link trên GitHub chỉ hiện mã nguồn:** tải repo về, mở file `Lab về nhà/prototype/index.html` bằng trình duyệt (Chrome / Edge).

**Sau khi bật GitHub Pages** (nếu có), prototype công khai tại:

`https://tai-tz.github.io/Day18-Track1-2A202600627-NguyenThanhTai/Lab%20v%E1%BB%81%20nh%C3%A0/prototype/index.html`

---

## Bài nộp gồm gì?

Trong prototype (`index.html`), sidebar trái dẫn tới đủ các phần theo cấu trúc lab (mục 16 PDF):

| Mục | Nội dung |
|-----|----------|
| **00 — Flow Map** | Phạm vi tính năng và vòng đời A → B → C → D |
| **01 — Onboarding** | T0: kỳ vọng AI, quyền dữ liệu, bỏ qua |
| **02 — Luồng chính** | T2 (Ask), T4 (nhiều phương án) |
| **03 — Sau hành động** | Review (dữ liệu vs suy luận, nháp, hoàn tác), T3, T9 |
| **04 — Failure & Recovery** | T6 (lịch quá dày), T7 (thông tin lỗi thời) |
| **05 — Feedback 2×2** | Đủ 4 ô: User/System × Explicit/Implicit + rationale |
| **06 — Design Rationale** | Tổng hợp quyết định thiết kế |
| **07 — Demo 5 phút** | Lộ trình trình bày theo thời lượng |

Mỗi màn hình chính có **thẻ design rationale** bên cạnh (AI biết/chưa biết, rủi ro, Act/Ask/Don't Act, recovery).

---

## Demo path (~5 phút)

Mở [**`index.html`**](Lab%20v%E1%BB%81%20nh%C3%A0/prototype/index.html) → chọn **「07 — Demo 5 phút」** trên sidebar, hoặc đi theo thứ tự:

1. **Flow Map** — người dùng, vấn đề, lát cắt  
2. **T0** — onboarding  
3. **T2** — AI hỏi thêm (Ask) trước khi lập kế hoạch  
4. **T4** — nhiều lịch trình hợp lý, user chọn  
5. **Review** — phân biệt dữ liệu / suy luận AI, nháp, chấp nhận / hoàn tác  
6. **T3** — explainability khi tiêu chí khách sạn xung đột  
7. **T6** — lịch quá dày: phát hiện → sửa → so sánh trước/sau  
8. **T7** — thông tin lỗi thời: độ tin, thay thế  
9. **Feedback 2×2** — bốn loại tín hiệu hai chiều  
10. **T9** — Act / Ask / Don't Act khi đặt dịch vụ  

---

## Kịch bản & đối chiếu yêu cầu tối thiểu

| ID | Kịch bản | Giai đoạn |
|----|----------|-----------|
| **T0** | Onboarding + quyền dữ liệu + bỏ qua | A |
| **T2** | Hỏi thêm trước lập kế hoạch (Ask) | B |
| **T4** | Nhiều phương án lịch trình hợp lý | B |
| **Review** | Xem lại kết quả · Dữ liệu vs AI · Nháp · Hoàn tác | C |
| **T3** | Tiêu chí KS xung đột + explainability | C |
| **T6** | Lịch quá dày + so sánh trước/sau + recovery | D |
| **T7** | Thông tin lỗi thời + recovery | D |
| **T9** | Đặt dịch vụ — Act / Ask / Don't Act | C |

**Tổng:** 1 onboarding + 7 kịch bản (yêu cầu tối thiểu: 4 kịch bản ngoài onboarding).

Checklist mục 19 PDF: onboarding ✅ · ≥4 kịch bản ✅ · vòng đời A→B→C→D ✅ · Act/Ask/Don't Act ✅ · feedback + recovery ✅ · ma trận 2×2 ✅ · lớp giải thích ✅ · rationale cạnh flow ✅

---

## Cấu trúc repository

```
Day18-Track1-2A202600627-NguyenThanhTai/
├── README.md                          ← file này
├── Lab về nhà/
│   └── prototype/
│       ├── index.html                 ← prototype chính (nộp bài)
│       └── LAB-AI-Travel-Planner.md   ← tài liệu thiết kế
└── Lab cá nhân trên lớp/              ← bài tập trên lớp (không thuộc lab về nhà)
```

---

*VinUni AI20k · Batch 02 · Day 18 — Human-Centered AI Design*
