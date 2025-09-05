# Quy Trình Mới - Gift Management System với UHub Integration

## Tổng Quan Bảng Quy Trình V2

|  | B1.Gift Planning | B2.Gift Delivery to Agency WHs | B3.Gift Delivery to Stores | B4. Gift Usage | B5. Gift Recall to Agency WHs | B6. Stock Management | B7. Stock Reduction/Recall/ Re-allocation (if any) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| KA | 1.Customer Alignment (Gift Recall Alignment) |  |  |  |  |  |  |
| BU | 2.Setup Gift on UGMS<br>3.Sync UGMS → UHub<br>4.Request Setup Campaign on ITU Log Form<br>5.Sync ITU Log Form → UHub Admin | **6.BU Review & Approval Tickets**<br>- Agency tickets (≤168h before live)<br>- Sales tickets (≤48h before live)<br>- 24h/12h approval SLA | | 11.BU xem UHub PowerBI Report<br>- Gift Report (NEW)<br>- Campaign Performance<br>+ 12.**BU Trigger Operations**<br>- Gift transfer between stores<br>- Agency changes |  |  | 17.**BU Raise & Forward Ticket**<br>via Email (Level-2 Approval)<br>+ UHub auto email alerts |
| BU Level-2 |  |  |  |  |  |  | 18.Review & Approval Ticket via Email<br>**(NEW: Approval with evidences)** |
| Utop Admin | 6.Cập nhật thông tin campaign<br>trên Utop Admin Portal<br>+ **Email BU theo Scheme ID** | | | 12.**UTOP Setup & Go-live Campaign**<br>**LOOP until Campaign ready = Yes**<br>(Campaign readiness gate)<br>theo Request của BU | |  | 19.**Utop Admin Confirm Ticket**<br>Sync UHub ↔ UGMS<br>(Điều chỉnh tồn + evidences) |
| DC | 7.Linfox Delivery → Agency WHs |  |  |  |  |  |  |
| Agency |  | 8.Nhận & kiểm tra hàng<br>**9.Agency Ticket System**<br>- Confirm theo Gift Vol (normal)<br>- Tạo ticket số thực tế + nguyên nhân<br>Xuất Allocation by store | 10.Deliver to Stores<br>theo Allocation by store | 13.PG operates campaign on UHub<br>(Sampling & Redemption) | 14.Recall Gifts to Agency WHs<br>(5 ngày sau end-campaign) | 15.Kiểm tra hàng thu hồi<br>16.**Agency Reconciliation**<br>- Digital Confirm report đối soát (normal)<br>=> Sync UHub ↔ UGMS​<br>- Submit ticket discrepancy |  |
| Sales |  |  | 11.Kiểm tra hàng<br>**12.Sales Ticket System**<br>- Confirm theo Allocation (normal)<br>- Tạo ticket số thực tế + nguyên nhân |  |  |  |  |


## Flow Tổng Quan Bảng Quy Trình

```plantuml
@startuml
title E2E Gift Management x UHub — Activity Diagram

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
|KA/BU/DC + Utop Admin|
start
:1.Gift Planning;
|Agency|
:2.Gift Delivery\nto Agency WHs;
|Agency/Sale|
:3.Gift Delivery\nto Stores;
|Agency/BU + Utop Admin|
:4.Gift Usage\n+ Dashboard;
|Agency|
:5.Gift Recall\nto Agency WHs;
:6.Stock Management;
|BU/BU Level-2|
:6.Approval:\nStock Reduction/\nRecall/\nRe-allocation (if any);
stop
@enduml
```

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

#### 11. Kiểm Tra Hàng - Sales
- **Quy trình**: Sales so sánh hàng nhận với Allocation by store
- **Lưu ý**: Vẫn có thể điều chỉnh Gift Vol trước go-live campaign nếu cần

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

#### 12. UTOP Setup UHub Campaign - Utop Admin (Campaign Readiness Gate)
- **TÍNH NĂNG MỚI ENHANCED**: **LOOP kiểm tra Campaign ready = Yes** trước khi cho phép vận hành
- **Quy trình**: Repeat loop "Campaign ready?" until = Yes
- **Điều kiện**: Chỉ khi Campaign ready = Yes, PG mới được phép thao tác
- **Đảm bảo**: Đồng bộ giữa inventory và campaign activation
- **Logic**: Blocking gate để đảm bảo campaign setup hoàn chỉnh

#### 11. BU PowerBI Report + Trigger Operations - BU
- **TÍNH NĂNG MỚI**: Gift Report được bổ sung + Operational triggers
- **Tần suất**: Refresh 3 lần/ngày
- **Nội dung Dashboard**:
  - **Gift Report**: Tracking số lượng quà đã phát/còn lại
  - **Campaign Performance**: Hiệu quả campaign theo KPIs
- **Parallel Process**: Chạy song song với PG campaign operations

#### 12. BU Trigger Operations - BU (TÍNH NĂNG HOÀN TOÀN MỚI)
- **Gift Transfer Between Stores**: BU trigger quy trình luân chuyển quà giữa các Store
  - Dựa trên performance data từ PowerBI Report
  - Điều chỉnh allocation để tối ưu hiệu quả
- **Agency Change Process**: BU trigger quy trình thay đổi Agency nếu cần
  - Dựa trên performance hoặc business requirements
  - Quản lý transfer inventory và campaign responsibility
- **Parallel Process**: Chạy song song với PG campaign operations

#### 13. PG Operates Campaign - Agency
- **Hoạt động chính**:
  - **UHub Sampling**: PG hỗ trợ shopper thử sản phẩm
  - **UHub Redemption**: PG trao quà theo mechanics
- **Tương tác**: Scan QR code, xác nhận trao quà real-time
- **Parallel Process**: Chạy song song với BU monitoring & trigger operations

### B5. Gift Recall to Agency WHs (Thu Hồi Quà Về Kho Agency)

#### 14. Recall Gifts to Agency WHs - Agency
- **Thời gian**: Trong vòng 5 ngày sau end-campaign
- **Quy trình**: Thu hồi quà thừa từ Store về Kho Agency
- **CẢI TIẾN**: Có Gift Recall Alignment rõ ràng từ bước 1

### B6. Stock Management (Quản Lý Kho)

#### 15. Kiểm Tra Hàng Thu Hồi - Agency
- **Quy trình**: Agency so sánh hàng thu hồi thực tế với report đối soát UHub
- **Mục đích**: Xác minh tính chính xác của inventory sau campaign

#### 16. Agency Reconciliation System - Agency
- **TÍNH NĂNG MỚI ENHANCED**: **LOOP kiểm tra reconciliation** cho đến khi approved
- **Logic xử lý Enhanced**:
  - **Nếu khớp**: Agency digital confirm số liệu report đối soát → Tự động Sync UHub ↔ UGMS
  - **Nếu có discrepancy**: Submit ticket đối soát với số điều chỉnh + nguyên nhân
- **Workflow Loop**: Repeat "BU Level-2 Approved?" until = Approved (not Rejected)
- **TÍNH NĂNG MỚI ENHANCED**: 
  - Digital reconciliation thay thế Excel thủ công
  - **Auto Email Alert**: UHub gửi auto email thông tin ticket cho BU
  - **Automatic Sync**: Nếu khớp, tự động cập nhật số Gift Usage post-campaign

### B7. Stock Reduction/Recall/Re-allocation (Nếu Cần)

#### 17. BU Raise & Forward Ticket (Level-2 Approval) - BU
- **Trigger**: Khi có discrepancy từ Agency reconciliation + UHub auto email alert
- **Enhanced Process**: BU Raise & Forward thay vì chỉ Raise
- **Nội dung ticket**:
  - Stock reduction (giảm kho)
  - Gift recall (thu hồi quà) 
  - Re-allocation (phân bổ lại)
- **Workflow Enhancement**: BU forward ticket lên Level-2 với context đầy đủ

#### 18. Review & Approval Ticket - BU Level-2
- **TÍNH NĂNG MỚI**: Approval workflow với cấp quản lý
- **Logic**:
  - **Approved**: Tiến hành sync UHub ↔ UGMS
  - **Rejected**: Yêu cầu Agency correction/re-check

#### 19. Utop Admin Confirm Ticket + Enhanced Sync - Utop Admin
- **Enhanced Process**: 2-step process với evidence tracking
  - **Step 1**: Utop Admin confirm ticket (audit trail)
  - **Step 2**: Sync UHub ↔ UGMS với evidences của BU Level-2 Approval
- **Mục đích**: Cập nhật inventory cuối cùng sau campaign với full traceability
- **TÍNH NĂNG MỚI ENHANCED**: 
  - Bi-directional sync để đảm bảo consistency
  - **Evidence Integration**: Sync kèm evidences của approval process
  - **Complete Audit Trail**: Link ticket → approval → system sync
- **Kết quả**: Finalize inventory status với complete governance

## Gift Management x UHub — Activity Diagram (Swimlanes)

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
  note right
    <b>Quy trình internal Unilever</b>
    * Cần triển khai trước ngày campaign live 15-30 ngày
    ====
    * 
  end note
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
  note right
    <b>Quy trình Unilever-Utop</b>
    * Cần triển khai trước ngày campaign live 10-15 ngày
    ====
    * 
  end note
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
  :Init thông tin campaign\nTrên Utop Admin Portal;
}
note right
  <b>Quy trình Unilever-Utop</b>
  * Utop Admin nhận email thông tin Log Form,
  thông tin campaign trên Admin Portal
  * UHub (CR Bổ sung tính năng):
  + Cập nhật Email BU (Theo Scheme ID)
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
  repeat
  :Agency tạo ticket nhận hàng theo số thực tế \n& nguyên nhân on UHub\n(Agency WHs);
  note right
    <b>Quy trình nhập kho Agency</b>
    * Agency phải tạo ticket trước ngày live, 7 day x 24 giờ: 168 giờ
    * Trường hợp cần tạo ticket cận ngày live <168 giờ, Agency phải nhờ
    BU Log Form CR để yêu cầu Admin Utop hỗ trợ
    ====
    * 
  end note
  :BU review,\ncập nhật Giftcode, Quantity, Allocation by store\nvà approval ticket\non UHub;
  note right
    <b>Quy trình nhập kho Agency</b>
    * BU phải re-allocation trước khi approval ticket
    * BU phải approval ticket trong vòng 24 giờ, tính từ lúc tạo
    * Quá 24 giờ ticket sẽ hết hạn
    ====
    * 
  end note
  repeat while (BU Approved?) is (Rejected) not (Approved)
  endif
  :Xuất thông tin Allocation by store\non UHub;
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
  repeat
    :Sales tạo ticket nhận hàng\ntheo số thực tế & nguyên nhân\non UHub\n(at Store);
    note right
      <b>Quy trình điều chỉnh kho Store</b>
      * Sale chỉ được thực hiện trước Campaign Live 48 giờ.
      * Trường hợp cần tạo ticket cận ngày live <48 giờ, Agency phải nhờ
      BU Log Form CR để yêu cầu Admin Utop hỗ trợ
      ====
      * 
    end note
    :BU review,\ncập nhật Giftcode, Quantity, Allocation by store\nvà approval ticket\non UHub;
    note right
      <b>Quy trình điều chỉnh kho Store</b>
      * BU phải re-allocation trước khi approval ticket
      * BU phải approval ticket trong vòng 12 giờ, tính từ lúc tạo
      * Quá 12 giờ, ticket sẽ hết hạn
      ====
      * 
    end note
  repeat while (BU Approved?) is (Rejected) not (Approved)
    
  Endif
}

'--- Campaign setup & readiness gate (loop until ready)
repeat
  partition "Utop Admin" {
    :UTOP Setup UHub Campaign\nVà Go-live theo Request setup campaign của BU;
  }
  
repeat while (Campaign ready?) is (No)

|B4. Gift Usage|
partition "Agency" {
  :PG operates campaign on UHub\n(UHub Sampling, UHub Redemption);
}

partition "BU" {
  :BU xem UHub PowerBI Report (refresh 3 lần/ngày)\n- (NEW) Gift Report\n- Campaign Performance;
}

partition "BU" {
  :BU Trigger quy trình luân chuyển quà giữa các Store (Nếu có);
  :BU Trigger quy trình thu hồi quà từ Stores về Agency WHs (Nếu có);
  :BU Trigger quy trình thay đổi Agency (Nếu có);
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
      :Agency Digital confirm report đối soát\non UHub;
      :Sync UHub <-> UGMS \n(Cập nhật số Gift Usage\npost-camapign);
    else (No)
      :Agency submit ticket đối soát\ntheo số điều chỉnh thực tế & nguyên nhân\non UHub;
      :UHub gửi auto email thông tin ticket cho BU;
      |7. Stock Reduction/Recall/ Re-allocation (if any)|
    partition "BU" {
      :BU Raise & Forward Ticket via Email\n(Get Level-2 Approval);
    }
    partition "BU Level-2" {
      :Review & Approval Ticket via Email;
    }
      if (BU Level-2 Approved?) then (Approved via email)
        partition "Utop Admin" {
          :Utop Admin Digital confirm ticket;
          :Sync UHub <-> UGMS \n(Điều chỉnh tồn post-camapign\n& evidences BU Level-2 Approved);
        }
      else (Rejected via email)
        partition "BU" {
          :BU Rejected ticket;
          :Request Agency\ncorrection / re-check;
        }
      endif
    ' }
    Endif
    
  }
  repeat while (BU Level-2 Approved?) is (Rejected) Not (Approved)

  :Quy trình tái sử dụng Gift;

:END;

stop
@enduml
```

