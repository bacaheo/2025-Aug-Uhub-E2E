# Danh sách Quy trình UHub - UGMS

## Tổng quan
Thư mục này chứa các quy trình nghiệp vụ của hệ thống UHub - UGMS được mô tả bằng PlantUML Activity Diagrams.

## Danh sách Quy trình

### 1. Quy trình đồng bộ UGMS => UHub
**File:** `1_Dong_bo_UGMS_UHub.puml`
- Đồng bộ dữ liệu Gift Code từ UGMS sang UHub
- Xử lý real-time và batch sync
- Retry mechanism và error handling

### 2. Quy trình bổ sung quà cùng Scheme
**File:** `2_Bo_sung_qua_cung_scheme.puml`
- Bổ sung thêm Gift Code cho campaign đang chạy
- Giữ nguyên Scheme để đảm bảo tính nhất quán
- Approval workflow theo quantity

### 3. Quy trình trả quà UHub => UGMS
**File:** `3_Tra_qua_UHub_UGMS.puml`
- Thu hồi Gift Code không sử dụng
- Xử lý damaged goods và write-off
- Cập nhật inventory và reconciliation

### 4. Quy trình đổi Agency
**File:** `4_Doi_Agency.puml`
- Chuyển đổi Agency quản lý Gift Code
- Remove phân bổ cũ và tạo phân bổ mới
- Maintain gift history và audit trail

### 5. Quy trình Log Form / Allocation / Plan Mechanic
**File:** `5_Log_Form_Allocation_Plan.puml`
- Setup campaign mechanics
- Configure allocation rules
- Automation workflow setup

### 6. Quy trình Review thực nhận
**File:** `6_Review_thuc_nhan.puml`
- Xác nhận thực nhận tại Store
- So sánh allocated vs actual
- Show danh sách store phân bổ

### 7. Quy trình phân bổ quà cho Campaign
**File:** `7_Phan_bo_qua_campaign.puml`
- Tạo allocation plan cho campaign
- Distribution matrix by store
- Real-time monitoring

### 8. Quy trình đối soát UHub => UGMS
**File:** `8_Doi_soat_UHub_UGMS.puml`
- Reconciliation định kỳ
- Three-way matching
- Discrepancy resolution

### 9. Quy trình chuyển Scheme
**File:** `9_Chuyen_scheme.puml`
- Transfer Gift Code sang Scheme mới
- Impact analysis
- Rollback plan

### 10. Quy trình điều chỉnh kho Agency
**File:** `10_Dieu_chinh_kho_Agency.puml`
- Xử lý inventory discrepancy qua ticket system
- Investigation workflow
- Adjustment approval matrix

### 11. Report vòng đời quà theo Scheme
**File:** `11_Report_vong_doi_qua.puml`
- Lifecycle analytics
- Performance metrics
- Multiple export formats

## Cách sử dụng

### Render PlantUML diagrams:
```bash
# Render một file cụ thể
java -jar plantuml.jar <filename>.puml

# Render tất cả files trong thư mục
java -jar plantuml.jar /Users/akechi/DU1/2025-Aug-Uhub-E2E/UHub/Quy-trinh/*.puml
```

### View online:
Copy nội dung file `.puml` và paste vào http://www.plantuml.com/plantuml/

## Chú thích

### Swimlanes (Làn bơi)
- **Màu xanh dương nhạt**: Hệ thống/Actor khởi tạo
- **Màu xanh lá nhạt**: Hệ thống xử lý chính
- **Màu vàng nhạt**: Integration/Middle layer
- **Màu hồng nhạt**: Approval/Review
- **Màu cam nhạt**: Support/Audit

### Decision Points
- **Diamond vàng cam**: Điểm quyết định quan trọng
- **if-then-else**: Luồng xử lý có điều kiện

### Notes
- **Note right/left**: Giải thích chi tiết
- **Legend**: Business rules và SLA

## Tài liệu liên quan
- `/Users/akechi/DU1/2025-Aug-Uhub-E2E/UHub/context/` - Context và analysis
- `/Users/akechi/DU1/2025-Aug-Uhub-E2E/UHub/bpmn/` - BPMN diagrams
- `/Users/akechi/DU1/2025-Aug-Uhub-E2E/UHub/puml/` - Các PlantUML diagrams khác

## Maintenance
Cập nhật lần cuối: 2024
Người tạo: UHub Development Team
