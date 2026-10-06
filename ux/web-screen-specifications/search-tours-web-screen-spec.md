# Web Screen Inventory

| # | Screen | Related UC | Actor | Route Suggestion |
|---|---|---|---|---|
| 23 | Search Tours | UC-24 | Guest / Traveler | `/tours` |

---

# Screen: Search Tours

## Related UC

- **UC-24 — Search Tours (`TM-70`)**
- **Single Source of Truth**:
  - `Report3_Software-Requirement-Specification.docx`, §3.5.1 Search Tours (pp. 115–118), §5.2 Constraint Requirements CR-01–CR-15 (pp. 275–276), §5.3 Messages (pp. 278–283).
  - Approved Backend Contract: `GET /api/v1/tours`.
  - Web Scope Matrix: `Capstone_FE/docs/WEB_SCOPE_MATRIX.md` (Classification: `SHARED_WEB_MOBILE`, Web Required: `Yes`).
- **Public Handoffs**:
  - From TripMate Landing Page (`/`): "Explore Tours" link in top navigation and Hero CTA.
  - From Public Top Navigation (`PublicNavigation`): Link to `/tours`.
  - To Tour Details (`/tours/[id]` for UC-26): Preserved for Phase 2; Phase 1 renders clickable cards with disabled/informative handoff until UC-26 is approved.
  - To Sign In (`/sign-in`) or Traveler Registration (`/register`): Public top navigation.

## Actor

- **Guest / Traveler**: Public and anonymous browsing supported. No login or authorization token required to search. Authenticated Travelers browse identical public results.

## Platform

- Responsive Next.js Web (Desktop $\ge$ 1024px, Tablet 768px–1023px, Mobile Web < 768px down to 360px).
- **Platform Pagination Decision**:
  - **Web**: Numbered pagination (`TourPagination`, 20 items per page, URL query parameter synchronization).
  - **Mobile (Flutter)**: Infinite scroll (`loadNextPage` at 85% scroll threshold with `tourId` deduplication) + pull-to-refresh (`RefreshIndicator`).

## Purpose

- Allow visitors and travelers to explore and search approved, publicly available tours across Central Vietnam by destination region, single departure date, and whole-VND price range, displaying verified schedule and real-time slot availability.

## Entry Conditions

- The Tour catalog service is accessible.
- Authentication is optional; guest users search identically to authenticated Travelers.

## Entry Points

- Landing Page (`/`) navigation link to `/tours`.
- Direct URL access to `/tours` (with optional query parameters).
- Top public navigation bar (`PublicNavigation`).

## Exit / Navigation

- Click logo or "Home" $\rightarrow$ Navigate to `/`.
- Click "Explore POIs" $\rightarrow$ Navigate to `/pois`.
- Click Tour Card $\rightarrow$ UC-26 Tour Details (handoff deferred to Phase 2; cards maintain accessible semantics).
- Browser Back / Forward $\rightarrow$ Restores previous query and page state from URL.

## Suggested Route

`/tours`

---

## Source Field Decision Matrix (TM-70 Owned Scope vs Out-of-Scope SRS Filters)

| Field / Feature | SRS Reference | Backend Support (`TM-70`) | Web & Mobile Implementation Decision |
|---|---|---|---|
| **destination** | §3.5.1 | Supported (`string`, max 300 chars, trimmed, literal match on `commerce.TourDestinations`) | **Owned Scope**: Text input, trimmed and normalized to Unicode NFC before enforcing length $\le$ 300 chars. |
| **departureDate** | §3.5.1 | Supported (`DateOnly` `yyyy-MM-dd`, evaluated against scheduled departures in `Asia/Ho_Chi_Minh`) | **Owned Scope**: Single departure date input. `departureDate` must be a valid YYYY-MM-DD calendar date. Past valid dates are accepted criteria and may return an empty result. |
| **minPrice** / **maxPrice** | §3.5.1, CR-08 | Supported (whole VND integer `0..9,999,999,999`, `minPrice <= maxPrice`) | **Owned Scope**: Whole-number VND inputs, client validation `0..9,999,999,999` and `minPrice <= maxPrice`. |
| **page** / **pageSize** | CR-01 | Supported (`page >= 1`, `pageSize` `1..100`, default `20`, deterministic sort `title ASC, tourId ASC`) | **Owned Scope**: Web uses numbered pagination (`pageSize = 20`); Mobile uses infinite scroll + pull-to-refresh (`pageSize = 20`). |
| **availability** | §3.5.1, BR-60 | Supported (`available`, `soldOut`, `noUpcomingSchedule`, `unknown`, `remainingSlots`, `departureAtUtc`) | **Owned Scope**: Rendered on every tour card with status badge, slot counter, and representative departure date. |
| **keyword** (title / operator) | Mentioned in wireframes | Not supported in `GET /api/v1/tours` | **Out of scope**: Omitted from search form to prevent misleading users. |
| **date range** (start / end) | Mentioned in §3.5.1 wireframe | Backend supports single `departureDate` | **Out of scope**: Single departure date picker provided; multi-day date range deferred. |
| **duration** | Mentioned in wireframes | Not a filter in `GET /api/v1/tours` (`durationDays` is output-only) | **Out of scope**: Displayed on tour cards; not exposed as a filter. |
| **category** / **theme** | Mentioned in wireframes | Not supported in `GET /api/v1/tours` | **Out of scope**: No category filter controls rendered. |
| **rating** / **reviews** | Mentioned in wireframes | Not in Tour DTO | **Out of scope**: Rating stars and review filters omitted. |
| **sort** | Mentioned in CR-01 | Fixed BE deterministic sort (`title ASC, tourId ASC`) | **Out of scope**: No sort dropdown rendered; results display in deterministic backend sort order. |
| **interest tags** | Mentioned in §3.5.1 | Not supported in `GET /api/v1/tours` | **Out of scope**: Deferred to future catalogue/recommendation expansion. |
| **recommendations** (UC-25) | UC-25 | Separate use case | **Out of scope**: No AI recommendation carousel or match score. |

---

## Main Layout Regions

### 1. Top Public Navigation Bar
- Reuses existing `PublicNavigation` component.
- Links to `/tours` via `ROUTES.tours`.
- Active styling indicates current route when on `/tours`.

### 2. Search & Filter Hero Section
- **Title & Description**: Informative heading (`Explore Authentic Local Tours`).
- **Search Form** (`TourFilterBar`):
  - **Destination Input**: Accessible label `Destination`, placeholder `Da Nang, Hoi An, Hue...`, normalized to Unicode NFC and trimmed before checking length $\le 300$ characters (no pre-normalization truncation on raw decomposed input).
  - **Departure Date Input**: Accessible label `Departure date`, date input accepting valid `YYYY-MM-DD` calendar dates (`departureDate` must be a valid YYYY-MM-DD calendar date; past valid dates are accepted criteria and may return an empty result).
  - **Price Range Inputs**:
    - `Min price (VND)`: Whole-number VND input (`0..9,999,999,999`).
    - `Max price (VND)`: Whole-number VND input (`0..9,999,999,999`).
  - **Action Buttons**:
    - **Search Button (`Search tours`)**: Primary button, submits search. Disabled while request is in-flight.
    - **Reset Button (`Reset`)**: Secondary button, resets all criteria to empty and loads default page 1.
  - **Execution Rule (CR-02)**: Search triggers **only on explicit form submission**, never on individual keystrokes or input blur.

### 3. Active Filters & Results Summary Bar
- **Results Count**: Announcements such as `Showing X–Y of Z tours` (using `aria-live="polite"`).
- **Stale Data Indicator (`TourStaleWarningBanner`)**: If a subsequent refresh, filter submission, or pagination request fails after valid results are already loaded, preserves the existing results, the `displayedCriteria` associated with those results, and pagination controls while retaining `requestedCriteria` in the editable form and displaying a non-blocking warning banner (`Results could not be refreshed. Availability may have changed.`) with a `Retry` action.

### 4. Main Results Grid
- **Responsive Layout**:
  - **Desktop ($\ge$ 1024px)**: 3-column grid (max-width 1280px).
  - **Tablet (768px – 1023px)**: 2-column grid.
  - **Mobile (< 768px)**: 1-column stacked list with 16px horizontal padding.
- **Tour Card Component** (`TourCard`):
  - **Top Banner / Badge**: Tour thumbnail (`thumbnailUrl` from TM-208/TM-209) or neutral accessible placeholder, destination tags, duration badge.
  - **Tour Title**: Clear, bold typography, max 2 lines with truncation.
  - **Operator Name**: Verified operator company name.
  - **Schedule & Departure**: Formatted departure date and time in Vietnam time (`Asia/Ho_Chi_Minh`).
  - **Availability Badge**:
    - `available`: Green badge with slot counter (`{n} slots left` / `Available`).
    - `soldOut`: Red badge (`Sold out`).
    - `noUpcomingSchedule`: Neutral gray badge (`No upcoming departures`).
    - `unknown`: Warning amber badge (`Availability unavailable`).
  - **Price Display**: Formatted base price in VND.

### 5. Numbered Pagination Controls (`TourPagination`)
- Renders numbered pages: `Previous`, `1`, `2`, `3`, `...`, `N`, `Next`.
- Summary text: `Page {page} / {totalPages} • Total {totalCount} tours`.
- Hidden when `totalPages <= 1`.
- Changing page updates URL search params and fetches the requested page.

---

## State Handling Regions

| State | Trigger / Condition | UI Representation |
|---|---|---|
| **Initial State** | Page loads without query params | Loads first 20 public tours, empty filter inputs, total count displayed. |
| **Loading State** | Initial fetch or new search submission with no prior data | Grid of 6 shimmer skeleton cards (`TourLoadingSkeleton`), submit button disabled. |
| **Loaded State** | HTTP 200 with `items.length > 0` | Render matching tour cards, total count, `displayedCriteria`, pagination. |
| **Empty State** | HTTP 200 with `totalCount === 0` | Empty illustration/icon, message **MSG64** (`No tour packages found matching your destination and dates.`), and a `Clear filters` button. |
| **Validation Error** | Client-side or URL query validation failure | Field-level `aria-invalid`, `aria-describedby`, and inline error messages under invalid inputs. Search request is NOT dispatched and normal empty state is NOT rendered. |
| **Server Error** | HTTP 500 or Network offline on initial load | Error alert box with message **MSG127** (`TripMate is temporarily unable to process your request. Please check your connection and try again.`), and an interactive `Retry` button. |
| **Stale Data Error** | Network/server error during subsequent search/refresh | Preserves previously loaded valid tour cards, `displayedCriteria`, and pagination, retains `requestedCriteria` in filter inputs, displays `TourStaleWarningBanner`, and provides a `Retry` button. |

---

## Application Messages & Copy Matrix

| Code / Key | Vietnamese Copy | English Copy (Resource Dictionary `TOUR_MESSAGES`) |
|---|---|---|
| **MSG01** | Trường này là bắt buộc. | This field is required. |
| **MSG64** | Không tìm thấy gói tour nào phù hợp với điểm đến và ngày của bạn. | No tour packages found matching your destination and dates. |
| **MSG65** | Gói tour này hiện không thể đặt chỗ. | This tour package is currently unavailable for booking. |
| **MSG127** | TripMate tạm thời không thể xử lý yêu cầu. Vui lòng kiểm tra kết nối và thử lại. | TripMate is temporarily unable to process your request. Please check your connection and try again. |
| `tourSearch.destinationTooLong` | Điểm đến không được vượt quá 300 ký tự. | Destination must not exceed 300 characters. |
| `tourSearch.invalidDepartureDate` | Ngày khởi hành không hợp lệ. | Enter a valid departure date. |
| `tourSearch.invalidPrice` | Giá phải là số nguyên VND từ 0 đến 9.999.999.999. | Enter a whole VND amount between 0 and 9,999,999,999. |
| `tourSearch.invalidPriceRange` | Giá tối đa phải lớn hơn hoặc bằng giá tối thiểu. | Maximum price must be greater than or equal to minimum price. |
| `tourSearch.staleResults` | Không thể làm mới kết quả. Tình trạng chỗ có thể đã thay đổi. | Results could not be refreshed. Availability may have changed. |
| `tourSearch.statusAvailable` | Còn {n} chỗ trống | {n} slots left |
| `tourSearch.statusSoldOut` | Hết chỗ | Sold out |
| `tourSearch.statusNoSchedule` | Chưa có lịch khởi hành | No upcoming departures |
| `tourSearch.statusUnknown` | Tình trạng chỗ chưa xác định | Availability unavailable |
