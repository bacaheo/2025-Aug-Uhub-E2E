## Mô Tả Chi Tiết Từng Bước Quy Trình Mới

### B1. Gift Planning (Lập Kế Hoạch Quà cho Campaign)

#### 1. Customer Alignment (Gift Recall Alignment) - KA
- **Mục đích**: KA thống nhất với khách hàng về chương trình khuyến mãi và quy trình thu hồi
- **Đặc điểm**: Quy trình internal Unilever, không số hóa ở giai đoạn này
- **Nội dung chính**:
  - Thỏa thuận mechanics campaign với khách hàng
  - **CẢI TIẾN MỚI**: Thống nhất rõ ràng về Gift Recall Alignment từ đầu
  - Xác định điều kiện và thời gian thu hồi quà thừa

#### 2. Setup Gift trên UGMS - BU
- **Quy trình**: BU cập nhật thông tin Setup Gift lên UGMS
- **Dữ liệu nhập**:
  - **GiftCode**: Mã định danh quà tặng
  - **Scheme**: Thông tin mechanics (Start, End, Mechanics, Quantity)
  - **Customer**: Thông tin khách hàng
  - **Ship_to**: Điểm giao hàng

#### 3. Sync UGMS → UHub (Integration) - Tự động
- **TÍNH NĂNG MỚI**: UGMS tự động đẩy thông tin cho UHub qua API
- **Dữ liệu đồng bộ**:
  - GiftCode, Scheme details, Customer info, Ship_to locations
- **Lợi ích**: Loại bỏ thao tác thủ công, đảm bảo tính chính xác

#### 4. Request Setup Campaign trên ITU Log Form - BU
- **Mục đích**: BU gửi yêu cầu thiết lập campaign cho Utop Admin
- **Thông tin bổ sung**:
  - Allocation by store (phân bổ theo cửa hàng)
  - Thông tin SKUs in trên hóa đơn của KA
  - Thông tin Agency vận hành campaign

#### 5. Sync ITU Log Form → UHub Admin - Auto Email
- **Cơ chế**: ITU Log Form tự động gửi email thông tin cho UHub Admin
- **Cải tiến cần thiết**: Bổ sung các trường dữ liệu từ UGMS:
  - Scheme ID, Giftcode ID, Agency ID
- **Tương lai**: Sẽ bỏ ITU Log Form, BU nhập trực tiếp trên Admin Portal

#### 6. Cập nhật Campaign trên Utop Admin Portal - Utop Admin
- **Tính năng cần bổ sung**:
  - Cập nhật Agency vận hành theo Scheme ID
  - Cập nhật Scheme ID khi tạo Campaign
  - Cập nhật Allocation by store theo Scheme ID + Campaign ID
  - Cập nhật GiftCode cho từng lần redemption

### B2. Gift Delivery to Agency WHs (Giao Quà Đến Kho Agency)

#### 7. Linfox Delivery → Agency WHs - DC
- **Quy trình**: Distribution Center sử dụng Linfox vận chuyển
- **Đặc điểm**: Quy trình internal Unilever, không số hóa
- **Luồng**: Từ Kho Unilever → Kho Agency

#### 8. Nhận & Kiểm Tra Hàng - Agency
- **Quy trình**: Agency nhận phiếu xuất và kiểm tra hàng thực tế
- **Lưu ý quan trọng**: Có thể điều chỉnh Gift Vol/Allocation by store trước go-live
- **Trường hợp đặc biệt**: Nếu nhập kho khác số dự kiến, cần BU cập nhật ITU Log Form đồng bộ

#### 9. Agency Ticket System trên UHub - Agency
- **TÍNH NĂNG MỚI ENHANCED**: Digital confirmation + Ticket workflow với SLA
- **Logic xử lý**:
  - **Nếu đủ hàng & không hư hỏng**: Confirm theo Gift Vol dự kiến
  - **Nếu có vấn đề**: Tạo ticket với số thực tế + nguyên nhân chi tiết
- **Ticket SLA & Timing**:
  - **Deadline**: Agency phải tạo ticket trước 168 giờ (7 ngày) so với ngày go-live
  - **Escalation**: Nếu cận ngày live <168h, Agency nhờ BU Log Form CR hỗ trợ
  - **BU Response SLA**: BU phải approval trong vòng 24 giờ
  - **Auto Expiry**: Ticket hết hạn nếu quá 24 giờ
- **Workflow Loop**: Ticket bị Rejected sẽ lặp lại cho đến khi Approved
- **Final Output**: Xuất thông tin Allocation by store trên UHub sau khi hoàn tất

### B3. Gift Delivery to Stores (Giao Quà Đến Store)

#### 10. Deliver to Stores - Agency
- **Quy trình**: Agency phân phối theo Allocation by store đã định
- **Cơ sở**: Dựa trên allocation đã được approve từ các bước trước
- **Đặc điểm**: Tuân theo thông tin Allocation by store đã xuất từ UHub ở bước 9

#### 11. Kiểm Tra Hàng - Sales
- **Quy trình**: Sales so sánh hàng nhận với Allocation by store
- **Lưu ý**: Vẫn có thể điều chỉnh Gift Vol trước go-live campaign nếu cần
- **Yêu cầu**: Sales phải chọn đúng Allocation by store của mình trên UHub trước khi confirm

#### 12. Sales Ticket System trên UHub - Sales
- **TÍNH NĂNG MỚI ENHANCED**: Digital confirmation + Ticket workflow với shorter SLA
- **Logic xử lý**:
  - **Nếu đủ hàng**: Confirm theo Allocation by store
  - **Nếu có vấn đề**: Tạo ticket với số thực tế + nguyên nhân chi tiết
- **Ticket SLA & Timing (Stricter than Agency)**:
  - **Deadline**: Sales chỉ được tạo ticket trước 48 giờ so với Campaign Live
  - **Escalation**: Nếu cận ngày live <48h, Agency phải nhờ BU Log Form CR hỗ trợ
  - **BU Response SLA**: BU phải approval trong vòng 12 giờ (nhanh hơn Agency)
  - **Auto Expiry**: Ticket hết hạn nếu quá 12 giờ
- **Workflow Loop**: Ticket bị Rejected sẽ lặp lại cho đến khi Approved
- **Strategic Rationale**: SLA ngắn hơn ở store level vì gần ngày go-live hơn

### B4. Gift Usage (Sử Dụng Quà Tặng)

#### 13. UTOP Setup UHub Campaign - Utop Admin (Campaign Readiness Gate)
- **TÍNH NĂNG MỚI ENHANCED**: **LOOP kiểm tra Campaign ready = Yes** trước khi cho phép vận hành
- **Quy trình**: Repeat loop "Campaign ready?" until = Yes
- **Điều kiện**: Chỉ khi Campaign ready = Yes, PG mới được phép thao tác
- **Đảm bảo**: Đồng bộ giữa inventory và campaign activation
- **Logic**: Blocking gate để đảm bảo campaign setup hoàn chỉnh
- **Vị trí**: Được thực hiện sau khi Store đã nhận hàng và confirm đủ điều kiện

#### 14. BU PowerBI Report + Trigger Operations - BU
- **TÍNH NĂNG MỚI**: Gift Report được bổ sung + Operational triggers
- **Tần suất**: Refresh 3 lần/ngày
- **Nội dung Dashboard**:
  - **Gift Report**: Tracking số lượng quà đã phát/còn lại
  - **Campaign Performance**: Hiệu quả campaign theo KPIs
- **Parallel Process**: Chạy song song với PG campaign operations

#### 15. BU Trigger Operations - BU (TÍNH NĂNG HOÀN TOÀN MỚI)
- **Gift Transfer Between Stores**: BU trigger quy trình luân chuyển quà giữa các Store
  - Dựa trên performance data từ PowerBI Report
  - Điều chỉnh allocation để tối ưu hiệu quả
- **Agency Change Process**: BU trigger quy trình thay đổi Agency nếu cần
  - Dựa trên performance hoặc business requirements
  - Quản lý transfer inventory và campaign responsibility
- **Parallel Process**: Chạy song song với PG campaign operations

#### 16. PG Operates Campaign - Agency
- **Hoạt động chính**:
  - **UHub Sampling**: PG hỗ trợ shopper thử sản phẩm
  - **UHub Redemption**: PG trao quà theo mechanics
- **Tương tác**: Scan QR code, xác nhận trao quà real-time
- **Parallel Process**: Chạy song song với BU monitoring & trigger operations

### B5. Gift Recall to Agency WHs (Thu Hồi Quà Về Kho Agency)

#### 17. Recall Gifts to Agency WHs - Agency
- **Thời gian**: Trong vòng 5 ngày sau end-campaign
- **Quy trình**: Thu hồi quà thừa từ Store về Kho Agency
- **CẢI TIẾN**: Có Gift Recall Alignment rõ ràng từ bước 1

### B6. Stock Management (Quản Lý Kho)

#### 18. Kiểm Tra Hàng Thu Hồi - Agency
- **Quy trình**: Agency so sánh hàng thu hồi thực tế với report đối soát UHub
- **Mục đích**: Xác minh tính chính xác của inventory sau campaign

#### 19. Agency Reconciliation System - Agency
- **TÍNH NĂNG MỚI ENHANCED**: **LOOP kiểm tra reconciliation** cho đến khi approved
- **Logic xử lý Enhanced**:
  - **Nếu khớp**: Agency digital confirm số liệu report đối soát → Tự động Sync UHub ↔ UGMS
  - **Nếu có discrepancy**: Submit ticket đối soát với số điều chỉnh + nguyên nhân
- **Workflow Loop**: Repeat "BU Level-2 Approved?" until = Approved (not Rejected)
- **Exit Condition**: Chỉ khi BU Level-2 Approved, quy trình mới kết thúc hoàn toàn
- **TÍNH NĂNG MỚI ENHANCED**: 
  - Digital reconciliation thay thế Excel thủ công
  - **Auto Email Alert**: UHub gửi auto email thông tin ticket cho BU
  - **Automatic Sync**: Nếu khớp, tự động cập nhật số Gift Usage post-campaign

### B7. Stock Adjustment (Điều Chỉnh Kho - Nếu Có Discrepancy)

**Lưu ý**: B7 chỉ được thực hiện khi có discrepancy trong quá trình reconciliation ở B6. Nếu không có discrepancy, quy trình kết thúc ở B6.

#### 20. BU Raise & Forward Ticket (Level-2 Approval) - BU
- **Trigger**: Khi có discrepancy từ Agency reconciliation + UHub auto email alert
- **Enhanced Process**: BU Raise & Forward thay vì chỉ Raise
- **Nội dung ticket**:
  - Stock reduction (giảm kho)
  - Gift recall (thu hồi quà) 
  - Re-allocation (phân bổ lại)
- **Workflow Enhancement**: BU forward ticket lên Level-2 với context đầy đủ

#### 21. Review & Approval Ticket - BU Level-2
- **TÍNH NĂNG MỚI**: Approval workflow với cấp quản lý
- **Logic**:
  - **Approved**: Tiến hành sync UHub ↔ UGMS
  - **Rejected**: Yêu cầu Agency correction/re-check

#### 22. Utop Admin Confirm Ticket + Enhanced Sync - Utop Admin
- **Enhanced Process**: 2-step process với evidence tracking
  - **Step 1**: Utop Admin confirm ticket (audit trail)
  - **Step 2**: Sync UHub ↔ UGMS với evidences của BU Level-2 Approval
- **Mục đích**: Cập nhật inventory cuối cùng sau campaign với full traceability
- **TÍNH NĂNG MỚI ENHANCED**: 
  - Bi-directional sync để đảm bảo consistency
  - **Evidence Integration**: Sync kèm evidences của approval process
  - **Complete Audit Trail**: Link ticket → approval → system sync
- **Kết quả**: Finalize inventory status với complete governance

## Kết Thúc Quy Trình

#### 23. Gift Ready for Re-use - Agency
- **Hoạt động cuối**: Nhập Gift thu hồi vào kho Agency, sẵn sàng để tái sử dụng
- **Quy trình tái sử dụng**: Gift có thể được sử dụng cho campaign tiếp theo
- **Trạng thái**: Inventory được cập nhật đầy đủ trên cả UHub và UGMS với full audit trail