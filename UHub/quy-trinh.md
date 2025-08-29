# Quy Trình Vận Hành E2E Gift Management - UHub System

**Tác giả:** [..] - Dựa trên tài liệu UHub hiện có
**Phiên bản:** 2025 - Quy trình mới với UHub Integration
**Mục đích:** Hướng dẫn business team vận hành end-to-end Gift Management System

## Tổng Quan Quy Trình

Quy trình Gift Management E2E bao gồm 7 giai đoạn chính từ lập kế hoạch đến quản lý kho:

```
B1. Gift Planning
 → B2. Gift Delivery to Agency
  → B3. Gift Delivery to Stores
   → B4. Gift Usage
    → B5. Gift Recall
     → B6. Stock Management
      → B7. Stock Adjustment
```

---

## B1. GIFT PLANNING (Lập Kế Hoạch Quà Tặng)

### 1.1 Customer Alignment với Gift Recall (KA)

**Người thực hiện:** Key Account Team
**Mục đích:** Thỏa thuận với các siêu thị về chương trình khuyến mãi

**Các bước thực hiện:**

- [ ] Thoả thuận với các đối tác Key Account (Coopmart, Big C, Lotte, Aeon, v.v.) về:
  - Danh mục và số lượng quà tặng chiến dịch
  - Khung thời gian triển khai marketing campaign
  - Cơ chế khuyến mãi (Các scheme tiêu chuẩn như: Combo giỏ hàng + E-voucher, Mua sản phẩm + quà tặng vật lý)
- [ ] **[MỚI]** Thống nhất nguyên tắc thu hồi quà tặng:
  - Điều kiện và tiêu chí thu hồi quà dư
  - Khung thời gian thu hồi sau chiến dịch
  - Quy trình xử lý và tái phân phối quà tặng

**Deliverable:** Tài liệu thỏa thuận đối tác với chi tiết chính sách thu hồi quà

### 1.2 UGMS Setup Gift Volume (BU)

**Người thực hiện:** Brand Team
**Mục đích:** Thiết lập thông tin quà tặng trong hệ thống UGMS

**Các bước thực hiện:**

- [ ] Cập nhật thông tin chiến dịch vào hệ thống UGMS:
  - Giftcode và mô tả chi tiết từng loại quà
  - Dự báo khối lượng quà theo từng phân khúc
  - Phân bổ ngân sách marketing
- [ ] Source/Input dữ liệu cho integration với UHub
- [ ] Review và approve với ITU team

**Deliverable:** Cấu hình UGMS hoàn chỉnh, sẵn sàng integration

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
- [ ] Kiểm thử tích hợp với bộ dữ liệu mẫu
- [ ] Xác nhận hoàn tất quá trình tích hợp

**Deliverable:** Hệ thống UHub được cập nhật với toàn bộ thông tin quà tặng

---

## B2. GIFT DELIVERY TO AGENCY WAREHOUSES

### 2.1 Linfox Delivery (DC)

**Người thực hiện:** Distribution Center
**Mục đích:** Vận chuyển quà từ kho chính đến kho Agency

**Các bước thực hiện:**

- [ ] Tạo lệnh giao hàng từ hệ thống UGMS
- [ ] Phối hợp với Linfox về kế hoạch logistics
- [ ] Theo dõi trạng thái và thời gian dự kiến của đợt giao hàng
- [ ] Cung cấp xác nhận giao hàng cho Agency

**Deliverable:** Quà tặng được chuyển thành công đến kho Agency

### 2.2 UHub Store Allocation (BU)

**Người thực hiện:** BU Team
**Mục đích:** Phân bổ quà tặng theo từng store trên UHub system

**Các bước thực hiện:**

- [ ] **[MỚI]** Sử dụng UHub allocation feature thay vì Excel
- [ ] Thiết lập các tham số phân bổ:
  - Mức độ ưu tiên của từng điểm bán
  - Dự báo hiệu quả chiến dịch
  - Phân tích dữ liệu lịch sử
- [ ] Tạo báo cáo phân bổ từ UHub
- [ ] Gửi hướng dẫn phân bổ đến Agency

**Deliverable:** Hoàn tất phân bổ quà tặng trên hệ thống UHub

### 2.3 Agency Confirmation on UHub (Agency)

**Người thực hiện:** Agency Team
**Mục đích:** Confirm nhận hàng trên UHub system

**Các bước thực hiện:**

- [ ] Agency tiếp nhận quà vật lý từ Linfox
- [ ] Kiểm tra số lượng thực tế so với phiếu giao hàng
- [ ] **[MỚI]** Đăng nhập UHub và xác nhận số lượng nhận được:
  - Số lượng quà thực tế
  - Các điểm khác biệt (nếu có)
  - Các mặt hàng bị hư hỏng (nếu có)
- [ ] Tải lên hình ảnh/tài liệu minh chứng nếu có sai lệch
- [ ] Gửi xác nhận chính thức

**Deliverable:** Xác nhận tồn kho Agency trên hệ thống UHub

---

## B3. GIFT DELIVERY TO STORES

### 3.1 Deliver to Store (Agency)

**Người thực hiện:** Agency Team
**Mục đích:** Phân phối quà từ Agency warehouse đến stores

**Các bước thực hiện:**

- [ ] Tạo lịch trình giao hàng từ phân bổ của UHub
- [ ] Chuẩn bị lô hàng theo phân bổ của từng điểm bán
- [ ] Điều phối việc giao hàng với các cửa hàng
- [ ] Cung cấp thông tin theo dõi vận chuyển

**Deliverable:** Quà tặng được giao đến các cửa hàng theo kế hoạch phân bổ

### 3.2 Sales Confirmation on UHub (Sales)

**Người thực hiện:** Sales Team tại Store
**Mục đích:** Confirm nhận hàng tại store level trên UHub

**Các bước thực hiện:**

- [ ] Sales nhận quà từ Agency giao
- [ ] Kiểm tra số lượng và chất lượng quà
- [ ] **[MỚI]** Đăng nhập UHub và xác nhận:
  - Số lượng quà nhận tại cửa hàng
  - Vị trí lưu trữ
  - Các vấn đề hoặc điểm khác biệt
- [ ] Cập nhật trạng thái tồn kho cửa hàng
- [ ] Thông báo cho đội PG về tình trạng quà tặng

**Deliverable:** Xác nhận tồn kho cửa hàng và sẵn sàng cho chiến dịch

---

## B4. GIFT USAGE (Sử Dụng Quà Tặng)

### 4.1 UTOP Setup UHUB Campaign (BU)

**Người thực hiện:** BU Team với UTOP Admin
**Mục đích:** Activate campaign trên UHub webapp

**Các bước thực hiện:**

- [ ] BU Log Form submit campaign setup request đến UTOP Admin
- [ ] UTOP Admin rà soát:
  - Tính khả thi về kỹ thuật
  - Tình trạng tồn kho quà tặng
  - Độ phức tạp của cơ chế chiến dịch
- [ ] Cấu hình chiến dịch trong UHub:
  - Quy tắc và điều kiện chiến dịch
  - Ánh xạ quà tặng
  - Sự tham gia của điểm bán
- [ ] **Điều kiện kích hoạt:** Campaign chỉ ready khi đã confirm gift inventory tại stores
- [ ] UAT testing với sample transactions
- [ ] Go-live campaign

**Deliverable:** Active campaign trên UHub webapp

### 4.2 UHub Gift Report (BU)

**Người thực hiện:** BU Team
**Mục đích:** Giám sát việc sử dụng quà tặng và tồn kho theo thời gian thực

**Các bước thực hiện:**

- [ ] **[MỚI]** Truy cập UHub reporting dashboard
- [ ] Theo dõi các chỉ số chính:
  - Số lượng quà đã phát theo ngày/cửa hàng
  - Tồn kho còn lại tại mỗi điểm bán
  - Các chỉ số hiệu quả chiến dịch
  - Tỷ lệ tham gia của khách hàng
- [ ] Thiết lập cảnh báo cho tồn kho thấp
- [ ] Tạo báo cáo hàng ngày/hàng tuần
- [ ] Chia sẻ thông tin chi tiết với các bên liên quan

**Deliverable:** Giám sát hiệu quả chiến dịch theo thời gian thực

### 4.3 PG Support Operations (Agency/PG)

**Người thực hiện:** PG tại Store
**Mục đích:** Hỗ trợ shoppers và phát quà tặng

**Các bước thực hiện:**

- [ ] PG login PG App với authorized campaigns
- [ ] Hỗ trợ shoppers:
  - Hướng dẫn truy cập UHub webapp
  - Hướng dẫn chụp hóa đơn
  - Hỗ trợ giải quyết các vấn đề phát sinh
- [ ] Phát quà vật lý:
  - Quét mã QR từ mục "Quà của tôi" của shopper
  - Xác nhận giao quà trong PG App
  - Cập nhật tồn kho theo thời gian thực
- [ ] Phát sản phẩm dùng thử:
  - Quét mã QR cá nhân của shopper
  - Chọn loại sản phẩm dùng thử
  - Xác nhận giao sản phẩm dùng thử
- [ ] **[MỚI]** Cập nhật tồn kho theo thời gian thực khi phát quà

**Deliverable:** Phân phối quà hiệu quả với real-time tracking

---

## B5. GIFT RECALL TO AGENCY WAREHOUSES

### 5.1 Gift Recall Process (Agency)

**Người thực hiện:** Agency Team
**Mục đích:** Thu hồi quà thừa từ stores về Agency warehouse

**Các bước thực hiện:**

- [ ] **[MỚI]** Tuân thủ nguyên tắc thu hồi quà đã thỏa thuận trong B1.1
- [ ] Tạo lịch trình thu hồi từ hệ thống UHub
- [ ] Điều phối với các cửa hàng về việc thu hồi
- [ ] Thực hiện thu hồi vật lý và vận chuyển
- [ ] Cập nhật hệ thống UHub với số lượng quà thu hồi

**Deliverable:** Thu hồi quà thừa theo thỏa thuận đã ký kết

---

## B6. STOCK MANAGEMENT (Quản Lý Kho)

### 6.1 Agency UHub Reconciliation (Agency)

**Người thực hiện:** Agency Team
**Mục đích:** Reconcile physical inventory với UHub report

**Các bước thực hiện:**

- [ ] **[MỚI]** Thực hiện kiểm kê tồn kho vật lý
- [ ] Nhập số lượng thực tế vào hệ thống UHub:
  - Quà còn lại tại các cửa hàng
  - Quà được thu hồi về kho
  - Các mặt hàng bị hư hỏng/hết hạn
- [ ] Hệ thống UHub tự động so sánh:
  - Kiểm kê vật lý so với hồ sơ hệ thống
  - Xác định các điểm chênh lệch
- [ ] Gửi báo cáo đối soát kèm giải trình cho các điểm khác biệt

**Deliverable:** Hoàn tất đối soát Agency trong UHub

### 6.2 BU Review & Confirm (BU)

**Người thực hiện:** BU Team
**Mục đích:** Rà soát và phê duyệt đối soát Agency

**Các bước thực hiện:**

- [ ] **[MỚI]** Nhận báo cáo đối soát từ UHub
- [ ] Rà soát các điểm khác biệt:
  - Điều tra nguyên nhân gốc rễ
  - Xác thực các giải trình từ Agency
- [ ] Phê duyệt hoặc yêu cầu chỉnh sửa:
  - Chấp nhận các chênh lệch hợp lý
  - Yêu cầu bổ sung tài liệu nếu cần
- [ ] Hoàn thiết trạng thái tồn kho trong UHub

**Deliverable:** Phê duyệt đối soát với trạng thái tồn kho cuối cùng

### 6.3 Comprehensive UHub Reconciliation (BU)

**Người thực hiện:** BU Team
**Mục đích:** Đối soát toàn diện trên tất cả hệ thống

**Các bước thực hiện:**

- [ ] **[MỚI]** Quy trình đối soát tự động của UHub:
  - Phân bổ ban đầu từ UGMS
  - Hồ sơ theo dõi của UHub
  - Báo cáo vật lý từ Agency
  - Số lượng quà khách hàng đã nhận thực tế
- [ ] Rà soát các điểm khác biệt và sai lệch
- [ ] Tạo báo cáo tổng kết chiến dịch:
  - Tổng số quà đã phân phối
  - Tồn kho còn lại
  - Tác động tài chính
  - Bài học kinh nghiệm

**Deliverable:** Báo cáo đối soát chiến dịch hoàn chỉnh

### 7.1 Ticket System với Level 2 Approval (BU)

**Người thực hiện:** BU Team với Management Approval
**Mục đích:** Xử lý các trường hợp đặc biệt yêu cầu điều chỉnh tồn kho

**Các bước thực hiện:**

- [ ] **[TÍNH NĂNG MỚI]** Tạo ticket trong hệ thống UHub cho:
  - **Giảm Tồn Kho (Stock Reduction):** Giảm tồn kho do hư hỏng/hết hạn
  - **Thu Hồi Quà (Gift Recall):** Thu hồi quà khẩn cấp
  - **Phân Bổ Lại (Re-allocation):** Phân bổ lại quà giữa các cửa hàng
- [ ] Gửi ticket với:
  - Lý do chi tiết
  - Tài liệu hỗ trợ
  - Kế hoạch hành động đề xuất
- [ ] **Level 2 Management Approval:**
  - Đánh giá tác động kinh doanh
  - Phê duyệt/từ chối kèm nhận xét
- [ ] **[MỚI]** Tích hợp tự động UHUB ↔ UGMS:
  - Cập nhật hồ sơ tồn kho
  - Đồng bộ tác động tài chính
  - Cập nhật các tham số chiến dịch nếu cần

**Deliverable:** Điều chỉnh tồn kho được phê duyệt với đầy đủ hồ sơ kiểm toán

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
