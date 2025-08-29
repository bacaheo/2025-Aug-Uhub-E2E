# Quy Trình Vận Hành E2E Gift Management - UHub System

**Tác giả:** BMad Master - Dựa trên tài liệu UHub hiện có
**Phiên bản:** 2025 - Quy trình mới với UHub Integration
**Mục đích:** Hướng dẫn business team vận hành end-to-end Gift Management System

## Tổng Quan Quy Trình

Quy trình Gift Management E2E bao gồm 7 giai đoạn chính từ lập kế hoạch đến quản lý kho:

```
B1. Gift Planning → B2. Gift Delivery to Agency → B3. Gift Delivery to Stores → B4. Gift Usage → B5. Gift Recall → B6. Stock Management → B7. Stock Adjustment
```

---

## B1. GIFT PLANNING (Lập Kế Hoạch Quà Tặng)

### 1.1 Customer Alignment với Gift Recall (KA)

**Người thực hiện:** Key Account Team
**Mục đích:** Thỏa thuận với các siêu thị về chương trình khuyến mãi

**Các bước thực hiện:**

- [ ] Thỏa thuận với KA (Coopmart, Big C, Lotte, Aeon, etc.) về:
  - Loại quà tặng và số lượng
  - Thời gian triển khai campaign
  - Mechanics (Standard Schemes như: Giỏ hàng + E-voucher, SKU + quà vật lý)
- [ ] **[MỚI]** Thống nhất Gift Recall Alignment:
  - Điều kiện thu hồi quà thừa
  - Thời gian thu hồi sau campaign
  - Quy trình xử lý quà thu hồi

**Deliverable:** Customer Alignment Document với Gift Recall Agreement

### 1.2 UGMS Setup Gift Volume (BU)

**Người thực hiện:** Brand Team
**Mục đích:** Thiết lập thông tin quà tặng trong hệ thống UGMS

**Các bước thực hiện:**

- [ ] Nhập thông tin campaign vào UGMS:
  - Gift codes và descriptions
  - Estimated volume theo từng loại quà
  - Budget allocation
- [ ] Source/Input dữ liệu cho integration với UHub
- [ ] Review và approve với ITU team

**Deliverable:** UGMS Setup hoàn tất với dữ liệu sẵn sàng integration

### 1.3 UGMS-UHub Integration (BU)

**Người thực hiện:** BU với hỗ trợ ITU
**Mục đích:** Đồng bộ tự động thông tin từ UGMS sang UHub

**Các bước thực hiện:**

- [ ] Trigger integration process từ UGMS
- [ ] Verify dữ liệu được đồng bộ chính xác:
  - Scheme mechanics
  - Gift codes
  - Số lượng quà theo từng SKU
  - Store allocation matrix
- [ ] Test integration với sample data
- [ ] Sign-off integration completion

**Deliverable:** UHub system updated với complete gift information

---

## B2. GIFT DELIVERY TO AGENCY WAREHOUSES

### 2.1 Linfox Delivery (DC)

**Người thực hiện:** Distribution Center
**Mục đích:** Vận chuyển quà từ kho chính đến kho Agency

**Các bước thực hiện:**

- [ ] Generate delivery orders từ UGMS
- [ ] Coordinate với Linfox cho logistics
- [ ] Track delivery status và estimated arrival
- [ ] Provide delivery confirmation đến Agency

**Deliverable:** Quà tặng delivered đến Agency warehouses

### 2.2 UHub Store Allocation (BU)

**Người thực hiện:** BU Team
**Mục đích:** Phân bổ quà tặng theo từng store trên UHub system

**Các bước thực hiện:**

- [ ] **[MỚI]** Sử dụng UHub allocation feature thay vì Excel
- [ ] Set allocation parameters:
  - Store priority levels
  - Expected campaign performance
  - Historical data analysis
- [ ] Generate allocation reports từ UHub
- [ ] Send allocation instructions đến Agency

**Deliverable:** Store allocation completed trong UHub system

### 2.3 Agency Confirmation on UHub (Agency)

**Người thực hiện:** Agency Team
**Mục đích:** Confirm nhận hàng trên UHub system

**Các bước thực hiện:**

- [ ] Agency nhận quà vật lý từ Linfox
- [ ] Kiểm tra số lượng thực tế vs delivery note
- [ ] **[MỚI]** Login UHub system và confirm received quantities:
  - Actual quantity received
  - Any discrepancies
  - Damaged items (if any)
- [ ] Upload photos/documents nếu có discrepancy
- [ ] Submit confirmation

**Deliverable:** Agency inventory confirmed trong UHub system

---

## B3. GIFT DELIVERY TO STORES

### 3.1 Deliver to Store (Agency)

**Người thực hiện:** Agency Team
**Mục đích:** Phân phối quà từ Agency warehouse đến stores

**Các bước thực hiện:**

- [ ] ?? Generate delivery schedule từ UHub allocation
- [ ] Prepare shipments theo store allocation
- [ ] Coordinate delivery với stores
- [ ] Provide delivery tracking information

**Deliverable:** Gifts delivered đến các stores theo allocation

### 3.2 Sales Confirmation on UHub (Sales)

**Người thực hiện:** Sales Team tại Store
**Mục đích:** Confirm nhận hàng tại store level trên UHub

**Các bước thực hiện:**

- [ ] Sales nhận quà từ Agency delivery
- [ ] Kiểm tra số lượng và chất lượng quà
- [ ] **[MỚI]** Login UHub system và confirm:
  - Store received quantities
  - Storage location
  - Any issues hoặc discrepancies
- [ ] Update store inventory status
- [ ] Notify PG team về gift availability

**Deliverable:** Store inventory confirmed và ready for campaign

---

## B4. GIFT USAGE (Sử Dụng Quà Tặng)

### 4.1 UTOP Setup UHUB Campaign (BU)

**Người thực hiện:** BU Team với UTOP Admin
**Mục đích:** Activate campaign trên UHub webapp

**Các bước thực hiện:**

- [ ] BU submit campaign setup request đến UTOP Admin
- [ ] UTOP Admin review:
  - Technical feasibility
  - Gift inventory availability
  - Campaign mechanics complexity
- [ ] Configure campaign trong UHub:
  - Campaign rules và conditions
  - Gift mapping
  - Store participation
- [ ] **Điều kiện kích hoạt:** Campaign chỉ ready khi đã confirm gift inventory tại stores
- [ ] UAT testing với sample transactions
- [ ] Go-live campaign

**Deliverable:** Active campaign trên UHub webapp

### 4.2 UHub Gift Report (BU)

**Người thực hiện:** BU Team
**Mục đích:** Monitor real-time gift usage và inventory

**Các bước thực hiện:**

- [ ] **[MỚI]** Access UHub reporting dashboard
- [ ] Monitor key metrics:
  - Số lượng quà đã phát theo ngày/store
  - Remaining inventory tại mỗi store
  - Campaign performance metrics
  - Shopper participation rates
- [ ] Set up alerts cho low inventory
- [ ] Generate daily/weekly reports
- [ ] Share insights với stakeholders

**Deliverable:** Real-time campaign performance monitoring

### 4.3 PG Support Operations (Agency/PG)

**Người thực hiện:** PG tại Store
**Mục đích:** Hỗ trợ shoppers và phát quà tặng

**Các bước thực hiện:**

- [ ] PG login PG App với authorized campaigns
- [ ] Hỗ trợ shoppers:
  - Hướng dẫn truy cập UHub webapp
  - Hướng dẫn chụp hóa đơn
  - Hỗ trợ troubleshooting
- [ ] Phát quà vật lý:
  - Scan QR code từ shopper's "Quà của tôi"
  - Confirm gift delivery trong PG App
  - Update inventory real-time
- [ ] Phát sampling:
  - Scan QR cá nhân của shopper
  - Select sampling type
  - Confirm sampling delivery
- [ ] **[MỚI]** Real-time inventory update khi phát quà

**Deliverable:** Efficient gift distribution với real-time tracking

---

## B5. GIFT RECALL TO AGENCY WAREHOUSES

### 5.1 Gift Recall Process (Agency)

**Người thực hiện:** Agency Team
**Mục đích:** Thu hồi quà thừa từ stores về Agency warehouse

**Các bước thực hiện:**

- [ ] **[MỚI]** Follow Gift Recall Alignment đã thỏa thuận trong B1.1
- [ ] Generate recall schedule từ UHub system
- [ ] Coordinate với stores cho pickup
- [ ] Physical recall và transportation
- [ ] Update UHub system với recalled quantities

**Deliverable:** Excess gifts recalled theo alignment agreement

---

## B6. STOCK MANAGEMENT (Quản Lý Kho)

### 6.1 Agency UHub Reconciliation (Agency)

**Người thực hiện:** Agency Team
**Mục đích:** Reconcile physical inventory với UHub records

**Các bước thực hiện:**

- [ ] **[MỚI]** Conduct physical inventory count
- [ ] Input actual quantities vào UHub system:
  - Gifts remaining at stores
  - Gifts recalled to agency
  - Damaged/expired items
- [ ] UHub system tự động so sánh:
  - Physical count vs system records
  - Identify discrepancies
- [ ] Submit reconciliation report với explanations cho discrepancies

**Deliverable:** Agency reconciliation completed trong UHub

### 6.2 BU Review & Confirm (BU)

**Người thực hiện:** BU Team
**Mục đích:** Review và approve Agency reconciliation

**Các bước thực hiện:**

- [ ] **[MỚI]** Receive reconciliation reports từ UHub
- [ ] Review discrepancies:
  - Investigate root causes
  - Validate explanations từ Agency
- [ ] Approve hoặc request corrections:
  - Accept reasonable discrepancies
  - Request additional documentation nếu cần
- [ ] Finalize inventory status trong UHub

**Deliverable:** Approved reconciliation với final inventory status

### 6.3 Comprehensive UHub Reconciliation (BU)

**Người thực hiện:** BU Team
**Mục đích:** Overall reconciliation across all systems

**Các bước thực hiện:**

- [ ] **[MỚI]** UHub automatic reconciliation process:
  - UGMS original allocation
  - UHub tracking records
  - Agency physical reports
  - Actual shopper redemptions
- [ ] Review discrepancies và variances
- [ ] Generate final campaign summary:
  - Total gifts distributed
  - Remaining inventory
  - Financial impact
  - Lessons learned

**Deliverable:** Complete campaign reconciliation report

---

## B7. STOCK REDUCTION/RECALL/RE-ALLOCATION

### 7.1 Ticket System với Level 2 Approval (BU)

**Người thực hiện:** BU Team với Management Approval
**Mục đích:** Handle special cases requiring inventory adjustments

**Các bước thực hiện:**

- [ ] **[TÍNH NĂNG MỚI]** Create tickets trong UHub system cho:
  - **Stock Reduction:** Giảm inventory do damage/expiry
  - **Gift Recall:** Thu hồi quà khẩn cấp
  - **Re-allocation:** Phân bổ lại quà giữa stores
- [ ] Submit ticket với:
  - Detailed justification
  - Supporting documentation
  - Proposed action plan
- [ ] **Level 2 Management Approval:**
  - Review business impact
  - Approve/reject với comments
- [ ] **[MỚI]** Auto-integration UHUB ↔ UGMS:
  - Update inventory records
  - Sync financial impact
  - Update campaign parameters nếu cần

**Deliverable:** Approved inventory adjustments với complete audit trail

---

## ROLES & RESPONSIBILITIES MATRIX


| Role           | Primary Responsibilities                                     | Key Systems    |
| ---------------- | -------------------------------------------------------------- | ---------------- |
| **KA Team**    | Customer alignment, Gift recall agreements                   | Manual process |
| **BU Team**    | End-to-end process ownership, Campaign setup, Reconciliation | UGMS, UHub     |
| **DC**         | Physical logistics và delivery                              | Linfox, UGMS   |
| **Agency**     | Warehouse operations, Store delivery, Inventory confirmation | UHub           |
| **Sales**      | Store-level inventory confirmation                           | UHub           |
| **UTOP Admin** | Campaign technical setup, L2 support                         | UHub           |
| **PG**         | Gift distribution, Shopper support                           | PG App, UHub   |
| **ITU**        | Technical support, System integration                        | UGMS, UHub     |

---

## SUCCESS METRICS & KPIs

### Operational Efficiency

- [ ] **Automation Rate:** % của processes được tự động hóa (Target: >80%)
- [ ] **Manual Intervention:** Số lượng manual interventions per campaign (Target: <5)
- [ ] **Processing Time:** Thời gian từ UGMS setup đến campaign live (Target: <7 days)

### Inventory Accuracy

- [ ] **Reconciliation Accuracy:** % discrepancy giữa physical vs system (Target: <2%)
- [ ] **Real-time Tracking:** % transactions được track real-time (Target: >95%)
- [ ] **Gift Wastage:** % quà bị waste do expired/damaged (Target: <3%)

### System Integration

- [ ] **Data Sync Accuracy:** % successful UGMS-UHub sync (Target: >99%)
- [ ] **System Uptime:** UHub system availability (Target: >99.5%)
- [ ] **User Adoption:** % users using digital confirmation vs manual (Target: >90%)

---

## ESCALATION MATRIX


| Issue Level | Description                             | Escalation To       | Response Time |
| ------------- | ----------------------------------------- | --------------------- | --------------- |
| **Level 1** | System errors, User support             | Hotline, UTOP Admin | 2 hours       |
| **Level 2** | Process discrepancies, Inventory issues | BU Team, ITU        | 4 hours       |
| **Level 3** | Major system failures, Business impact  | Management, Vendor  | 1 hour        |
| **Level 4** | Strategic issues, Compliance            | Executive Team      | Immediate     |

---

## CONTINUOUS IMPROVEMENT

### Monthly Review Process

- [ ] Campaign performance analysis
- [ ] System efficiency metrics review
- [ ] User feedback collection
- [ ] Process optimization recommendations

### Quarterly Business Review

- [ ] Overall ROI analysis
- [ ] Technology roadmap review
- [ ] Stakeholder satisfaction survey
- [ ] Strategic alignment assessment

---

*Tài liệu này sẽ được cập nhật theo định kỳ dựa trên feedback từ business operations và system enhancements.*
