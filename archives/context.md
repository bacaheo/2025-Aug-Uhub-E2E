# Ngữ Cảnh UHub - Gift Management E2E System

## 1. Tổng Quan Hệ Thống

### 1.1 Thông tin cơ bản
- **Tên hệ thống**: UHub (Unilever Hub)
- **Chủ sở hữu**: Unilever Vietnam
- **Vendor phát triển**: Utop
- **Thời gian bắt đầu**: Tháng 05/2020
- **Mục đích**: Quản lý toàn bộ vòng đời quà tặng (Gift Management E2E) cho các chương trình khuyến mãi (CTKM)

### 1.2 Hệ thống liên quan
- **UGMS** (Unilever Gift Management System): Hệ thống quản lý quà tặng phía Unilever
- **UHub**: Hệ thống webapp cho Shopper và PG
- **ITU Log Form**: Form yêu cầu setup campaign từ BU
- **Admin Portal**: Công cụ quản trị cho Utop Admin

---

## 2. Định Nghĩa Thuật Ngữ

### 2.1 Thuật ngữ nghiệp vụ
- **U**: Công ty Unilever
- **KA (Key Account)**: Các chuỗi siêu thị là khách hàng của Unilever (Coopmart, Go & Big C, Lotte, Mega, Aeon, Emart)
- **CTKM (Chương Trình Khuyến Mãi)**: Campaign/Promotion Program
- **SKU (Stock Keeping Unit)**: Mã sản phẩm cụ thể
- **Gift Vendor**: Nhà cung cấp quà E-Gift (GotIt, Urbox)
- **Scheme**: Kế hoạch phân bổ quà cho một campaign cụ thể
- **Ship-to Code**: Mã địa chỉ cửa hàng/kho nhận hàng
- **Allocation by store**: Phân bổ quà theo từng cửa hàng

### 2.2 Thuật ngữ kỹ thuật
- **OCR (Optical Character Recognition)**: Công nghệ nhận dạng ký tự quang học
- **E-Gift**: Quà tặng điện tử (voucher, mã giảm giá)
- **QR Code**: Mã vạch 2D để xác thực người nhận quà
- **API Sync**: Đồng bộ dữ liệu giữa các hệ thống
- **Ticket System**: Hệ thống tạo và phê duyệt yêu cầu điều chỉnh
- **Multi-level Approval**: Quy trình phê duyệt nhiều cấp

---

## 3. Các Actors Trong Hệ Thống

### 3.1 Người dùng cuối (End Users)

#### A. Shopper (Người tiêu dùng)
- Mua sản phẩm Unilever tại siêu thị (MT) và cửa hàng (GT)
- Tham gia CTKM qua webapp UHub
- Chụp hóa đơn để nhận quà
- **Quyền trên hệ thống**:
  - Đăng nhập bằng số điện thoại + SMS OTP
  - Chụp và submit hóa đơn
  - Nhận quà E-Gift/E-Voucher
  - Xem quà của tôi (My Gifts)
  - Hiển thị QR code để nhận quà vật lý

#### B. PG (Promoter Girl)
- Nhân viên khuyến mãi được Unilever thuê
- Hỗ trợ khách hàng tại cửa hàng
- **Quyền trên hệ thống**:
  - Đăng nhập PG App bằng username/password
  - Chọn campaign được phân quyền
  - Scan QR code của shopper
  - Xác nhận trao quà vật lý
  - Phát mẫu thử (Sampling)

### 3.2 Đội ngũ Unilever

#### A. RI (Retail Innovation Team)
- Đội ngũ Đổi mới Bán lẻ
- Xây dựng và triển khai giải pháp công nghệ mới
- Tối ưu hóa chi phí chạy CTKM
- Đơn vị khởi tạo dự án UHub

#### B. BT (Brand Team)
- Các đội ngũ thương hiệu
- Chạy các chương trình khuyến mãi sử dụng UHub
- Lập kế hoạch và quản lý campaign

#### C. BU (Business Unit)
- Team Unilever quản lý campaign end-to-end
- **Quyền trên hệ thống**:
  - Setup Gift trên UGMS
  - Request Setup Campaign qua ITU Log Form
  - Review và approval tickets (Agency/Sales/Reconciliation)
  - Xem PowerBI Reports
  - Trigger các operations (transfer, recall, agency change)

#### D. BU Level-2
- Cấp phê duyệt cao hơn
- **Quyền trên hệ thống**:
  - Review và approval ticket qua Email
  - Approval với evidences (chứng từ)

#### E. ITU (IT Unilever)
- Đội ngũ IT của Unilever
- Hỗ trợ các business team (RI, BT)
- Làm việc với technical vendor
- Review effort và timeline dự án

### 3.3 Đội ngũ logistics

#### A. DC (Distribution Center)
- Trung tâm phân phối của Unilever
- Sử dụng Linfox để vận chuyển quà
- Giao hàng từ kho Unilever → kho Agency

#### B. Agency
- Đơn vị logistics trung gian
- **Quyền trên hệ thống**:
  - Nhận và kiểm tra hàng từ DC
  - Digital confirm nhận hàng trên UHub (Agency WHs)
  - Tạo ticket điều chỉnh (≤168 giờ trước campaign live)
  - Xuất thông tin Allocation by store
  - Deliver quà đến Stores
  - Recall quà từ Stores về kho Agency
  - Digital confirm report đối soát

#### C. Sales
- Nhân viên bán hàng tại Store
- **Quyền trên hệ thống**:
  - Nhận và kiểm tra hàng từ Agency
  - Confirm nhận hàng theo Allocation by store
  - Tạo ticket điều chỉnh (≤48 giờ trước campaign live)

### 3.4 Đội ngũ Utop

#### A. Utop Admin (L2 Support)
- Quản trị viên cấp 2
- **Quyền trên hệ thống**:
  - Nhận yêu cầu setup CTKM từ Unilever
  - Tư vấn và đánh giá tính khả thi
  - Setup campaign trên UHub Admin Portal
  - Confirm ticket sau BU Level-2 approval
  - Sync dữ liệu UHub ↔ UGMS

#### B. Hotline (L1 Support)
- Nhân viên hỗ trợ khách hàng
- **Quyền trên hệ thống**:
  - Xét duyệt hóa đơn bị từ chối không chính xác
  - Thẩm định & xét duyệt hóa đơn vi phạm rule gian lận
  - Hướng dẫn tham gia CTKM
  - Xử lý khiếu nại
  - Hỗ trợ liên hệ trao quà

---

## 4. Quy Trình E2E - 7 Giai Đoạn Chính

### Flow tổng quan
```
B1. Gift Planning & Scheme Setup
  ↓
B2. Gift Delivery to Agency WHs (với Ticket System)
  ↓
B3. Gift Delivery to Stores (với Ticket System)
  ↓
B4. Gift Usage (PG phát quà, Dashboard tracking)
  ↓
B5. Gift Recall to Agency WHs
  ↓
B6. Stock Reconciliation & Verification
  ↓
B7. Stock Adjustment (Multi-level Approval)
```

### B1. Gift Planning & Scheme Setup
**Actors**: KA, BU, Utop Admin, DC

**Các bước**:
1. **B1_01. Customer Alignment** (KA)
   - Team KA của Unilever plan campaign với KA
   - Thống nhất quy trình thu hồi quà
   - Không số hóa bước này

2. **B1_02. Setup Gift trên UGMS** (BU)
   - Cập nhật GiftCode, Scheme, Customer, Ship_to
   - Cần triển khai trước ngày campaign live 15-30 ngày

3. **B1_03. Sync Setup Gift UGMS → UHub** (Tự động)
   - Đồng bộ: GiftCode, Scheme (Start, End, Mechanics, Quantity), Customer, Ship_to
   - UGMS đẩy thông tin qua API

4. **B1_04. Request Setup Campaign → ITU Log Form** (BU)
   - BU cập nhật: GiftCode, Scheme, Allocation by store
   - Cần triển khai trước ngày campaign live 10-15 ngày

5. **B1_05. Sync Request ITU Log Form → UHub Admin** (Auto Email)
   - Thông tin: Scheme ID, GiftCode, SKUs, Allocation by store, Agency info

6. **B1_06. Init campaign trên Utop Admin Portal** (Utop Admin)
   - Cập nhật Email BU (theo Scheme ID)
   - Cập nhật Agency vận hành campaign
   - Cập nhật Allocation by store

7. **B1_07. Linfox Delivery → Agency WHs** (DC)
   - Quy trình kho vận từ kho Unilever sang kho Agency
   - Không số hóa bước này

### B2. Gift Delivery to Agency WHs (với Ticket System)
**Actors**: Agency, BU

**SLA**: Ticket phải tạo trước campaign live ≥168 giờ (7 ngày)

**Các bước**:
1. **B2_01. Nhận phiếu xuất và kiểm tra hàng** (Agency)
2. **B2_02. Decision: Đủ hàng & không hư hỏng?**
   - **Yes**:
     - B2_03_01. Agency chọn Allocation by store trên UHub
     - B2_03_02. Agency Digital confirm nhận hàng theo Gift Vol
   - **No** (Loop until approved):
     - B2_04. Agency tạo ticket nhận hàng theo số thực tế & nguyên nhân
     - B2_05. BU review, cập nhật Allocation và approval ticket
     - B2_06. Decision: BU Approved?
       - **Rejected**: quay lại B2_04
       - **Approved**: tiếp tục
3. **B2_07. Nhập kho Agency** theo thông tin đã confirm
4. **B2_08. Xuất thông tin Allocation by store** trên UHub

**Ticket SLA**:
- Agency tạo ticket: ≤168 giờ trước campaign live
- BU approval: trong vòng 24 giờ
- Quá 24 giờ: ticket hết hạn

### B3. Gift Delivery to Stores (với Ticket System)
**Actors**: Agency, Sales, BU, Utop Admin

**SLA**: Ticket phải tạo trước campaign live ≥48 giờ

**Các bước**:
1. **B3_01. Deliver to Stores** theo Allocation by store (Agency)
2. **B3_02. Kiểm tra hàng** so với Allocation by store (Sales)
3. **B3_03. Decision: Đủ hàng & không hư hỏng?**
   - **Yes**:
     - B3_04_01. Sales chọn Allocation by store của mình
     - B3_04_02. Sales Confirm nhận hàng
   - **No** (Loop until approved):
     - B3_05. Sales tạo ticket nhận hàng theo số thực tế & nguyên nhân
     - B3_06. BU review, cập nhật Allocation và approval ticket
     - B3_07. Decision: BU Approved?
       - **Rejected**: quay lại B3_05
       - **Approved**: tiếp tục
4. **B3_08. Nhập kho cửa hàng** theo thông tin đã confirm

**Campaign Readiness Gate** (Loop until ready):
5. **B3_09. UTOP Setup UHub Campaign & Go-live** (Utop Admin)
6. **B3_10. Decision: Campaign ready?**
   - **No**: quay lại B3_09
   - **Yes**: tiếp tục B4

**Ticket SLA**:
- Sales tạo ticket: ≤48 giờ trước campaign live
- BU approval: trong vòng 12 giờ
- Quá 12 giờ: ticket hết hạn

### B4. Gift Usage
**Actors**: Agency, BU, Utop Admin

**Các bước**:
1. **B4_01. PG operates campaign trên UHub** (Agency)
   - UHub Sampling
   - UHub Redemption

2. **B4_02. BU xem UHub PowerBI Report** (BU)
   - Refresh 3 lần/ngày
   - Gift Report (NEW)
   - Campaign Performance

3. **B4_03. BU Trigger các operations** (BU) - Nếu có:
   - Luân chuyển quà giữa Stores
   - Thu hồi quà từ Stores về Agency WHs
   - Thay đổi Agency

**Note**: Phase 1 chưa số hóa B3.Gift Delivery to Stores, nên chưa số hóa 3 quy trình operations này.

### B5. Gift Recall to Agency WHs
**Actors**: Agency

**Timeline**: Trong vòng 5 ngày tính từ ngày end-campaign

**Các bước**:
1. **B5_01. Recall Gifts to Agency WHs** (Agency)
   - Agency thu hồi quà từ Store về Kho Agency
   - Hoạt động trong 5 ngày sau end-campaign
   - Bước này Agency tự vận hành, chưa số hóa

### B6. Stock Reconciliation & Verification
**Actors**: Agency, BU, BU Level-2, Utop Admin

**Các bước** (Loop until approved):
1. **B6_01. Kiểm tra tổng hàng thu hồi thực tế** (Agency)
   - So sánh với report đối soát trên UHub
   - Số tồn ban đầu: B2_07
   - Số đã phát: B4_01
   - Số còn lại: B2_07 - B4_01

2. **B6_02. Decision: Thu hồi đủ số còn lại & không hư hỏng?**
   - **Yes**:
     - B6_03. Agency Digital confirm report đối soát
     - B6_04. Sync UHub → UGMS (Cập nhật số Gift Usage B4_01)
   - **No**:
     - B6_05. Agency submit ticket đối soát theo số thực tế & nguyên nhân
     - B6_06. UHub gửi auto email thông tin ticket cho BU

### B7. Stock Adjustment (Multi-level Approval)
**Actors**: BU, BU Level-2, Utop Admin

**Các bước** (Loop until approved):
1. **B6_07. BU Raise & Forward Ticket via Email** (BU)
   - Get Level-2 Approval

2. **B6_08. Review & Approval Ticket via Email** (BU Level-2)

3. **B6_09. Decision: BU Level-2 Approved?**
   - **Approved via email**:
     - B6_10_01. Utop Admin Digital confirm ticket
     - B6_10_02. Sync UHub → UGMS (Cập nhật Gift Usage + số điều chỉnh + evidences)
   - **Rejected via email**:
     - B6_11_01. BU Rejected ticket
     - B6_11_02. Request/Work with Agency correction / re-check
     - Quay lại B6_01

4. **B6_12. Decision: BU Level-2 Approved?**
   - **Rejected**: quay lại B6_01
   - **Approved**: tiếp tục

5. **B6_13. Nhập Gift thu hồi vào kho Agency**, ready để tái sử dụng

6. **B6_14. Quy trình tái sử dụng Gift**

---

## 5. Integration APIs (UGMS ↔ UHub)

### 5.1 Authentication Mechanism
- **Security**: HTTPS (TLS 1.2+), IP whitelisting
- **Method**: RSA digital signature (SHA-256)
- **Key**: RSA 2048 bits, PKCS#1 v1.5 padding

### 5.2 Danh sách 6 APIs

#### API 1: Sync Giftcode (UGMS → UHub)
**Endpoint**: POST /api/v1/gift/create

**Input**:
```json
{
  "gift_code": "GFT001",
  "gift_name": "Áo thun Unilever",
  "gift_type": "vật phẩm",
  "uom": "Cái",
  "gift_group": "Textiles"
}
```

**Trigger**: Khi tạo mới/cập nhật thông tin GiftCode trên UGMS

---

#### API 2: Sync Scheme (UGMS → UHub)
**Endpoint**: POST /api/v1/scheme/create

**Input**:
```json
{
  "scheme_id": "SCHM001",
  "scheme_name": "Chương trình Tết 2025",
  "start_date": "2025-12-01",
  "end_date": "2026-01-15",
  "requester": "Nguyễn Văn A",
  "list_gift": [
    {
      "gift_code": "GFT001",
      "gift_name": "Lì xì",
      "quantity": 500
    }
  ]
}
```

**Trigger**: Khi tạo mới/cập nhật Scheme cho campaign Redemption/Sampling

---

#### API 3: Sync Store/Customer (UGMS → UHub)
**Endpoint**: POST /api/v1/customer/create

**Input**:
```json
{
  "customer_id": "CUS123",
  "customer_name": "Aeon",
  "ship_to_code": "ST456",
  "ship_to_name": "Aeon Tân Phú",
  "province": "Hồ Chí Minh"
}
```

**Trigger**: Khi tạo mới/cập nhật thông tin Store/Agency

---

#### API 4: Sync Xuất Kho đến Store (UGMS → UHub)
**Endpoint**: POST /api/v1/utop/receive-gift

**Input**:
```json
{
  "ship_to_code": "ST001",
  "receive_date": "2025-07-04",
  "scheme_id": "SCHM001",
  "shipment_num": "SH12345",
  "sup_code": "SUP001",
  "trans_code": "TRANS001",
  "gift_list": [
    {
      "gift_code": "GFT001",
      "quantity": 100
    }
  ]
}
```

**Trigger**: Khi tạo mới phiếu xuất kho giao hàng

---

#### API 5: Sync Điều Chỉnh Kho (UGMS → UHub)
**Endpoint**: POST /api/v1/utop/receive-gift

**Input**:
```json
{
  "ship_to_code": "ST001",
  "receive_date": "2025-07-04",
  "scheme_id": "SCHM001",
  "shipment_num": "ADJ12345",
  "sup_code": "SUP001",
  "trans_code": "TRANS001",
  "gift_list": [
    {
      "gift_code": "GFT001",
      "quantity": -20
    }
  ]
}
```

**Note**: Quantity âm (-) để điều chỉnh giảm tồn kho

**Trigger**: Theo request của PO để điều chỉnh số tồn kho, ghi log thông tin

---

#### API 6: Sync Gift Usage (UHub → UGMS)
**Endpoint**: POST https://ugift.vn/api/v1/utop/actual_delivery_gift

**Input**:
```json
{
  "ship_to_code": "ST001",
  "scheme_id": "SCHM001",
  "gift_list": [
    {
      "gift_code": "GFT001",
      "quantity": 20
    }
  ]
}
```

**Trigger**: Khi quà đã sử dụng sau chương trình (thay đổi tồn kho)

---

## 6. Shopper Journey - Chụp Hóa Đơn, Nhận Quà

### 6.1 Quy trình chụp hóa đơn

**Bước 1: Truy cập và đăng nhập**
- Shopper truy cập URL của UHub
- Đăng nhập với số điện thoại và SMS OTP
- Cập nhật profile và đồng ý consent

**Bước 2: Loại CTKM**
- Loại 1: Mua giỏ hàng xxx VND → Nhận E-Gift qua SMS
- Loại 2: Mua giỏ hàng xxx VND → Nhận quà vật lý tại store
- Điều kiện khác: Số lượng SKU, tổng tiền loại sản phẩm, brand cụ thể

**Bước 3: Chụp và xử lý hóa đơn**
1. Shopper chụp hóa đơn theo hướng dẫn
2. Hệ thống gọi API Azure OCR để lấy dữ liệu text + toạ độ
3. Nhận diện KA (Key Account/siêu thị)
4. Trích xuất thông tin hóa đơn
5. Kiểm tra điều kiện chương trình
6. Tính toán danh sách quà dự kiến
7. Kiểm tra gian lận (Abnormal Detection)

**Bước 4: Trạng thái xử lý hóa đơn**

**Thứ tự xử lý tuần tự**:
```
Processing → Invalid → Pending → Valid → Check → ApprovedByShopper/RejectByPG/ApprovedByPG
```

**Các trạng thái chính**:
1. **Processing**: Đang xử lý OCR và validation
2. **Invalid**: Không đạt yêu cầu cơ bản → Có thể xét duyệt lại bởi hotline
3. **Pending**: Vi phạm rule Abnormal Detection → Cần hotline kiểm tra
4. **Valid**: Hợp lệ, chờ shopper xác nhận
5. **Check**: Shopper yêu cầu hotline xác minh
6. **ApprovedByShopper**: Shopper tự xác nhận → Nhận quà
7. **RejectByPG**: Hotline từ chối → Vẫn có thể xét duyệt lại
8. **ApprovedByPG**: Hotline phê duyệt → Nhận quà

**Bước 5: Nhận quà**
- **Quà E-Gift**: Gọi API Gift Vendor để phát quà qua SMS/Zalo ZNS
- **Quà vật lý**: Tạo order, hiển thị QR code, chờ PG xác nhận tại store

### 6.2 Lỗi hệ thống

#### A. Lỗi hệ thống và kỹ thuật
- SystemError, OCR thất bại, ExceptionBill
- BlurInvoice, MaxAngleBill, ReverseBill
- TwoInvoice, IncorrectImageSequence

#### B. Lỗi về thông tin hóa đơn
- MallNull/StoreNull, UnsupportedMall/UnsupportedStore
- ReceiptIdBillIsNull, BillDateIsEmpty
- BillExist, ReprintBill/Reprint

#### C. Lỗi về điều kiện tham gia
- NotEnoughAmount, ExpiredDatetime
- InvalidValidityTime, MinimumProductCount
- CampaignNotStart, MStoreIsNotSupported

#### D. Lỗi về giới hạn và gian lận
- BlackList, FingerLimit, LimitGift
- MaxCapture, MaxTotalAmountU
- MemberCodeInValid, BillCodeDuplicate

#### E. Lỗi về quà tặng
- StockIsEmpty, NotEnoughCoin, NotEnoughTurn
- LuckyDrawNoProduct
- NoVoucherAllocate, UnableAllocateVoucher
- ErrorCallAPIGotIt

---

## 7. PG Journey - Phát Quà & Sampling

### 7.1 Quy trình trao quà vật lý

**Điều kiện đầu vào**:
- Shopper đã hoàn tất chụp hóa đơn
- Có thông tin quà vật lý trong "Quà của tôi"

**Các bước**:
1. PG đăng nhập PG App bằng user/password
2. Chọn campaign đã được phân quyền
3. Scan QR code nhận quà của shopper (từ Chi tiết quà)
4. Xác nhận trao quà trên PG App
5. Trao quà vật lý cho shopper

### 7.2 Quy trình phát mẫu thử (Sampling)

**Điều kiện đầu vào**:
- Shopper có tài khoản UHub (đã đăng nhập ít nhất 1 lần)
- PG có quyền truy cập campaign sampling
- Có sẵn stock mẫu thử tại cửa hàng

**Các bước**:
1. PG đăng nhập PG App
2. Chọn campaign sampling
3. Hướng dẫn shopper đăng nhập UHub → "QR của tôi"
4. PG scan mã QR cá nhân của shopper
5. Chọn loại mẫu sampling phù hợp
6. Xác nhận phát mẫu thử trên PG App
7. Trao mẫu thử vật lý cho shopper

---

## 8. Standard Scheme (Các dạng campaign chuẩn)

| Loại Campaign | Mô tả | Ví dụ |
|---|---|---|
| **Giỏ hàng Unilever + E-Voucher** | Mua giỏ hàng Unilever xxx VNĐ → Nhận 1 e-voucher | Đơn hàng Unilever 599K VNĐ → Voucher GotIt 50K |
| **Giỏ hàng Unilever + Quà vật lý** | Mua giỏ hàng Unilever xxx VNĐ → Nhận quà tại store | Mua giỏ hàng 349K chăm sóc sắc đẹp → 1 mẫu thử |
| **Giỏ hàng Unilever + Quay may mắn** | Mua giỏ hàng Unilever xxx VNĐ → 1 lượt quay | - |
| **Giỏ hàng + SKU + Ủng hộ quỹ** | Mua giỏ hàng xxx VNĐ có SKU → Góp tiền quỹ | Đơn 399K có Lifebuoy/Clear → Góp 5K quỹ |
| **Số lượng SKU + E-Voucher** | Mua N số lượng SKU → Nhận 1 e-voucher | Mua 02 gói KNORR 900G → E-Voucher 50K |
| **Số lượng SKU + Quà vật lý** | Mua N số lượng SKU → Nhận quà tại store | Buy 3 Closeup White Now → 1 gấu đeo chéo |
| **Giỏ hàng SKU + E-Voucher** | Mua giỏ hàng SKU xxx VNĐ → Nhận 1 e-voucher | Giỏ hàng 89K P/S và Close Up → Voucher 10K |
| **Giỏ hàng SKU + Quà vật lý** | Mua giỏ hàng SKU xxx VNĐ → Nhận quà tại store | Mua 299K sản phẩm Giặt Giũ → 1 chai dầu ăn |
| **Kết hợp + Quay may mắn** | Mua giỏ hàng SKU + có SKU cụ thể → Quay may mắn | Đơn 599K Unilever có Comfort 3.2L + Sunlight → Vòng quay |

---

## 9. Pain Points Của Quy Trình Cũ

### 9.1 Các vấn đề chính
1. **No Gift Recall Alignment**: Không thể thu hồi quà sau campaign
2. **No System Control Gift-in/Gift-out at Agency WHs**: Thiếu kiểm soát hệ thống tại kho Agency
3. **No liquidation for Gift-in/Gift out at Stores level**: Không có thanh lý tự động tại Store
4. **Manual Reconciliation Post-campaign**: Đối soát thủ công sau campaign

### 9.2 Quy trình cũ
- BU sử dụng Excel để phân bổ quà (Allocation)
- Agency confirm nhận hàng thủ công
- Không có confirm nhận hàng tại store level
- Đối soát thủ công giữa Excel và dữ liệu UHUB
- Agency báo cáo bằng Excel số lượng quà còn lại

---

## 10. Danh Sách Quy Trình Cần Số Hóa

| Quy trình | Trạng thái |
|---|---|
| Quy trình đồng bộ từ UGMS → UHub | ✅ Đã có API |
| Quy trình bổ sung quà, cùng scheme | 🔲 Cần phân tích |
| Quy trình trả quà về UHub → UGMS | 🔲 Cần phân tích |
| Quy trình đổi Agency (remove phân bổ → chuyển agency) | 🔲 Cần phân tích |
| Quy trình Log Form / Allocation / Plan Mechanic | 🔲 Cần phân tích |
| Quy trình review thực nhận Show danh sách store phân bổ | 🔲 Cần phân tích |
| Quy trình phân bổ quà cho campaign | ✅ Đã có (B2, B3) |
| Quy trình đối soát, UHub → UGMS | ✅ Đã có (B6) |
| Quy trình chuyển scheme | 🔲 Cần phân tích |
| Quy trình điều chỉnh kho Agency → ticket | ✅ Đã có (B2, B3, B6) |
| Report | 🔲 Cần phân tích |
| Rpt vòng dời quà theo scheme | 🔲 Cần phân tích |

---

## 11. Ghi Chú Kỹ Thuật

### 11.1 Các tính năng cần bổ sung cho UHub (CR)
- Cập nhật Email BU (theo Scheme ID)
- Cập nhật thông tin Agency vận hành campaign (theo Scheme ID)
- Cập nhật Scheme ID khi tạo Campaign
- Cập nhật Allocation by store theo Scheme ID + Campaign ID
- Cập nhật GiftCode cho mỗi lượt redemption

### 11.2 Các tính năng cần bổ sung cho ITU Log Form
- Scheme ID
- Giftcode ID
- Agency ID
- Xử lý trường hợp New Gift Code

### 11.3 Timeline quan trọng
- Setup Gift trên UGMS: 15-30 ngày trước campaign live
- Request Setup Campaign: 10-15 ngày trước campaign live
- Agency ticket deadline: ≤168 giờ (7 ngày) trước campaign live
- Sales ticket deadline: ≤48 giờ trước campaign live
- Agency ticket SLA: BU approval trong 24 giờ
- Sales ticket SLA: BU approval trong 12 giờ
- Gift Recall timeline: 5 ngày sau end-campaign

---

**Tài liệu tham khảo**:
- `/Users/akechi/DU1/2025-Aug-Uhub-E2E/UHub/context/`
- Lịch sử phát triển: Tháng 05/2020 đến nay
- Tác giả: TriNM - PM của dự án UHub
