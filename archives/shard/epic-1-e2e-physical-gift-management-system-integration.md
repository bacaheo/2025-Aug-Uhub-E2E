# Epic 1: E2E Physical Gift Management System Integration

**Epic Goal**: Chuyển đổi quản lý quà tặng thủ công bằng Excel thành hệ thống E2E tự động bằng cách tích hợp UGMS với UHub, cho phép quy trình xác nhận số hóa (digital confirmation workflows), theo dõi thời gian thực (real-time tracking), và đối soát tự động (automatic reconciliation) trong khi duy trì chức năng UHub hiện có và trải nghiệm người dùng.

**Integration Requirements**: Tích hợp liền mạch với hành trình khách hàng UHub hiện có, khả năng tương thích ngược với quy trình PG App hiện tại, và không gây gián đoạn cho các chiến dịch đang diễn ra trong quá trình triển khai.

## Story 1.1: UGMS-UHub API Foundation Setup

Với tư cách là **thành viên Đội BU**,  
Tôi muốn **dữ liệu chiến dịch UGMS tự động đồng bộ với hệ thống UHub**,  
để **tôi có thể loại bỏ việc nhập dữ liệu Excel thủ công và đảm bảo tính nhất quán dữ liệu trên các nền tảng**.

### Acceptance Criteria
1. Tích hợp UGMS API xác thực thành công sử dụng chữ ký số RSA 2048-bit
2. Các kế hoạch chiến dịch, mã quà tặng, số lượng tự động đồng bộ từ UGMS đến cơ sở dữ liệu UHub
3. Dữ liệu phân bổ cửa hàng được ánh xạ chính xác với hệ thống phân cấp cửa hàng UHub hiện tại
4. Các quy tắc xác thực dữ liệu ngăn chặn dữ liệu chiến dịch không hợp lệ vào hệ thống UHub
5. Xử lý lỗi cung cấp phản hồi rõ ràng khi cuộc gọi UGMS API thất bại

### Integration Verification
- **IV1:** Các điểm kết nối API UHub hiện có tiếp tục hoạt động mà không bị suy giảm hiệu suất
- **IV2:** Quy trình xác thực người dùng hiện tại không bị ảnh hưởng bởi tích hợp UGMS mới
- **IV3:** Hiệu suất cơ sở dữ liệu duy trì thời gian phản hồi truy vấn hiện tại với các bảng theo dõi quà tặng mới

## Story 1.2: Agency Digital Confirmation Interface

Với tư cách là **Quản lý kho Agency**,  
Tôi muốn **giao diện di động để xác nhận số hóa việc nhận quà tặng với khả năng tải lên hình ảnh**,  
để **tôi có thể thay thế xác nhận thủ công bằng giấy và cung cấp cập nhật tồn kho thời gian thực**.

### Acceptance Criteria
1. Giao diện phản hồi di động có thể truy cập trên thiết bị iOS 13+ và Android 9+
2. Chức năng tải lên hình ảnh cho tài liệu biên nhận với tối ưu hóa nén
3. Chụp chữ ký số để xác thực xác nhận
4. Khả năng offline với đồng bộ khi kết nối được khôi phục
5. Cập nhật tồn kho tự động trong hệ thống UHub khi xác nhận

### Integration Verification
- **IV1:** Giao diện di động tuân theo các mẫu thiết kế PWA UHub hiện có và tiêu chuẩn hiệu suất
- **IV2:** Lưu trữ hình ảnh tích hợp với hạ tầng Azure CDN hiện có
- **IV3:** Cập nhật tồn kho duy trì tính toàn vẹn tham chiếu với dữ liệu chiến dịch hiện có

## Story 1.3: Sales Store-Level Confirmation System

Với tư cách là **Đại diện Sales**,  
Tôi muốn **biểu mẫu di động đơn giản để xác nhận tồn kho quà tặng tại cấp độ cửa hàng**,  
để **tôi có thể cung cấp dữ liệu tồn kho cửa hàng thời gian thực chính xác và phối hợp hiệu quả với đội PG**.

### Acceptance Criteria
1. Biểu mẫu di động đơn giản được tối ưu hóa cho việc nhập dữ liệu nhanh chóng
2. Hiển thị tồn kho quà tặng cụ thể của cửa hàng với phân bổ hiện tại so với thực tế
3. Giao diện giao tiếp với đội PG để phối hợp
4. Xác minh GPS để đảm bảo độ chính xác xác nhận dựa trên vị trí
5. Tích hợp với hệ thống phân cấp cửa hàng UHub hiện có

### Integration Verification
- **IV1:** Biểu mẫu tuân theo các mẫu thiết kế di động UHub hiện có cho trải nghiệm người dùng nhất quán
- **IV2:** Tích hợp dữ liệu cửa hàng duy trì tính toàn vẹn dữ liệu địa lý UHub hiện có
- **IV3:** Hiệu suất được tối ưu hóa cho các khu vực có kết nối mạng kém

## Story 1.4: Enhanced PG App Integration với Real-time Inventory

Với tư cách là **PG (Promotional Girl)**,  
Tôi muốn **tự động trừ tồn kho khi quét mã QR trong quá trình phân phối quà tặng**,  
để **theo dõi tồn kho luôn chính xác trong thời gian thực mà không cần cập nhật thủ công**.

### Acceptance Criteria
1. Quét mã QR tự động kích hoạt việc trừ tồn kho trong hệ thống UHub
2. Phản hồi xác nhận thời gian thực cho PG về việc trừ tồn kho thành công
3. Xử lý ngoại lệ khi tồn kho không đủ hoặc mã QR không hợp lệ
4. Khả năng xếp hàng offline với đồng bộ khi có kết nối
5. Tích hợp với quy trình ứng dụng PG hiện có với sự gián đoạn tối thiểu

### Integration Verification
- **IV1:** Chức năng nâng cấp tương thích ngược với các tính năng ứng dụng PG hiện có
- **IV2:** Đồng bộ thời gian thực duy trì tiêu chuẩn hiệu suất của quy trình PG hiện tại
- **IV3:** Xử lý ngoại lệ không làm gián đoạn các quy trình phân phối quà tặng hiện có

## Story 1.5: Real-time Gift Dashboard Implementation

Với tư cách là **thành viên Đội BU**,  
Tôi muốn **bảng điều khiển trực tiếp thay thế báo cáo Excel với theo dõi quà tặng thời gian thực**,  
để **tôi có thể giám sát hiệu suất chiến dịch và đưa ra quyết định dựa trên dữ liệu trong các chiến dịch đang hoạt động**.

### Acceptance Criteria
1. Bảng điều khiển hiển thị mức tồn kho thời gian thực theo chiến dịch và cửa hàng
2. Báo cáo ngoại lệ với khả năng phân tích nguyên nhân gốc
3. Chức năng xuất dữ liệu cho các yêu cầu báo cáo quy định
4. Phân tích hiệu suất so với dữ liệu chiến dịch lịch sử
5. Thiết kế phản hồi di động cho truy cập thực địa

### Integration Verification
- **IV1:** Bảng điều khiển tích hợp với điều hướng giao diện quản trị UHub hiện có
- **IV2:** Tạo báo cáo duy trì tiêu chuẩn hiệu suất UHub hiện có
- **IV3:** Độ chính xác dữ liệu được xác minh so với các phương pháp báo cáo Excel hiện tại

## Story 1.6: Systematic Gift Recall Workflow

Với tư cách là **thành viên Đội BU**,  
Tôi muốn **quy trình thu hồi quà tặng số hóa với quy trình xác nhận**,  
để **tôi có thể thu hồi có hệ thống các quà tặng chưa sử dụng sau chiến dịch và giảm lãng phí**.

### Acceptance Criteria
1. Quy trình khởi tạo thu hồi từ bảng điều khiển chiến dịch
2. Yêu cầu xác nhận số hóa từ cấp độ Agency và Cửa hàng
3. Theo dõi tiến trình thu hồi với cập nhật trạng thái
4. Tích hợp với các kênh giao tiếp Key Account hiện có
5. Đường kiểm toán cho tất cả các hoạt động thu hồi

### Integration Verification
- **IV1:** Quy trình thu hồi không làm gián đoạn các chiến dịch đang diễn ra
- **IV2:** Tích hợp giao tiếp duy trì mối quan hệ stakeholder hiện có
- **IV3:** Khả năng kiểm toán đáp ứng các yêu cầu tuân thủ UHub hiện có

## Story 1.7: Automatic Reconciliation Engine Implementation

Với tư cách là **thành viên Đội BU**,  
Tôi muốn **đối soát tự động so sánh hồ sơ UGMS với theo dõi UHub và tồn kho thực tế**,  
để **thời gian đối soát sau chiến dịch giảm từ nhiều ngày xuống vài giờ với độ chính xác cao hơn**.

### Acceptance Criteria
1. Công cụ so sánh tự động xác định sự khác biệt qua ba nguồn dữ liệu
2. Báo cáo ngoại lệ với các loại khác biệt được phân loại
3. Quy trình giải quyết các ngoại lệ đã xác định với quy trình phê duyệt
4. Theo dõi lịch sử của các cải tiến độ chính xác đối soát
5. Tích hợp với các yêu cầu báo cáo tài chính hiện có

### Integration Verification
- **IV1:** Quy trình đối soát duy trì tính toàn vẹn dữ liệu tài chính hiện có
- **IV2:** Quy trình ngoại lệ tích hợp với các cơ chế phê duyệt UHub hiện có
- **IV3:** Tối ưu hóa hiệu suất đảm bảo đối soát hoàn thành trong giờ làm việc