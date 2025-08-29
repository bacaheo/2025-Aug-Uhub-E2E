# Quy Trình Cũ - Gift Management System

## Tổng Quan Bảng Quy Trình

|  | B1.Gift Planning | B2.Gift Delivery to Agency WHs | B3.Gift Delivery to Stores | B4. Gift Usage | B5. Gift Recall to Agency WHs | 6\. Stock Management | PainPoint |
| --- | --- | --- | --- | --- | --- | --- | --- |
| KA | 1.Customer Alignment |  |  |  |  |  | Painpoint<br>1.No Gift Recall Alignment Cannot recall gift post campaign<br>2.No System control Gift-on/Gift-out at Agency WHs<br>3.No liquidation for Gift-in/Gift out at Stores level<br>4.Manual Reconciliation Post-campaign |
| BU | 2.1.UGMS Setup Gift Vol | 2.2.Excel Allocation |  | 2.3.UTOP Setup UHUB campaign |  | 9.Manual Reconciliation (Excel x UHUB) |  |
| DC | 3.Linfox Delivery |  |  |  |  |  |  |
| Agency |  | 4.Manual Confirm | 5.Deliver to Store | 6.PG (UHUB Support) | 7.Recall to Agency WHs | 8.Excel Reporting Actual Gift Left |  |
| Sales |  |  |  |  |  |  |  |

## Mô Tả Chi Tiết Từng Bước

### B1. Gift Planning (Lập Kế Hoạch Quà cho Campaign)

#### 1. Customer Alignment (KA)
- Unilever thỏa thuận với **Key Account (KA)** về các chương trình khuyến mãi
- Xác định loại quà tặng, số lượng, thời gian triển khai tại các siêu thị (Coopmart, Big C, Lotte, etc.)
- Thống nhất về Mechanics của campaigns (giỏ hàng + E-voucher, SKU + quà vật lý, etc.)

#### 2.1. UGMS Setup Gift Vol (BU)
- **Brand Team (BT)** thiết lập số lượng quà tặng trong hệ thống UGMS
- Tính toán dự kiến số lượng quà cần chuẩn bị cho từng campaign

### B2. Gift Delivery to Agency WHs (Giao Quà Đến Kho Agency)

#### 2.2. Excel Allocation (BU)
- **BU (Business Unit)** phân bổ quà tặng theo store bằng Excel thủ công

#### 3. Linfox Delivery (DC)
- **Distribution Center (DC)** sử dụng Linfox để vận chuyển quà tặng
- Giao hàng từ kho chính đến các kho Agency

#### 4. Manual Confirm (Agency)
- **Agency** thực hiện confirm nhận hàng thủ công
- Không có hệ thống tự động theo dõi inventory
- **Pain Point**: Thiếu thanh lý tự động cho Gift-in/Gift-out tại Kho Agency

### B3. Gift Delivery to Stores (Giao Quà Đến Store)

#### 5. Deliver to Store (Agency)
- **Agency** phân phối quà tặng đến các Store
- Không có confirm nhận hàng tại store level
- **Pain Point**: Thiếu thanh lý tự động cho Gift-in/Gift-out tại Store

### B4. Gift Usage (Sử Dụng Quà Tặng)

#### 2.3. UTOP Setup UHUB Campaign (BU)
- **BU** yêu cầu **UTOP Admin** thiết lập campaign trên UHub
- **UTOP Admin** tư vấn tính khả thi và setup chương trình CTKM

#### 6. PG (UHUB Support) (Agency)
- **PG (Promoter Girl)** tại Store hỗ trợ shopper:
  - Hướng dẫn tham gia CTKM qua webapp UHub
  - Phát quà mẫu thử và quà vật lý cho khách hàng
  - Scan QR code từ shopper để xác nhận trao quà
  - Xác nhận phát mẫu sampling trên PG App

### B5. Gift Recall to Agency WHs (Thu Hồi Quà Về Kho Agency)

#### 7. Recall to Agency WHs (Agency)
- **Agency** thu hồi quà thừa từ Store về kho Agency
- **Pain Point**: Không có Gift Recall Alignment - không thể thu hồi quà sau campaign

### B6. Stock Management (Quản Lý Kho)

#### 8. Excel Reporting Actual Gift Left (Agency)
- **Agency** báo cáo bằng Excel số lượng quà còn lại thực tế
- Hoàn toàn thủ công, không tự động

#### 9. Manual Reconciliation (Excel x UHUB) (BU)
- **BU** thực hiện đối soát thủ công giữa Excel và dữ liệu UHUB
- **Pain Point**: Manual Reconciliation Post-campaign

## Các Pain Points Chính Trong Quy Trình Cũ

1. **No Gift Recall Alignment**: Không thể thu hồi quà sau campaign
2. **No System Control Gift-in/Gift-out at Agency WHs**: Thiếu kiểm soát hệ thống tại kho Agency
3. **No liquidation for Gift-in/Gift out at Stores level**: Không có thanh lý tự động tại Store
4. **Manual Reconciliation Post-campaign**: Đối soát thủ công sau campaign

## Kết Luận

Quy trình cũ này hoàn toàn dựa vào Excel và quy trình thủ công, tạo ra nhiều điểm yếu trong việc theo dõi và quản lý quà tặng từ kho đến tay người tiêu dùng. Điều này dẫn đến việc cần thiết phải phát triển hệ thống UHub để tự động hóa và kiểm soát tốt hơn toàn bộ quy trình gift management.