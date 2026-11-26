# TripMate Web — Cập nhật thiết kế Lưu trú UC-31 (Traveler Web & Hotel Operator)

Bản cập nhật thiết kế trên nền giao diện **TripMate Coastal Heritage** hiện có (tham chiếu `/itinerary/1` và `ProviderWorkspaceShell`), bao gồm **prototype tương tác xuyên suốt hai vai trò**, **12 mockup Desktop (`1440×900`)** và **3 mockup màn hình hẹp (`390×844`)**.

---

## 1. Prototype tương tác xuyên suốt (Traveler Web ↔ Hotel Operator)

Mở trực tiếp [`prototype/index.html`](prototype/index.html) trên trình duyệt để trải nghiệm trọn vẹn luồng nghiệp vụ trong **cùng một phiên dữ liệu đồng bộ trực tiếp**:

1. **Mở lịch trình (`/itinerary/1`):** Xem lộ trình 8 điểm tham quan Hội An (`28–30/11`) tách riêng khỏi khối **Lưu trú** và danh sách **Gợi ý lưu trú**.
2. **Mở chi tiết khách sạn & gửi yêu cầu trên Web:** Chọn **Phố Cổ Riverside Homestay** → điền ngày nhận/trả dự kiến (`27/11–01/12`), số khách (`4 khách`), tên, email/SĐT, ghi chú tùy chọn → kiểm tra thông điệp trao đổi trực tiếp → bấm **`Gửi yêu cầu lưu trú`** → nhận mã **`STAY-...`** với trạng thái **`Đã gửi`**.
3. **Hotel Operator xử lý trong Provider Web:** Chuyển sang cổng **`Hotel Operator`** → thử bật/tắt nhận yêu cầu tại *Hồ sơ cơ sở lưu trú* (đồng bộ trực tiếp sang Traveler `Chưa mở nhận yêu cầu`) → mở chi tiết yêu cầu của Mai → bấm **`Gửi phản hồi`** (hoặc thử **`Từ chối yêu cầu`**).
4. **Traveler xem phản hồi & chọn khách sạn làm điểm xuất phát:** Quay lại phía Traveler Web → thấy trạng thái **`Cơ sở đã phản hồi`** cùng nội dung phản hồi → bấm **`Dùng làm điểm xuất phát`** → form **Lập lịch trình** điền sẵn tên & địa chỉ khách sạn vào **`Điểm xuất phát`** → bấm **`Tạo lịch trình gợi ý`** → quay lại `/itinerary/1` thấy khách sạn xuất hiện trên bản đồ với nhãn **`Điểm xuất phát`**, còn khối **Lưu trú** và 8 điểm tham quan vẫn tách biệt rõ ràng.

> Có nút nổi **`Luồng xuyên suốt (Traveler ↔ Hotel Operator)`** ở góc dưới phải để chuyển nhanh 1 chạm tới bất kỳ bước hoặc trạng thái nào.

---

## 2. Danh mục Mockup Desktop & Màn hình hẹp (15 ảnh đã kiểm thử Playwright)

### Traveler Web — Tìm nơi ở, Gửi yêu cầu lưu trú & Theo dõi phản hồi (`1440×900`)
- [`screenshots/01-traveler-itinerary-suggestions-1440.png`](screenshots/01-traveler-itinerary-suggestions-1440.png) — Chi tiết lịch trình (`/itinerary/1`): Khối `Lưu trú` riêng + `Gợi ý lưu trú`, tách khỏi tuyến 8 điểm tham quan
- [`screenshots/02-traveler-stays-catalog-1440.png`](screenshots/02-traveler-stays-catalog-1440.png) — Trang Lưu trú riêng (`/stays`): tên, địa chỉ, ảnh nếu có (kèm thẻ chưa có ảnh), trạng thái nhận yêu cầu & nút tách biệt `Dùng làm điểm xuất phát`
- [`screenshots/03-traveler-hotel-detail-form-1440.png`](screenshots/03-traveler-hotel-detail-form-1440.png) — Chi tiết khách sạn & Biểu mẫu **`Gửi yêu cầu lưu trú`** (ngày dự kiến, số khách, liên hệ, ghi chú, thông điệp trao đổi trực tiếp; đã bỏ hạng phòng/tồn/giá/checkout/Mobile)
- [`screenshots/04-traveler-hotel-closed-1440.png`](screenshots/04-traveler-hotel-closed-1440.png) — Cơ sở tạm đóng (**`Chưa mở nhận yêu cầu`** — *Khách sạn Cố Đô Huế*): vẫn xem đầy đủ thông tin & dùng làm điểm xuất phát
- [`screenshots/05-traveler-request-sent-1440.png`](screenshots/05-traveler-request-sent-1440.png) — Kết quả sau khi gửi: Mã `STAY-20261126-0148` · Trạng thái **`Đã gửi`**
- [`screenshots/09-traveler-request-responded-1440.png`](screenshots/09-traveler-request-responded-1440.png) — Trạng thái **`Cơ sở đã phản hồi`** (hiển thị phản hồi của cơ sở + nút **`Dùng làm điểm xuất phát`**)
- [`screenshots/10-traveler-request-rejected-1440.png`](screenshots/10-traveler-request-rejected-1440.png) — Trạng thái **`Đã từ chối`** (hiển thị lý do từ chối & lối tìm nơi ở khác)

### Traveler Web — Dùng khách sạn làm Điểm xuất phát (`1440×900`)
- [`screenshots/11-traveler-planner-hotel-start-1440.png`](screenshots/11-traveler-planner-hotel-start-1440.png) — Trang Lập lịch trình: `Điểm xuất phát` điền sẵn *Phố Cổ Riverside Homestay*, `Địa điểm muốn ghé` chỉ hiển thị điểm tham quan, Traveler chủ động bấm `Tạo lịch trình gợi ý`
- [`screenshots/12-traveler-itinerary-final-start-map-1440.png`](screenshots/12-traveler-itinerary-final-start-map-1440.png) — Chi tiết lịch trình sau khi tính tuyến: khách sạn hiện trên bản đồ với nhãn **`Điểm xuất phát`**, khối `Lưu trú` (`Cơ sở đã phản hồi`) tách riêng khỏi 8 điểm tham quan

### Hotel Operator — Phần Lưu trú trong Provider Web (`1440×900`)
- [`screenshots/06-operator-stay-profile-1440.png`](screenshots/06-operator-stay-profile-1440.png) — **Hotel Operator**: Hồ sơ cơ sở đã Staff xác minh + công tắc bật/tắt nhận yêu cầu
- [`screenshots/07-operator-requests-list-1440.png`](screenshots/07-operator-requests-list-1440.png) — **Hotel Operator**: Danh sách yêu cầu lưu trú (lọc theo `Đã gửi`, `Cơ sở đã phản hồi`, `Đã từ chối`)
- [`screenshots/08-operator-request-detail-1440.png`](screenshots/08-operator-request-detail-1440.png) — **Hotel Operator**: Chi tiết yêu cầu (ngày ở dự kiến, số khách, liên hệ, ghi chú) với 2 thao tác **`Gửi phản hồi`** và **`Từ chối yêu cầu`**

### Màn hình hẹp (`390×844`)
- [`screenshots/13-narrow-traveler-detail-form-390.png`](screenshots/13-narrow-traveler-detail-form-390.png) — Màn hình hẹp `390px`: Chi tiết khách sạn & Biểu mẫu `Gửi yêu cầu lưu trú`
- [`screenshots/14-narrow-traveler-itinerary-390.png`](screenshots/14-narrow-traveler-itinerary-390.png) — Màn hình hẹp `390px`: Chi tiết lịch trình với khối `Lưu trú` tách riêng + bản đồ `Điểm xuất phát`
- [`screenshots/15-narrow-operator-detail-390.png`](screenshots/15-narrow-operator-detail-390.png) — Màn hình hẹp `390px`: **Hotel Operator** — Chi tiết yêu cầu với thanh điều hướng di động & nút `Gửi phản hồi` / `Từ chối yêu cầu`

---

## 3. Tài liệu đặc tả & nghiệp vụ

- [`../../../requirements/TM-77-UC31-hotel-stay-inquiry.md`](../../../requirements/TM-77-UC31-hotel-stay-inquiry.md) — Đặc tả nghiệp vụ TM-77 / UC-31: yêu cầu liên hệ lưu trú, phân quyền Hotel Operator / Staff và dùng khách sạn làm điểm xuất phát.
- [`01_WEB_TRAVELER_DESIGN_SPEC.md`](01_WEB_TRAVELER_DESIGN_SPEC.md) — Đặc tả chi tiết 3 phần thiết kế UI/UX, bảng so sánh điều chỉnh từ bản cũ và sơ đồ luồng xuyên suốt.
- [`prototype/index.html`](prototype/index.html) — Prototype tương tác độc lập (hỗ trợ cả chế độ trình chiếu `?mode=presentation&scenario=...`).
