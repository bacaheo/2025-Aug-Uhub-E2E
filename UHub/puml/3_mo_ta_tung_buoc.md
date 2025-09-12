# Gift Management System x UHub – Mô Tả Chi Tiết Từng Bước

*Phiên bản được đồng bộ hóa với PlantUML Workflow v2.0 - Ngày cập nhật: 2024-09-10*

## Mục Lục

- [B1. Gift Planning (Lập Kế Hoạch Quà cho Campaign)](#b1-gift-planning)
- [B2. Gift Delivery to Agency WHs (Giao Quà Đến Kho Agency)](#b2-gift-delivery-to-agency-whs)
- [B3. Gift Delivery to Stores (Giao Quà Đến Store)](#b3-gift-delivery-to-stores)
- [B4. Gift Usage (Sử Dụng Quà Tặng)](#b4-gift-usage)
- [B5. Gift Recall to Agency WHs (Thu Hồi Quà Về Kho Agency)](#b5-gift-recall-to-agency-whs)
- [B6. Stock Management (Quản Lý Kho)](#b6-stock-management)
- [B7. Stock Adjustment (Điều Chỉnh Kho)](#b7-stock-adjustment)
- [Appendix A: Data Dictionary & API Payload](#appendix-a-data-dictionary--api-payload)
- [Appendix B: SLA Matrix](#appendix-b-sla-matrix)

## Quy Ước

- **Actor abbreviations**: KA (Key Account), BU (Business Unit), DC (Distribution Center), Agency, Sales, Utop Admin, BU Level-2
- **Decision nodes**: Được đánh dấu rõ ràng với logic Yes/No path
- **Loop conditions**: Repeat-until-approved workflow được mô tả chi tiết
- **Phase mapping**: ID từ PlantUML được giữ nguyên để đảm bảo traceability

---

## B1. Gift Planning

### B1_01. Customer Alignment (Gift Recall Alignment) - KA

**Mục đích**: KA thống nhất với khách hàng về chương trình khuyến mãi và quy trình thu hồi**Hệ thống**: Quy trình internal Unilever (không số hóa)**Input Data**: Thông tin campaign mechanics từ khách hàng**Output**: Gift Recall Alignment agreement**Đặc điểm**:

- Quy trình internal Unilever, không số hóa ở giai đoạn này
- **CẢI TIẾN MỚI**: Thống nhất rõ ràng về Gift Recall Alignment từ đầu
- Xác định điều kiện và thời gian thu hồi quà sau khi campaign end

### B1_02. Setup Gift trên UGMS - BU

**Mục đích**: BU cập nhật thông tin Setup Gift Vol lên UGMS**Hệ thống**: UGMS**Timeline**: **15-30 ngày trước ngày campaign live****Input Data**:

- **GiftCode**: Mã định danh quà tặng
- **Scheme**: Thông tin mechanics (Start, End, Mechanics, Quantity)
- **Customer**: Thông tin khách hàng
- **Ship_to**: Điểm giao hàng

### B1_03. Sync UGMS → UHub (Integration) - Tự động

**Mục đích**: **TÍNH NĂNG MỚI** - UGMS tự động đẩy thông tin cho UHub qua API**Hệ thống**: UGMS ↔ UHub API Integration**Input Data**: Dữ liệu từ B1_02**Output**: UHub nhận thông tin Setup Gift qua API**Dữ liệu đồng bộ**:

- GiftCode, Scheme details, Customer info, Ship_to locations
- **Lợi ích**: Loại bỏ thao tác thủ công, đảm bảo tính chính xác

### B1_04. Request Setup Campaign trên ITU Log Form - BU

**Mục đích**: BU gửi yêu cầu thiết lập campaign cho Utop Admin**Hệ thống**: ITU Log Form**Timeline**: **10-15 ngày trước ngày campaign live****Input Data**:

- GiftCode, Scheme từ B1_02
- **Allocation by store** (phân bổ theo cửa hàng)
- **Thông tin SKUs** in trên hóa đơn của KA
- **Thông tin Agency** vận hành campaign

### B1_05. Sync ITU Log Form → UHub Admin - Auto Email

**Mục đích**: ITU Log Form tự động gửi email thông tin cho UHub Admin
**Hệ thống**: ITU Log Form → UHub Admin Email
**Input Data**: Dữ liệu từ B1_04
**Output**: UHub Admin nhận email setup campaign

**Cải tiến cần thiết - Bổ sung các trường dữ liệu từ UGMS**:

- **Scheme ID**
- **Giftcode ID**
- **Agency ID**
- **Chú ý**: Trường hợp New Gift Code

**Tương lai**: Sẽ bỏ ITU Log Form, BU nhập trực tiếp trên Admin Portal

### B1_06. Cập nhật Campaign trên Utop Admin Portal - Utop Admin

**Mục đích**: Utop Admin khởi tạo campaign trên Admin Portal
**Hệ thống**: UHub Admin Portal
**Input Data**: Email từ B1_05

**Tính năng cần bổ sung (CR Requirements)**:

- **Cập nhật Email BU** theo Scheme ID
- **Cập nhật thông tin Agency** vận hành campaign theo Scheme ID
- **Cập nhật Scheme ID** khi tạo Campaign
- **Cập nhật Allocation by store** theo Scheme ID + Campaign ID
- **Cập nhật GiftCode** cho từng lần redemption

*Lưu ý: Point này cần phân tích kiến trúc*

### B1_07. Linfox Delivery → Agency WHs - DC

**Mục đích**: Distribution Center vận chuyển quà từ kho Unilever đến kho Agency
**Hệ thống**: Quy trình kho vận internal Unilever
**Luồng**: Kho Unilever → Kho Agency
**Đặc điểm**: Quy trình internal Unilever, không số hóa

*Lưu ý: Mặc dù bước này thuộc luồng logistics của phase B2 trong overview, nhưng giữ nguyên ID PlantUML để đảm bảo traceability*

---

## B2. Gift Delivery to Agency WHs

### B2_01. Nhận & Kiểm Tra Hàng - Agency

**Mục đích**: Agency nhận phiếu xuất từ kho Unilever và kiểm tra hàng thực tế**Hệ thống**: Agency Warehouse Management**Input Data**: Phiếu xuất từ DC**Lưu ý quan trọng**:

- Có thể điều chỉnh Gift Vol/Allocation by store trước go-live campaign
- Nếu nhập kho khác số giao từ kho Unilever, cần BU cập nhật ITU Log Form để đồng bộ (Giftcode, Quantity, Allocation by store)

### DEC_B2_02. Decision: Đủ hàng & không hư hỏng?

**Logic xử lý**:

- **YES** → Tiến hành B2_03_01 (Digital confirmation flow)
- **NO** → Tiến hành B2_04 (Ticket creation flow)

#### B2_03_01. Agency Chọn Allocation by Store - Agency

**Điều kiện**: Đủ hàng & không hư hỏng
**Hệ thống**: UHub
**Input Data**: Allocation by store từ B1_04 & B1_06
**Phụ thuộc**: Cần có thông tin Allocation by store từ setup campaign
**Lưu ý**: Nếu chưa có "Allocation by store" sẽ không Digital Confirm được

#### B2_03_02. Agency Digital Confirm Nhận Hàng - Agency

**Mục đích**: Agency xác nhận nhận hàng theo Gift Vol dự kiến
**Hệ thống**: UHub Digital Confirmation
**Input Data**: Gift Vol từ setup + Allocation by store từ B2_03_01
**Output**: Digital confirmation record trên UHub

#### B2_04. Agency Tạo Ticket Nhận Hàng - Agency

**Điều kiện**: Có vấn đề với hàng nhận
**Hệ thống**: UHub Ticket System
**Input Data**: Số thực tế + nguyên nhân chi tiết

**Ticket SLA & Timing**:

- **Deadline**: Agency phải tạo ticket trước **168 giờ** (7 ngày) so với ngày go-live
- **Escalation**: Nếu cận ngày live <168h, Agency phải nhờ BU Log Form CR để yêu cầu Admin Utop hỗ trợ

#### B2_05. BU Review & Approval Ticket - BU

**Mục đích**: BU xem xét và phê duyệt ticket điều chỉnh**Hệ thống**: UHub Ticket System**Input Data**: Ticket từ B2_04**Process**:

- BU phải **re-allocation** trước khi approval ticket
- BU phải **approval ticket trong vòng 24 giờ**, tính từ lúc tạo
- **Auto Expiry**: Quá 24 giờ ticket sẽ hết hạn

#### DEC_B2_06. Decision: BU Approved?

**Loop Logic**:

- **REJECTED** → Return to B2_04 (Repeat ticket creation)
- **APPROVED** → Continue to B2_07

### B2_07. Nhập Kho Agency - Agency

**Mục đích**: Nhập kho Agency theo thông tin đã được Digital Confirm hoặc BU Approved**Hệ thống**: Agency Warehouse System**Input Data**:

- Digital confirmation từ B2_03_02 HOẶC
- BU approved ticket từ B2_05

### B2_08. Xuất Allocation by Store - Agency

**Mục đích**: Export thông tin phân bổ cho các store
**Hệ thống**: UHub
**Output**: Allocation by store data available for B3 phase

---

## B3. Gift Delivery to Stores

### B3_01. Deliver to Stores - Agency

**Mục đích**: Agency phân phối theo Allocation by store đã định
**Hệ thống**: Agency Distribution
**Input Data**: Allocation by store từ B2_08
**Cơ sở**: Dựa trên allocation đã được approve từ các bước trước

*Lưu ý: Phase 1 - Chưa số hóa quy trình B3.Gift Delivery to Stores*

### B3_02. Kiểm Tra Hàng - Sales

**Mục đích**: Sales so sánh hàng nhận với Allocation by store**Hệ thống**: Store Inventory Check**Input Data**: Hàng thực tế + Allocation by store**Lưu ý**:

- Vẫn có thể điều chỉnh Gift Vol trước go-live campaign nếu cần
- Nếu nhập kho khác số giao, cần BU cập nhật ITU Log Form để đồng bộ

### DEC_B3_03. Decision: Đủ hàng theo Allocation by store & không hư hỏng?

**Logic xử lý**:

- **YES** → Tiến hành B3_04_01 (Digital confirmation flow)
- **NO** → Tiến hành B3_05 (Ticket creation flow)

#### B3_04_01. Sales Chọn Allocation by Store - Sales

**Điều kiện**: Đủ hàng theo Allocation
**Hệ thống**: UHub
**Yêu cầu**: Sales phải chọn đúng Allocation by store của mình trên UHub trước khi confirm

#### B3_04_02. Sales Digital Confirm Nhận Hàng - Sales

**Mục đích**: Sales xác nhận nhận hàng theo Allocation by store
**Hệ thống**: UHub Digital Confirmation
**Location**: At Store

#### B3_05. Sales Tạo Ticket Nhận Hàng - Sales

**Điều kiện**: Có vấn đề với hàng nhận
**Hệ thống**: UHub Ticket System
**Input Data**: Số thực tế + nguyên nhân chi tiết

**Ticket SLA & Timing (Stricter than Agency)**:

- **Deadline**: Sales chỉ được tạo ticket trước **48 giờ** so với Campaign Live
- **Escalation**: Nếu cận ngày live <48h, Agency phải nhờ BU Log Form CR để yêu cầu Admin Utop hỗ trợ

#### B3_06. BU Review & Approval Ticket - BU

**Mục đích**: BU xem xét và phê duyệt ticket điều chỉnh của Sales**Hệ thống**: UHub Ticket System**Process**:

- BU phải **re-allocation** trước khi approval ticket
- BU phải **approval ticket trong vòng 12 giờ** (nhanh hơn Agency), tính từ lúc tạo
- **Auto Expiry**: Quá 12 giờ ticket sẽ hết hạn

**Strategic Rationale**: SLA ngắn hơn ở store level vì gần ngày go-live hơn

#### DEC_B3_07. Decision: BU Approved?

**Loop Logic**:

- **REJECTED** → Return to B3_05 (Repeat ticket creation)
- **APPROVED** → Continue to B3_08

### B3_08. Nhập Kho Store - Sales

**Mục đích**: Nhập kho cửa hàng theo thông tin đã được Digital Confirm hoặc BU Approved
**Hệ thống**: Store Inventory System

### B3_09. UTOP Setup UHub Campaign - Utop Admin

**Mục đích**: Utop Admin setup campaign và chuẩn bị go-live
**Hệ thống**: UHub Admin Portal
**Input Data**: Request setup campaign từ BU

### DEC_B3_10. Campaign Ready Gate: Campaign ready?

**TÍNH NĂNG MỚI**: **LOOP kiểm tra Campaign ready = Yes** trước khi cho phép vận hành

**Loop Logic**:

- **NO** → Return to B3_09 (Repeat setup until ready)
- **YES** → Continue to B4 (Allow PG operations)

**Đảm bảo**:

- Đồng bộ giữa inventory và campaign activation
- Blocking gate để đảm bảo campaign setup hoàn chỉnh
- Chỉ khi Campaign ready = Yes, PG mới được phép thao tác

---

## B4. Gift Usage

### B4_01. PG Operates Campaign - Agency

**Mục đích**: PG vận hành campaign tại store**Hệ thống**: UHub Sampling & Redemption**Hoạt động chính**:

- **UHub Sampling**: PG phát mẫu thử cho shopper qua UHub
- **UHub Redemption**: Shopper chụp hoá đơn, nhận quà qua UHub. PG xác nhận trao quà qua UHub.
- **Tương tác**: Scan QR code, xác nhận trao quà real-time

**Process Type**: **PARALLEL** - Chạy song song với BU monitoring & trigger operations

### B4_02. BU PowerBI Report Monitoring - BU

**Mục đích**: **TÍNH NĂNG MỚI** - BU theo dõi campaign performance
**Hệ thống**: UHub PowerBI Dashboard
**Tần suất**: Refresh **3 lần/ngày**

**Nội dung Dashboard**:

- **Gift Report**: Tracking số lượng quà đã phát/còn lại (**NEW**)
- **Campaign Performance**: Hiệu quả campaign theo KPIs

**Process Type**: **PARALLEL** - Chạy song song với PG campaign operations

### B4_03. BU Trigger Gift Transfer Between Stores - BU

**Mục đích**: **TÍNH NĂNG HOÀN TOÀN MỚI** - BU trigger quy trình luân chuyển quà giữa các Store
**Hệ thống**: UHub Operations Management
**Trigger**: Dựa trên performance data từ PowerBI Report
**Mục tiêu**: Điều chỉnh allocation để tối ưu hiệu quả

*Lưu ý: Phase 1 - Chưa số hóa quy trình này*

### B4_04. BU Trigger Gift Recall từ Stores - BU

**Mục đích**: BU trigger quy trình thu hồi quà từ Stores về Agency WHs (nếu có)
**Hệ thống**: UHub Operations Management

*Lưu ý: Phase 1 - Chưa số hóa quy trình này*

### B4_05. BU Trigger Agency Changes - BU

**Mục đích**: **TÍNH NĂNG HOÀN TOÀN MỚI** - BU trigger quy trình thay đổi Agency nếu cần
**Hệ thống**: UHub Operations Management
**Trigger**: Dựa trên performance hoặc business requirements
**Scope**: Quản lý transfer inventory và campaign responsibility

*Lưu ý: Phase 1 - Chưa số hóa quy trình này*

---

## B5. Gift Recall to Agency WHs

### B5_01. Recall Gifts to Agency WHs - Agency

**Mục đích**: Thu hồi quà thừa từ Store về Kho Agency
**Hệ thống**: Agency Logistics
**Thời gian**: Trong vòng **5 ngày sau end-campaign**
**CẢI TIẾN**: Có Gift Recall Alignment rõ ràng từ bước B1_01

*Lưu ý: Bước này Agency tự vận hành, chưa số hóa*

---

## B6. Stock Management

### B6_01. Kiểm Tra Hàng Thu Hồi - Agency

**Mục đích**: Agency so sánh hàng thu hồi thực tế với report đối soát UHub**Hệ thống**: Agency Warehouse + UHub Report**Input Data**:

- Số tồn ban đầu từ B2_07
- Số đã phát trên UHub từ B4_01
- Số còn lại: B2_07 - B4_01

### DEC_B6_02. Decision: Thu hồi đủ số còn lại & không hư hỏng?

**Logic xử lý**:

- **YES** → Tiến hành B6_03 (Digital confirmation flow)
- **NO** → Tiến hành B6_05 (Ticket discrepancy flow)

#### B6_03. Agency Digital Confirm Report Đối Soát - Agency

**Điều kiện**: Thu hồi đủ số còn lại
**Hệ thống**: UHub Digital Reconciliation
**Output**: Digital confirmation report đối soát

#### B6_04. Automatic Sync UHub → UGMS - Tự động

**Mục đích**: **TÍNH NĂNG MỚI** - Tự động cập nhật số Gift Usage post-campaign
**Hệ thống**: UHub ↔ UGMS API Integration
**Input Data**: Gift Usage từ B4_01
**Output**: UGMS được cập nhật inventory status

#### B6_05. Agency Submit Ticket Đối Soát - Agency

**Điều kiện**: Có discrepancy trong reconciliation
**Hệ thống**: UHub Ticket System
**Input Data**: Số điều chỉnh thực tế + nguyên nhân chi tiết

#### B6_06. UHub Auto Email Alert - UHub System

**Mục đích**: **TÍNH NĂNG MỚI** - UHub gửi auto email thông tin ticket cho BU
**Hệ thống**: UHub Email Integration
**Phụ thuộc**: Email BU từ B1_06 & B1_04
**Trigger**: Ticket được tạo ở B6_05

#### B6_07. BU Raise & Forward Ticket (Level-2 Approval) - BU

**Mục đích**:
**Enhanced Process** 
- BU Raise & Forward thay vì chỉ Raise manual via email
**Hệ thống**: Email Workflow
**Trigger**: UHub auto email alert từ B6_06
**Nội dung ticket**:
- Stock reduction (giảm kho)
- Gift recall (thu hồi quà)
- Re-allocation (phân bổ lại)
- **Workflow Enhancement**: BU forward ticket lên Level-2 với context đầy đủ

#### B6_08. Review & Approval Ticket - BU Level-2

**Mục đích**: **TÍNH NĂNG MỚI** - Approval workflow với cấp quản lý
**Hệ thống**: Email Approval System
**Input Data**: Ticket từ B6_07

#### DEC_B6_09. Decision: BU Level-2 Approved?

**Loop Logic**:

- **APPROVED via email** → Tiến hành B6_10_01 (Utop Admin confirm)
- **REJECTED via email** → Tiến hành B6_11_01 (BU reject & correction)

##### B6_10_01. Utop Admin Digital Confirm Ticket - Utop Admin

**Điều kiện**: BU Level-2 Approved
**Mục đích**: **Enhanced Process** - Audit trail confirmation
**Hệ thống**: UHub Admin Portal

##### B6_10_02. Enhanced Sync UHub ↔ UGMS - Utop Admin

**Mục đích**: **TÍNH NĂNG MỚI** - Bi-directional sync với evidences
**Hệ thống**: UHub ↔ UGMS Integration
**Input Data**: Điều chỉnh tồn post-campaign + evidences BU Level-2 Approval
**Features**:
- **Evidence Integration**: Sync kèm evidences của approval process
- **Complete Audit Trail**: Link ticket → approval → system sync
- **Bi-directional sync** để đảm bảo consistency

##### B6_11_01. BU Rejected Ticket - BU

**Điều kiện**: BU Level-2 Rejected
**Hệ thống**: UHub Ticket System

##### B6_11_02. Request Agency Correction/Re-check - BU

**Mục đích**: Yêu cầu Agency correction hoặc re-check
**Hệ thống**: Communication workflow

#### DEC_B6_12. Loop Control: BU Level-2 Approved?

**TÍNH NĂNG MỚI ENHANCED**: **LOOP kiểm tra reconciliation** cho đến khi approved

**Loop Logic**:

- **REJECTED** → Return to B6_05 (Repeat reconciliation process)
- **APPROVED** → Continue to B6_13 (Complete process)

**Exit Condition**: Chỉ khi BU Level-2 Approved, quy trình mới kết thúc hoàn toàn

### B6_13. Nhập Gift Thu Hồi Vào Kho Agency - Agency

**Mục đích**: Finalize gift inventory trong kho Agency
**Hệ thống**: Agency Warehouse System
**Status**: Ready để tái sử dụng
**Kết quả**: Inventory được cập nhật đầy đủ trên cả UHub và UGMS với full audit trail

### B6_14. Quy Trình Tái Sử Dụng Gift - Agency

**Mục đích**: Gift có thể được sử dụng cho campaign tiếp theo
**Hệ thống**: Gift Lifecycle Management
**Status**: Placeholder cho future enhancements

---

## B7. Stock Adjustment

**Lưu ý**: B7 chỉ được thực hiện khi có discrepancy trong quá trình reconciliation ở B6. Nếu không có discrepancy, quy trình kết thúc ở B6.

*Các bước B7 đã được integrate vào B6 workflow (B6_07 đến B6_12) để tạo thành complete reconciliation loop.*

---

## Appendix A: Data Dictionary & API Payload

### Core Data Elements


| Field                   | Description                         | Source       | Target System      |
| ------------------------- | ------------------------------------- | -------------- | -------------------- |
| **GiftCode**            | Mã định danh quà tặng          | UGMS         | UHub               |
| **Scheme ID**           | ID của scheme campaign             | UGMS         | UHub, ITU Log Form |
| **Scheme Details**      | Start, End, Mechanics, Quantity     | UGMS         | UHub               |
| **Customer**            | Thông tin khách hàng             | UGMS         | UHub               |
| **Ship_to**             | Điểm giao hàng                   | UGMS         | UHub               |
| **Allocation by Store** | Phân bổ theo cửa hàng           | ITU Log Form | UHub               |
| **Agency ID**           | ID của Agency vận hành           | ITU Log Form | UHub               |
| **SKUs Info**           | Thông tin SKUs in trên hóa đơn | ITU Log Form | UHub               |

### API Integration Points

1. **UGMS → UHub** (B1_03)
2. **ITU Log Form → UHub Admin** (B1_05)
3. **UHub ↔ UGMS** (B6_04, B6_10_02)

---

## Appendix B: SLA Matrix


| Actor          | Process                | SLA Window         | Auto Expiry | Notes             |
| ---------------- | ------------------------ | -------------------- | ------------- | ------------------- |
| **Agency**     | Ticket Creation        | ≤168h before live | 24h         | 7 days lead time  |
| **BU**         | Agency Ticket Approval | 24h from creation  | Yes         | Agency workflow   |
| **Sales**      | Ticket Creation        | ≤48h before live  | 12h         | Stricter timeline |
| **BU**         | Sales Ticket Approval  | 12h from creation  | Yes         | Store workflow    |
| **BU Level-2** | Discrepancy Approval   | Email-based        | Manual      | Governance layer  |

### Escalation Matrix


| Situation            | Escalation Path                        | Mechanism              |
| ---------------------- | ---------------------------------------- | ------------------------ |
| Agency ticket <168h  | BU Log Form CR → Admin Utop           | Manual                 |
| Sales ticket <48h    | Agency → BU Log Form CR → Admin Utop | Manual                 |
| BU Level-2 rejection | Agency correction/re-check             | Communication workflow |

---

*Tài liệu này được đồng bộ hóa với PlantUML Workflow và Overview Document để đảm bảo tính nhất quán và traceability hoàn chỉnh.*
