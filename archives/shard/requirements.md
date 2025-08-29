# Requirements

## Functional

1. **FR1:** Hệ thống UHub sẽ tích hợp với UGMS API để tự động đồng bộ hóa các kế hoạch chiến dịch (campaign schemes), mã quà tặng (gift codes), số lượng và phân bổ cửa hàng, thay thế hoàn toàn việc nhập dữ liệu Excel thủ công hiện tại

2. **FR2:** Người dùng Agency sẽ có giao diện di động để xác nhận số hóa việc nhận quà tặng với khả năng tải lên hình ảnh và cập nhật tồn kho tự động trong hệ thống UHub

3. **FR3:** Đội ngũ Sales sẽ có biểu mẫu di động đơn giản để xác nhận tồn kho quà tặng tại cấp độ cửa hàng với đồng bộ thời gian thực về bảng điều khiển trung tâm UHub

4. **FR4:** PG app integration sẽ được nâng cấp để tự động trừ tồn kho khi quét mã QR trong quá trình phân phối quà tặng, duy trì độ chính xác thời gian thực

5. **FR5:** UHub Gift Report dashboard sẽ thay thế báo cáo Excel với theo dõi tồn kho trực tiếp theo chiến dịch và cửa hàng, có thể truy cập bởi các đội BU

6. **FR6:** Hệ thống sẽ hỗ trợ quy trình thu hồi quà tặng có hệ thống với quy trình xác nhận số hóa thay vì phối hợp thủ công hiện tại

7. **FR7:** Automatic reconciliation engine sẽ so sánh hồ sơ UGMS ↔ Theo dõi UHub ↔ Tồn kho thực tế với báo cáo ngoại lệ và quy trình giải quyết

## Non Functional

1. **NFR1:** API response time <2 seconds cho standard operations, <5 seconds cho complex reconciliation để maintain user experience standards hiện tại của UHub

2. **NFR2:** System phải support 200+ concurrent users during peak campaign periods mà không degradation performance

3. **NFR3:** Mobile webapp phải compatible với 95% devices used by field teams (iOS 13+, Android 9+) với offline capability cho poor connectivity areas

4. **NFR4:** Data sync latency <30 seconds cho UGMS-UHub integration updates để ensure real-time accuracy

5. **NFR5:** System uptime >99.5% during campaign periods để avoid business disruption

6. **NFR6:** GDPR-compliant data handling với role-based access control và audit logging cho tất cả gift transactions

## Compatibility Requirements

1. **CR1: Existing UHub API Compatibility** - Tất cả current UHub shopper journey APIs phải remain functional without breaking changes

2. **CR2: PG App Integration Compatibility** - Enhanced PG functionality phải backward compatible với existing PG app workflows

3. **CR3: UI/UX Consistency** - New mobile interfaces phải follow existing UHub design patterns và component libraries

4. **CR4: Azure Infrastructure Compatibility** - New components phải leverage existing Azure setup để minimize additional infrastructure costs

5. **CR5: UGMS Vendor Compatibility** - Integration phải work within existing UGMS contract constraints và API limitations
