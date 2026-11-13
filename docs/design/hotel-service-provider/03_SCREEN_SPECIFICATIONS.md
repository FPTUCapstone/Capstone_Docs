# 03 — SCREEN SPECIFICATIONS & UI ARCHITECTURE
## TRIPMATE HOTEL & COMMERCIAL SERVICE PROVIDER WORKFLOW
### Production-Grade UI Specifications, Component Hierarchies & State Inventories

---

## 1. MOBILE TRAVELER SCREENS

---

### SCREEN M74: Commercial Services Search & List

| Metadata Field | Specification Details |
|---|---|
| **Screen ID** | `M74` (SRS Screen #74, §3.6.1) |
| **Actor & Role** | Traveler (Authenticated Active Account) |
| **Platform / Viewports** | Flutter Mobile — Android targets: 390×844 dp (Base), 360×800 dp (Small), 412×915 dp (Large) |
| **Use Case Traceability** | UC-30 (View Commercial Service) |
| **Primary Job** | Allow Travelers to discover, search, and filter verified commercial hotels in Central Vietnam with live availability cues. |

#### Visual Hierarchy & Layout Composition
```
┌────────────────────────────────────────────────────────┐
│  Status Bar (09:41)                                    │
├────────────────────────────────────────────────────────┤
│  ← Khám phá dịch vụ lưu trú                [Bell Icon] │
│  Khách sạn & Khu nghỉ dưỡng tại Miền Trung             │
├────────────────────────────────────────────────────────┤
│  [ 🔍 Tìm tên khách sạn, địa điểm (Đà Nẵng, Hội An)...]│
├────────────────────────────────────────────────────────┤
│  Category Pills: (● Khách sạn)  (Xe tự lái)  (Ẩm thực) │
│  Filters: [Tất cả địa điểm ▼] [Mức giá ▼] [Đánh giá ▼] │
├────────────────────────────────────────────────────────┤
│  Found 24 hotels in Da Nang & Quang Nam                │
│                                                        │
│  ┌──────────────────────────────────────────────────┐  │
│  │ [ Hotel Image: Hoi An Riverside Resort        ]  │  │
│  │ [ Badge: Mở đặt phòng ]       [ ★ 4.8 (128)   ]  │  │
│  │                                                  │  │
│  │ Hội An Riverside Heritage Resort                 │  │
│  │ 📍 188 Cửa Đại, Cẩm Châu, Hội An                 │  │
│  │ Từ 1.450.000 ₫ – 2.800.000 ₫ / đêm               │  │
│  │ 🏷️ Hạng phòng: Deluxe River, Superior Suite       │  │
│  │ 🟢 Còn phòng trống hôm nay (Tham khảo)           │  │
│  └──────────────────────────────────────────────────┘  │
│                                                        │
│  ┌──────────────────────────────────────────────────┐  │
│  │ [ Hotel Image: Da Nang Coastal Boutique Villa ]  │  │
│  │ [ Badge: Mở đặt phòng ]       [ ★ 4.7 (96)    ]  │  │
│  │                                                  │  │
│  │ Da Nang Beachfront Boutique Resort               │  │
│  │ 📍 Võ Nguyên Giáp, Ngũ Hành Sơn, Đà Nẵng         │  │
│  │ Từ 1.850.000 ₫ – 3.500.000 ₫ / đêm               │  │
│  │ 🏷️ Hạng phòng: Ocean King, Family Pool Villa     │  │
│  │ 🟢 Còn phòng trống hôm nay (Tham khảo)           │  │
│  └──────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────┘
```

#### Fields, Form Controls & Data Elements
1. **Search Bar:** Placeholder `Tìm kiếm khách sạn theo tên hoặc khu vực...`. Triggers query on explicit submit or clear (CR-02).
2. **Category Chip Group:** Radio behavior: `Khách sạn (Hotel)` [Active], `Thuê xe (Vehicle Rental)`, `Nhà hàng (Restaurant)`.
3. **Filter Pills:** Region (Đà Nẵng, Hội An, Huế), Price Tier (< 1 triệu, 1-2 triệu, > 2 triệu), Rating (4.5+).
4. **Hotel Card Data:**
   - Property Name (Manrope SemiBold 16pt, `#00152A`).
   - Category Badge (`Khách sạn`, `#006B5F` soft tint).
   - Address snippet with pin icon (`#64748B`, 13pt).
   - Price range formatted with currency separator (e.g. `1.450.000 ₫ – 2.800.000 ₫ / đêm`).
   - Aggregate star rating (`★ 4.8 (128 đánh giá)`).
   - Live availability badge: `Còn phòng trống (Tham khảo)` (BR-55 compliant).

#### Required States & Fallbacks
- **Initial Loading State:** Staggered shimmer skeleton cards (image box + 3 line bars) matching editorial aspect ratios (16:10).
- **Populated State:** Scrollable vertical card list with subtle divider lines and 16dp horizontal safe margins.
- **Empty State (MSG128):** Illustrated heritage lantern graphic + headline *"Không tìm thấy khách sạn phù hợp"* + supporting text *"Vui lòng thử tìm kiếm với từ khóa khác hoặc điều chỉnh bộ lọc khu vực."*
- **Network / API Failure (MSG127):** Warning icon + alert text *"TripMate is temporarily unable to process your request. Please check your connection and try again."* + `[Thử lại (Retry)]` button.

---

### SCREEN M75: Commercial Service Details & Booking

| Metadata Field | Specification Details |
|---|---|
| **Screen ID** | `M75` (SRS Screen #75, §3.6.1 & §3.6.2) |
| **Actor & Role** | Traveler (Authenticated Active Account) |
| **Platform / Viewports** | Flutter Mobile (390×844 dp base) |
| **Use Case Traceability** | UC-30 (View Details), UC-31 (Book Service) |
| **Primary Job** | Enable Travelers to examine room options, verify display-time availability, configure booking parameters, submit a booking request, and track approval outcomes. |

#### STATE A — Hotel Details View
- **Hero Image Carousel:** High-resolution photography of Hoi An heritage riverside architecture, room interiors, swimming pool, and dining courtyard with pagination dots.
- **Header Block:**
  - Category Badge: `Khách sạn (Hotel)`
  - Title: `Hội An Riverside Heritage Resort` (Bricolage Grotesque Bold 24pt)
  - Aggregate Rating: `★ 4.8 / 5.0 (128 lượt đánh giá thực tế)`
  - Location: `188 Cửa Đại, Cẩm Châu, TP. Hội An, Quảng Nam`
  - Contact: `Hotline: 0235 3922 888 | info@hoianriverside.vn`
  - Opening Hours: `Mở cửa 24/7 (Check-in: 14:00 - Check-out: 12:00)`
- **Category-Specific Room Showcase:**
  - Card 1: *Deluxe Riverside Suite* — 45m², 1 King Bed, Ban công hướng sông Thu Bồn, Bồn tắm nằm, Bao gồm ăn sáng. Giá ước tính: `1.850.000 ₫/đêm`.
  - Card 2: *Superior Garden View* — 35m², 2 Twin Beds, Hướng vườn nhiệt đới, Bao gồm ăn sáng. Giá ước tính: `1.450.000 ₫/đêm`.
  - Card 3: *Heritage Family Suite* — 65m², 2 Phòng ngủ, 4 khách, Phòng khách riêng biệt. Giá ước tính: `2.800.000 ₫/đêm`.
- **Availability Status Badge:** `🟢 Đang mở đặt dịch vụ (Tình trạng phòng tại thời điểm xem - Không phải giữ chỗ trước)` (Strict compliance with BR-55 and BR-88).
- **Persistent Bottom Action Bar:**
  - `[Xem trên bản đồ (View on Map)]` (Outlined Button, `#006B5F`).
  - `[Đặt phòng ngay (Book Service)]` (Primary Solid Button, `#006B5F`). Enabled only if POI is Active and open for booking (BR-88).

#### STATE B — Booking Request Form (Modal Bottom Sheet)
- **Header:** `Yêu cầu đặt phòng khách sạn` + Close Button.
- **Service Summary:** Selected Hotel Name + Selected Room Type thumbnail pill.
- **Input Fields & Form Validations:**
  1. **Hạng phòng (Selected Service Option):** Select Dropdown with Deluxe Riverside, Superior Garden, Heritage Family. Required.
  2. **Ngày nhận phòng (Requested Date):** Date Picker. Must be in the future (Validation: past date -> MSG76 `The date must be in the future.`).
  3. **Giờ nhận phòng dự kiến (Requested Time):** Time Picker (Defaults to 14:00).
  4. **Số lượng phòng (Quantity):** Stepper control (1, 2, 3...). Validated against available units (Validation: exceeds -> MSG71 `Only {Available_Quantity} available for the selected date.`).
  5. **Yêu cầu đặc biệt (Special Request):** Textarea (Optional, max 300 chars, e.g. *"Tầng cao, phòng không hút thuốc, nhận phòng muộn sau 18h"*).
  6. **Thông tin liên hệ (Contact Group):**
     - Họ và tên người liên hệ (`Contact Full Name`): Pre-filled from traveler profile. Required (MSG01).
     - Số điện thoại (`Contact Phone Number`): Pre-filled, validated format. Required (MSG01).
     - Email (`Contact Email`): Pre-filled, validated email format. Required (MSG01/MSG143).
- **Estimated Amount Calculation (BR-63 & BR-196):**
  - Informational Amount Card:
    - *Số đêm dự kiến:* 1 đêm x 1 phòng = `1.850.000 ₫`
    - *Thuế & phí dịch vụ:* Đã bao gồm
    - **Tổng số tiền ước tính:** `1.850.000 ₫`
    - *Chú thích bắt buộc (BR-196):* **"Số tiền ước tính mang tính tham khảo. Quý khách thanh toán trực tiếp cho khách sạn khi nhận phòng. TripMate không thu tiền và không xử lý hoàn tiền cho đơn đặt này."**
- **Action Buttons:**
  - `[Gửi yêu cầu (Submit Request)]`: Triggers modal confirmation dialog CR-05.
  - `[Hủy bỏ (Cancel)]`: Closes sheet, retains previous details view.

#### STATE C — Request Submission Result View
- **Success Graphic:** Elegant hospitality key / verified check emblem in deep teal.
- **Status Title:** `Đã gửi yêu cầu đặt dịch vụ thành công`
- **Notice Banner:** `Đơn đặt đang ở trạng thái: CHỜ XÁC NHẬN (Pending Confirmation)`
- **Request Details Card:**
  - Mã yêu cầu: `HBR-20261113-082`
  - Cơ sở lưu trú: `Hội An Riverside Heritage Resort`
  - Hạng phòng: `Deluxe Riverside Suite (1 phòng)`
  - Thời gian: `14:00, 15/12/2026`
  - Số tiền ước tính: `1.850.000 ₫ (Thanh toán tại chỗ)`
- **Operational Explanation:** *"Yêu cầu của bạn đã được chuyển đến bộ phận lễ tân khách sạn. Khách sạn sẽ phản hồi trong vòng 24 giờ. Đơn đặt chỉ có hiệu lực khi khách sạn xác nhận."*
- **Action CTA:** `[Xem trong lịch trình của tôi]` / `[Về trang chủ]`.

#### STATE D — Booking Outcome & Lifecycle States
- **D1 — Pending Confirmation:** Shows amber badge `Chờ xác nhận`. Exposes `[Hủy yêu cầu đặt phòng]` CTA. Tapping triggers CR-05 modal -> cancels with toast MSG186.
- **D2 — Confirmed:** Shows green badge `Đã xác nhận`. Displays check-in instructions, hotel hotline, map directions, and itinerary pin. Note confirms: *"Thanh toán 1.850.000 ₫ trực tiếp tại quầy lễ tân khi làm thủ tục nhận phòng."* (MSG72).
- **D3 — Rejected:** Shows red badge `Đã từ chối`. Prominently renders the Service Provider's mandatory rejection reason (BR-195), e.g.: *"Lý do từ chối: Khách sạn đã kín phòng do đoàn hội nghị vào ngày 15/12/2026. Quý khách vui lòng chọn ngày khác."* (MSG73). Offers `[Đặt lại với ngày khác]` CTA.
- **D4 — Expired:** Shows grey badge `Đã hết hạn`. Notice: *"Yêu cầu đã hết hạn sau 24 giờ do không nhận được phản hồi từ cơ sở lưu trú. Không có dịch vụ nào được giữ chỗ."* (BR-194 proposal).

---

## 2. WEB SERVICE PROVIDER WORKSPACE SCREENS

---

### WEB SHELL: Service Provider Desktop Shell

| Metadata Field | Specification Details |
|---|---|
| **Platform / Target Viewports** | Next.js Responsive Web — Desktop Primary: 1440×900 px; Laptop: 1280×800 px; Tablet: 1024×768 px |
| **Actor & Role** | Service Provider (Commercial Hotel Partner) |
| **Primary Route** | `/provider/*` (Default Landing: `/provider/bookings`) |

#### Shell Architecture & Layout
```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ [TM Logo] TripMate Partner | Khách sạn Hội An Riverside Heritage    [Bell] [Lan Tran ▼] │
├─────────────────┬──────────────────────────────────────────────────────────────────────┤
│ 🏨 DỊCH VỤ CỦA TÔI│ Booking Requests (Yêu cầu đặt dịch vụ)                                │
│ Hội An Riverside│ Quản lý và xử lý các yêu cầu đặt phòng từ khách du lịch TripMate      │
│                 ├──────────────────────────────────────────────────────────────────────┤
│ ── QUẢN LÝ ──   │ [Filter Bar: Status ▼ | Date Range: dd/MM/yyyy - dd/MM/yyyy | Search]│
│ 📋 Đơn đặt dịch │ ──────────────────────────────────────────────────────────────────── │
│    vụ (Screen103│ [ Data Table: 20 records/page (CR-01)                              ] │
│ 💳 Bảng kê hoa  │  Code | Service | Room | Date/Time | Qty | Traveler | Status | Action │
│    hồng(Screen10│  ...  | ...     | ...  | ...       | ... | ...      | ...    | View   │
│                 │ ──────────────────────────────────────────────────────────────────── │
│ ── THÔNG TIN ── │  Showing 1 - 20 of 48 requests          [ <  1  2  3  > ]            │
│ ℹ️ Hồ sơ cơ sở  └──────────────────────────────────────────────────────────────────────┘
│ 🚪 Đăng xuất    │
└─────────────────┴──────────────────────────────────────────────────────────────────────┘
```

#### Shell Elements
- **Top Bar:** TripMate brand mark, Current Hotel Identity Pill (`Hội An Riverside Heritage Resort - Active`), Notification Center (MSG205, MSG207 alerts), User Profile Popover (Staff Name, Phone, Change Password, Logout).
- **Sidebar Navigation:**
  - Group: Quản lý dịch vụ lưu trú
    - Item 1: `Yêu cầu đặt dịch vụ` (Screen W103, Route: `/provider/bookings`) — Active by default.
    - Item 2: `Bảng kê hoa hồng` (Screen W105, Route: `/provider/statements`).
  - Strict Boundary Guard: No Tour Operator links (tours, coupons, manifests) and no Web Administration links are ever rendered.

---

### SCREEN W103: Service Booking Requests

| Metadata Field | Specification Details |
|---|---|
| **Screen ID** | `W103` (SRS Screen #103, §3.6.3) |
| **Actor & Role** | Service Provider |
| **Use Case Traceability** | UC-78 (View Service Booking Requests) |
| **Primary Job** | Allow hotel operations staff to view, filter, sort, and access incoming traveler booking requests for their linked properties. |

#### Filter Area Specifications
- **Status Filter:** Select dropdown: `Tất cả trạng thái (All)` [Default], `Chờ xác nhận (Pending Confirmation)`, `Đã xác nhận (Confirmed)`, `Đã từ chối (Rejected)`, `Đã hủy (Cancelled)`, `Đã hết hạn (Expired)`, `Hoàn thành (Completed)`, `Vắng mặt (No-show)`.
- **Requested Date Range:** Start Date (`Từ ngày`) and End Date (`Đến ngày`) input fields formatted as `dd/MM/yyyy` (CR-07).
- **Keyword Input:** Text field searching Traveler Name, Request Code, or Room Option.
- **Search Trigger:** `[Tìm kiếm (Search)]` button. Applied on submit only (CR-02 / BR-52).

#### Request Data Table (Mandatory Columns per SRS §3.6.3)
1. **Mã yêu cầu (Request Code):** Monospace Bold (e.g. `HBR-20261113-082`).
2. **Dịch vụ (Service):** Property Name (e.g. `Hội An Riverside Resort`).
3. **Lựa chọn (Option):** Room type (e.g. `Deluxe Riverside Suite`).
4. **Ngày đặt (Requested Date):** `dd/MM/yyyy` (e.g. `15/12/2026`).
5. **Giờ nhận phòng (Requested Time):** `HH:mm` (e.g. `14:00`).
6. **Số lượng (Quantity):** Integer count (e.g. `1 phòng`).
7. **Tên khách hàng (Traveler Name):** Customer full name (e.g. `Nguyễn Văn An`).
8. **Trạng thái (Status):** Semantic Badge with icon and text.
9. **Thời điểm gửi (Submitted At):** Timestamp `dd/MM/yyyy HH:mm`.
10. **Thao tác (Action):** `[Xem chi tiết (View Details)]` link button.

#### Status Badges & Color Tokens
- `Chờ xác nhận (Pending Confirmation)`: Amber background (`#FEF3E2`), text (`#D97706`), clock icon.
- `Đã xác nhận (Confirmed)`: Green background (`#E6F7F0`), text (`#0E9F6E`), check icon.
- `Đã từ chối (Rejected)`: Red background (`#FDECEF`), text (`#E02D4D`), cancel icon.
- `Đã hủy (Cancelled)`: Slate background (`#EEF2F6`), text (`#64748B`), block icon.
- `Hết hạn (Expired)`: Gray background (`#F1F5F9`), text (`#94A3B8`), timer_off icon.
- `Hoàn thành (Completed)`: Teal background (`#E8F7F4`), text (`#006B5F`), verified icon.
- `Vắng mặt (No-show)`: Purple background (`#F1EBFE`), text (`#7C3AED`), person_off icon.

#### Table Sorting & Pagination Rules
- **Default Sort Order:** Pending Confirmation requests appear first; secondary sort by requested date (SRS §3.6.3 step 3).
- **Pagination (CR-01):** 20 records per page. Pager control displays total count: *"Hiển thị 1 - 20 trên tổng số 48 yêu cầu"*. Preserves filters and current page when navigating to details and back.

---

### SCREEN W104: Service Booking Request Details & Outcome

| Metadata Field | Specification Details |
|---|---|
| **Screen ID** | `W104` (SRS Screen #104, §3.6.4 & §3.6.5) |
| **Actor & Role** | Service Provider |
| **Use Case Traceability** | UC-79 (Respond to Request), UC-80 (Record Service Outcome) |
| **Primary Job** | Review complete booking context, confirm or reject eligible requests, and record final customer outcomes after the stay. |

#### SECTION A: Booking Request Details (Display & Context)
- **Top Navigation:** Breadcrumbs (`Yêu cầu đặt dịch vụ` > `HBR-20261113-082`) + Back link.
- **Header Card:**
  - Request Code: `HBR-20261113-082`
  - Current Status Badge: `Chờ xác nhận` / `Đã xác nhận` / `Hoàn thành`
  - Response Deadline Indicator: `Hạn phản hồi: Còn 18 giờ 24 phút (24h từ lúc gửi)` (BR-194 proposal).
- **Two-Column Information Grid:**
  - **Left Column (Service & Stay Parameters):**
    - Cơ sở lưu trú: `Hội An Riverside Heritage Resort`
    - Hạng phòng đã chọn: `Deluxe Riverside Suite`
    - Ngày nhận phòng: `15/12/2026`
    - Giờ nhận phòng: `14:00`
    - Số lượng: `1 phòng (2 người lớn)`
    - Số tiền ước tính: `1.850.000 ₫ (Khách thanh toán tại chỗ)`
    - Thời điểm gửi yêu cầu: `13/11/2026 14:15`
  - **Right Column (Guest Identity & Notes):**
    - Khách hàng: `Nguyễn Văn An`
    - Số điện thoại: `0912 345 678`
    - Email liên hệ: `an.nguyen@example.com`
    - Yêu cầu đặc biệt: *"Gia đình có con nhỏ, vui lòng chuẩn bị cũi em bé và phòng tầng cao yên tĩnh nếu có thể."*

#### SECTION B: Provider Decision (Pending Confirmation Only)
- Visible and actionable **only** when Status = `Chờ xác nhận (Pending Confirmation)`.
- **Actions Bar:**
  - `[Xác nhận yêu cầu (Confirm)]`:
    - Opens CR-05 confirmation dialog: *"Xác nhận tiếp nhận đơn đặt phòng HBR-20261113-082? Khách hàng sẽ nhận được thông báo xác nhận và thông tin phòng sẽ được thêm vào lịch trình của khách."*
    - On confirm: Sets status to `Confirmed`, records acting provider account and Asia/Ho_Chi_Minh timestamp, sends push MSG72 to traveler, displays toast MSG203 (`Request confirmed. The Traveler has been notified.`), and transitions view to Confirmed state.
  - `[Từ chối yêu cầu (Reject)]`:
    - Opens Rejection Dialog requiring a mandatory reason (BR-195).
    - Presets + custom textarea: *"Hết phòng trong ngày được chọn"*, *"Cơ sở đang bảo trì kỹ thuật"*, *"Khác..."*.
    - Validation: If reason is empty, triggers inline error MSG123 (`Please enter a reason.`).
    - On confirm: Sets status to `Rejected`, stores reason, notifies traveler with MSG73 + reason, displays toast MSG204 (`Request rejected. The Traveler has been notified with the reason.`).
  - `[Quay lại danh sách (Back)]`: Navigates to Screen W103, preserving filters.

#### SECTION C: Record Service Outcome (Confirmed & Date Passed Only)
- Visible **only** when Status = `Đã xác nhận (Confirmed)` AND requested service date is in the past (§3.6.5).
- If date has not passed: Displays locked state notice: *"Chức năng ghi nhận kết quả dịch vụ sẽ mở sau ngày nhận phòng (15/12/2026)."*
- **Outcome Form Controls:**
  1. **Số tiền ước tính ban đầu:** `1.850.000 ₫` (Read-only comparison).
  2. **Số tiền thực tế đã thu (Actual Amount):** Currency numeric input field in VND.
     - Validation: Required, numeric, must be > 0. If empty or invalid -> MSG143 (`The entered value is out of the allowed range or is not valid. Check the highlighted field and try again.`).
  3. **Tiền hoa hồng TripMate (10% Commission Preview):** Dynamically calculated: `= 10% x Actual Amount VND` (BR-198 proposal). E.g. `185.000 ₫`. Clearly annotated: *"Tỷ lệ hoa hồng 10% theo quy định nền tảng (Đề xuất chờ Owner phê duyệt)."*
- **Actions Bar:**
  - `[Ghi nhận hoàn thành (Mark Completed)]`:
    - Prompts CR-05 confirmation dialog with actual amount and calculated commission.
    - On confirm: Status updates to `Hoàn thành (Completed)`, actual amount and commission stored, toast MSG206 (`Outcome recorded.`) displayed, screen switches to immutable read-only state (PC-02).
  - `[Khách không đến (Mark No-show)]`:
    - Prompts CR-05 confirmation dialog: *"Xác nhận ghi nhận khách không đến cho yêu cầu này? Đơn đặt sẽ có hoa hồng bằng 0 VND."*
    - On confirm: Status updates to `Vắng mặt (No-show)`, commission set to `0 ₫`, toast MSG206 displayed, screen switches to read-only state.

---

### SCREEN W105: Commission Statements

| Metadata Field | Specification Details |
|---|---|
| **Screen ID** | `W105` (SRS Screen #105, §3.6.6) |
| **Actor & Role** | Service Provider |
| **Use Case Traceability** | UC-81 (Statement Generation), UC-82 (Pay Commission Statement) |
| **Primary Job** | View monthly commission statements issued by TripMate, review audited completed bookings, and submit bank transfer reference receipts for reconciliation. |

#### Statement Master Table Specifications
- **Header:** `Bảng kê hoa hồng đối tác` + Subtitle *"Theo dõi và thanh toán hoa hồng định kỳ hàng tháng cho các dịch vụ đã hoàn thành."*
- **Table Columns (SRS §3.6.6):**
  1. **Kỳ bảng kê (Period):** e.g. `Tháng 11/2026 (01/11/2026 - 30/11/2026)`.
  2. **Số yêu cầu hoàn thành (Completed Requests):** e.g. `24 đơn`.
  3. **Tổng doanh thu thực tế (Total Actual Amount):** e.g. `44.400.000 ₫`.
  4. **Tiền hoa hồng (Commission Amount):** e.g. `4.440.000 ₫` (10%).
  5. **Hạn thanh toán (Due Date):** e.g. `07/12/2026` (7 days from issue per BR-199 proposal).
  6. **Trạng thái (Status):** Badge: `Đã phát hành (Issued)`, `Đã báo thanh toán (Payment Reported)`, `Đã thanh toán (Paid)`, `Quá hạn (Overdue)`.
  7. **Thao tác (Action):** `[Xem chi tiết & Báo thanh toán (View Details)]`.

#### Statement Details & Payment Reporting Drawer / View
- **Summary Banner:** Statement Code `COMM-202611-0012` | Kỳ: `11/2026` | Hoa hồng: `4.440.000 ₫` | Hạn: `07/12/2026`.
- **Completed Bookings Audit Table:**
  - Expandable breakdown of all 24 completed requests (Code, Option, Guest, Checkout Date, Actual Charged, 10% Commission).
- **TripMate Official Bank Account Information (Placeholders):**
  - Ngân hàng thụ hưởng: `Ngân hàng TMCP Ngoại thương Việt Nam (Vietcombank) - CN Đà Nẵng`
  - Tên chủ tài khoản: `CONG TY CO PHAN TRIPMATE VIETNAM`
  - Số tài khoản: `0071000888999`
  - Nội dung chuyển khoản chuẩn: `COMM-202611-0012 HOIANRIVERSIDE`
- **Bank Transfer Reporting Form:**
  - Label: `Mã giao dịch ngân hàng / Số bút toán ủy nhiệm chi (Bank Transfer Reference)`
  - Input: Text input field (e.g. `VCB-20261205-982341`).
  - Validation: Cannot be empty. If empty when submitted -> MSG172 (`Please enter the bank transfer reference before confirming the return.`).
  - Button `[Báo thanh toán (Report Payment)]`:
    - On submit: Status transitions to `Đã báo thanh toán (Payment Reported)`, records reporting timestamp, displays toast MSG208 (`Payment reported. TripMate will confirm it after bank reconciliation.`).
    - Note: Statement is **not** set to Paid yet. It remains `Payment Reported` until internal Staff validates bank records in Screen W106 (PC-02).

---

## 3. SUPPORTING STAFF WEB ADMINISTRATION SCREENS

---

### SCREEN W102: Create Service Provider Account

| Metadata Field | Specification Details |
|---|---|
| **Screen ID** | `W102` (SRS Screen #102, §3.9.1.5) |
| **Actor & Role** | Staff (Operational Admin) |
| **Platform / Route** | Next.js Web Administration Console — `/admin/users/provider/new` |
| **Use Case Traceability** | UC-77 (Create Service Provider Account) |
| **Primary Job** | Provision verified commercial partner credentials and link active commercial POIs to their operational account. |

#### Layout & Form Architecture
```
┌────────────────────────────────────────────────────────────────────────┐
│  Admin Console > Quản lý người dùng > Tạo tài khoản đối tác thương mại │
│                                                                        │
│  Tạo tài khoản Đối tác dịch vụ thương mại (Service Provider)           │
│  Thiết lập tài khoản vận hành cho cơ sở lưu trú, thuê xe hoặc ẩm thực. │
├────────────────────────────────────────────────────────────────────────┤
│  THÔNG TIN DOANH NGHIỆP & ĐẠI DIỆN                                     │
│  Tên doanh nghiệp / Khách sạn (*): [ Hội An Riverside Heritage Resort]│
│  Loại hình dịch vụ (*):            [ Khách sạn (Hotel)              ▼] │
│  Họ và tên người đại diện (*):     [ Trần Thị Lan                    ] │
│  Email tài khoản (*):              [ lan.tran@hoianriverside.vn      ] │
│  Số điện thoại liên hệ (*):        [ 0905 123 456                    ] │
├────────────────────────────────────────────────────────────────────────┤
│  ĐIỂM DU LỊCH THƯƠNG MẠI LIÊN KẾT (POI)                                │
│  Chọn cơ sở thương mại đã kích hoạt (*):                               │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │ [🔍 Tìm theo tên POI hoặc mã POI...]                             │  │
│  │ ☑️ Hội An Riverside Heritage Resort (POI-DANANG-0042) - Active    │  │
│  │    📍 188 Cửa Đại, Hội An | Trạng thái: Đang hoạt động           │  │
│  └──────────────────────────────────────────────────────────────────┘  │
│  ⚠️ Một cơ sở thương mại chỉ được liên kết với tối đa 1 tài khoản (BR-192)│
├────────────────────────────────────────────────────────────────────────┤
│  [Hủy bỏ]                                    [Tạo tài khoản & Gửi MK]  │
└────────────────────────────────────────────────────────────────────────┘
```

#### Fields & Validations (SRS §3.9.1.5)
1. **Tên doanh nghiệp (`Business Name`):** Text input, required (MSG01).
2. **Loại hình dịch vụ (`Category`):** Select: `Hotel`, `Vehicle Rental`, `Restaurant`. Required.
3. **Họ và tên người đại diện (`Full Name`):** Text input, required (MSG01).
4. **Email tài khoản (`Email Address`):** Email format validated (MSG143). Uniqueness validated against user directory (Duplicate -> MSG05 `An account with this email already exists. Please sign in or use another email.`).
5. **Số điện thoại (`Phone Number`):** Vietnamese phone regex, required (MSG01).
6. **Điểm POI liên kết (`Linked Points of Interest`):** Multi-select lookup limited to `Active` commercial POIs.
   - Validation 1: At least one POI must be selected and must have status `Active` (MSG34 `This point of interest is no longer available.`).
   - Validation 2: POI must not already be linked to another Service Provider (Conflict -> MSG202 `This point of interest is already linked to another Service Provider account.`).

#### Actions & Execution
- **`[Tạo tài khoản (Create Account)]`:**
  - Generates secure random temporary password, hashes it, stores hash (BR-03).
  - Emails temporary password and first-time login link to provider. First login forces password change (BR-192).
  - Writes creation event to immutable audit log (BR-174).
  - Displays toast MSG201 (`Service Provider account created. A temporary password has been sent to the email address.`).
  - Redirects to `/admin/users`.
- **`[Hủy bỏ (Cancel)]`:** Returns to user list without persisting data.

---

### SCREEN W106: Commission Reconciliation

| Metadata Field | Specification Details |
|---|---|
| **Screen ID** | `W106` (SRS Screen #106, §3.6.7) |
| **Actor & Role** | Staff (Operational Admin) |
| **Platform / Route** | Next.js Web Administration Console — `/admin/commercial-settlements/reconciliation` |
| **Use Case Traceability** | UC-83 (Confirm Commission Receipt) |
| **Primary Job** | Audit reported bank wire payments against bank statements and record official receipt, settling commission statements. |

#### Layout & Table Architecture
- **Header:** `Đối soát thu hoa hồng dịch vụ thương mại` + Description *"Xác minh tiền hoa hồng đã ghi có vào tài khoản ngân hàng của TripMate và xác nhận bảng kê."*
- **Filter Bar:**
  - Service Provider dropdown (All or specific Hotel).
  - Period dropdown (`Tháng 11/2026`, `Tháng 10/2026`...).
  - Status dropdown: `Đã báo thanh toán (Payment Reported)` [Default], `Tất cả`, `Đã thanh toán (Paid)`, `Quá hạn (Overdue)`.
  - `[Tìm kiếm]` button.
- **Statement Grid Columns (SRS §3.6.7):**
  1. **Đối tác dịch vụ (Service Provider):** e.g. `Hội An Riverside Heritage Resort`.
  2. **Kỳ bảng kê (Period):** e.g. `11/2026`.
  3. **Tiền hoa hồng (Commission Amount):** e.g. `4.440.000 ₫`.
  4. **Hạn thanh toán (Due Date):** `dd/MM/yyyy`.
  5. **Trạng thái (Status):** Badge (`Đã báo thanh toán`).
  6. **Mã giao dịch ngân hàng (Bank Transfer Reference):** Monospace Bold (e.g. `VCB-20261205-982341`).
  7. **Thời điểm báo (Reported At):** `dd/MM/yyyy HH:mm`.
  8. **Thao tác (Action):** `[Đối soát & Xác nhận (Record Receipt)]` (Enabled only for `Payment Reported`).

#### Confirmation & Audit Execution
- Clicking `[Đối soát & Xác nhận]` opens CR-05 modal dialog:
  - *"Xác nhận đã nhận đủ số tiền hoa hồng 4.440.000 VND từ đối tác Hội An Riverside Heritage Resort theo mã giao dịch VCB-20261205-982341 vào tài khoản ngân hàng TripMate?"*
- On Confirmation:
  - Statement status updates to `Đã thanh toán (Paid)`.
  - System records Staff Account ID and timestamp in Asia/Ho_Chi_Minh (PC-01).
  - Audit log entry written according to BR-174 and CR-14.
  - If provider was locked under BR-200, system evaluates whether all overdue statements are now paid; if so, marks account eligible for unlocking under UC-48 (BR-200).
  - Displays toast MSG209 (`Receipt recorded. The statement is now Paid.`).
  - Row updates immediately to `Paid` badge; action button is replaced with read-only audit badge.
