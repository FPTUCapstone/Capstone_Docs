# TM-77 / UC-31 — Yêu cầu lưu trú và điểm xuất phát từ khách sạn

**Trạng thái:** Đặc tả nghiệp vụ TM-77 / UC-31 (Yêu cầu lưu trú & Điểm xuất phát từ khách sạn), đi kèm bộ thiết kế UI/UX & prototype tương tác Web tại [`docs/design/web-traveler-accommodation/README.md`](../docs/design/web-traveler-accommodation/README.md). Bản này hỗ trợ tự gửi hồ sơ nhận quản lý cơ sở đã có trong danh mục. Quy trình bổ sung cơ sở chưa có trong danh mục cần nghiệm thu riêng. Trong chi tiết lịch trình đã tạo, thao tác dùng khách sạn làm điểm xuất phát mở biểu mẫu lập lịch với tọa độ đã điền. Traveler tạo lịch trình từ biểu mẫu đó.

Đặc tả này thay thế phần Hotel trong `TM-77-hotel-nightly-availability-spec.md`, plan tương ứng và các brief cũ cho hai session. SRS hiện mô tả yêu cầu đặt dịch vụ thương mại, gồm Hotel, theo quy tắc đặt dịch vụ hiện hành. Bản sửa đổi cần ghi vào SRS/Jira là Hotel chuyển sang **yêu cầu liên hệ lưu trú** và có điểm vào từ Traveler Web lẫn Mobile. Nhà hàng và thuê xe tiếp tục dùng luồng UC-31 theo khung giờ hiện có.

## 1. Ai làm gì

| Vai trò | Trách nhiệm |
| --- | --- |
| Đại diện khách sạn | Dùng tài khoản TripMate của mình để gửi hồ sơ xác minh và đề nghị quản lý một cơ sở. Sau khi được duyệt, đăng và sửa bài giới thiệu của cơ sở, nhận yêu cầu lưu trú và ghi nhận phản hồi. Trên Provider Web có thể hiển thị tên vai trò là **Hotel Operator**. |
| Staff TripMate | Xác minh người đại diện và duyệt quyền quản lý. Đối chiếu cơ sở đã có để tránh hồ sơ trùng. Trường hợp cơ sở mới, duyệt hồ sơ vị trí và quyền đại diện trước khi bài được công bố. |
| Traveler | Xem cơ sở trong **Lưu trú** hoặc **Gợi ý lưu trú**, gửi yêu cầu và theo dõi phản hồi. Phòng trống, giá cuối cùng, giữ chỗ, chính sách hủy và thanh toán do Traveler thỏa thuận trực tiếp với cơ sở. Traveler có thể dùng vị trí khách sạn làm điểm xuất phát chuyến đi. |

Giữ role `ServiceProvider` và tái sử dụng quyền gắn theo từng cơ sở từ `ProviderPoiLinks`. Staff duyệt quyền trên tài khoản người đại diện đã thiết lập. Luồng tài khoản đi theo xác thực và cấp quyền hiện hành. Khi phê duyệt, Backend bảo đảm cơ sở chưa có người quản lý khác hoặc hồ sơ đi theo quy trình xử lý tranh chấp. Phạm vi đầu tiên giữ tối đa một tài khoản Provider quản lý mỗi cơ sở theo ràng buộc hiện có. Một Provider có thể được duyệt quản lý nhiều cơ sở.

## 2. Hồ sơ và bài giới thiệu khách sạn

- Tách **cơ sở** (định danh, địa chỉ, tọa độ và liên kết quyền) với **thông tin hiển thị do Provider quản lý** (tên hiển thị, mô tả, ảnh và kênh liên hệ đã có dữ liệu). Định danh `poiId` có thể dùng lại để mở thông tin, định tuyến thông báo và đặt tọa độ xuất phát.
- Với cơ sở đã có, Provider yêu cầu quyền quản lý trên đúng `poiId`. Với cơ sở mới, lấy thông tin địa chỉ và tọa độ từ hồ sơ đã duyệt. Sử dụng lại định danh và cơ chế chống tạo trùng của danh mục. Quy trình duyệt cấp quyền và dữ liệu cơ sở cần xử lý an toàn khi hai yêu cầu trùng nhau gửi đồng thời.
- Backend kiểm tra Provider đang hoạt động và có quyền đúng cơ sở trên từng thao tác xem nội bộ, đăng, sửa, ngừng công bố hoặc xử lý yêu cầu. Danh mục và gợi ý dùng thông tin công bố, với trạng thái nhận yêu cầu riêng. Một khách sạn tạm ngừng nhận yêu cầu vẫn được phân loại là cơ sở lưu trú và có thể xem thông tin công khai nếu bài còn công bố.
- Phân loại lưu trú dựa trên hồ sơ cơ sở đã được xác minh hoặc dữ liệu phân loại chuẩn đã có. Các liên kết Hotel trong dữ liệu hiện tại có thể hỗ trợ chuyển tiếp. Cơ sở có thể được phân loại và gợi ý dù chưa có hạng phòng, giá đêm hoặc lịch tồn trong TripMate. Bộ chọn tham quan và bộ lập tuyến dùng cùng quy tắc phân loại.

## 3. UC-31 dành cho yêu cầu liên hệ lưu trú

1. Traveler mở một cơ sở từ **Lưu trú** hoặc **Gợi ý lưu trú**. Cơ sở đang nhận yêu cầu hiển thị **Gửi yêu cầu lưu trú**. Traveler nhập ngày nhận/trả dự kiến theo ngày Việt Nam, số khách, tên và kênh liên hệ, kèm ghi chú tùy chọn. Có thể gửi độc lập hoặc kèm `itineraryId` của mình. Nếu mở từ lịch trình, ngày có thể được gợi ý điền trước. Traveler có thể đến sớm, về muộn hoặc cả hai.
2. Backend kiểm tra quyền Traveler, quyền sở hữu itinerary nếu có, cơ sở đang nhận yêu cầu, ngày trả sau ngày nhận và số khách hợp lệ. Lưu yêu cầu với trạng thái **Đã gửi**. Gửi thông báo cho đúng Provider và mở được chi tiết yêu cầu từ chuông. Retry cùng khóa và payload trả lại cùng yêu cầu và thông báo chỉ được ghi một lần.
3. Provider xem thông tin được chia sẻ và có thể phản hồi hoặc từ chối. Traveler theo dõi **Đã gửi**, **Cơ sở đã phản hồi** hoặc **Đã từ chối**. Trạng thái **Cơ sở đã phản hồi** ghi nhận trao đổi, còn việc có phòng và giữ phòng do hai bên xác nhận trực tiếp. Cần có dòng giải thích rõ này ở form gửi và màn hình kết quả. Chốt việc áp dụng nguyên tắc hủy/hết hạn 24 giờ từ UC-31 hiện hành trong plan trước khi đặt thời hạn trên UI.
4. Nếu yêu cầu kèm lịch trình, trình bày thông tin hoặc trạng thái liên hệ trong mục **Lưu trú**. Luồng gửi yêu cầu dùng một bản ghi liên hệ. Chọn nơi ở làm điểm xuất phát là một thao tác riêng do Traveler quyết định.

Luồng mới không thu tiền, tạo giao dịch, tính tổng tiền theo giá đêm, giữ tồn hay tính hoa hồng phòng. Tái sử dụng xác thực, liên kết cơ sở, phân quyền theo cơ sở, các mẫu retry và hạ tầng thông báo. `ServiceBooking` hiện chứa số tiền, trạng thái xác nhận, hoàn tất và hoa hồng. Đề xuất dùng bản ghi yêu cầu lưu trú riêng nếu mở rộng `ServiceBooking` sẽ buộc đưa dữ liệu giá giả vào hoặc kích hoạt các tác vụ tài chính. Giữ khả năng đọc lịch sử Hotel đã lưu trong giai đoạn cũ và kiểm tra cách ngừng tạo Hotel mới qua đường đặt tồn cũ khi cutover.

### Hợp đồng Backend đề xuất để duyệt

| Nghiệp vụ | Hợp đồng và quyền |
| --- | --- |
| Tìm cơ sở và gợi ý lưu trú | Tái sử dụng danh mục/chi tiết POI, phân trang và tìm gần khu vực. Khi cần gợi ý theo lịch trình, dùng truy vấn chỉ đọc, kiểm tra quyền itinerary và trả định danh, tên, địa chỉ, tọa độ, trạng thái nhận yêu cầu. |
| Đăng và sửa bài | Provider đã được duyệt và có liên kết cơ sở. Staff xác minh hoặc xử lý hồ sơ cấp quyền. Trả lỗi rõ khi cơ sở đã có người đại diện hoặc tài khoản thao tác ngoài phạm vi. |
| Gửi và xem yêu cầu | Endpoint Traveler lưu `poiId`, ngày dự kiến, số khách, thông tin liên hệ, ghi chú tùy chọn và `itineraryId` tùy chọn. Trả mã yêu cầu và trạng thái. Danh sách và chi tiết chỉ trả yêu cầu của Traveler đó. |
| Xem và xử lý yêu cầu | Endpoint Provider giới hạn theo cơ sở được gán. Chỉ Provider phụ trách nhận thông báo yêu cầu mới. Phản hồi/từ chối thông báo lại Traveler. API và dữ liệu thông báo định danh đúng loại yêu cầu để dẫn đến chi tiết phù hợp. |

Tên route và DTO chốt khi duyệt plan sau khi xác định điểm dùng lại. Dùng hợp đồng chung cho Traveler Web và Mobile. Mã MSG đang dùng cho đặt dịch vụ theo giờ tiếp tục theo ngữ nghĩa đó. Hợp đồng mới cần chốt key hoặc mẫu thông báo phù hợp với yêu cầu liên hệ lưu trú.

## 4. Chọn khách sạn làm điểm xuất phát

- Trên thẻ hoặc chi tiết khách sạn, Traveler có thể bấm **Dùng làm điểm xuất phát**. Điều kiện để thực hiện là cơ sở có tọa độ. Thao tác này hoạt động với cả cơ sở chưa nhận yêu cầu lưu trú. Traveler có thể đã đặt qua kênh của khách sạn.
- Với lịch trình đang lập, điền tọa độ vào `startLatitude` và `startLongitude` theo trình lập lịch hiện có. Trường **Địa điểm muốn ghé**, `mandatoryPoiIds`, ứng viên tham quan và bộ chọn thêm/đổi điểm dừng bỏ qua cơ sở lưu trú. Khi tùy chọn quay về điểm xuất phát đang bật, tuyến dùng tọa độ xuất phát hiện tại để quay về như các điểm xuất phát khác.
- Với lịch trình đã tạo, giữ lịch trình hiện tại khi khách chỉ xem thông tin hoặc gửi yêu cầu. Nếu Traveler chọn dùng khách sạn làm điểm xuất phát, dùng luồng chỉnh sửa/lập lại phù hợp và để Traveler xác nhận tính lại. Tọa độ đầu vào có thể làm thay đổi hướng tuyến, thời lượng di chuyển và gợi ý. Hàm tối ưu và các quy tắc tính tuyến giữ nguyên. Ngày nhận/trả trong yêu cầu lưu trú chỉ phục vụ trao đổi với cơ sở.

## 5. Phạm vi chuyển từ các nhánh cũ và nghiệm thu

Ưu tiên rà và giữ phần lọc cơ sở lưu trú khỏi tuyến tham quan, phần đọc chi tiết và nút mở từ mục Lưu trú nếu phù hợp. Các thay đổi dở dành cho bảng tồn theo đêm, xác nhận giữ phòng và giá ước tính nên được loại khỏi kế hoạch triển khai mới. Các nhánh cũ có sửa đổi chưa commit. Rà diff và chuyển phần dùng lại sang worktree riêng sau khi duyệt spec. Giữ lịch sử dữ liệu và giữ phạm vi làm việc trên môi trường test.

| Ca nghiệm thu | Kết quả |
| --- | --- |
| Đại diện đăng ký, Staff duyệt, đại diện đăng bài | Chỉ Provider đã được duyệt có thể quản lý cơ sở được gán. Tài khoản cơ sở B bị chặn khi sửa cơ sở A. |
| Traveler gửi yêu cầu tới A từ Web hoặc Mobile | A nhận đúng một thông báo ngay cả khi retry. Khi A phản hồi, Traveler xem đúng trạng thái. Hồ sơ yêu cầu không có giao dịch hoặc thay đổi tồn. |
| Cơ sở tạm ngừng nhận yêu cầu hoặc thiếu tọa độ | Cơ sở đã công bố vẫn thuộc Lưu trú. Trạng thái cho biết có thể gửi yêu cầu hay chọn làm điểm xuất phát. |
| Cơ sở A xuất hiện trong Lưu trú và được chọn làm điểm xuất phát | Form dùng tọa độ của A. A không có trong `mandatoryPoiIds` hoặc tập điểm tham quan. Xem hay gửi yêu cầu giữ nguyên tuyến. Tính lại chỉ chạy khi Traveler xác nhận. |
| Lịch trình liên kết thuộc Traveler khác, gửi lặp hoặc cấp quyền trùng | Kiểm tra quyền và idempotency, bảo toàn quyền quản lý và dữ liệu của từng Traveler/Provider. |
| Hồi quy | Xem được hồ sơ Hotel cũ. Luồng yêu cầu nhà hàng, thuê xe, đặt tour và thông báo tương ứng qua được các kiểm thử hiện có. |
