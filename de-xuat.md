# ĐỀ XUẤT DANH SÁCH QUY TRÌNH E2E PHYSICAL GIFT MANAGEMENT

**Tác giả:** Nana (AI Agent)  
**Ngày tạo:** 2025-10-04  
**Dự án:** E2E Physical Gift Management System - UHub x UGMS Integration

---

## TỔNG QUAN

Hệ thống E2E Physical Gift Management quản lý toàn bộ vòng đời quà tặng vật lý từ **Planning** → **Delivery** → **Usage** → **Reconciliation** → **Recall/Reuse**.

Dựa trên phân tích 3 tài liệu chính (Lịch sử UHub, Quy trình PUML, PRD) và tham khảo mô hình UGMS (10 quy trình), hệ thống E2E Gift được chia thành **12 quy trình chính**.

---

## BẢNG TỔNG HỢP CÁC QUY TRÌNH

| # | Tên Quy Trình | Business Phase | Epic Liên Quan | Actors Chính |
|---|---|---|---|---|
| **B1** | Gift Planning & Scheme Setup | Planning | Epic 1 | BU, ITU, UGMS |
| **B2** | Gift Delivery to Agency Warehouse | Delivery | Epic 2 | Agency, DC, BU |
| **B3** | Gift Delivery to Stores | Delivery | Epic 2 | Agency, Sales, BU |
| **B4** | Campaign Readiness Gate | Pre-Usage | Epic 2, 3 | Utop Admin, BU |
| **B5** | Gift Distribution (PG Operations) | Usage | Epic 2 | PG, Shopper |
| **B6** | Gift Usage Monitoring | Usage | Epic 3 | BU, Utop Admin |
| **B7** | Stock Adjustment - Agency Level | Exception | Epic 3 | Agency, BU |
| **B8** | Stock Adjustment - Store Level | Exception | Epic 3 | Sales, BU |
| **B9** | Gift Recall from Stores | Post-Campaign | Epic 4 | Agency, Sales, BU |
| **B10** | Stock Reconciliation & Verification | Post-Campaign | Epic 3, 4 | Agency, BU, Utop Admin |
| **B11** | Multi-Level Approval Workflow | Governance | Epic 3, 4 | BU L1, BU L2, Utop Admin |
| **B12** | Gift Reuse & Liquidation | Reuse | Epic 4 | BU, Agency, UGMS |

---

## CHI TIẾT CÁC QUY TRÌNH

### **B1. Gift Planning & Scheme Setup**
**Hoạch định quà tặng và thiết lập Scheme**

#### Mục đích
Đồng bộ thông tin Gift Scheme từ UGMS sang UHub để BU có thể yêu cầu Utop Admin setup campaign với đầy đủ thông tin quà tặng.

#### Actors
- **BU Team:** Tạo Scheme trên UGMS, điền ITU Log Form
- **UGMS System:** Đồng bộ dữ liệu qua API
- **ITU Team:** Review Log Form, forward cho Utop Admin
- **Utop Admin:** Init campaign trên Admin Portal

#### Input
- Gift Code, Scheme ID từ UGMS
- Customer Alignment (KA agreements)
- Allocation by Store requirements

#### Output
- Scheme được sync sang UHub
- Campaign được init trên Admin Portal với Scheme ID, Gift Code, Agency thông tin
- Email notification gửi BU xác nhận

#### Các bước chính
1. BU align với KA về gift recall agreement (B1_01)
2. BU setup Gift trên UGMS: GiftCode, Scheme, Customer, Ship_to (B1_02)
3. UGMS sync data sang UHub qua API (B1_03)
4. BU điền ITU Log Form request setup campaign (B1_04)
5. ITU Log Form gửi auto email cho Utop Admin (B1_05)
6. Utop Admin init campaign trên Admin Portal với Scheme ID (B1_06)
7. DC (Linfox) delivery quà từ kho Unilever sang kho Agency (B1_07)

#### Epic/Story liên quan
- **Epic 1:** Foundation & UGMS Integration
- **Story 1.2:** UGMS API Integration Framework
- **Story 1.3:** Basic Gift Tracking Data Model
- **FR1:** Hệ thống tự động đồng bộ UGMS-UHub

---

### **B2. Gift Delivery to Agency Warehouse**
**Giao quà đến kho Agency**

#### Mục đích
Agency nhận quà từ DC/Linfox, kiểm tra và digital confirm số lượng thực tế nhận được. Nếu có sai lệch, tạo ticket điều chỉnh cần BU approval.

#### Actors
- **Agency:** Nhận hàng, kiểm tra, digital confirm/tạo ticket
- **BU Team:** Review và approval ticket điều chỉnh
- **DC/Linfox:** Giao hàng từ kho Unilever

#### Input
- Phiếu xuất kho từ DC/Linfox
- Allocation by Store từ B1_06

#### Output
- Agency digital confirmation trên UHub
- Inventory record tại Agency Warehouse
- Ticket điều chỉnh (nếu có sai lệch) được BU approved

#### Các bước chính
1. Agency nhận hàng từ DC và kiểm tra với phiếu xuất (B2_01)
2. **Nếu đủ hàng & không hư hỏng:**
   - Agency chọn Allocation by Store trên UHub (B2_03_01)
   - Agency digital confirm số lượng nhận (B2_03_02)
3. **Nếu thiếu/hư hỏng:**
   - Agency tạo ticket nhận hàng theo số thực tế + nguyên nhân (B2_04)
   - BU review, cập nhật Allocation, approval ticket trong 24h (B2_05)
   - Lặp lại cho đến khi BU approved (B2_06)
4. Agency nhập kho theo số đã confirm/approved (B2_07)
5. Agency xuất thông tin Allocation by Store trên UHub (B2_08)

#### Điều kiện đặc biệt
- Ticket phải tạo trước campaign live ≥168 giờ (7 ngày)
- BU phải approval trong 24 giờ, nếu không ticket hết hạn
- Nếu <168 giờ trước live, Agency nhờ BU Log Form CR để Utop Admin hỗ trợ

#### Epic/Story liên quan
- **Epic 2:** Digital Confirmation Workflows
- **Story 2.1:** Agency Mobile Confirmation Interface
- **Story 2.4:** Real-time Inventory Synchronization
- **FR2:** Agency confirm digital nhận hàng với photo upload

---

### **B3. Gift Delivery to Stores**
**Giao quà đến cửa hàng**

#### Mục đích
Agency giao quà đến từng store theo Allocation. Sales tại store kiểm tra và digital confirm. Nếu có sai lệch, tạo ticket cần BU approval.

#### Actors
- **Agency:** Deliver quà đến stores
- **Sales:** Nhận hàng, kiểm tra, digital confirm/tạo ticket
- **BU Team:** Review và approval ticket điều chỉnh

#### Input
- Allocation by Store từ B2_08
- Gift inventory từ Agency Warehouse

#### Output
- Sales digital confirmation trên UHub
- Inventory record tại Store level
- Ticket điều chỉnh (nếu có) được BU approved

#### Các bước chính
1. Agency deliver quà đến stores theo Allocation (B3_01)
2. Sales kiểm tra hàng so với Allocation by Store (B3_02)
3. **Nếu đủ hàng & không hư hỏng:**
   - Sales chọn Allocation by Store của mình (B3_04_01)
   - Sales digital confirm nhận hàng (B3_04_02)
4. **Nếu thiếu/hư hỏng:**
   - Sales tạo ticket theo số thực tế + nguyên nhân (B3_05)
   - BU review, cập nhật Allocation, approval ticket trong 12h (B3_06)
   - Lặp lại cho đến khi BU approved (B3_07)
5. Sales nhập kho store theo số đã confirm/approved (B3_07)

#### Điều kiện đặc biệt
- Ticket phải tạo trước campaign live ≥48 giờ
- BU phải approval trong 12 giờ
- Nếu <48 giờ trước live, Sales nhờ BU Log Form CR để Utop Admin hỗ trợ

#### Ghi chú Phase 1
- **Chưa số hóa** quy trình B3 Gift Delivery to Stores trong Phase 1
- Agency tự vận hành offline, chưa track trên hệ thống

#### Epic/Story liên quan
- **Epic 2:** Digital Confirmation Workflows
- **Story 2.2:** Sales Store Inventory Confirmation
- **Story 2.4:** Real-time Inventory Synchronization
- **FR3:** Sales confirm digital gift inventory tại store level

---

### **B4. Campaign Readiness Gate**
**Kiểm tra sẵn sàng Campaign**

#### Mục đích
Đảm bảo campaign chỉ được go-live khi tất cả điều kiện đã sẵn sàng: inventory confirmed, allocation complete, PG được phân quyền.

#### Actors
- **Utop Admin:** Setup campaign, kiểm tra readiness
- **BU Team:** Confirm campaign ready
- **System:** Validate readiness conditions

#### Input
- Campaign setup từ B1_06
- Inventory confirmations từ B2, B3
- PG assignments

#### Output
- Campaign status = "Active"
- PG App enabled cho campaign
- Notification cho BU, Agency, PG

#### Các bước chính
1. Utop Admin setup UHub campaign theo request của BU (B3_09)
2. Hệ thống kiểm tra Campaign Readiness Conditions:
   - Inventory confirmed tại Agency/Store?
   - Allocation complete?
   - PG được assign và có quyền?
   - Scheme sync từ UGMS?
3. **Nếu chưa ready:** Loop lại bước 1-2 (B3_10)
4. **Nếu ready:** Campaign go-live, PG App được enable

#### Điều kiện readiness
- ✅ Agency digital confirmed (B2)
- ✅ Sales digital confirmed (B3) - hoặc BU accept risk
- ✅ PG được phân quyền cho campaign
- ✅ Scheme đã sync từ UGMS
- ✅ Gift Code mapping hoàn tất

#### Epic/Story liên quan
- **Epic 2:** Digital Confirmation Workflows
- **Story 2.3:** PG App Integration Enhancement (Campaign ready validation)
- **FR11:** Campaign Ready Condition logic

---

### **B5. Gift Distribution (PG Operations)**
**Phát quà qua PG**

#### Mục đích
PG scan QR của shopper, xác nhận phát quà vật lý, hệ thống ghi nhận gift-out và trừ inventory real-time.

#### Actors
- **PG:** Scan QR, phát quà
- **Shopper:** Nhận quà
- **UHub System:** Track gift distribution

#### Input
- Campaign active từ B4
- Shopper QR code (từ UHub redemption/sampling flow)
- Store inventory

#### Output
- Gift distribution record với timestamp
- Inventory deduction real-time
- Shopper confirmation trong "Quà của tôi"

#### Các bước chính
1. PG đăng nhập PG App với user/password
2. PG chọn campaign đã được phân quyền
3. PG scan QR code của shopper (từ UHub redemption/sampling)
4. Hệ thống validate:
   - Campaign còn active?
   - Store còn inventory?
   - Shopper đủ điều kiện nhận?
5. PG confirm phát quà trên PG App
6. Hệ thống ghi nhận gift-out, trừ inventory, cập nhật "Quà của tôi"

#### Parallel tracking (Phase 1)
- **E2E System:** Cung cấp baseline quantities (số đầu kỳ theo Scheme/GiftCode)
- **UHub:** Track số thực tế phát theo campaign với timestamp

#### Epic/Story liên quan
- **Epic 2:** Digital Confirmation Workflows
- **Story 2.3:** PG App Integration Enhancement
- **FR4:** PG App tracking gift distribution với parallel inventory management

---

### **B6. Gift Usage Monitoring**
**Giám sát sử dụng quà**

#### Mục đích
BU theo dõi real-time gift usage qua PowerBI dashboard, monitoring campaign performance và inventory remaining.

#### Actors
- **BU Team:** Xem dashboard, phân tích
- **PowerBI System:** Refresh data 3 lần/ngày

#### Input
- Gift distribution data từ B5
- Inventory data từ B2, B3
- Campaign data từ B1

#### Output
- PowerBI Report: Gift Usage, Remaining Inventory
- Campaign Performance Analytics
- Exception alerts (low stock, high waste, etc.)

#### Các bước chính
1. BU truy cập UHub PowerBI Report (B4_02)
2. Xem các báo cáo:
   - **Gift Report:** Gift-in, Gift-out, Remaining theo Scheme/Store
   - **Campaign Performance:** Distribution rate, utilization %, waste %
3. PowerBI auto refresh 3 lần/ngày (8h, 13h, 18h)
4. Hệ thống gửi alerts nếu có exception

#### Trigger cho các quy trình khác
- **B4_03:** BU trigger luân chuyển quà giữa stores (nếu cần)
- **B4_04:** BU trigger thu hồi quà từ stores về Agency (nếu cần)
- **B4_05:** BU trigger thay đổi Agency (nếu cần)

#### Ghi chú Phase 1
- 3 quy trình B4_03, B4_04, B4_05 **chưa số hóa** trong Phase 1

#### Epic/Story liên quan
- **Epic 3:** Reconciliation & Exception Management
- **Story 3.3:** PowerBI Dashboard Integration
- **FR5:** PowerBI dashboard refresh 3 lần/ngày

---

### **B7. Stock Adjustment - Agency Level**
**Điều chỉnh tồn kho - Cấp Agency**

#### Mục đích
Xử lý sai lệch inventory tại Agency Warehouse trong quá trình campaign (hư hỏng, mất mát, điều chỉnh).

#### Actors
- **Agency:** Báo cáo sai lệch, tạo ticket
- **BU L1:** Review, approval/reject ticket
- **Utop Admin:** Digital confirm nếu cần escalation

#### Input
- Inventory discrepancy phát hiện tại Agency
- Stock check report

#### Output
- Ticket adjustment được approved
- Inventory được điều chỉnh trên UHub
- Audit trail đầy đủ

#### Các bước chính
1. Agency phát hiện sai lệch (hư hỏng, mất mát, inventory không khớp)
2. Agency tạo ticket adjustment với:
   - Số lượng điều chỉnh
   - Nguyên nhân (damage, lost, recount)
   - Photo evidence
3. Hệ thống gửi email cho BU (B6_06)
4. BU L1 review ticket:
   - **Approve:** Inventory điều chỉnh tự động
   - **Reject:** Agency phải re-check/correction
5. Nếu cần Level 2 approval → chuyển sang B11

#### Điều kiện approval
- Sai lệch <5%: BU L1 có thể approve trực tiếp
- Sai lệch ≥5%: Cần Level 2 approval (B11)

#### Epic/Story liên quan
- **Epic 3:** Reconciliation & Exception Management
- **Story 3.4:** Level 1 Approval Workflow
- **FR8:** Multi-level Approval workflow cho stock adjustments

---

### **B8. Stock Adjustment - Store Level**
**Điều chỉnh tồn kho - Cấp Store**

#### Mục đích
Xử lý sai lệch inventory tại Store trong quá trình campaign.

#### Actors
- **Sales:** Báo cáo sai lệch, tạo ticket
- **BU L1:** Review, approval/reject ticket
- **Utop Admin:** Digital confirm nếu cần escalation

#### Input
- Inventory discrepancy phát hiện tại Store
- Stock check report from Sales

#### Output
- Ticket adjustment được approved
- Inventory được điều chỉnh trên UHub
- Audit trail đầy đủ

#### Các bước chính
1. Sales phát hiện sai lệch tại store
2. Sales tạo ticket adjustment tương tự B7
3. BU L1 review và approval/reject
4. Nếu cần Level 2 approval → B11

#### Điều kiện đặc biệt
- Store-level adjustments thường nhỏ hơn Agency level
- Có thể có threshold khác cho Level 2 escalation

#### Epic/Story liên quan
- **Epic 3:** Reconciliation & Exception Management
- **Story 3.4:** Level 1 Approval Workflow
- **FR8:** Multi-level Approval workflow

---

### **B9. Gift Recall from Stores**
**Thu hồi quà từ cửa hàng**

#### Mục đích
Sau khi campaign kết thúc, Agency thu hồi quà còn thừa từ stores về Agency Warehouse để tái sử dụng/liquidation.

#### Actors
- **Agency:** Thực hiện thu hồi logistics
- **Sales:** Xuất kho, giao quà cho Agency
- **BU Team:** Monitor recall progress

#### Input
- End-campaign trigger
- Remaining inventory tại stores từ B6
- KA recall agreement từ B1

#### Output
- Quà được recall về Agency Warehouse
- Inventory transfer records
- Recall completion report

#### Các bước chính
1. Campaign kết thúc → BU trigger quy trình recall (B5_01)
2. Agency thu hồi quà từ stores trong 5 ngày (B5_01)
3. Sales xuất kho, ký biên bản giao cho Agency
4. Agency vận chuyển về warehouse
5. Agency nhập kho recalled gifts

#### Ghi chú Phase 1
- Quy trình thu hồi vật lý **chưa số hóa** trong Phase 1
- Agency tự vận hành offline

#### Epic/Story liên quan
- **Epic 4:** Gift Recall Implementation & Advanced Approval Workflows
- **Story 4.1:** Gift Recall Workflow Framework
- **FR7:** Gift Recall Alignment workflow với KA agreements

---

### **B10. Stock Reconciliation & Verification**
**Đối soát và xác minh tồn kho**

#### Mục đích
Đối soát inventory post-campaign giữa UGMS records ↔ UHub tracking ↔ Physical count. Phát hiện và xử lý GAP.

#### Actors
- **Agency:** Kiểm đếm vật lý, digital confirm/tạo ticket
- **BU L1:** Review reconciliation results
- **BU L2:** Approval cho adjustments lớn
- **Utop Admin:** Digital confirm sau BU L2 approval
- **UGMS System:** Nhận sync data post-campaign

#### Input
- Initial inventory từ B2_07
- Gift usage từ B4_01 (PG distribution)
- Recalled gifts từ B9
- Physical count từ Agency

#### Output
- Reconciliation report: Expected vs Actual
- GAP identified với root cause
- UGMS sync với actual usage + adjustments
- Audit trail đầy đủ

#### Các bước chính
1. Agency kiểm tra tổng hàng thu hồi so với report trên UHub (B6_01):
   - Số tồn ban đầu (B2_07)
   - Số đã phát (B4_01)
   - Số còn lại = B2_07 - B4_01
2. **Nếu thu hồi đủ số còn lại & không hư hỏng:**
   - Agency digital confirm report đối soát (B6_03)
   - UHub sync sang UGMS: Gift Usage số thực tế (B6_04)
3. **Nếu có sai lệch:**
   - Agency submit ticket đối soát với số điều chỉnh + nguyên nhân (B6_05)
   - UHub gửi auto email cho BU (B6_06)
   - **BU L1 review:**
     - Nếu cần Level 2 → B11 (Multi-level Approval)
     - Nếu approve trực tiếp → Utop Admin confirm, sync UGMS
   - **Lặp lại** cho đến khi approved (B6_12)
4. Agency nhập recalled gifts vào kho, ready tái sử dụng (B6_13)

#### BU Review & Confirm (FR12)
- BU có dashboard pending Agency reconciliation
- Review interface: Expected vs Actual comparison
- Approve/Reject với mandatory comments
- Notification cho Agency khi approved/rejected

#### Epic/Story liên quan
- **Epic 3:** Reconciliation & Exception Management
- **Story 3.1:** Reconciliation Engine Core Logic
- **Story 3.5:** BU Review & Confirm Reconciliation
- **Epic 4:** Gift Recall Implementation
- **FR6:** Reconciliation engine hỗ trợ so sánh UGMS ↔ UHub ↔ Physical
- **FR12:** BU Review & Confirm workflow

---

### **B11. Multi-Level Approval Workflow**
**Quy trình phê duyệt đa cấp**

#### Mục đích
Xử lý các tickets điều chỉnh/đối soát cần approval từ BU Level 2 (senior manager) với comprehensive audit trail.

#### Actors
- **BU L1:** Raise ticket, forward via email
- **BU L2:** Review và approval via email
- **Utop Admin:** Digital confirm approval, sync UGMS
- **System:** Audit trail, event-driven sync

#### Input
- Ticket từ B7, B8, B10 cần Level 2 approval
- Evidence documents (photos, reports, explanations)

#### Output
- Level 2 approval decision
- UGMS sync với adjustment data + approval evidence
- Immutable audit trail

#### Các bước chính
1. BU L1 raise ticket via email cho BU L2 (B6_07)
2. BU L2 review và approval via email (B6_08)
3. **Nếu BU L2 approved:**
   - Utop Admin digital confirm ticket trên UHub (B6_10_01)
   - UHub sync sang UGMS: Gift Usage + Adjustment + Evidence (B6_10_02)
4. **Nếu BU L2 rejected:**
   - BU L1 reject ticket (B6_11_01)
   - Request Agency correction/re-check (B6_11_02)

#### Workflow Engine Requirements
- Escalation rules: >5% sai lệch → Level 2
- Email notifications với escalation timing
- Queue management cho batch processing
- Event-driven architecture cho automatic sync

#### Epic/Story liên quan
- **Epic 3:** Reconciliation & Exception Management
- **Story 3.4:** Level 1 Approval Workflow
- **Epic 4:** Advanced Approval Workflows
- **Story 4.1:** Gift Recall Workflow Framework (Level 2 integration)
- **FR8:** Multi-level Approval workflow với ticket system, automatic UGMS-UHub sync
- **NFR7:** Zero data loss trong UGMS-UHub integration với bi-directional sync

---

### **B12. Gift Reuse & Liquidation**
**Tái sử dụng và thanh lý quà**

#### Mục đích
Quản lý quà recalled để tái sử dụng cho campaigns mới hoặc liquidation, giảm waste từ 5-10% xuống <2% budget.

#### Actors
- **BU Team:** Quyết định reuse/liquidation
- **Agency:** Quản lý kho recalled gifts
- **UGMS System:** Track gift lifecycle

#### Input
- Recalled gifts từ B9, B10
- Gift quality assessment
- Upcoming campaign requirements

#### Output
- Reuse plan cho campaigns mới
- Liquidation decisions
- UGMS updated với gift status
- Waste reduction metrics

#### Các bước chính
1. Agency nhập recalled gifts vào kho (B6_13)
2. BU review recalled gift inventory:
   - Quality check results
   - Expiry dates
   - Demand forecast
3. BU quyết định:
   - **Reuse:** Allocate cho campaign mới (B6_14)
   - **Liquidation:** Thanh lý/donate/destroy
4. UGMS sync gift lifecycle status

#### Quy trình tái sử dụng (B6_14)
- BU tạo scheme mới trên UGMS sử dụng recalled GiftCode
- Hoặc BU chuyển tồn kho sang chương trình khác
- Quay lại B1 cho campaign mới

#### Ghi chú SLOB
- Nếu sau 3 tháng không reuse → chuyển vào hàng SLOB (Slow-Moving and Obsolete)
- BU làm việc với Finance/Procurement cho liquidation

#### Epic/Story liên quan
- **Epic 4:** Gift Recall Implementation & Advanced Approval Workflows
- **Story 4.1:** Gift Recall Workflow Framework
- **FR7:** Gift Recall Alignment workflow
- Pain Point #1 solution: Giảm 60% lãng phí

---

## MA TRẬN QUY TRÌNH - EPIC - PAIN POINTS

| Quy Trình | Epic 1 | Epic 2 | Epic 3 | Epic 4 | Pain Point Addressed |
|---|:---:|:---:|:---:|:---:|---|
| B1 - Planning & Setup | ✅ | | | | #2 - No system gift-in/out tracking |
| B2 - Agency Delivery | | ✅ | | | #2 - Manual Viber confirmation |
| B3 - Store Delivery | | ✅ | | | #3 - No liquidation tracking |
| B4 - Readiness Gate | | ✅ | ✅ | | #2 - Prevent premature distribution |
| B5 - Gift Distribution | | ✅ | | | #2 - Digital gift-out tracking |
| B6 - Usage Monitoring | | | ✅ | | #4 - Real-time visibility |
| B7 - Adjustment Agency | | | ✅ | | #4 - Reduce manual reconciliation |
| B8 - Adjustment Store | | | ✅ | | #4 - Reduce manual reconciliation |
| B9 - Gift Recall | | | | ✅ | #1 - No recall agreements |
| B10 - Reconciliation | | | ✅ | ✅ | #4 - 90% time savings target |
| B11 - Multi-Level Approval | | | ✅ | ✅ | Governance & audit trail |
| B12 - Reuse & Liquidation | | | | ✅ | #1 - 60% waste reduction |

---

## ROADMAP TRIỂN KHAI THEO PHASE

### Phase 1 (MVP - Q1 2026)
**Foundation & Core Workflows**
- ✅ B1: Planning & Setup (Epic 1)
- ✅ B2: Agency Delivery (Epic 2)
- ⚠️ B3: Store Delivery (Epic 2 - **simplified, Agency offline**)
- ✅ B4: Readiness Gate (Epic 2)
- ✅ B5: Gift Distribution (Epic 2)
- ✅ B6: Usage Monitoring (Epic 3)
- ✅ B7: Adjustment Agency (Epic 3)
- ✅ B8: Adjustment Store (Epic 3)
- ✅ B10: Reconciliation (Epic 3)
- ✅ B11: Multi-Level Approval (Epic 3)

### Phase 2 (Post-MVP)
**Advanced Features**
- ✅ B3: Store Delivery (full digitalization với GPS tracking)
- ✅ B9: Gift Recall (Epic 4 - systematic recall workflows)
- ✅ B12: Reuse & Liquidation (Epic 4 - predictive analytics)
- ✅ B4_03: Store-to-Store transfer
- ✅ B4_04: Store-to-Agency recall triggers
- ✅ B4_05: Agency change management

---

## KHUYẾN NGHỊ TIẾP THEO

### Bước 1: Vẽ BPMN chi tiết
Với mỗi quy trình B1-B12, tạo BPMN diagram sử dụng PlantUML với:
- Swimlanes cho từng Actor
- Decision gates rõ ràng
- Exception handling flows
- Integration points với UGMS/UHub

### Bước 2: Thiết kế Database Schema
Dựa trên 12 quy trình, định nghĩa:
- Core entities: Gift, Scheme, Campaign, Inventory, Ticket, Approval
- Relationships và foreign keys
- Audit trail tables
- Index strategy cho performance

### Bước 3: API Specifications
Định nghĩa RESTful APIs cho:
- UGMS-UHub sync (B1)
- Digital confirmations (B2, B3)
- Ticket management (B7, B8, B10)
- Approval workflows (B11)
- PowerBI data feeds (B6)

### Bước 4: UI/UX Wireframes
Mockups cho:
- Agency Confirmation Screen (B2)
- Sales Inventory Screen (B3)
- PG Distribution Screen (B5)
- BU Dashboard (B6)
- Exception Management Screen (B7, B8, B10, B11)

---

**Kết thúc đề xuất**

*Tài liệu này là nền tảng để triển khai chi tiết các quy trình E2E Physical Gift Management. Vui lòng review và feedback trước khi proceed sang giai đoạn BPMN design.*
