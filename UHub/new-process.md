# Quy Trình Mới - Gift Management System với UHub Integration

## Tổng Quan Bảng Quy Trình

|  | B1.Gift Planning | B2.Gift Delivery to Agency WHs | B3.Gift Delivery to Stores | B4. Gift Usage | B5. Gift Recall to Agency WHs | 6\. Stock Management | 7\. Stock Reduction/Recall / Re-allocation (if any) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| KA | 1.Customer Alignment (Gift recall Alignment) |  |  |  |  |  |  |
| BU | 2.1.UGMS Setup Gift Vol<br>=(integration)=>2.2 Đồng bộ thông tin cho Uhub (Schme, GiftCode, số lượng, phân bổ) | 2.2.(New) Uhub store Allocation<br>=> 4. Agency kiểm tra thực tế và confirm | 2.3.(New) Uhub Gift Report | 2.4.UTOP Setup UHUB campaign<br>=> 7. campaign ready thì PG mới thao tác & tặng quà được |  | 10.(New) Uhub Reconciliation | 11.(New) Raise ticket to UGMS/ UHUB => Level 2 - approval =(Integration)=>UHUB/UGMS |
| DC | 3.Linfox Delivery |  |  |  |  |  |  |
| Agency |  | 4.(New) Confirmn on Uhub | 5.Deliver to Store<br>=>6. Sale at store kiểm tra thực tế & confirm | 7.PG (UHUB Support) | 8.Recall to Agency WHs | 9.(New) Uhub Reconciliation<br>=> 10. BU review & confirm |  |
| Sales |  |  | 6.(New) Confirm on Uhub |  |  |  |  |

## Mô Tả Chi Tiết Từng Bước Quy Trình Mới

### B1. Gift Planning (Lập Kế Hoạch Quà cho Campaign)

#### 1. Customer Alignment với Gift Recall Alignment (KA)
- **Key Account (KA)** thỏa thuận với Unilever về các chương trình khuyến mãi
- **CẢI TIẾN MỚI**: Bổ sung **Gift Recall Alignment** - thống nhất về việc thu hồi quà sau campaign
- Xác định:
  - Loại quà tặng, số lượng, thời gian triển khai
  - Mechanics của campaigns (Standard Schemes như trong lịch sử UHub)
  - **Quy trình thu hồi quà** rõ ràng từ đầu
  - Điều kiện và thời gian thu hồi quà thừa

#### 2.1. UGMS Setup Gift Vol (BU)
- **Brand Team (BT)** thiết lập số lượng quà tặng trong hệ thống UGMS
- Tính toán dự kiến số lượng quà cần chuẩn bị cho từng campaign
- **CẢI TIẾN MỚI**: Chuẩn bị dữ liệu cho integration với UHub

#### 2.2. Đồng Bộ Thông Tin UGMS-UHub (Integration) (BU)
- **TÍNH NĂNG MỚI**: Tự động đồng bộ từ UGMS sang UHub:
  - **Scheme**: Thông tin mechanics campaign
  - **Gift Code**: Mã quà tặng từ UGMS
  - **Số lượng**: Volume quà từng loại
  - **Phân bổ**: Allocation theo store/region
- Thay thế Excel thủ công bằng integration tự động
- Đảm bảo tính chính xác và real-time update

### B2. Gift Delivery to Agency WHs (Giao Quà Đến Kho Agency)

#### 3. Linfox Delivery (DC)
- **Distribution Center (DC)** sử dụng Linfox để vận chuyển quà tặng
- Giao hàng từ kho chính đến các kho Agency (tương tự quy trình cũ)

#### 2.2. UHub Store Allocation (BU)
- **TÍNH NĂNG MỚI**: UHub tự động phân bổ quà theo store
- Thay thế Excel Allocation thủ công
- Dựa trên dữ liệu đã đồng bộ từ UGMS
- Real-time tracking số lượng phân bổ

#### 4. Agency Kiểm Tra Thực Tế và Confirm on UHub (Agency)
- **CẢI TIẾN MỚI**: Agency confirm nhận hàng trực tiếp trên UHub thay vì thủ công
- **Quy trình**:
  1. Agency nhận quà vật lý từ Linfox
  2. Kiểm tra số lượng thực tế
  3. **Confirm trên UHub system** (không phải Excel)
  4. Hệ thống tự động cập nhật inventory
- **Lợi ích**: Kiểm soát Gift-in tại Agency WHs tự động

### B3. Gift Delivery to Stores (Giao Quà Đến Store)

#### 5. Deliver to Store (Agency)
- Agency phân phối quà tặng đến các Store

#### 6. Sales Confirm on UHub (Sales)
- **TÍNH NĂNG MỚI**: Sales tại store confirm nhận hàng trên UHub
- **Quy trình**:
  1. Sales nhận quà từ Agency
  2. Kiểm tra số lượng thực tế tại store
  3. **Confirm trên UHub system**
  4. Hệ thống tự động cập nhật inventory tại store level
- **Lợi ích**: Kiểm soát Gift-in/Gift-out tại Store level tự động

### B4. Gift Usage (Sử Dụng Quà Tặng)

#### 2.3. UHub Gift Report (BU)
- **TÍNH NĂNG MỚI**: Báo cáo quà tặng tự động từ UHub
- Real-time tracking:
  - Số lượng quà đã phát
  - Số lượng quà còn lại tại mỗi store
  - Performance của từng campaign
- Thay thế báo cáo Excel thủ công

#### 2.4. UTOP Setup UHUB Campaign (BU)
- **BU** yêu cầu **UTOP Admin** thiết lập campaign trên UHub
- **Điều kiện**: Chỉ khi campaign ready, PG mới được phép thao tác & tặng quà
- Đảm bảo đồng bộ giữa inventory và campaign activation

#### 7. PG (UHUB Support) (Agency)
- **PG (Promoter Girl)** tại Store hỗ trợ shopper (tương tự quy trình cũ):
  - Hướng dẫn tham gia CTKM qua webapp UHub
  - Phát quà mẫu thử và quà vật lý cho khách hàng
  - Scan QR code từ shopper để xác nhận trao quà
  - Xác nhận phát mẫu sampling trên PG App
- **CẢI TIẾN MỚI**: Real-time update report Gift Usage khi PG confirm trao quà

### B5. Gift Recall to Agency WHs (Thu Hồi Quà Về Kho Agency)

#### 8. Recall to Agency WHs (Agency)
- Agency thu hồi quà thừa từ Store về kho Agency
- **CẢI TIẾN MỚI**: Có Gift Recall Alignment từ bước 1
- Quy trình thu hồi đã được thống nhất từ đầu

### B6. Stock Management (Quản Lý Kho)

#### 9. UHub Reconciliation (Agency)
- **TÍNH NĂNG MỚI**: Agency thực hiện reconciliation trên UHub
- **Quy trình**:
  1. Agency kiểm tra số lượng quà thực tế còn lại
  2. Nhập thông tin vào UHub system
  3. Hệ thống tự động so sánh với dữ liệu tracking
  4. Báo cáo discrepancy (nếu có)

#### 10. BU Review & Confirm (BU)
- **TÍNH NĂNG MỚI**: BU review và confirm reconciliation từ Agency
- **Quy trình**:
  1. BU nhận báo cáo reconciliation từ UHub
  2. Review các discrepancy (nếu có)
  3. Confirm chấp nhận hoặc yêu cầu điều chỉnh
  4. Finalize inventory status

#### 10. UHub Reconciliation (BU)
- **TÍNH NĂNG MỚI**: BU thực hiện reconciliation tổng thể trên UHub
- Thay thế Manual Reconciliation Excel x UHUB
- Automatic matching data giữa:
  - UGMS records
  - UHub tracking
  - Agency reports
  - Actual usage data

### B7. Stock Reduction/Recall/Re-allocation (Nếu Cần)

#### 11. Raise Ticket to UGMS/UHUB với Level 2 Approval (BU)
- **TÍNH NĂNG HOÀN TOÀN MỚI**: Ticket system với approval workflow
- **Quy trình**:
  1. BU tạo ticket trong system cho:
     - Stock reduction (giảm kho)
     - Gift recall (thu hồi quà)
     - Re-allocation (phân bổ lại)
  2. **Level 2 Approval**: Cần phê duyệt từ cấp quản lý
  3. **Integration**: Tự động sync giữa UHUB và UGMS
  4. Update inventory và campaign settings tương ứng

## So Sánh Cải Tiến với Quy Trình Cũ

### Giải Quyết Các Pain Points Cũ

| Pain Point Cũ | Giải Pháp Mới |
|---|---|
| **No Gift Recall Alignment** | ✅ Gift Recall Alignment trong Customer Alignment (Bước 1) |
| **No System Control Gift-in/Gift-out at Agency WHs** | ✅ Agency Confirm on UHub (Bước 4) |
| **No liquidation for Gift-in/Gift out at Stores level** | ✅ Sales Confirm on UHub (Bước 6) |
| **Manual Reconciliation Post-campaign** | ✅ UHub Reconciliation (Bước 9, 10) |

### Các Tính Năng Mới

1. **UGMS-UHub Integration**: Đồng bộ tự động thông tin quà tặng
2. **Digital Confirmation**: Agency và Sales confirm trên UHub thay vì thủ công  
3. **Real-time Reporting**: UHub Gift Report thay thế Excel báo cáo
4. **Automatic Reconciliation**: So sánh tự động thay vì thủ công
5. **Approval Workflow**: Ticket system với Level 2 approval
6. **End-to-end Tracking**: Theo dõi quà từ kho đến tay người tiêu dùng

## Lợi Ích Của Quy Trình Mới

### 1. Tự Động Hóa
- Giảm thiểu thao tác thủ công
- Tự động đồng bộ dữ liệu giữa các system
- Real-time inventory tracking

### 2. Kiểm Soát Tốt Hơn  
- Theo dõi chi tiết từng bước trong chuỗi cung ứng
- Digital confirmation tại mọi điểm chuyển giao
- Abnormal detection và fraud prevention

### 3. Báo Cáo Chính Xác
- Real-time dashboard và reporting
- Automatic reconciliation
- Data-driven decision making

### 4. Compliance & Governance
- Approval workflow rõ ràng
- Audit trail đầy đủ  
- Risk management tốt hơn

### 5. Hiệu Quả Vận Hành
- Giảm thời gian processing
- Giảm sai sót do thủ công
- Tối ưu hóa chi phí vận hành

```plantuml
@startuml
title Gift Management x UHub — Activity Diagram (Swimlanes)

skinparam activity {
  BackgroundColor White
  BarColor #999999
}
skinparam arrow {
  Color #555555
}
skinparam partition {
  BorderColor #888888
  BackgroundColor #F9F9F9
}

|B1.Gift Planning|
start

partition "KA" {
  :Customer Alignment\n(Gift Recall Alignment);
}
note right
  <b>Quy trình internal Unilever</b>
  * Team KA của Unilever plan campaign/quy trình
   thu hồi quà với KA
  ====
  * Bước này để thống nhất giữa Unilever và KA
  * Không số hoá bước này
end note


partition "BU" {
  :Setup Gift\n(GiftCode,Scheme,Customer,Ship_to)\n --> trên App UGMS;
  :Sync Setup Gift UGMS --> UHub\n(\nGiftCode, \nScheme (Start,End,Mechanics,Giftcode,Quantity),\nCustomer, \nShip_to\n) ;
    note right
      <b>Quy trình Unilever-Utop</b>
      * BU cập nhật thông tin Setup Gift
      lên UGMS (từ thông tin đã thống nhất với KA)
      * UGMS (New Feature): đẩy thông tin Setup Gift
      cho UHub qua API
      ====
      * UHub (New Feature): nhận thông tin Setup Gift
      qua API từ bước này
    end note
  :Request Setup Campaign\n(GiftCode, Scheme, Allocation by store)\n-->Trên ITU Log Form;
  :Sync Request Setup Campaign\n ITU Log Form --> UHub Admin via auto email\n(\nGiftCode, \nScheme (Start,End,Mechanics,Giftcode,Quantity),\nThông tin SKUs in trên hoá đơn\nAllocation by store\nThông tin Agency vận hành campaign\n) ;
    note right
      <b>Quy trình Unilever-Utop</b>
      * BU cập nhật thông tin Request Setup Campaign
      lên ITU Log Form (từ thông tin đã thống nhất với KA)
      * ITU Log Form (Đã có): đẩy thông tin Request Setup Campaign
      cho UHub Admin qua Auto Email
      * ITU Log Form (Cần bổ sung thêm trường dữ liệu từ UGMS):
      + Scheme ID
      + Giftcode ID
      + Agency ID
      * Chú ý trường hợp New Gift Code
      ====
      * Bước này để Unilever gửi Request Setup Campaign cho Utop Admin
      * Không số hoá bước này ở phase này
    end note
  
}

partition "Utop Admin" {
  :Cập nhật thông tin campaign\nTrên Utop Admin Portal;
}
note right
  <b>Quy trình Unilever-Utop</b>
  * Utop Admin nhận email thông tin Log Form,
  thông tin campaign trên Admin Portal
  * UHub (CR Bổ sung tính năng):
  + Cập nhật thông tin Agency vận hành campaign (Theo Scheme ID)
  + Cập nhật Scheme ID khi tạo Campaign
  + Cập nhật Allocation by store theo Scheme ID + Campaign ID
  + Cập nhật GiftCode cho mỗi lượt redemption
  ====
  * Point này cần phân tích kiến trúc
  * Về sau sẽ bỏ ITU Log Form để BU nhập trực tiếp trên Admin Portal
end note


partition "DC" {
  :Linfox Delivery --> Agency WHs;
}
note right
  <b>Quy trình internal Unilever</b>
  * Quy trình kho vận
  từ Kho Unilever sang kho Agency
  ====
  * Không số hoá bước này
end note
|B2.Gift Delivery to Agency WHs|
partition "Agency" {
  :Nhận phiếu xuất từ kho Unilever\nvà kiểm tra hàng;
  note right
    <b>Quy trình nhập kho Agency</b>
    * Quy trình điều chỉnh Gift Vol/Allocation by store
     trước go-live campaign
    * Trường hợp nhập kho khác số giao từ kho Unilever,
    cần BU cập nhật ITU Log Form để đồng bộ 
    (Giftcode, Quantity, Allocation by store)
    ====
    * 
  end note
  if (Đủ hàng & không hư hỏng?) then (Yes)
  :Agency confirm nhận hàng theo Gift Vol on UHub\n(Agency WHs);
  else (No)
  :Agency confirm nhận hàng theo số thực tế \n& nguyên nhân on UHub\n(Agency WHs);
  
  endif
  |B3.Gift Delivery to Stores|
  :Deliver to Stores\ntheo Allocation by store;
}

partition "Sales" {
  :Kiểm tra hàng so với Allocation by store;
  note right
      <b>Quy trình nhập kho Store</b>
      * Quy trình điều chỉnh Gift Vol / Allocation by store
      trước go-live campaign
      * Trường hợp nhập kho khác số giao từ kho Unilever,
      cần BU cập nhật ITU Log Form để đồng bộ 
      (Giftcode, Quantity, Allocation by store)
      ====
      * 
    end note

  if (Đủ hàng & không hư hỏng?) Then (Yes)
  :Sales Confirm nhận hàng\ntheo Allocation by store\non UHub\n(at Store);
  Else (No)
  :Sales confirm nhận hàng\ntheo số thực tế & nguyên nhân\non UHub\n(at Store);
    
  Endif
}

'--- Campaign setup & readiness gate (loop until ready)
repeat
  partition "Utop Admin" {
    :UTOP Setup UHub Campaign;
  }
  
repeat while (Campaign ready?) is (No)

|B4. Gift Usage|
partition "Agency" {
  :PG operates campaign on UHub\n(UHub Sampling, UHub Redemption);
}

partition "BU" {
  :BU xem UHub PowerBI Report (refresh 3 lần/ngày)\n- (NEW) Gift Report\n- Campaign Performance;
}

|B5. Gift Recall to Agency WHs|
'--- Recall & reconciliation cycle
partition "Agency" {
  :Recall Gifts\nto Agency WHs;
  note right
    <b>Quy trình thu hồi quà từ Store</b>
    * Khi End-campaign, Agency thu hồi quà
    từ Store về Kho Agency
    * Hoạt động thu hồi trong vòng 5 ngày
    tính từ ngày end-campaign
    ====
    * 
  end note
}

|6. Stock Management|
'--- Reconciliation loop until inventory is balanced
' repeat
  repeat
  partition "Agency" {
    
    :Kiểm tra hàng thu hồi thực tế\nso với report đối soát on UHub;

    if (Đủ hàng & không hư hỏng?) Then (Yes)
      :Agency confirm số của report đối soát\non UHub;
    else (No)
      :Agency submit ticket đối soát\ntheo số điều chỉnh thực tế & nguyên nhân\non UHub;
      |7. Stock Reduction/Recall/ Re-allocation (if any)|
    partition "BU" {
      :Raise Ticket via Email\n(Level-2 Approval);
    }
    partition "BU Level-2" {
      :Review & Approval Ticket via Email;
    }
      if (L2 Approved?) then (Yes)
        partition "Utop Admin" {
          :Sync UHub <-> UGMS \n(Điều chỉnh tồn\npost-camapign);
        }
      else (No)
        partition "BU" {
          :Request Agency\ncorrection / re-check;
        }
      endif
    ' }
    Endif
    
  }
  repeat while (BU Level-2 Approved?) is (No)

:END;

stop
@enduml

@enduml


```