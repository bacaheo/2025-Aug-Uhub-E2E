# E2E Gift Product Requirements Document (PRD)

**Author:** Jea
**Date:** 2025-10-06
**Project Level:** Level 3 (Full product)
**Project Type:** Web application
**Target Scale:** 12-40 stories, 2-5 epics

---

## Description, Context and Goals

### Project Description

E2E Physical Gift Management System là giải pháp quản lý toàn diện quy trình quà tặng vật lý cho Unilever Việt Nam, tự động hóa chuỗi giá trị từ kho Agency đến tay người tiêu dùng cuối. Hệ thống tích hợp liền mạch với UHub platform hiện có và UGMS (Unilever Gift Management System), thay thế quy trình thủ công bằng Excel bằng digital workflows có tính kiểm soát cao.

**Phạm vi core:**

**6 Business Workflows chính:**

1. **B1. Gift Planning** - Lập kế hoạch quà tặng và đồng bộ thông tin từ UGMS
   - Customer Alignment với Key Accounts
   - Setup Gift trên UGMS (Giftcode, Scheme, Customer, Ship_to)
   - Sync UGMS → UHub qua API
   - Request Setup Campaign với ITU Log Form
   - Init campaign trên Utop Admin Portal

2. **B2. Gift Delivery to Agency WHs** - Giao quà đến kho Agency
   - Agency nhận và kiểm tra hàng từ Linfox delivery
   - Digital confirmation nhận hàng theo Gift Volume
   - Ticket system cho trường hợp không đủ hàng/hư hỏng
   - BU review và approval workflow
   - Xuất thông tin Allocation by store

3. **B3. Gift Delivery to Stores** - Giao quà đến cửa hàng
   - Sales kiểm tra và confirm nhận hàng theo Allocation
   - Ticket system cho điều chỉnh kho Store
   - Campaign Ready validation trước khi go-live
   - PG chỉ được phép phát quà khi campaign status = "Active"

4. **B4. Gift Usage** - Sử dụng quà trong campaign
   - PG operates campaign trên UHub (Sampling/Redemption)
   - BU xem PowerBI Report (refresh 3 lần/ngày)
   - Quản lý luân chuyển quà giữa các Store
   - Thu hồi quà từ Stores về Agency WHs

5. **B5. Gift Recall to Agency WHs** - Thu hồi quà sau campaign
   - Recall gifts từ Store về kho Agency
   - Hoạt động thu hồi trong vòng 5 ngày từ end-campaign

6. **B6. Stock Management** - Quản lý tồn kho và đối soát
   - Agency kiểm tra tổng hàng thu hồi vs report đối soát
   - Digital confirmation hoặc ticket system cho discrepancy
   - Multi-level Approval workflow (Level 1 + Level 2)
   - Automatic sync UHub → UGMS post-campaign
   - Quy trình tái sử dụng Gift

**Key Actors:**

- **Agency Operations Manager** - Digital confirmation nhận hàng, reconciliation
- **Sales Representative** - Store inventory confirmation
- **PG (Promotion Girl)** - Gift distribution tracking qua QR scanning
- **BU Team Member/Lead** - Campaign planning, approval workflows, PowerBI monitoring
- **Utop Admin** - L2 Support, campaign setup, ticket resolution

### Deployment Intent

**MVP for Early Users** - Triển khai thử nghiệm hệ thống với nhóm pilot users (2-3 Agency operations, 5-10 Sales reps, 10-15 PG) trong Q4 2025, validate workflows và thu thập feedback trước khi scale ra toàn bộ Unilever Vietnam operations (200+ campaigns/năm). MVP tập trung vào core workflows B1-B6 với foundation mạnh mẽ cho automation và digital confirmation, defer advanced analytics và AI features sang Phase 2.

### Context

**Current State - UHub Platform Success & Scale**

UHub đã vận hành thành công từ tháng 05/2020, phục vụ **200-300 chiến dịch quà tặng vật lý mỗi năm** cho Unilever Việt Nam với ngân sách trung bình 50M-500M VNĐ/campaign (tổng ~15-50 tỷ VNĐ/năm). Platform hiện có **~200K MAU (Monthly Active Users)** và 5 user groups chính: Shoppers, PG in-store, RI/BT teams, Utop Admin, và Hotline support, xử lý hàng nghìn transactions mỗi tuần thông qua digital workflows cho shopper redemption và PG distribution.

**The Challenge - 4 Interconnected Pain Points**

Mặc dù thành công trong digital shopper experience, UHub vẫn thiếu end-to-end visibility cho physical gift management, dẫn đến 4 pain points nghiêm trọng và liên kết với nhau:

**Pain Point 1: Thiếu thỏa thuận thu hồi quà với Key Accounts**
- Không có legal framework để thu hồi quà thừa sau campaign
- Dẫn đến lãng phí **5-10% ngân sách mỗi campaign** (~750M-2.5B VNĐ/năm)
- Gift inventory bị "stuck" tại stores không có cơ chế liquidation
- **Impact cascade:** Quà stuck → Over-ordering cho campaign tiếp theo → Vicious cycle of waste

**Pain Point 2: Không kiểm soát gift-in/gift-out tại Agency warehouses**
- Quy trình nhập kho thủ công qua điện thoại/Viber confirmation
- Không có digital evidence cho delivery receipts
- Thiếu photo documentation khi có discrepancy
- **Impact cascade:** Thiếu tracking Agency → Không biết baseline inventory → Store allocation không chính xác (PP3)

**Pain Point 3: Không có tracking ở store level**
- Sales confirm inventory qua Excel và phone calls
- PG App chỉ track distribution nhưng không link với baseline allocation
- Không có real-time visibility vào store inventory status
- **Impact cascade:** Thiếu visibility store → Không phát hiện kịp thời stock issues → Campaign delays và customer complaints

**Pain Point 4: Manual reconciliation tốn 2,000+ giờ hành chính/năm**
- Post-campaign reconciliation mất trung bình **40 giờ mỗi campaign** (200 campaigns × 40h = 8,000 giờ, nhưng chỉ 25% campaigns có full reconciliation = ~2,000 giờ thực tế)
- So sánh UGMS records ↔ UHub tracking ↔ Physical inventory thủ công
- Không có automated exception detection và resolution workflow
- **Impact cascade:** Manual reconciliation → Delayed learnings → Không optimize allocation cho campaign tiếp theo → Perpetuate PP1 waste

**Total Business Impact:**
- **Financial:** 750M-2.5B VNĐ/năm waste (không thu hồi được quà, không re-use được quà) + 2,000+ giờ administrative cost
- **Time/Resource:** Spending significant time trên manual processes thay vì strategic campaign optimization
- **Operational:** Campaign delays, brand reputation risk khi gift shortages tại stores
- **Strategic:** Missed opportunities cho data-driven gift allocation optimization

**Previous Attempts:**
Unilever đã thử cải thiện bằng cách yêu cầu **Agency nhập liệu trực tiếp trên UGMS**, nhưng không thành công do:
- Agency thiếu quyền access và training cho UGMS
- UGMS không có mobile-friendly interface cho warehouse operations
- Không có workflow cho discrepancy resolution và approval
- Thiếu integration với store-level tracking

Excel templates chuẩn hóa và Viber group communications cũng đã được thử nhưng không giải quyết được root cause: **thiếu digital infrastructure kết nối UGMS ↔ Agency ↔ Stores ↔ PG App**.

**Why Now - Strategic Window of Opportunity**

ITU team đang chuẩn bị nâng cấp UHub platform trong **Q4 2025/đầu 2026**. Đây là **strategic window** để:
1. **Integrate E2E Gift Management** với modernized UHub architecture từ đầu (tránh retrofit costs sau này)
2. **Leverage UGMS API** mới đang được develop (RSA 2048-bit integration ready)
3. **Align với business need** - BU teams đang pressure để giải quyết gift waste trước fiscal year 2026
4. **Avoid technical debt** - Build on fresh architecture thay vì patch legacy system

### Goals

**G1. Tự động hóa 80% quy trình quản lý quà tặng vật lý**
- Thay thế Excel thủ công bằng digital workflows
- Digital confirmation cho Agency/Sales thay vì phone/Viber
- Automated UGMS-UHub sync thay vì manual data entry
- **Success Metric:** ≥80% transactions qua digital workflows (không qua Excel/phone)

**G2. Tạo end-to-end visibility từ kho Agency đến người tiêu dùng cuối**
- Real-time tracking gift movement từ Linfox delivery → Agency WHs → Stores → Shoppers
- PowerBI dashboard với refresh 3 lần/ngày cho BU monitoring
- Complete audit trail cho mọi gift transaction
- **Success Metric:** 100% gift transactions có digital trail với timestamps

**G3. Giảm 90% thời gian đối soát post-campaign**
- Từ 40 giờ xuống 4 giờ mỗi campaign thông qua reconciliation engine
- Automated comparison: UGMS ↔ UHub ↔ Physical inventory
- Exception detection và highlighting điểm GAP tự động
- **Success Metric:** Post-campaign reconciliation time ≤4 giờ/campaign

**G4. Đạt inventory accuracy <2% sai lệch**
- Giữa physical inventory và hệ thống records
- Multi-level approval workflow cho stock adjustments
- Real-time sync UGMS-UHub với <30 giây latency
- **Success Metric:** Inventory accuracy ≥98% (sai lệch <2%)

**G5. Foundation cho Gift Recall và compliance tracking**
- Gift Recall Alignment workflow với Key Account agreements
- Systematic recall của unused gifts post-campaign
- Legal compliance tracking và audit trail
- **Success Metric:** MVP enables ≥1 pilot recall workflow before Dec 2025

## Requirements

### Functional Requirements

**FR Group 1: UGMS Integration & Data Sync**

**FR1: Automated UGMS-UHub Data Synchronization**
Hệ thống tự động đồng bộ thông tin từ UGMS sang UHub bao gồm Schemes (campaign mechanics, start/end dates), Giftcodes (unique identifiers), Quantities (allocation volumes), Customer (Key Account details), và Ship_to (delivery locations) thay thế nhập Excel thủ công. Sync trigger khi BU setup gift trên UGMS, với latency <30 giây và retry mechanism cho failed syncs.

**FR2: Bi-directional Stock Adjustment Sync**
Post-campaign và post-approval, hệ thống tự động sync stock adjustments từ UHub về UGMS với complete audit trail (adjustment reasons, approver identity, timestamps, evidence photos). Support batch processing cho multiple adjustments và automatic rollback nếu UGMS API fails.

**FR3: RSA 2048-bit API Authentication**
Implement RSA 2048-bit digital signature authentication cho tất cả UGMS API calls, ensure data integrity và secure communication channel. Include token refresh mechanism và automatic re-authentication khi tokens expire.

**FR Group 2: Digital Confirmation Workflows**

**FR4: Agency Digital Confirmation Interface**
Agency Operations Manager có thể digital confirm nhận hàng từ Linfox delivery trực tiếp trên UHub với mobile-responsive interface, bao gồm:
- Photo upload cho delivery receipts (multiple photos, auto-compression)
- Quantity verification input với actual vs expected comparison
- Mandatory reason codes khi có discrepancy
- Digital signature capture
- Real-time inventory update sau confirmation

**FR5: Sales Store Inventory Confirmation**
Sales Representative có thể confirm gift inventory tại store level với:
- View current Allocation by store từ Agency distribution
- Quick confirmation buttons cho standard quantities (match allocation)
- Manual adjustment input với reason code requirements
- Photo documentation cho inventory discrepancies
- Real-time sync với central inventory system
- Timestamp recording cho audit trail

**FR6: Campaign Ready Validation Logic**
Hệ thống enforce "Campaign Ready Condition" - PG App chỉ allow gift distribution khi:
- Campaign status = "Active" (BU đã activate)
- All store allocations confirmed bởi Sales (hoặc approved via ticket)
- Agency warehouse confirmation completed
- No blocking tickets (critical discrepancies unresolved)
- Validation results hiển thị trên BU dashboard và PG App

**FR Group 3: Ticket System & Approval Workflows**

**FR7: Multi-level Ticket Creation and Management**
Agency và Sales có thể tạo tickets cho inventory discrepancies với:
- Ticket types: Agency Warehouse Discrepancy (deadline: 168 giờ trước live), Store Discrepancy (deadline: 48 giờ trước live)
- Mandatory fields: Actual quantity, Expected quantity, Reason codes, Evidence photos
- Automatic email notifications tới BU (based on Scheme ID → BU email mapping)
- Ticket status tracking: Open → Under Review → Approved/Rejected → Closed
- Deadline warnings và escalation alerts

**FR8: BU Level 1 Approval Workflow**
BU Team Lead có thể review và approve/reject tickets với:
- Dashboard hiển thị pending tickets sorted by deadline
- Detail comparison view (expected vs actual, với photos)
- Approve action: Cập nhật UGMS Giftcode/Quantity/Allocation, sync UHub
- Reject action: Mandatory comment field, notification về Agency/Sales
- SLA tracking: Agency tickets 24h, Store tickets 12h
- Approval history audit trail

**FR9: Level 2 Approval for High-value Adjustments**
Cho stock adjustments vượt threshold (to be defined, e.g., >10% variance hoặc >50 units), BU Level 1 phải raise ticket qua email để Level 2 (senior manager) approval trước khi Utop Admin có thể digital confirm. Include:
- Email template với adjustment details và business justification
- Evidence attachment (photos, reconciliation reports)
- Level 2 approval via email confirmation
- Utop Admin portal hiển thị Level 2 approval status
- Automatic UGMS-UHub sync sau khi Admin confirm

**FR Group 4: Reconciliation & Exception Management**

**FR10: Automated Reconciliation Engine**
Hệ thống hỗ trợ 3-way comparison giữa UGMS records ↔ UHub tracking (PG distribution) ↔ Physical inventory (Agency recall confirmation) với:
- Automated discrepancy detection với configurable tolerance thresholds
- Categorization: Quantity variance, Timing issues, Location mismatches
- GAP highlighting với visual indicators (Green=match, Yellow=minor variance, Red=major discrepancy)
- Bulk reconciliation processing cho post-campaign analysis
- Historical data retention cho trending analysis

**FR11: Exception Reporting and Resolution Dashboard**
BU Team có thể view và manage exceptions với:
- Exception dashboard với filtering (campaign, store, severity, date range)
- Root cause analysis suggestions based on historical patterns
- Assignment workflow (assign to Agency/Sales/Utop Admin for resolution)
- Status tracking: Open → In Progress → Resolved → Verified
- Resolution time metrics và performance KPIs
- Exception summary reports exportable cho management review

**FR12: PowerBI Dashboard Integration**
Hệ thống cung cấp data feed cho PowerBI với scheduled refresh 3 lần/ngày (8AM, 2PM, 6PM) bao gồm:
- Gift usage analytics (distributed vs allocated, by campaign/store/date)
- Remaining inventory real-time status
- Campaign performance metrics (distribution rate, waste %, utilization)
- Store-level inventory drill-down capabilities
- Exception summary và resolution status
- Historical trend analysis cho inventory forecasting

**FR Group 5: Gift Recall & Compliance**

**FR13: Gift Recall Workflow Framework**
Hệ thống support Gift Recall process với:
- Recall initiation workflow (BU triggers recall cho specific campaign/stores)
- Automated notifications tới Agency với recall requirements và timeline (5 ngày từ end-campaign)
- Recall tracking dashboard (collection progress, outstanding items by store)
- Agency digital confirmation của recalled items với photo evidence
- Reconciliation report: Initial allocation - Distributed - Recalled = Variance
- Integration với ticket system cho recall discrepancies

**FR14: BU Review & Confirm Agency Reconciliation**
BU Team Lead có thể review và confirm Agency reconciliation results với:
- Dashboard hiển thị pending Agency reconciliation submissions post-campaign
- Detail comparison interface (expected vs actual inventory)
- Approve action: Finalize inventory status, mark gifts available for re-use
- Reject action: Mandatory feedback, request Agency re-check/correction
- Notification system tới Agency khi reconciliation approved/rejected
- Audit trail cho final inventory status decisions

**FR Group 6: Mobile & User Experience**

**FR15: Progressive Web App (PWA) for Field Teams**
Agency và Sales interfaces implement PWA capabilities với:
- Mobile-first responsive design (optimized cho smartphones/tablets)
- App-like experience (install to home screen, full-screen mode)
- Offline-capable cho viewing recent data (read-only)
- Camera integration cho photo upload
- Touch-optimized UI components
- Fast loading với caching strategies

**FR16: Role-based Access Control (RBAC)**
Hệ thống enforce permissions theo user roles:
- Agency Operations Manager: Warehouse confirmations, tickets, reconciliation
- Sales Representative: Store confirmations, store tickets
- PG: Gift distribution (view only gift inventory status)
- BU Team Member: View dashboards, approve tickets (Level 1)
- BU Team Lead: All BU permissions + Level 2 approvals coordination
- Utop Admin: Campaign setup, Level 2 ticket confirmation, system configuration
- Audit log cho all permission-based actions

**FR17: Comprehensive Audit Trail**
Mọi transaction trong hệ thống có immutable audit trail bao gồm:
- User identity (who), Action type (what), Timestamp (when), Location/context (where)
- Before/after state cho data changes
- Evidence attachments (photos, documents)
- Approval chain cho multi-level workflows
- Sync status với UGMS (success/failure, retry attempts)
- Audit trail exportable cho compliance reporting và internal audits

### Non-Functional Requirements

**NFR1: API Response Time & System Performance**
- Standard operations (view dashboards, confirmations, ticket creation): API response time <2 giây
- Complex reconciliation queries (3-way comparison, bulk processing): <5 giây
- Photo upload processing (compression + storage): <3 giây per image
- PowerBI data feed generation: <10 giây per refresh cycle
- Page load time cho mobile interfaces: <3 giây on 4G connection
- **Measurement:** 95th percentile response times monitored via Azure Application Insights

**NFR2: Concurrent User Support**
- Hệ thống hỗ trợ tối thiểu **50+ concurrent users** trong pilot phase (2-3 Agency, 5-10 Sales, 10-15 PG, 10-20 BU/Admin)
- No performance degradation under pilot load
- Scalability plan cho 200+ concurrent users khi full rollout
- Session management với automatic timeout sau 30 phút inactive
- **Measurement:** Load testing với simulated concurrent users, monitor resource utilization

**NFR3: UGMS-UHub Data Sync Latency**
- Sync latency <30 giây cho UGMS → UHub data updates
- Guaranteed delivery với retry mechanism (3 attempts với exponential backoff)
- Sync failure alerts tới ITU team và BU lead within 1 phút
- Bi-directional sync UHub → UGMS post-approval: <1 phút
- Sync status tracking dashboard cho monitoring team
- **Measurement:** Timestamp tracking từ UGMS trigger đến UHub data availability

**NFR4: Mobile Device Compatibility**
- Mobile-responsive interface tương thích **95%+ thiết bị** trong pilot user pool:
  - iOS 13+ (iPhone 8 trở lên)
  - Android 9+ (Samsung, Oppo, Xiaomi mainstream models từ 2019)
- Browser support: Chrome Mobile, Safari Mobile (latest 2 versions)
- Screen size optimization: 375px (iPhone SE) to 428px (iPhone Pro Max) và Android equivalents
- Touch target minimum 44px × 44px (WCAG accessibility guidelines)
- Stable internet connectivity required (minimum 3G, recommend 4G)
- **Measurement:** Device analytics tracking, cross-device testing matrix

**NFR5: System Uptime & Reliability**
- Target **99.5% uptime** during campaign periods (định nghĩa: 7 ngày trước campaign start đến 10 ngày sau campaign end)
- Planned maintenance windows: Off-peak hours only (1AM-4AM Vietnam time), maximum 3 giờ/tháng
- Automatic failover capabilities cho critical services (UGMS sync, ticket notifications)
- Recovery Time Objective (RTO): <2 giờ cho major outages
- Recovery Point Objective (RPO): <15 phút data loss maximum
- **Measurement:** Uptime monitoring dashboard, incident tracking, SLA reports

**NFR6: Comprehensive Audit Trail & Data Integrity**
- **100% transactions** có complete audit trail với immutable logging
- Audit log retention: 3 năm minimum (compliance requirement)
- Data encryption at rest: AES-256 cho database và file storage
- Data encryption in transit: TLS 1.2+ cho all API communications
- Database backup: Daily full backup + hourly incremental backups
- Backup retention: 30 ngày rolling window
- **Measurement:** Audit trail completeness checks, encryption validation scans

**NFR7: Zero Data Loss trong UGMS-UHub Integration**
- Bi-directional sync cho stock adjustments với guaranteed consistency
- Automatic rollback capabilities nếu UGMS API fails mid-transaction
- Real-time consistency validation: Compare UGMS ↔ UHub data checksums every 6 giờ
- Conflict resolution logic cho concurrent updates (last-write-wins với audit trail)
- Manual reconciliation tool cho rare edge cases
- **Measurement:** Sync success rate monitoring, data consistency audit reports

**NFR8: Authentication & Authorization Security**
- RSA 2048-bit authentication cho UGMS API integration
- OAuth 2.0 với JWT tokens cho user sessions
- Token expiry: 8 giờ, with automatic refresh mechanism
- Multi-factor authentication (MFA) optional cho BU Team Lead và Utop Admin
- Password policy: Minimum 8 characters, complexity requirements
- Session security: HTTPOnly cookies, CSRF protection
- **Measurement:** Security audit logs, penetration testing reports

**NFR9: Mobile Network Resilience**
- Offline-capable cho viewing recent data (last 24 hours cached, read-only)
- Automatic retry với exponential backoff cho failed network requests
- Clear user feedback khi offline (banner notification, disabled actions)
- Queue mechanism cho photo uploads when network unstable (retry when reconnected)
- Progressive image loading với low-res previews
- **Measurement:** Network error recovery success rate, user experience feedback

**NFR10: Usability & User Experience**
- Mobile interfaces pass WCAG 2.1 AA compliance (accessibility standards)
- User training requirement: <2 giờ cho Agency/Sales field teams
- Task completion time benchmarks:
  - Agency warehouse confirmation: <5 phút
  - Sales store confirmation: <3 phút
  - BU ticket approval: <10 phút review + decision
- Error messages in Vietnamese với clear actionable guidance
- Help documentation embedded trong app (contextual tooltips)
- **Measurement:** User satisfaction surveys (target ≥4/5), task completion time tracking

**NFR11: Data Privacy & Compliance**
- Personal data handling compliant với Vietnam Personal Data Protection regulations
- User consent management cho photo uploads và location data (if implemented)
- Data retention policies clearly documented
- Right to erasure support (user data deletion requests)
- Privacy policy accessible within app
- Annual privacy audit preparation
- **Measurement:** Compliance checklist validation, privacy audit results

**NFR12: Integration Testing & Quality Assurance**
- Minimum **80% code coverage** cho unit tests
- Integration testing cho all UGMS API endpoints với mock và live environments
- End-to-end testing cho complete workflows (B1-B6)
- Mobile UI testing trên diverse devices (minimum 5 representative devices)
- Performance testing under pilot load scenarios
- Security testing: OWASP Top 10 vulnerabilities scan
- **Measurement:** Test coverage reports, bug density metrics, QA sign-off criteria

## User Journeys

### Journey 1: Agency Operations Manager - Warehouse Confirmation Flow

**Persona:** Minh - Agency Operations Manager tại ABC Logistics
**Context:** Nhận delivery 500 units physical gifts cho campaign "Tết 2026" từ Linfox, cần digital confirm trên UHub
**Goal:** Confirm nhận hàng chính xác, xử lý discrepancy nếu có, enable store allocation

**Pre-conditions:**
- BU đã setup gift trên UGMS và sync sang UHub
- ITU Log Form đã được gửi tới Utop Admin
- Utop Admin đã init campaign với Allocation by store
- Minh có account và permissions cho Agency warehouse confirmation

**Journey Steps:**

**1. Receive Delivery Notification (Offline)**
- Linfox driver giao hàng tại warehouse ABC
- Minh nhận delivery note với expected quantity: 500 units, Giftcode: GC2026TET001
- Physical inspection: Count actual units, check for damages

**2. Access UHub Mobile Interface**
- Minh mở UHub trên smartphone (iPhone 11)
- Login với số điện thoại + SMS OTP
- Navigate tới "Agency Warehouse Confirmation" section
- View pending deliveries list: Campaign "Tết 2026" - Expected 500 units

**3. Decision Point: Delivery Matches Expected?**

**Path A: Full Match (Happy Path - 70% cases)**
- Actual quantity = 500 units, no damages
- Minh clicks "Confirm Receipt - Full Match"
- System prompts: "Upload delivery receipt photo (optional but recommended)"
- Minh uploads 2 photos: Delivery note + Stacked boxes overview
- Digital signature capture on screen
- Clicks "Submit Confirmation"
- **System Actions:**
  - Real-time inventory update: Agency WH inventory = 500 units
  - Send notification tới BU: "ABC Logistics confirmed 500 units GC2026TET001"
  - Enable "Allocation by Store" export for Minh
  - Update campaign status: "Warehouse Confirmed ✓"
- **User sees:** Success message "Confirmation recorded. You can now allocate to stores."

**Path B: Partial Discrepancy (20% cases)**
- Actual quantity = 480 units (20 units short), no damages
- Minh clicks "Report Discrepancy"
- System shows form:
  - Expected: 500 units (pre-filled)
  - Actual: [Input field] → Minh enters 480
  - Discrepancy: -20 units (auto-calculated)
  - Reason code: [Dropdown] → Minh selects "Delivery shortage"
  - Evidence photos: [Upload] → Minh uploads delivery note + package count photo
  - Additional notes: [Text] → "Driver confirmed shortage from DC, will report back"
- Clicks "Create Ticket"
- **System Actions:**
  - Create ticket #TKT-20260106-001 with status "Open"
  - Calculate deadline: Current time + 168 hours = Jan 13, 2026 11:30 AM
  - Send auto-email tới BU (based on Scheme ID → BU email mapping)
  - Email subject: "🚨 Agency Warehouse Discrepancy - Tết 2026 - Deadline: Jan 13"
  - Temporary inventory: 480 units (pending approval)
- **User sees:** "Ticket created. BU Team will review within 24 hours. Deadline: Jan 13, 11:30 AM"

**Path C: Major Discrepancy or Damage (10% cases)**
- Actual quantity = 350 units, 150 units damaged/missing
- Minh clicks "Report Major Issue"
- Similar form với higher severity flags
- **System Actions:**
  - Create high-priority ticket
  - Immediate escalation email tới BU Lead + Utop Admin
  - Campaign status: "Warehouse Issue - Blocked ⚠️"
  - PG App blocks gift distribution until resolved

**4. Post-Confirmation: Allocation by Store**

**If Path A (approved):**
- Minh navigates to "Allocation by Store"
- Views pre-defined allocation từ ITU Log Form:
  - Store A (Big C Thảo Điền): 150 units
  - Store B (Coopmart Cống Quỳnh): 200 units
  - Store C (Lotte Nowzone): 150 units
- Minh confirms allocation matches warehouse capacity
- Clicks "Finalize Allocation"
- **System Actions:**
  - Lock allocation (cannot change without ticket)
  - Notify Sales reps tại 3 stores: "Your allocation ready for confirmation"
  - Update campaign progress: "Ready for Store Delivery"

**If Path B (ticket pending):**
- Allocation temporarily shows 480 units
- System suggests: "Adjust allocation by -20 units or wait for BU approval"
- Minh waits for BU decision (next 24h)
- **BU approves adjusted quantity:**
  - Allocation auto-updated: Store A: 144, Store B: 192, Store C: 144 (proportional reduction)
  - Minh receives notification: "Ticket approved. Proceed with allocation."
  - Minh finalizes allocation như Path A

**Pain Points Addressed:**
✅ Thay thế phone/Viber confirmation bằng digital workflow với evidence (PP2)
✅ Real-time inventory tracking thay vì manual Excel (PP2)
✅ Automated ticket creation với SLA tracking thay vì email chaos (PP4)
✅ Clear deadline management (168h) cho Agency adjustments (New process improvement)

---

### Journey 2: Sales Representative - Store Inventory Confirmation Flow

**Persona:** Lan - Sales Representative phụ trách Big C Thảo Điền
**Context:** Nhận allocation 150 units từ ABC Logistics cho campaign "Tết 2026", cần confirm trước go-live
**Goal:** Verify physical inventory matches allocation, ensure campaign ready

**Pre-conditions:**
- Agency đã finalize Allocation by Store (150 units allocated to Big C Thảo Điền)
- Physical delivery từ Agency warehouse tới store completed
- Lan có account và permissions cho Store confirmation
- Campaign go-live: Jan 20, 2026 (5 ngày nữa)

**Journey Steps:**

**1. Receive Allocation Notification**
- Lan nhận SMS/app notification: "Your allocation for Tết 2026 ready: 150 units. Please confirm before Jan 18, 8AM (48h deadline)"
- Lan coordinates với store manager để verify physical stock

**2. Physical Count at Store**
- Lan counts physical gifts tại storage area
- Finds: 145 units (5 units short)
- Checks delivery notes từ Agency: Expected 150 units

**3. Access UHub and Review Allocation**
- Lan login UHub mobile app
- Navigate to "My Store Confirmations"
- Views pending confirmation:
  - Campaign: Tết 2026
  - Allocated: 150 units
  - Status: Pending Confirmation
  - Deadline: Jan 18, 8AM (42 hours remaining) ⚠️

**4. Decision Point: Inventory Matches Allocation?**

**Path A: Full Match (60% cases)**
- If Lan counted 150 units matching allocation
- Clicks "Quick Confirm - Match Allocation"
- System prompts optional photo upload
- Lan uploads 1 photo of stacked gifts
- Clicks "Submit"
- **System Actions:**
  - Update store inventory: Big C Thảo Điền = 150 units confirmed ✓
  - Check campaign readiness:
    - Agency confirmed ✓
    - Store A confirmed ✓
    - Store B pending
    - Store C pending
    - → Campaign status: "Partially Ready (1/3 stores)"
  - Notify BU dashboard: "Big C Thảo Điền confirmed"
- **User sees:** "Confirmation successful. Campaign will activate when all stores ready."

**Path B: Minor Discrepancy - Create Ticket (30% cases)**
- Actual count = 145 units (-5 variance)
- Lan clicks "Report Discrepancy"
- Form appears:
  - Allocated: 150 units
  - Actual: [Input] → 145
  - Variance: -5 units (auto-calculated, <10% threshold)
  - Reason: [Dropdown] → "Delivery shortage from Agency"
  - Evidence: [Upload] → Photo of inventory + delivery note
  - Notes: "Checked with Agency driver, will investigate"
- Calculate deadline: 48 hours from now = Jan 18, 8AM
- Clicks "Create Ticket"
- **System Actions:**
  - Create ticket #TKT-20260116-002 status "Open"
  - Email BU Team: "Store discrepancy - Big C Thảo Điền - 48h deadline"
  - Temporary inventory: 145 units
  - Campaign status: "Store Issue - Under Review"
- **User sees:** "Ticket created. BU will review within 12 hours."

**5. BU Reviews Ticket (Within 12 hours)**
- BU Team Lead reviews ticket in dashboard
- Options:
  - **Approve 145 units:** Accept shortage, proceed with reduced allocation
  - **Reject & Request Investigation:** Lan must re-count or coordinate với Agency
  - **Escalate to Level 2:** If high-value adjustment

**BU Decision Path: Approve 145 units**
- BU clicks "Approve" with comment: "Shortage acceptable, Agency will compensate in next delivery"
- **System Actions:**
  - Update UGMS: Big C Thảo Điền allocation = 145 units
  - Sync UHub: Confirmed inventory = 145 units
  - Notify Lan: "Ticket approved. Proceed with 145 units."
  - Update campaign readiness check
- **Lan sees:** Push notification "Your ticket approved. Campaign ready with 145 units."

**6. Campaign Ready Validation**
- All 3 stores confirmed (Store B: 200✓, Store C: 148✓ after similar ticket)
- **System performs Campaign Ready Check:**
  - ✅ Agency warehouse confirmed
  - ✅ All store allocations confirmed (with approved adjustments)
  - ✅ No blocking tickets
  - ✅ Campaign status = "Active"
- **PG App unlocked:** PGs can now scan QR codes và distribute gifts
- **BU dashboard shows:** "Campaign Tết 2026 READY - 493 units (adjusted from 500)"

**Pain Points Addressed:**
✅ Sales không cần Excel + phone calls, all digital (PP3)
✅ Real-time visibility vào store inventory (PP3)
✅ Campaign Ready validation prevents PG distribution với incomplete inventory (New FR6)
✅ Ticket deadline enforcement (48h) ensures timely resolution (New process)

---

### Journey 3: BU Team Lead - Post-Campaign Reconciliation & Recall Flow

**Persona:** Nam - BU Team Lead cho Home Care category
**Context:** Campaign "Tết 2026" ended Jan 25, cần reconcile inventory và recall unused gifts
**Goal:** Finalize campaign results, enable gift re-use cho next campaign

**Pre-conditions:**
- Campaign "Tết 2026" status: "Ended" (Jan 25, 2026)
- Initial allocation: 493 units (adjusted)
- PG distribution tracking: 450 units distributed during campaign
- Expected recall: 493 - 450 = 43 units remaining
- 5-day recall window: Jan 25-30, 2026

**Journey Steps:**

**1. Monitor PowerBI Dashboard During Campaign**
- Nam access PowerBI dashboard daily (refresh 8AM, 2PM, 6PM)
- Views metrics:
  - Distribution rate: 91% (450/493)
  - Store performance:
    - Big C Thảo Điền: 140/145 distributed (96%)
    - Coopmart Cống Quỳnh: 195/200 (97%)
    - Lotte Nowzone: 115/148 (78%) ⚠️ Low utilization
  - Exception alerts: 2 minor discrepancies auto-resolved
- Nam notes: Lotte underperforming, investigate for next campaign

**2. Initiate Gift Recall (Jan 25 - End of Campaign)**
- Nam navigates to "Campaign Management" → "Tết 2026"
- Clicks "Initiate Recall Process"
- System shows expected recall by store:
  - Big C: 5 units
  - Coopmart: 5 units
  - Lotte: 33 units
- Nam confirms: "Start Recall - 5 day deadline (Jan 30)"
- **System Actions:**
  - Send automated notifications tới ABC Agency:
    - "Recall required: 43 units from 3 stores by Jan 30"
    - Breakdown by store attached
    - Recall confirmation form link
  - Update campaign status: "Recall In Progress"
  - Start 5-day countdown timer

**3. Agency Collects Gifts from Stores (Jan 26-29)**
- ABC Agency coordinates với stores
- Physical collection:
  - Big C: Collected 5 units ✓
  - Coopmart: Collected 5 units ✓
  - Lotte: Collected 30 units (3 units discrepancy - damaged during campaign)

**4. Agency Reconciliation Submission (Jan 29)**
- Agency Operations Manager (Minh) accesses "Post-Campaign Reconciliation"
- Views auto-generated reconciliation report:
  - Initial allocation (after adjustments): 493 units
  - Distributed (from UHub PG tracking): 450 units
  - Expected recall: 43 units
  - Actual recall: 40 units
  - Variance: -3 units
- **Decision Point: Variance exists?**

**Path A: Perfect Match (40% cases)**
- If actual = expected (43 units)
- Minh clicks "Confirm Reconciliation - Full Match"
- Uploads photos of recalled gifts
- Digital signature
- Submit
- **Skip to Step 6 - BU Auto-Approval**

**Path B: Minor Variance - Create Reconciliation Ticket (60% cases)**
- Actual = 40 units (-3 variance)
- Minh clicks "Submit Reconciliation with Variance"
- Form:
  - Expected: 43
  - Actual: 40
  - Variance: -3 (6.9%)
  - Reason: [Dropdown] → "Damaged during campaign"
  - Evidence: Photos of damaged units + collection receipts
  - Store breakdown:
    - Big C: 5/5 ✓
    - Coopmart: 5/5 ✓
    - Lotte: 30/33 (3 damaged)
- Clicks "Submit for BU Review"
- **System Actions:**
  - Create reconciliation ticket #REC-20260129-001
  - Email Nam (BU Lead): "Agency reconciliation submitted - 3 units variance (damaged)"
  - Ticket includes complete audit trail:
    - Initial: 493 units
    - Distributed: 450 units (with PG timestamps)
    - Recalled: 40 units (with photos)
    - Variance: -3 units (damaged evidence)

**5. BU Review & Decision**
- Nam receives email, opens reconciliation dashboard
- Views detailed comparison:
  - UGMS records: 493 initial allocation
  - UHub tracking: 450 distributed (matched with PG app data ✓)
  - Physical recall: 40 units (Agency photos attached)
  - GAP: -3 units
- Reviews evidence photos: Clear damage visible
- **Decision Options:**
  - **Approve:** Accept variance, finalize campaign
  - **Reject:** Request Agency re-check or additional evidence
  - **Escalate Level 2:** If variance >10% or high-value

**Nam's Decision: Approve with threshold check**
- Variance: 3 units = 0.6% of initial allocation (below 2% threshold ✓)
- Damage evidence clear
- Nam clicks "Approve Reconciliation"
- Adds comment: "Damage acceptable. Lotte store needs process review for future campaigns."
- **System Actions:**
  - Finalize inventory status:
    - Total distributed: 450 units (final)
    - Total recalled: 40 units (available for re-use)
    - Total waste: 3 units (0.6%)
  - **Sync to UGMS:**
    - Update campaign final usage: 450 units
    - Update Agency inventory: +40 units available
    - Mark GC2026TET001: 40 units ready for next allocation
  - Mark campaign: "Reconciliation Complete ✓"
  - Notify Agency: "Reconciliation approved. 40 units available for re-use."

**6. Post-Reconciliation Analytics**
- Nam views final campaign report:
  - **Success Metrics:**
    - ✅ Distribution rate: 91% (target was 85%)
    - ✅ Waste rate: 0.6% (target <2%)
    - ✅ Reconciliation time: 2.5 hours (vs 40 hours previously) → **93% reduction**
    - ✅ Inventory accuracy: 99.4% (target ≥98%)
  - **Learnings:**
    - Lotte Nowzone underperformed (78% vs 96-97% other stores)
    - Action: Reduce Lotte allocation by 20% in next campaign
    - Re-usable gifts: 40 units saved (value ~20M VNĐ)
- Export report cho management presentation

**Pain Points Addressed:**
✅ Automated 3-way reconciliation (UGMS ↔ UHub ↔ Physical) giảm 93% time (PP4)
✅ Gift recall systematic với 5-day workflow thay vì ad-hoc (PP1)
✅ Digital approval với audit trail thay vì email threads (PP4)
✅ Re-use enablement: 40 units marked available cho next campaign (PP1 - reduce waste)

## UX Design Principles

### 1. Mobile-First Field Optimization

**Principle:** Optimize for one-handed mobile operation in warehouse và store environments với poor lighting và distractions.

**Application:**
- Primary actions (Confirm, Submit, Upload Photo) positioned in thumb-reach zone (bottom 1/3 of screen)
- Touch targets minimum 44px × 44px với generous spacing (8px minimum)
- High contrast UI (WCAG AA 4.5:1 ratio) cho outdoor visibility
- Large, bold fonts (minimum 16px body text, 20px+ headings) cho quick scanning
- Progressive disclosure: Show only essential info first, details on tap/expand

**Example:** Agency confirmation screen shows "Confirm Receipt" button prominently at bottom, với "Report Discrepancy" as secondary action.

---

### 2. One-Tap Confirmation for Happy Paths

**Principle:** Minimize friction cho most common scenarios (70-80% cases) với single-tap actions.

**Application:**
- "Quick Confirm - Match Allocation" button cho full-match scenarios
- Pre-filled forms với smart defaults (expected quantities, common reason codes)
- Bulk actions: "Confirm All" cho multiple pending items
- Skip optional fields unless user explicitly needs them
- Auto-save drafts để prevent data loss nếu interrupted

**Example:** Sales rep can confirm store inventory với 1 tap nếu matches allocation, optional photo upload sau confirmation.

---

### 3. Progressive Evidence Capture

**Principle:** Make photo documentation easy và non-blocking, but encourage compliance through smart nudges.

**Application:**
- Camera integration direct from confirmation screens (no app switching)
- "Recommended" tags on photo upload fields (not mandatory cho happy path)
- Auto-compression và thumbnail previews cho quick verification
- Batch photo upload với progress indicators
- Offline queue: Photos upload automatically when connection restored
- Smart prompts: "Add photo to strengthen your submission" cho discrepancy reports

**Example:** Agency discrepancy form shows "📷 Evidence Photos (Recommended)" với one-tap camera access.

---

### 4. Real-time Status Transparency

**Principle:** Users always know where they stand với clear status indicators và next actions.

**Application:**
- Traffic light colors: Green (completed/approved), Yellow (pending/under review), Red (issue/blocked)
- Status badges với icons: ✓ Confirmed, ⏳ Pending, ⚠️ Issue, 🚫 Blocked
- Progress bars cho multi-step workflows
- Real-time updates: "BU Team reviewing your ticket" với estimated response time
- Campaign Ready dashboard: Visual checklist showing which stores confirmed
- Notification center: Aggregated alerts với priority sorting

**Example:** BU dashboard shows campaign status card: "Tết 2026 - Partially Ready (2/3 stores confirmed ✓)" với visual progress.

---

### 5. Deadline-Driven Workflows

**Principle:** Surface time-sensitive tasks prominently với clear urgency indicators và auto-escalation.

**Application:**
- Countdown timers cho tickets: "42 hours remaining" với color coding (Green >24h, Yellow 12-24h, Red <12h)
- Sort by deadline by default trong task lists
- Push notifications at key milestones: 48h, 24h, 6h, 1h before deadline
- Auto-escalation visual cues: "⚡ Urgent - Deadline in 3 hours"
- Calendar integration: Add deadlines to user calendar
- Deadline extension requests with justification

**Example:** Sales rep sees "⚠️ Confirm by Jan 18, 8AM (6 hours remaining)" prominently on pending allocation.

---

### 6. Error Prevention over Error Handling

**Principle:** Design workflows to prevent errors rather than just handle them gracefully.

**Application:**
- Inline validation: Show errors immediately as user types (quantity must be ≤ allocation)
- Smart constraints: Disable "Create Ticket" until mandatory fields filled
- Confirmation dialogs for destructive actions: "Rejecting this ticket will notify Agency. Continue?"
- Auto-save drafts every 30 seconds
- Pre-flight checks: "Campaign Ready" validation before allowing PG distribution
- Guided flows: Wizard-style UI for complex workflows với progress steps

**Example:** Allocation screen shows "⚠️ Total: 494 units exceeds available 493" khi Agency over-allocates.

---

### 7. Contextual Help & Self-Service

**Principle:** Embed help where users need it, minimize training requirements.

**Application:**
- Tooltips on hover/tap: "ℹ️ Reason codes help us improve allocation accuracy"
- Contextual examples: "e.g., Delivery shortage from DC" in placeholder text
- Video tutorials embedded in workflow: 30-second clips showing "How to confirm warehouse delivery"
- FAQ accordion panels: Expand common questions inline
- Search-powered help center với Vietnamese content
- Chatbot integration for L1 support (future Phase 2)
- Role-based onboarding: Different quick-start guides for Agency vs Sales vs BU

**Example:** First-time Agency user sees "👋 New to warehouse confirmation? Watch 1-min tutorial" với skip option.

---

### 8. Offline-First Resilience

**Principle:** Gracefully handle poor connectivity common in warehouse và remote store environments.

**Application:**
- Cached data: Last 24 hours readable offline (read-only mode)
- Clear offline indicators: Banner "You're offline. Changes will sync when connected."
- Queue mechanism: Actions queued locally, auto-retry when online
- Progressive image loading: Low-res previews load first, full-res in background
- Smart sync: Prioritize critical data (ticket submissions) over nice-to-have (historical reports)
- Conflict resolution: Last-write-wins với audit trail if concurrent edits
- Network diagnostics: "Connection slow. Using cached data from 2 hours ago."

**Example:** Sales rep loses signal mid-confirmation, sees "✓ Saved locally. Will sync when connected" và can continue working.

---

### 9. Data Visualization over Data Tables

**Principle:** Use charts, graphs, và visual indicators to communicate insights faster than raw numbers.

**Application:**
- PowerBI dashboards: Bar charts cho store performance, pie charts cho distribution breakdown
- Sparklines: Mini-charts showing trends inline with numbers (distribution rate trending ↗)
- Heat maps: Color-code stores by performance (green=high utilization, red=low)
- Comparison views: Side-by-side expected vs actual với GAP highlighting
- Icon-driven metrics: 📊 91% distribution, 🎯 98% accuracy, ⏱️ 2.5h reconciliation
- Progressive detail: Summary cards → Drill-down charts → Detailed tables
- Export capabilities: Download charts as PNG for presentations

**Example:** BU dashboard shows store performance as horizontal bars: Big C 96% (green), Lotte 78% (yellow).

---

### 10. Consistent Design Language with UHub

**Principle:** Maintain visual và interaction consistency với existing UHub platform to reduce cognitive load.

**Application:**
- Reuse UHub component library: Buttons, cards, forms, navigation patterns
- Consistent color palette: Blue (primary actions), Green (success), Red (errors), Yellow (warnings)
- Same authentication flow: Phone number + SMS OTP như shopper login
- Unified navigation: E2E Gift as new section in existing UHub menu
- Shared notification system: Same bell icon, same notification format
- Brand consistency: Unilever logo, typography (existing UHub fonts)
- Responsive breakpoints aligned với UHub mobile design

**Example:** Agency confirmation screen uses same button styles, color scheme, và navigation header như PG App familiar to users.

## Epics

### Epic Overview

Dự án được breakdown thành **4 epics** phục vụ phased delivery cho MVP trong Q4 2025:

| Epic | Focus | Stories | Priority | Dependencies |
|------|-------|---------|----------|--------------|
| **E1: UGMS Integration Foundation** | API integration, data sync, security | 8 stories | P0 - Critical | None |
| **E2: Digital Confirmation Workflows** | Agency/Sales confirmations, tickets | 12 stories | P0 - Critical | E1 |
| **E3: Reconciliation & Analytics** | Post-campaign reconciliation, PowerBI | 10 stories | P1 - High | E1, E2 |
| **E4: Gift Recall & Compliance** | Recall workflows, audit trail | 6 stories | P2 - Medium | E2, E3 |

**Total: 36 stories** (within 12-40 range cho Level 3)

---

### Epic 1: UGMS Integration Foundation
**Goal:** Establish secure, real-time bi-directional integration với UGMS để enable automated data sync thay thế manual Excel workflows.

**Success Metrics:**
- UGMS-UHub sync latency <30 giây
- Zero data loss (100% sync success rate)
- RSA 2048-bit authentication implemented

**Stories (8 total, 21 story points):**

**E1.S1: UGMS API Authentication Setup** (3 pts, P0)
- Implement RSA 2048-bit digital signature authentication
- Token management (generation, refresh, expiry)
- Secure key storage trong Azure Key Vault
- Error handling cho authentication failures
- **Acceptance:** Successfully authenticate all UGMS API calls với RSA 2048-bit

**E1.S2: UGMS → UHub Data Sync (Schemes, Giftcodes)** (5 pts, P0)
- Sync Schemes: Campaign mechanics, start/end dates
- Sync Giftcodes: Unique identifiers, quantities
- Sync Customer: Key Account details
- Sync Ship_to: Delivery locations
- Trigger mechanism: Real-time on UGMS setup
- **Acceptance:** BU setup gift trên UGMS → Data appears trong UHub <30s

**E1.S3: Bi-directional Sync (UHub → UGMS)** (5 pts, P0)
- Post-approval stock adjustment sync UHub → UGMS
- Audit trail: Reasons, approver, timestamps, photos
- Batch processing support
- Automatic rollback nếu UGMS API fails
- **Acceptance:** BU approve ticket → UGMS updated within 1 minute với audit trail

**E1.S4: Sync Retry & Error Handling** (3 pts, P0)
- Exponential backoff retry mechanism (3 attempts)
- Dead letter queue cho failed syncs
- Alert system: Email ITU + BU lead within 1 minute
- Manual retry interface for admins
- **Acceptance:** Simulated UGMS downtime → System retries 3x, alerts sent, manual retry succeeds

**E1.S5: Data Consistency Validation** (2 pts, P1)
- Checksum comparison UGMS ↔ UHub every 6 hours
- Conflict resolution logic (last-write-wins với audit)
- Consistency audit reports
- Manual reconciliation tool for edge cases
- **Acceptance:** Detect data drift >0.1% và alert within 6h cycle

**E1.S6: Sync Status Monitoring Dashboard** (2 pts, P1)
- Real-time sync status tracking
- Sync latency metrics visualization
- Failed sync queue management
- Historical sync performance trends
- **Acceptance:** ITU can view sync status dashboard với <5s latency metrics

**E1.S7: Campaign Setup Automation (Utop Admin)** (1 pt, P1)
- Utop Admin portal: Init campaign from UGMS data
- Allocation by store configuration
- Campaign status management (Draft → Active → Ended)
- Email notification workflow setup
- **Acceptance:** Admin init campaign từ UGMS sync trong <2 minutes

**E1.S8: Integration Testing Suite** (0 pts - QA task, P0)
- Mock UGMS environment setup
- Integration test scenarios (happy path, failures, retries)
- Performance testing: 100 concurrent sync requests
- Security testing: Authentication bypass attempts
- **Acceptance:** 100% integration test pass rate, zero security vulnerabilities

---

### Epic 2: Digital Confirmation Workflows
**Goal:** Replace manual Excel/phone confirmations với digital mobile workflows cho Agency và Sales, với automated ticket system cho discrepancies.

**Success Metrics:**
- ≥80% confirmations qua digital workflow (không qua Excel/phone)
- Average confirmation time: Agency <5 min, Sales <3 min
- Ticket resolution SLA: Agency 24h, Store 12h

**Stories (12 total, 34 story points):**

**E2.S1: Agency Warehouse Confirmation - Happy Path** (5 pts, P0)
- Mobile-responsive confirmation interface
- View pending deliveries list
- "Confirm Receipt - Full Match" one-tap button
- Optional photo upload (delivery receipts)
- Digital signature capture
- Real-time inventory update
- **Acceptance:** Agency confirm 500 units delivery trong <3 taps, inventory updated real-time

**E2.S2: Agency Warehouse Confirmation - Discrepancy Flow** (5 pts, P0)
- "Report Discrepancy" form
- Actual vs Expected quantity comparison (auto-calculate variance)
- Mandatory reason codes dropdown
- Evidence photo upload (multi-photo support)
- Additional notes text field
- Auto-create ticket với 168h deadline
- **Acceptance:** Agency report 20-unit shortage → Ticket created, BU notified via email <1 min

**E2.S3: Sales Store Confirmation - Happy Path** (3 pts, P0)
- View allocation by store
- "Quick Confirm - Match Allocation" button
- Optional photo upload
- Real-time central inventory sync
- Timestamp audit trail
- **Acceptance:** Sales confirm 150 units tại store trong <2 taps, synced real-time

**E2.S4: Sales Store Confirmation - Discrepancy Flow** (3 pts, P0)
- Similar to E2.S2 but 48h deadline
- Store-specific reason codes
- Auto-email notification to BU
- Temporary inventory status (pending approval)
- **Acceptance:** Sales report 5-unit variance → Ticket created với 48h deadline

**E2.S5: Multi-level Ticket Management System** (5 pts, P0)
- Ticket types: Agency WH Discrepancy, Store Discrepancy
- Status tracking: Open → Under Review → Approved/Rejected → Closed
- Deadline calculation và countdown display
- Email notification engine (Scheme ID → BU mapping)
- Escalation alerts (24h, 12h, 6h before deadline)
- **Acceptance:** Ticket lifecycle từ creation → resolution tracked với status updates

**E2.S6: BU Level 1 Approval Dashboard** (5 pts, P0)
- Pending tickets dashboard (sorted by deadline)
- Detail comparison view (expected vs actual, photos)
- Approve action: Update UGMS + UHub sync
- Reject action: Mandatory comment, notify Agency/Sales
- SLA tracking visualization
- Approval history audit trail
- **Acceptance:** BU approve ticket → UGMS/UHub updated, Agency notified <2 min

**E2.S7: Level 2 Approval Workflow (High-value)** (2 pts, P1)
- Threshold configuration (>10% variance OR >50 units)
- Email template generation (adjustment details + justification)
- Evidence attachment (photos, reports)
- Utop Admin portal: View Level 2 approval status
- Auto-sync post Admin confirmation
- **Acceptance:** 15% variance triggers Level 2 workflow, email sent to senior manager

**E2.S8: Campaign Ready Validation Logic** (3 pts, P0)
- Validation rules engine:
  - Campaign status = "Active" ✓
  - All store allocations confirmed (or approved) ✓
  - Agency WH confirmed ✓
  - No blocking tickets ✓
- BU dashboard: Campaign Ready checklist
- PG App: Lock/unlock gift distribution based on validation
- **Acceptance:** Campaign passes all checks → PG App unlocked, BU sees "Ready" status

**E2.S9: Allocation by Store Management** (2 pts, P1)
- Agency view pre-defined allocation (from ITU Log Form)
- Allocation lock mechanism (prevent changes without ticket)
- Proportional adjustment logic (khi approved quantity < expected)
- Notify Sales reps when allocation finalized
- **Acceptance:** Agency finalize allocation → Sales notified, allocation locked

**E2.S10: Progressive Web App (PWA) Infrastructure** (1 pt, P1)
- PWA manifest configuration
- Service worker setup cho offline caching
- Install to home screen prompt
- App-like navigation (no browser chrome)
- **Acceptance:** Agency/Sales can install app to phone home screen, works offline (read-only)

**E2.S11: Mobile Camera Integration & Photo Upload** (2 pts, P1)
- Direct camera access from confirmation screens
- Auto-compression (target 500KB per image)
- Thumbnail preview generation
- Batch upload với progress indicators
- Offline queue: Auto-upload when connected
- **Acceptance:** Upload 5 photos từ mobile camera, auto-compressed, queued khi offline

**E2.S12: Role-based Access Control (RBAC)** (3 pts, P0)
- User role definitions (Agency, Sales, PG, BU, Admin)
- Permission matrix implementation
- Login flow: Phone + SMS OTP (reuse UHub auth)
- Session management (8h expiry, auto-refresh)
- Audit log: All permission-based actions
- **Acceptance:** Each role can only access permitted features, all actions logged

---

### Epic 3: Reconciliation & Analytics
**Goal:** Automate post-campaign reconciliation từ 40 giờ → 4 giờ thông qua 3-way comparison engine và PowerBI analytics.

**Success Metrics:**
- Reconciliation time ≤4 giờ/campaign (90% reduction)
- Inventory accuracy ≥98% (variance <2%)
- PowerBI refresh 3x/day with <10s latency

**Stories (10 total, 28 story points):**

**E3.S1: Automated Reconciliation Engine - 3-way Comparison** (5 pts, P0)
- Compare UGMS records ↔ UHub PG tracking ↔ Physical inventory
- Configurable tolerance thresholds (default 2%)
- Discrepancy categorization: Quantity, Timing, Location
- GAP highlighting: Green (match), Yellow (minor <2%), Red (major >2%)
- Bulk processing support (multiple campaigns)
- **Acceptance:** Run reconciliation cho campaign với 493 units → GAP report generated <5s

**E3.S2: Exception Detection & Reporting** (3 pts, P1)
- Automated exception detection rules
- Exception dashboard với filtering (campaign, store, severity, date)
- Root cause analysis suggestions (based on historical patterns)
- Assignment workflow (assign to Agency/Sales/Admin)
- Status tracking: Open → In Progress → Resolved → Verified
- **Acceptance:** System detect 3-unit variance → Exception raised, assigned to Agency

**E3.S3: Reconciliation Dashboard for BU** (3 pts, P1)
- Pending reconciliation submissions list
- Detail comparison interface (UGMS vs UHub vs Physical)
- Side-by-side expected vs actual với visual GAP indicators
- Evidence photo viewer
- Variance approval threshold logic (auto-approve <2%, manual >2%)
- **Acceptance:** BU view reconciliation dashboard, approve 0.6% variance in 1 click

**E3.S4: Historical Data Retention & Trending** (2 pts, P2)
- Store reconciliation history (3 năm retention)
- Trending analysis: Waste rate over time, accuracy by campaign
- Predictive insights: "Lotte consistently underperforms by 15%"
- Exportable historical reports (CSV, PDF)
- **Acceptance:** View 12-month waste trend chart, export to CSV

**E3.S5: PowerBI Data Feed Integration** (5 pts, P0)
- Scheduled data refresh: 8AM, 2PM, 6PM (3x/day)
- Data schema design cho PowerBI consumption:
  - Gift usage analytics (distributed vs allocated)
  - Remaining inventory real-time
  - Campaign performance metrics
  - Store-level drill-down data
  - Exception summary
- API endpoint cho PowerBI connector
- Refresh monitoring và error alerts
- **Acceptance:** PowerBI dashboard refreshes tại 8AM, data latency <10s

**E3.S6: PowerBI Dashboard Design - BU View** (3 pts, P1)
- Campaign overview card (distribution rate, waste %, utilization)
- Store performance bar charts (sortable by rate)
- Heat map: Store utilization by geography
- Exception summary table với drill-down
- Time-series: Distribution progress during campaign
- Export capabilities (PNG charts for presentations)
- **Acceptance:** BU view campaign performance dashboard, drill-down to store level, export chart

**E3.S7: Real-time Inventory Sync & Accuracy Tracking** (2 pts, P0)
- Real-time inventory updates (Agency confirm → UHub → PowerBI)
- Inventory accuracy calculation: (Physical / System) × 100%
- Accuracy dashboard với threshold alerts (target ≥98%)
- Store-level accuracy tracking
- **Acceptance:** Inventory accuracy tracked real-time, alert when <98%

**E3.S8: Performance Metrics & KPIs** (2 pts, P1)
- Reconciliation time tracking (start to approval)
- Resolution time metrics cho tickets và exceptions
- User engagement metrics (digital workflow adoption %)
- Campaign success metrics aggregation
- Management reports: Weekly/Monthly summaries
- **Acceptance:** Generate weekly report: 91% distribution rate, 2.5h reconciliation avg

**E3.S9: Export & Reporting Capabilities** (2 pts, P2)
- Exception summary reports (exportable cho management)
- Reconciliation audit reports (compliance)
- Campaign final reports (post-approval)
- Scheduled email reports (weekly to BU leads)
- Format support: PDF, CSV, Excel
- **Acceptance:** Export reconciliation report to PDF, email to BU lead

**E3.S10: Audit Trail & Compliance Logging** (1 pt, P0)
- Immutable audit trail: Who, What, When, Where
- Before/after state logging cho data changes
- Evidence attachment linking
- Approval chain tracking
- Sync status logging (UGMS success/failure)
- Exportable audit logs cho compliance
- **Acceptance:** 100% transactions logged, export audit trail for 6-month period

---

### Epic 4: Gift Recall & Compliance
**Goal:** Enable systematic gift recall post-campaign với 5-day workflow, reduce waste thông qua gift re-use mechanism.

**Success Metrics:**
- ≥1 pilot recall workflow completed before Dec 2025
- Recall completion rate ≥90% within 5-day window
- Re-usable gifts marked available cho next campaigns

**Stories (6 total, 15 story points):**

**E4.S1: Gift Recall Initiation Workflow** (3 pts, P1)
- BU trigger recall cho specific campaign/stores
- Expected recall calculation: Allocation - Distributed
- Store-level recall breakdown
- Automated notifications tới Agency (email + app)
- Recall requirements và timeline (5 ngày) communication
- 5-day countdown timer activation
- **Acceptance:** BU initiate recall → Agency notified với breakdown, timer starts

**E4.S2: Recall Tracking Dashboard** (3 pts, P1)
- Collection progress visualization by store
- Outstanding items tracking
- Daily reminder notifications (D-4, D-3, D-2, D-1)
- Escalation alerts khi <24h remaining
- Store completion status (collected vs pending)
- **Acceptance:** Agency view recall progress: 2/3 stores completed, 1 pending

**E4.S3: Agency Recall Confirmation Interface** (3 pts, P1)
- Digital confirm recalled items by store
- Photo evidence upload (recalled gifts)
- Actual vs Expected recall comparison
- Variance explanation (if actual ≠ expected)
- Submit for BU review
- **Acceptance:** Agency confirm recall 40/43 units với photos, variance explanation

**E4.S4: BU Review & Approve Recall Reconciliation** (2 pts, P1)
- Dashboard: Pending recall reconciliations
- Detail view: Initial - Distributed - Recalled = Variance
- Approve action: Finalize inventory, mark for re-use
- Reject action: Request re-check, feedback to Agency
- Notification system (approval/rejection)
- **Acceptance:** BU approve recall reconciliation → 40 units marked "Available for re-use"

**E4.S5: Gift Re-use Mechanism** (2 pts, P2)
- Mark recalled gifts as "Available" trong inventory
- Link to Agency warehouse inventory
- Re-use allocation planning cho next campaign
- Historical re-use tracking (cost savings)
- **Acceptance:** 40 re-used units allocated to next campaign, savings tracked

**E4.S6: Recall Compliance & Reporting** (2 pts, P2)
- Recall completion reports (per campaign)
- Legal compliance tracking (Key Account agreements)
- Waste reduction metrics (re-use rate)
- Audit trail: Complete recall lifecycle
- Annual compliance summary reports
- **Acceptance:** Generate recall compliance report: 90% completion, 40 units re-used, ~20M VNĐ saved

---

## Epic Dependencies & Phasing

**Phase 1 (Months 1-2): Foundation** → E1 complete
- UGMS integration stable
- Authentication working
- Data sync operational

**Phase 2 (Months 2-3): Core Workflows** → E2 complete (depends on E1)
- Digital confirmations live
- Ticket system operational
- Campaign Ready validation working

**Phase 3 (Month 3-4): Analytics** → E3 complete (depends on E1, E2)
- Reconciliation engine operational
- PowerBI dashboards live
- Real-time metrics available

**Phase 4 (Month 4): Recall & Optimization** → E4 complete (depends on E2, E3)
- Recall workflow operational
- Re-use mechanism active
- Full compliance tracking

**MVP Launch: Dec 2025** - All 4 epics complete, pilot users onboarded

---

## Story Point Distribution

| Epic | Stories | Story Points | % of Total |
|------|---------|--------------|------------|
| E1: UGMS Integration | 8 | 21 | 21% |
| E2: Digital Confirmations | 12 | 34 | 35% |
| E3: Reconciliation & Analytics | 10 | 28 | 29% |
| E4: Gift Recall | 6 | 15 | 15% |
| **Total** | **36** | **98** | **100%** |

**Team Capacity Estimate:**
- Team: 1 FE, 1 BE, 1 QA
- Sprint velocity: ~20-25 points/2-week sprint
- Timeline: 4 sprints × 2 weeks = 8 weeks (~2 months) + 1 month buffer
- **Total: ~3 months development** cho MVP (Sept-Nov 2025, Dec 2025 pilot launch)

## Out of Scope

The following features are explicitly **out of scope** for MVP (Dec 2025) and deferred to Phase 2 (2026+):

### Phase 2 Features (Post-MVP)

**1. Advanced AI/ML Capabilities**
- Predictive allocation algorithms (AI-based demand forecasting)
- Anomaly detection for fraud prevention
- Computer vision for automated gift counting từ photos
- Chatbot L1 support automation
- Smart recommendation engine cho optimal allocation

**2. Extended Integration & Automation**
- Direct integration với Key Account systems (beyond UGMS)
- Linfox logistics API integration (real-time delivery tracking)
- SAP integration cho financial reconciliation
- Automated invoice generation và matching
- Blockchain-based gift provenance tracking

**3. Advanced Analytics & Reporting**
- Predictive analytics dashboard (forecast waste trends)
- Machine learning insights cho allocation optimization
- Advanced geospatial analytics (store clustering)
- Real-time sentiment analysis từ PG feedback
- Custom report builder với drag-and-drop

**4. Extended User Experience**
- Native mobile apps (iOS/Android) thay vì PWA
- Multi-language support (English, Vietnamese, regional languages)
- Dark mode UI theme
- Voice commands cho hands-free warehouse operations
- AR visualization cho gift allocation planning

**5. Extended Compliance & Governance**
- Automated regulatory reporting (customs, tax)
- Advanced fraud detection workflows
- Legal contract management cho Key Account agreements
- Automated compliance audit scheduling
- GDPR/international privacy law compliance (nếu expand ra ngoài Vietnam)

### Explicitly Excluded from All Phases

**1. Financial Management**
- Campaign budget management và tracking (owned by Finance team)
- Payment processing cho gift purchases (handled by Procurement)
- Cost allocation và chargeback workflows (Finance responsibility)

**2. Gift Sourcing & Procurement**
- Supplier management và negotiations (Procurement owns)
- Gift design và manufacturing oversight (Brand team owns)
- Quality control tại manufacturing level (Supply Chain owns)

**3. Campaign Marketing & Execution**
- Campaign creative development (Marketing team owns)
- Shopper engagement mechanics (UHub shopper-facing features already exist)
- PG training và performance management (HR/Operations owns)
- In-store promotional material management (Marketing owns)

**4. Store Operations Management**
- General store inventory management (beyond gifts)
- Store staff scheduling và management
- Point-of-sale system integration
- Store performance analytics (beyond gift distribution)

### Technical Constraints for MVP

**1. Scale Limitations**
- MVP supports pilot: 2-3 Agencies, 5-10 Sales, 10-15 PG, 10-20 BU/Admin
- Full scale (200+ concurrent users) requires infrastructure upgrade post-MVP
- PowerBI refresh limited to 3x/day (real-time streaming deferred)

**2. Platform Limitations**
- Mobile web only (PWA), no native apps
- Single region deployment (Vietnam only)
- Vietnamese language only (no i18n)
- Manual Level 2 approval via email (no automated workflow)

**3. Integration Limitations**
- UGMS integration only (no other enterprise systems)
- Email notifications only (no SMS, push notifications to external apps)
- Photo evidence manual review (no AI auto-validation)

---

## Next Steps

### Immediate Actions (Week 1-2)

**1. Stakeholder Validation**
- [ ] Present PRD to BU Team Leads (Home Care, Personal Care, Foods)
- [ ] Review goals and success metrics với Senior Management
- [ ] Validate MVP scope và timeline với ITU team
- [ ] Confirm pilot user selection (2-3 Agencies, 5-10 Sales, 10-15 PG)

**2. Technical Architecture Planning**
- [ ] Route to `/bmad:bmm:workflows:3-solutioning` for architecture design
- [ ] Architect review: System design, integration patterns, data models
- [ ] Infrastructure assessment: Azure resources, scaling strategy
- [ ] UGMS API documentation review và technical feasibility study

**3. Team Mobilization**
- [ ] Onboard development team (1 FE, 1 BE, 1 QA)
- [ ] Setup development environment (Azure DevOps, test environments)
- [ ] Sprint planning: Map 36 stories to 4 sprints (2-week sprints)
- [ ] Establish QA strategy và testing environments

### Development Phase (Month 1-3: Sept-Nov 2025)

**Sprint 1 (Weeks 1-2): Foundation**
- Complete Epic 1: UGMS Integration (8 stories, 21 pts)
- Setup CI/CD pipeline
- Establish code review và testing protocols

**Sprint 2 (Weeks 3-4): Core Workflows Part 1**
- Start Epic 2: Digital Confirmations (stories E2.S1-S6, ~18 pts)
- Agency confirmation flows
- Ticket management system foundation

**Sprint 3 (Weeks 5-6): Core Workflows Part 2**
- Complete Epic 2 (stories E2.S7-S12, ~16 pts)
- Sales confirmation flows
- Campaign Ready validation
- RBAC implementation

**Sprint 4 (Weeks 7-8): Analytics & Recall**
- Epic 3: Reconciliation & Analytics (10 stories, 28 pts)
- Epic 4: Gift Recall (6 stories, 15 pts)
- Integration testing và bug fixes

### Pre-Launch Activities (Month 4: Nov-Dec 2025)

**Week 9-10: UAT & Training**
- [ ] User Acceptance Testing với pilot users
- [ ] Training sessions: Agency (2h), Sales (2h), BU (4h)
- [ ] Documentation: User guides, video tutorials (Vietnamese)
- [ ] Helpdesk setup: L1/L2 support readiness

**Week 11-12: Pilot Launch Preparation**
- [ ] Production deployment checklist validation
- [ ] Data migration: Sync pilot campaigns từ UGMS
- [ ] Go-live readiness review: Technical, operational, support
- [ ] Communication plan: Announce to pilot users

**Week 13: MVP Launch (Target: Dec 2025)**
- [ ] Soft launch: Enable access cho pilot users
- [ ] Monitor system performance và user adoption
- [ ] Daily standups cho rapid issue resolution
- [ ] Collect feedback for iteration planning

### Post-Launch (Month 5+: Jan 2026+)

**Week 14-16: Stabilization & Feedback**
- [ ] Monitor success metrics: 80% digital adoption, 4h reconciliation, 98% accuracy
- [ ] Collect user feedback và pain points
- [ ] Bug fixes và minor enhancements
- [ ] Prepare lessons learned document

**Month 6+: Scale Planning**
- [ ] Evaluate MVP success against goals
- [ ] Plan full rollout: All agencies, stores, BU teams
- [ ] Phase 2 feature prioritization (AI/ML, advanced analytics)
- [ ] Infrastructure scaling for 200+ concurrent users

### Key Decisions Required

**Decision 1: Pilot Campaign Selection**
- **Owner:** BU Team Leads
- **Timeline:** Week 1
- **Question:** Which 2-3 campaigns will pilot the system? (Recommend: Mix of high-value và high-complexity campaigns)

**Decision 2: Level 2 Approval Threshold**
- **Owner:** Senior Management + BU Leads
- **Timeline:** Week 2
- **Question:** What variance threshold triggers Level 2 approval? (Recommend: >10% OR >50 units)

**Decision 3: UGMS API Access & Credentials**
- **Owner:** ITU + UGMS Team
- **Timeline:** Week 1
- **Question:** Provision API access, RSA keys, test environment

**Decision 4: PowerBI Workspace Setup**
- **Owner:** Analytics Team + ITU
- **Timeline:** Week 3
- **Question:** Create dedicated workspace, configure refresh schedules, permissions

### Success Criteria for MVP

Before declaring MVP success, validate:

✅ **Functional Completeness**
- All 36 stories delivered và accepted
- 4 epics complete với acceptance criteria met
- Zero P0/P1 bugs in production

✅ **Performance Metrics**
- API response time <2s (standard), <5s (complex)
- UGMS sync latency <30s
- PowerBI refresh <10s
- 99.5% uptime during campaigns

✅ **Business Goals**
- ≥80% digital workflow adoption (vs Excel/phone)
- ≤4h reconciliation time (vs 40h baseline)
- ≥98% inventory accuracy
- ≥1 successful recall workflow completed

✅ **User Satisfaction**
- User satisfaction score ≥4/5
- Task completion time: Agency <5min, Sales <3min
- <2h training requirement achieved
- Positive feedback from 80%+ pilot users

### Risk Mitigation

**Risk 1: UGMS API delays**
- Mitigation: Start API integration testing in Sprint 1, escalate blockers immediately
- Contingency: Build mock UGMS for parallel development

**Risk 2: Team capacity constraints (3-person team)**
- Mitigation: Ruthless prioritization, defer P2 stories if needed
- Contingency: Request additional dev support from ITU pool

**Risk 3: User adoption resistance**
- Mitigation: Extensive training, hands-on support during pilot
- Contingency: Extend pilot phase, gather feedback, iterate UX

**Risk 4: Dec 2025 timeline pressure**
- Mitigation: Weekly sprint reviews, agile adaptation
- Contingency: Launch with core epics (E1, E2) only, defer E3/E4 to Jan 2026

## Document Status

- [ ] Goals and context validated with stakeholders
- [ ] All functional requirements reviewed
- [ ] User journeys cover all major personas
- [ ] Epic structure approved for phased delivery
- [ ] Ready for architecture phase

_Note: See technical-decisions.md for captured technical context_

---

_This PRD adapts to project level Level 3 (Full product) - providing appropriate detail without overburden._