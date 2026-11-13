# 05 — INTERACTION & MOTION SPECIFICATIONS
## TRIPMATE HOTEL & COMMERCIAL SERVICE PROVIDER WORKFLOW
### Micro-Interactions, Modal Behaviors, Form Validations & Animation Guidelines

---

## 1. MOTION PRINCIPLES & TIMING TOKENS

Motion in TripMate is **purposeful, functional, and minimal**. It exists to communicate state changes, confirm critical actions, and guide focus — never to decorate or delay user tasks.

### Core Timing & Easing Tokens

| Token | Duration | Easing Function | Application |
|---|---|---|---|
| `--motion-fast` | 150ms | `cubic-bezier(0.25, 0.46, 0.45, 0.94)` (Quad Out) | Button press feedback, checkbox toggle, hover fades |
| `--motion-normal` | 250ms | `cubic-bezier(0.16, 1, 0.3, 1)` (Cubic Out) | Dropdown menus, tooltips, tab switcher underline |
| `--motion-modal` | 300ms | `cubic-bezier(0.16, 1, 0.3, 1)` (Spring Cubic Out) | Modal dialog scale-in, slide-over drawer entrance |
| `--motion-sheet` | 350ms | `cubic-bezier(0.32, 0.72, 0, 1)` (Decelerate) | Mobile bottom-sheet upward reveal |

### Reduced Motion Mandate
```css
@media (prefers-reduced-motion: reduce) {
  *, ::before, ::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

## 2. MODAL & DIALOG BEHAVIORS (CR-05 COMPLIANCE)

Common Requirement **CR-05** mandates that every irreversible or high-impact operational action must be explicitly confirmed via an accessible modal dialog before sending the request.

```
┌──────────────────────────────────────────────────────────────┐
│  Xác nhận tiếp nhận đơn đặt phòng                  [ × ]     │
├──────────────────────────────────────────────────────────────┤
│  Bạn có chắc chắn muốn xác nhận đơn đặt phòng                │
│  HBR-20261113-082 của khách hàng Nguyễn Văn An?             │
│                                                              │
│  🏨 Hạng phòng: Deluxe Riverside Suite (1 phòng)            │
│  📅 Thời gian:  14:00, 15/12/2026                            │
│  💰 Số tiền:    1.850.000 ₫ (Khách thanh toán tại quầy)      │
│                                                              │
│  Sau khi xác nhận, khách hàng sẽ nhận được thông báo tức thì │
│  và thông tin đặt dịch vụ sẽ được thêm vào lịch trình.       │
├──────────────────────────────────────────────────────────────┤
│  [ Quay lại ]                           [ Xác nhận tiếp nhận ]│
└──────────────────────────────────────────────────────────────┘
```

### Dialog Specifications by Operational Action

| Action | Dialog Trigger | Message / Text Content | Destructive / Primary Variant | Focus Management |
|---|---|---|---|---|
| **Confirm Booking (UC-79)** | Tap `[Xác nhận yêu cầu]` on W104 | *"Xác nhận tiếp nhận đơn đặt phòng {Request_Code} của khách hàng {Traveler_Name}?"* Details summarized. | Primary Teal (`#174B4A`) | Focus defaults to `[Xác nhận tiếp nhận]` |
| **Reject Booking (UC-79)** | Tap `[Từ chối yêu cầu]` on W104 | Rejection modal requiring mandatory reason (BR-195). Textarea with inline error MSG123 if empty. | Destructive Red (`#E02D4D`) | Focus defaults to Rejection Reason textarea |
| **Cancel Pending (Traveler)** | Tap `[Hủy yêu cầu]` on M75-D | *"Bạn có chắc chắn muốn hủy yêu cầu đặt phòng này? Thao tác này không thể hoàn tác."* | Destructive Coral (`#B83D18`) | Focus defaults to `[Hủy yêu cầu]` |
| **Mark No-show (UC-80)** | Tap `[Khách không đến]` on W104 | *"Xác nhận ghi nhận khách không đến cho yêu cầu {Request_Code}? Hoa hồng phát sinh sẽ bằng 0 VND."* | Warning Copper (`#B96D4C`) | Focus defaults to `[Xác nhận khách không đến]` |
| **Mark Completed (UC-80)** | Tap `[Ghi nhận hoàn thành]` on W104 | *"Xác nhận đã thu thực tế {Actual_Amount} VND và ghi nhận hoàn thành dịch vụ? Hoa hồng TripMate sẽ tính là {Commission_Amount} VND."* | Primary Teal (`#174B4A`) | Focus defaults to `[Xác nhận]` |
| **Record Receipt (UC-83)** | Tap `[Đối soát & Xác nhận]` on W106 | *"Xác nhận đã nhận đủ số tiền hoa hồng {Amount} VND từ đối tác {Provider_Name} theo mã GD {Bank_Ref}?"* | Primary Teal (`#174B4A`) | Focus defaults to `[Xác nhận đối soát]` |

### Modal Accessibility & Keyboard Trap Rules
1. Background overlay: `rgba(0, 21, 42, 0.45)` with 4px backdrop blur.
2. Clicking outside or pressing `Escape` cancels and dismisses dialog without mutating record.
3. Focus is trapped inside modal via tab order.
4. Screen readers announce dialog title and role `role="dialog"` with `aria-modal="true"`.

---

## 3. FORM BEHAVIOR & INLINE VALIDATION (CR-03 & CR-04)

### Validation Lifecycle
1. **On Blur / Touch:** Validates format and mandatory presence immediately upon user leaving the input field.
2. **On Submit:** Executes comprehensive client-side form validation pass. If invalid:
   - Prevents network transmission.
   - Automatically scrolls to and focuses the first invalid field.
   - Renders exact SRS error message directly beneath the input field in red (`#E02D4D`, 12px) (CR-04).
3. **In-Flight State (CR-13):**
   - The submit button is immediately disabled and displays an inline spinner.
   - Accidental double-clicks are rejected.

### Validation Matrix for Hotel Flow

| Field | Error Condition | SRS Code | User-Facing Error Text |
|---|---|---|---|
| **Any required field** | Left blank on submit | **MSG01** | `This field is required.` |
| **Requested Date (M75)** | Selected date is in the past | **MSG76** | `The date must be in the future.` |
| **Requested Date/Time (M75)** | Service option has no room capacity | **MSG70** | `The selected date or time is not available. Please choose another.` |
| **Quantity (M75)** | Rooms requested exceed remaining quota | **MSG71** | `Only {Available_Quantity} available for the selected date.` |
| **Actual Amount (W104)** | Non-numeric, empty, or <= 0 | **MSG143** | `The entered value is out of the allowed range or is not valid. Check the highlighted field and try again.` |
| **Rejection Reason (W104)** | Reason textarea is whitespace/empty | **MSG123** | `Please enter a reason.` |
| **Bank Reference (W105)** | Transfer reference code is blank | **MSG172** | `Please enter the bank transfer reference before confirming the return.` |
| **Email Address (W102)** | Invalid RFC 5322 format | **MSG143** | `The entered value is out of the allowed range or is not valid. Check the highlighted field and try again.` |
| **Email Address (W102)** | Already exists in user directory | **MSG05** | `An account with this email already exists. Please sign in or use another email.` |
| **POI Selector (W102)** | POI already linked to another provider | **MSG202** | `This point of interest is already linked to another Service Provider account.` |

---

## 4. SEARCH, FILTER & PAGINATION BEHAVIORS (CR-01 & CR-02)

### Filter Interaction Rules
1. **Explicit Search Execution (CR-02 & BR-52):**
   - In Screen W103 (Requests) and Screen W106 (Reconciliation), typing into the keyword input does **not** trigger queries per keystroke.
   - Queries execute only when the user clicks `[Tìm kiếm (Search)]` or presses `Enter`.
2. **State Preservation (CR-01):**
   - Navigating from Screen W103 into Screen W104 (Details) and clicking `[Quay lại (Back)]` preserves applied filters (Status, Date Range, Keyword) and current page number.
3. **Empty Results State (CR-02):**
   - When no records match, the table body is replaced with an intentional empty state component displaying **MSG128** (`No records found matching your criteria.`), rather than a blank table.

---

## 5. TOAST NOTIFICATION LIFECYCLE (CR-06)

Toasts provide instant, non-blocking confirmation after successful state transitions.

```
┌────────────────────────────────────────────────────────┐
│  ✓  Yêu cầu đã được xác nhận. Khách hàng đã được thông │
│     báo kết quả. (MSG203)                        [ × ] │
└────────────────────────────────────────────────────────┘
```

### Toast Specifications
- **Position:** Top-right on Web Desktop (24px from top/right); Bottom-center on Mobile (24px above navigation bar).
- **Auto-Dismiss:** 5000ms duration with optional manual close `[×]`.
- **Styling:** Surface pure (`#FFFFFF`), 1px subtle border (`#DCE2DD`), deep drop shadow, semantic icon.
- **Queueing:** Maximum 2 toasts visible simultaneously; subsequent messages queue in memory.

### Toast Message Mapping
- Booking Request Submitted -> **MSG69** (*"Booking request sent. You will be notified when the provider confirms."*)
- Provider Confirmed Request -> **MSG203** (*"Request confirmed. The Traveler has been notified."*)
- Provider Rejected Request -> **MSG204** (*"Request rejected. The Traveler has been notified with the reason."*)
- Service Outcome Recorded -> **MSG206** (*"Outcome recorded."*)
- Commission Payment Reported -> **MSG208** (*"Payment reported. TripMate will confirm it after bank reconciliation."*)
- Staff Recorded Commission Receipt -> **MSG209** (*"Receipt recorded. The statement is now Paid."*)
- Traveler Cancelled Pending Request -> **MSG186** (*"Your booking request has been cancelled."*)
- Staff Created Provider Account -> **MSG201** (*"Service Provider account created. A temporary password has been sent to the email address."*)
