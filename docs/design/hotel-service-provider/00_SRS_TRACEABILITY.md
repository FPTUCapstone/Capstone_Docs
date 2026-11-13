# 00 — SRS TRACEABILITY MATRIX
## TRIPMATE HOTEL & COMMERCIAL SERVICE PROVIDER WORKFLOW
### Web + Mobile Experience Design

---

## 1. SOURCE OF TRUTH VERIFICATION

This traceability specification maps every UI element, user action, business validation, data field, and system feedback against the official, authoritative TripMate Software Requirement Specification (SRS).

| Attribute | Canonical Authority Value |
|---|---|
| **Authoritative Document** | `Report3_Software-Requirement-Specification.docx` (Original Report 3) |
| **Official Online Source** | `https://docs.google.com/document/d/1VwPkqRyC6ZlbXBQC0ztdektt0vAyE3qt/edit` |
| **Document Date & Baseline** | Da Nang, November 2026 (Revision Log updated 12/11/2026 by Trinh Viet Hoang / Nguyen Van Linh) |
| **Prohibited Overrides** | Report 3 V2, Jira tickets, unapproved pull requests, or legacy mockups |
| **Anthropic Skill Baseline** | `frontend-design` from `anthropics/skills` (HEAD Commit SHA: `2c5958ecac7cee2b583967a2c4981d0f4a75c579`, Apache License 2.0) |

---

## 2. ACTOR & BOUNDARY DEFINITIONS (§2.1)

| Actor | Primary Platform | Scope & Authorization Boundaries | Prohibited Access |
|---|---|---|---|
| **Traveler** | Flutter Mobile | Discovers commercial services (Hotel), reviews room details, submits booking requests, tracks status, views rejection reasons, cancels pending requests, pays provider directly upon check-in. | Cannot access Web Service Provider or Staff Console. Cannot pay online via TripMate for commercial services (BR-196). |
| **Service Provider** | Next.js Web | Commercial partner operating Hotel (or Rental/Restaurant). Signs in to isolated Service Provider Workspace to view booking requests for linked POIs, confirm/reject pending requests, record service outcome (Completed/No-show), view monthly commission statements, report bank transfer references. | Cannot access Tour Operator Workspace. Cannot access Web Administration. Cannot view requests belonging to other providers (BR-193). |
| **Staff** | Next.js Web | Operational administrator in Web Administration Console. Provisions Service Provider accounts, links active commercial POIs, reconciles bank commission transfers, records commission receipt. | Cannot act as a Service Provider. Cannot confirm or reject booking requests directly. |
| **Guest** | Web & Mobile | Unauthenticated visitor. May browse landing page or published tours. | Cannot submit commercial booking requests (requires Active Traveler sign-in). |

---

## 3. SCREEN & USE CASE TRACEABILITY MATRIX

### Summary Matrix

| Screen ID | Screen Name | Actor | Platform | SRS Section | Use Case | Business Rules | Common Req. | App Messages | Compliance Status |
|---|---|---|---|---|---|---|---|---|---|
| **M74** | Commercial Services Search & List | Traveler | Mobile | §3.6.1, §3.1.2.2 | UC-30 | BR-34, BR-87, BR-88 | CR-01, CR-02, CR-12 | MSG34, MSG75, MSG127, MSG128 | **COMPLIANT** |
| **M75** | Commercial Service Details & Booking | Traveler | Mobile | §3.6.1, §3.6.2 | UC-30, UC-31 | BR-34, BR-55, BR-63, BR-87, BR-88, BR-89, BR-196 | CR-03, CR-04, CR-05, CR-06, CR-07, CR-08 | MSG01, MSG34, MSG69, MSG70, MSG71, MSG72, MSG73, MSG75, MSG76, MSG127, MSG186 | **COMPLIANT** |
| **W103** | Service Booking Requests | Service Provider | Web | §3.6.3, §3.1.2.1 | UC-78 | BR-52, BR-193 | CR-01, CR-02, CR-07, CR-11, CR-12 | MSG126, MSG127, MSG128 | **COMPLIANT** *(Sort direction AMBIGUOUS)* |
| **W104** | Service Booking Request Details & Outcome | Service Provider | Web | §3.6.4, §3.6.5 | UC-79, UC-80 | BR-89, BR-193, BR-194, BR-195, BR-196, BR-197, BR-198 | CR-03, CR-04, CR-05, CR-06, CR-07, CR-08, CR-11, CR-12 | MSG123, MSG126, MSG127, MSG133, MSG143, MSG203, MSG204, MSG206 | **COMPLIANT** *(BR-194, 197, 198 PENDING OWNER APPROVAL)* |
| **W105** | Commission Statements | Service Provider | Web | §3.6.6 | UC-82 | BR-198, BR-199, BR-200 | CR-01, CR-03, CR-04, CR-05, CR-06, CR-07, CR-08, CR-11, CR-12 | MSG126, MSG127, MSG133, MSG172, MSG208 | **COMPLIANT** *(BR-198, 199, 200 PENDING OWNER APPROVAL)* |
| **W102** | Create Service Provider Account | Staff | Web | §3.9.1.5 | UC-77 | BR-03, BR-174, BR-192 | CR-03, CR-04, CR-06, CR-11, CR-12, CR-14 | MSG01, MSG05, MSG34, MSG126, MSG127, MSG143, MSG201, MSG202 | **COMPLIANT** *(BR-192 PENDING OWNER APPROVAL)* |
| **W106** | Commission Reconciliation | Staff | Web | §3.6.7 | UC-83 | BR-174, BR-199, BR-200 | CR-01, CR-05, CR-06, CR-07, CR-08, CR-11, CR-12, CR-14 | MSG126, MSG127, MSG133, MSG209 | **COMPLIANT** |

---

## 4. DETAILED FIELD & ACTION LEVEL TRACEABILITY

### Screen M74: Commercial Services Search & List (UC-30)
- **Data Elements:** Search Query text, Category filter (Hotel selected by default in scenario), Location/Address, Price Range, Aggregate Rating, Opening Hours, Thumbnail Image, Availability Status badge.
- **Actions:**
  - Search submit (CR-02: executes on submit, not on each keystroke).
  - Select item -> navigates to Screen M75 (UC-30).
- **Validation & Messages:**
  - Empty search results -> MSG128 ("No records found matching your criteria.").
  - Inactive POI -> MSG34 ("This point of interest is no longer available.").
  - Network error -> MSG127 ("TripMate is temporarily unable to process your request. Please check your connection and try again.").

### Screen M75: Commercial Service Details & Booking (UC-30 & UC-31)
- **State A — Hotel Details:**
  - Data Elements: Image gallery, Hotel Name, Category (`Hotel`), Verified Address, Opening Hours, Phone & Email Contact, Price Range (e.g. 850.000 ₫ – 2.500.000 ₫/đêm), Aggregate Star Rating, Category-specific section (Room types: Deluxe River View, Superior King, Family Suite with amenities and capacity), Availability Indicator for selected date (BR-55: informational at display time, not a reservation).
  - Actions: `[Book Service]` (enabled only when Active and open for booking per BR-88), `[View on Map]`, `[Back]`.
- **State B — Booking Form:**
  - Required Fields: Requested Date (`Requested Date`), Requested Time (`Requested Time`), Quantity (`Quantity` - rooms/units), Selected Service Option (`Selected Service Option` - room type), Special Request (`Special Request` - optional).
  - Contact Information: Contact Full Name (`Contact Full Name`), Contact Phone Number (`Contact Phone Number`), Contact Email (`Contact Email`).
  - Pricing display: Estimated Amount (`Estimated Amount`) computed by TripMate (BR-63). Clearly labeled informational: Traveler pays hotel directly upon arrival; TripMate collects no payment and processes no refunds (BR-196).
  - Actions: `[Submit Request]` -> CR-05 Confirmation Dialog -> creates Pending Confirmation request; `[Cancel]`.
  - Validation:
    - Empty mandatory field -> MSG01 inline under field ("This field is required.").
    - Requested date in past -> MSG76 inline ("The date must be in the future.").
    - Date/time not available -> MSG70 ("The selected date or time is not available. Please choose another.").
    - Quantity exceeds availability -> MSG71 ("Only {Available_Quantity} available for the selected date.").
    - Service closed for booking -> MSG75 ("This service is not open for booking at the moment.").
- **State C — Request Submission Result:**
  - Displays: Request Code (e.g. `HBR-20261113-082`), Hotel Name, Estimated Amount, Status: `Chờ xác nhận (Pending Confirmation)`, Informational banner stating the request is transmitted to hotel staff and must be confirmed before reservation is secured. Toast MSG69 ("Booking request sent. You will be notified when the provider confirms.").
- **State D — Booking Outcome Lifecycle:**
  - `Pending Confirmation`: Can be cancelled by Traveler -> CR-05 Modal -> Cancelled + MSG186 ("Your booking request has been cancelled.").
  - `Confirmed`: Confirmed badge, arrival instructions, reflected in Traveler Itinerary timeline (PC-03), MSG72 notification.
  - `Rejected`: Rejected badge, displays Service Provider's mandatory rejection reason (BR-195), MSG73 notification.
  - `Expired`: Automatic 24-hour timeout (BR-194 proposal).

---

### Screen W103: Service Booking Requests (UC-78)
- **Workspace Navigation:** Sidebar showing Hotel Identity, Booking Requests (`Yêu cầu đặt dịch vụ` - Screen 103), Commission Statements (`Bảng kê hoa hồng` - Screen 105), User account popover.
- **Filter Area:** Status select (All, Chờ xác nhận, Đã xác nhận, Đã từ chối, Đã hủy, Hết hạn, Hoàn thành, Vắng mặt), Requested Date range (From Date - To Date), Keyword text input, `[Tìm kiếm (Search)]` button.
- **Table Columns (Mandatory SRS):**
  1. Request Code (`Mã yêu cầu`)
  2. Service (`Dịch vụ / Khách sạn`)
  3. Option (`Hạng phòng / Lựa chọn`)
  4. Requested Date (`Ngày đặt`)
  5. Requested Time (`Giờ nhận phòng`)
  6. Quantity (`Số lượng`)
  7. Traveler Name (`Tên khách hàng`)
  8. Status (`Trạng thái`)
  9. Submitted At (`Thời điểm gửi`)
  10. Row Action: `[Xem chi tiết (View Details)]`
- **Behavior & Constraints:**
  - Scoped strictly to signed-in Service Provider's linked POIs (BR-193).
  - Default ordering: Pending Confirmation requests appear first; secondary sort by requested date (SRS §3.6.3 step 3).
  - Pagination: 20 records per page by default (CR-01).
  - Empty search result: MSG128 ("No records found matching your criteria.").
  - Unauthorized: MSG126 ("You do not have permission to access this function.").

---

### Screen W104: Service Booking Request Details (UC-79 & UC-80)
- **Section A: Request Details Display:**
  - Request Code, Commercial Service Name, Selected Option (Room type), Requested Date & Time, Quantity, Estimated Amount, Current Status Badge, Response Deadline (24 hours from submission per BR-194), Traveler Full Name, Phone Number, Email Address, Special Request notes.
- **Section B: Provider Decision (Pending Confirmation only):**
  - Allowed Actions: `[Xác nhận (Confirm)]`, `[Từ chối (Reject)]`, `[Quay lại (Back)]`.
  - Confirm Flow: Opens CR-05 confirmation dialog. On confirmation, sets status to `Confirmed`, notifies Traveler (MSG72), displays toast MSG203 ("Request confirmed. The Traveler has been notified.").
  - Reject Flow: Opens rejection modal. Requires mandatory Rejection Reason (BR-195). If empty, shows MSG123 inline ("Please enter a reason."). On success, sets status to `Rejected`, notifies Traveler with reason (MSG73), displays toast MSG204 ("Request rejected. The Traveler has been notified with the reason.").
  - If request is no longer Pending Confirmation: blocked with MSG133 ("This action is not available for the current status of the record.").
- **Section C: Record Service Outcome (Confirmed & Date Passed only):**
  - Visible only when Status = `Confirmed` and service date is in the past (§3.6.5).
  - Inputs: Estimated Amount (read-only reference), Actual Amount charged input (`Số tiền thực tế`), Commission preview (10% per BR-198 proposal).
  - Actions:
    - `[Ghi nhận hoàn thành (Mark Completed)]`: Actual Amount is required, must be numeric and > 0. If invalid, shows MSG143 inline ("The entered value is out of the allowed range or is not valid."). On success, sets status to `Completed`, calculates commission, displays toast MSG206 ("Outcome recorded.").
    - `[Khách không đến (Mark No-show)]`: Opens CR-05 confirmation dialog. On success, sets status to `No-show`, commission is 0 VND (BR-198), displays toast MSG206 ("Outcome recorded.").
  - Lockout rule: Once recorded and statement issued, outcome becomes immutable (PC-02).

---

### Screen W105: Commission Statements (UC-82)
- **Table Columns:** Period (`Kỳ bảng kê` e.g. 10/2026), Completed Requests count (`Số yêu cầu hoàn thành`), Total Actual Amount (`Tổng doanh thu thực tế`), Commission Amount (`Tiền hoa hồng phải nộp`), Due Date (`Hạn nộp` - 7 days per BR-199 proposal), Status (`Trạng thái`: Issued, Payment Reported, Paid, Overdue).
- **Statement Details Panel / Modal:**
  - Breakdown table of included Completed requests (Request Code, Option, Date, Actual Amount, Commission).
  - TripMate Bank Transfer Info: Bank Name (Vietcombank), Account Holder Name (CONG TY CP TRIPMATE VIETNAM), Sample Account Number (`0071000888999`), Sample Transfer Reference Format (`COMM-{Period}-{ProviderID}`).
  - Bank Transfer Reference Input (`Mã giao dịch ngân hàng / Ủy nhiệm chi`).
  - Actions: `[Báo thanh toán (Report Payment)]` -> validates reference is non-empty (if empty -> MSG172 "Please enter the bank transfer reference before confirming the return."). On success, sets status to `Payment Reported`, displays toast MSG208 ("Payment reported. TripMate will confirm it after bank reconciliation.").
  - State behavior: Statement is NOT marked Paid immediately. It remains `Payment Reported` until Staff reconciles in W106 (PC-02).

---

### Screen W102: Create Service Provider Account (UC-77)
- **Platform:** Next.js Web Administration Console (Staff only).
- **Input Fields:** Full Name (`Họ và tên đại diện`), Email Address (`Địa chỉ email`), Phone Number (`Số điện thoại liên hệ`), Business Name (`Tên doanh nghiệp / Khách sạn`), Category (`Hotel` / `Vehicle Rental` / `Restaurant`), Linked Points of Interest selector (`Điểm du lịch thương mại liên kết`).
- **Validation Rules:**
  - Missing field -> MSG01.
  - Invalid email format -> MSG143.
  - Duplicate email -> MSG05 ("An account with this email already exists.").
  - No POI selected or POI not Active -> MSG34.
  - POI already linked to another provider -> MSG202 ("This point of interest is already linked to another Service Provider account.").
- **Actions:** `[Tạo tài khoản (Create Account)]`, `[Hủy (Cancel)]`.
- **Success:** Creates account, sends temporary password via email, forces password change on first sign-in (BR-192), writes to audit log (BR-174), displays toast MSG201 ("Service Provider account created. A temporary password has been sent to the email address.").

---

### Screen W106: Commission Reconciliation (UC-83)
- **Platform:** Next.js Web Administration Console (Staff only).
- **Filter Area:** Service Provider dropdown, Period selector, Status dropdown (All, Payment Reported, Paid, Overdue), Search.
- **Table Columns:** Service Provider, Period, Commission Amount, Due Date, Status, Bank Transfer Reference, Reported At.
- **Row Actions:** `[Đối soát & Xác nhận (Record Receipt)]`, `[Xem chi tiết (View Details)]`.
- **Behavior:**
  - Action enabled only for statements in `Payment Reported` status.
  - Opens CR-05 confirmation dialog to confirm that full commission funds have cleared the bank account.
  - On confirm: Sets status to `Paid`, records acting Staff ID and timestamp (PC-01), writes audit log entry (BR-174), displays toast MSG209 ("Receipt recorded. The statement is now Paid.").
  - If provider was locked under BR-200 and this pays their last overdue statement, account becomes eligible for unlocking (BR-200).

---

## 5. COMPLETE MESSAGE DICTIONARY

Every application message used across the Hotel & Commercial Service flow is verified from Section 5.3 (Table 20):

| Code | Type | Authoritative Context | Exact Message Content | Used In Screens |
|---|---|---|---|---|
| **MSG01** | Red inline | Input-required fields are empty in form | `This field is required.` | M75, W102, W104 |
| **MSG05** | Inline error | Email address is already registered | `An account with this email already exists. Please sign in or use another email.` | W102 |
| **MSG34** | Inline error | POI does not exist or is no longer Active | `This point of interest is no longer available.` | M74, M75, W102 |
| **MSG69** | Toast | Commercial service booking request created | `Booking request sent. You will be notified when the provider confirms.` | M75 |
| **MSG70** | Inline error | Requested date or time is not available | `The selected date or time is not available. Please choose another.` | M75 |
| **MSG71** | Inline error | Requested quantity exceeds available quantity | `Only {Available_Quantity} available for the selected date.` | M75 |
| **MSG72** | Push / Notif | Provider confirmed service booking | `Your booking for {Service_Name} has been confirmed.` | M75, W104 |
| **MSG73** | Push / Notif | Provider rejected service booking | `Your booking request for {Service_Name} was declined by the provider.` | M75, W104 |
| **MSG75** | Inline error | Commercial service not open for booking | `This service is not open for booking at the moment.` | M74, M75 |
| **MSG76** | Red inline | Requested date is in the past | `The date must be in the future.` | M75 |
| **MSG123** | Red inline | Mandatory reason is empty | `Please enter a reason.` | W104 |
| **MSG126** | Inline / Alert | Attempt to access screen/action outside assigned role | `You do not have permission to access this function.` | W102, W103, W104, W105, W106 |
| **MSG127** | Alert banner | Generic network or server failure | `TripMate is temporarily unable to process your request. Please check your connection and try again.` | All screens |
| **MSG128** | Inline empty | Generic list/search has no results | `No records found matching your criteria.` | M74, W103, W106 |
| **MSG133** | Inline error | Action is not allowed in current status | `This action is not available for the current status of the record.` | W104, W105, W106 |
| **MSG143** | Inline error | Invalid value or format | `The entered value is out of the allowed range or is not valid. Check the highlighted field and try again.` | M75, W102, W104 |
| **MSG172** | Inline error | Return / transfer reference is empty | `Please enter the bank transfer reference before confirming the return.` | W105 |
| **MSG186** | Toast | Commercial service request cancelled by Traveler | `Your booking request has been cancelled.` | M75 |
| **MSG199** | Inline banner | Account locked under BR-189/BR-200 with overdue debt | `This account cannot be unlocked until its outstanding amount of {Debt_Amount} VND is fully paid.` | W102 / Admin |
| **MSG201** | Toast | Service Provider account created (UC-77) | `Service Provider account created. A temporary password has been sent to the email address.` | W102 |
| **MSG202** | Inline error | POI already linked to another Service Provider | `This point of interest is already linked to another Service Provider account.` | W102 |
| **MSG203** | Toast | Request confirmed by Service Provider (UC-79) | `Request confirmed. The Traveler has been notified.` | W104 |
| **MSG204** | Toast | Request rejected by Service Provider (UC-79) | `Request rejected. The Traveler has been notified with the reason.` | W104 |
| **MSG205** | Notification | New service booking request received | `New booking request {Request_Code} for {Service_Name} on {Date}. Please respond within 24 hours.` | W103, W104 |
| **MSG206** | Toast | Service outcome recorded (UC-80) | `Outcome recorded.` | W104 |
| **MSG207** | Notification | Commission statement issued or overdue (UC-81) | `Commission statement for {Period}: {Commission_Amount} VND, due on {Due_Date}. Please transfer it to TripMate and report the payment.` | W105 |
| **MSG208** | Toast | Commission payment reported (UC-82) | `Payment reported. TripMate will confirm it after bank reconciliation.` | W105 |
| **MSG209** | Toast | Commission receipt recorded by Staff (UC-83) | `Receipt recorded. The statement is now Paid.` | W106 |
| **MSG210** | Notification | Service Provider account locked for unpaid statement | `The account has been locked because the commission statement for {Period} is unpaid 15 days after its due date. Please pay it to have the account unlocked.` | W103, W105 |

---

## 6. COMPLIANCE & GOVERNANCE SUMMARY

- **Compliant Screens:** 7 of 7 required screens fully represented.
- **Pending Approvals Identified:** BR-192, BR-194, BR-197, BR-198, BR-199, BR-200 documented as pending Owner confirmation in Revision History and tracked in `06_OPEN_DECISIONS.md`.
- **Ambiguities Identified:** Default sort direction on W103 (requested date ascending vs descending) explicitly flagged.
- **Strict Scope Boundaries:** No backend endpoints, API contracts, or production business logic modified. Isolated high-fidelity UI/UX design and interactive prototype only.
