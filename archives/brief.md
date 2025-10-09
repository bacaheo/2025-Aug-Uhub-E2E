# Project Brief: E2E Physical Gift Management System

## Executive Summary

**E2E Physical Gift Management System** là dự án tối ưu hóa toàn bộ quy trình quản lý quà tặng vật lý từ lập kế hoạch đến thu hồi cho các chương trình khuyến mãi (CTKM) của Unilever tại Việt Nam. Hệ thống tích hợp UHub và UGMS để tự động hóa theo dõi quà tặng, giải quyết các pain points trong quy trình thủ công hiện tại, và minh bạch hoàn toàn từ khâu kho vận đến tay người tiêu dùng cuối.

**Vấn đề cốt lõi:** Quy trình quản lý quà vật lý hiện tại còn dựa vào Excel và xử lý thủ công, dẫn đến thiếu kiểm soát tồn kho (kho Agency, kho cửa hàng), không thể thu hồi quà sau campaign, và đối soát thủ công tốn thời gian.

**Giải pháp:** Hệ thống E2E Physical Gift Management tích hợp UGMS-UHub với digital confirmation, PowerBI Report tracking, automatic reconciliation và approval workflow.

**Giá trị cốt lõi:** Tự động hóa 80% quy trình, giảm 90% thời gian đối soát, và tạo ra end-to-end visibility cho toàn bộ chuỗi cung ứng quà tặng.

## Problem Statement

### Tình Trạng Hiện Tại và Pain Points

Quy trình quản lý quà tặng vật lý cho các CTKM của Unilever tại Việt Nam hiện đang gặp phải 4 pain points chính:

**1. No Gift Recall Alignment**

- Không có thỏa thuận rõ ràng với Key Account về việc thu hồi quà sau campaign
- Không thể thu hồi quà thừa một cách có hệ thống
- Dẫn đến lãng phí quà tặng và khó khăn trong quản lý budget

**2. No System Control Gift-in/Gift-out at Agency Warehouses**

- Agency confirm nhận hàng thủ công, không có digital tracking
- Thiếu kiểm soát hệ thống về luồng Gift-in từ DC và Gift-out đến stores
- Không có real-time visibility về tồn kho tại kho Agency

**3. No Liquidation for Gift-in/Gift-out at Store Level**

- Không có confirm nhận hàng tại store level
- Sales không có công cụ để track số lượng quà thực tế tại cửa hàng
- PG phát quà nhưng không có automatic inventory update

**4. Manual Reconciliation Post-Campaign**

- Đối soát sau campaign hoàn toàn thủ công giữa Excel và UHub
- Tốn nhiều thời gian và dễ sai sót
- Khó khăn trong việc identify discrepancies và root causes

### Tác Động Kinh Doanh

**Tác động tài chính:**

- Lãng phí quà tặng do không thu hồi được: ước tính 5-10% budget campaign
- Chi phí nhân công cho manual reconciliation: ~20 giờ/campaign
- Rủi ro sai sót trong báo cáo tài chính do reconciliation thủ công

**Tác động vận hành:**

- Thiếu minh bạch trong chuỗi cung ứng quà tặng
- Khó khăn trong inventory planning cho campaigns tiếp theo
- Giảm hiệu quả trong việc phân bổ quà tặng theo performance thực tế

**Thời điểm cấp thiết:**

- Unilever đang tăng trưởng số lượng campaigns
- Key Accounts yêu cầu minh bạch cao hơn trong quản lý quà
- ITU team có kế hoạch nâng cấp UHub system trong Q4 2025/Đầu 2026

## Proposed Solution

### Tổng Quan Giải Pháp E2E Physical Gift Management

**E2E Physical Gift Management System** tạo ra một quy trình tự động hóa hoàn chỉnh gồm 7 giai đoạn:

```
B1. Gift Planning (with Recall Alignment)
→ B2. Gift Delivery to Agency (Digital Confirmation) 
→ B3. Gift Delivery to Stores (Digital Confirmation)
→ B4. Gift Usage (Real-time Tracking)
→ B5. Gift Recall (Systematic Process)
→ B6. Stock Management (Auto Reconciliation)
→ B7. Stock Adjustment (Approval Workflow)
```

### Core Components

**1. UGMS-UHub Integration**

- Tự động đồng bộ thông tin: Schemes, Gift codes, Quantities, Store allocation
- Real-time data sync thay thế Excel manual input
- Đồng bộ thông tin UGMS-Uhub with audit trail

**2. Digital Confirmation Workflow**

- Agency confirm nhận hàng trực tiếp trên UHub system
- Sales confirm nhận quà trực tiếp trên UHub system tại store level với inventory update (Phase 2)
- PG automatic inventory deduction khi scan QR trao quà

**3. PowerBI Reporting & Analytics**

- UHub Gift Report dashboard thay thế Excel reporting
- Report inventory tracking theo store và campaign
- Report hỗ trợ predictive analytics để tối ưu việc phân bổ quà cho stores

**4. Reconciliation Engine (Engine đối soát)**

- So sánh: UGMS records ↔ UHub tracking ↔ Physical inventory
- Exception reporting với root cause analysis
- Quy trình giải quyết được sắp xếp hợp lý (Streamlined resolution workflow)

**5. Hệ thống phê duyệt & Ticket request**

- Level 2 Approval Workflow cho điều chỉnh hàng tồn kho
- Integration ticket system giữa UHUB và UGMS
- Complete audit trail cho compliance

### Key Differentiators

**So Với Quy Trình Thủ Công:**

- 80% tự động hóa (Automation) thay vì 100% thủ công
- PowerBI Report theo dõi thay vì end-campaign report
- Xử lý ngoại lệ chủ động (Proactive Exception Handling) thay vì giải quyết vấn đề thụ động

**So Với Giải Pháp Đối Thủ:**

- Tích hợp sâu với shopper journey hiện tại của UHub
- Hỗ trợ các dạng Mechanic đặc thù của Unilever
- Tối ưu hóa cho các KA của thị trường Việt Nam

**Technical Innovation:**

- Tích hợp API UGMS-UHub với xác thực RSA (RSA Authentication)
- Mobile-first Design cho Agency và Store
- Reconciliation Algorithms phát hiện sự khác biệt

## Target Users

### Primary User Segment: BU Team (Brand Team & Retail Innovation)

**Demographic Profile:**

- 5-15 nhân viên Unilever tại Việt Nam
- Vai trò: Brand Managers, Retail Innovation Team
- Kinh nghiệm: 2-5 năm kinh nghiệm FMCG/bán lẻ

**Current Behaviors:**

- Sử dụng Excel để theo dõi Campaign Performance và Gift Allocation
- Manual Coordination với các đội Agency qua email/điện thoại
- Quarterly Business Reviews với khối lượng công việc đối soát nặng
- BU phải cập nhật Mechanics/Quà/số lượng/Phân bổ cho UTOP Admin bằng Log Form thủ công

**Pain Points:**

- Thiếu PowerBI report theo dõi quà tặng và hàng tồn kho
- Quy trình đối soát tốn thời gian mỗi campaign
- Khó lập plan phân bổ quà cho các campaign chồng chéo (Overlapping Campaigns)
- Nhập liệu và tính toán thủ công dễ sai sót (Manual Error-prone Data Entry)

**Goals & Success Metrics:**

- Giảm 70% spend-time quản lý Campaign
- Tăng độ chính xác trong dự báo và lập kế hoạch hàng tồn kho (Inventory Forecasting)
- PowerBI report theo dõi Campaign Performance
- Streamlined Approval Workflow cho các điều chỉnh

### Secondary User Segment: Agency & Sales Operations

**Demographic Profile:**

- 20-50 nhân viên Agency và Sale
- Địa điểm: Phân bố trên khắp các cửa hàng của KA tại Việt Nam
- Mức độ thoải mái với công nghệ: Trung bình, quen thuộc với các ứng dụng di động cơ bản

**Current Behaviors:**

- Quy trình xác nhận thủ công, theo dõi bằng giấy tờ
- Giao tiếp qua Viber/điện thoại về tình trạng quà tồn kho
- Kiểm đếm quà tồn kho mà không cập nhật hệ thống
- Phụ thuộc vào PG để phối hợp phân phối quà tặng

**Pain Points:**

- Không có Digital Tools để xác nhận hàng tồn kho
- Communication Gaps giữa Agency → Sale → PG
- Trì hoãn thời gian trong báo cáo thay đổi quà tồn kho
- Thiếu PowerBI Report theo dõi phân bổ quà tặng

**Goals & Success Metrics:**

- Simple Mobile Interface xác nhận hàng tồn kho
- PowerBI report thông tin quà tặng tại cửa hàng
- Giảm giấy tờ thủ công và phối hợp qua điện thoại
- Trách nhiệm rõ ràng (Clear Accountability) trong chuỗi phân phối quà tặng

## Goals & Success Metrics

### Business Objectives

- **Hiệu Quả Vận Hành (Operational Efficiency):** Tăng 80% tỷ lệ tự động hóa (Automation Rate) trong quy trình quản lý quà tặng (từ 20% lên 80%)
- **Giảm Chi Phí (Cost Reduction):** Giảm 40% chi phí lao động thủ công cho đối soát và quản lý
- **Độ Chính Xác Hàng Tồn Kho (Inventory Accuracy):** Đạt tỷ lệ sai lệch <2% giữa hàng tồn kho vật lý và hệ thống
- **Tốc Độ Chiến Dịch (Campaign Speed):** Giảm 50% thời gian từ thiết lập UGMS đến triển khai chiến dịch (từ 14 ngày xuống 7 ngày)
- **Giảm Lãng Phí Quà Tặng (Gift Wastage Reduction):** Giảm 60% lãng phí quà tặng thông qua quy trình thu hồi có hệ thống

### User Success Metrics

- **Năng Suất Đội Ngũ BU (BU Team Productivity):** Giảm spend-time cho campaign từ 40 giờ xuống 12 giờ mỗi chiến dịch
- **PowerBI Report:** 95% các giao dịch hàng tồn kho được theo dõi
- **Chấp Nhận Người Dùng (User Adoption):** 90% tỷ lệ chấp nhận xác nhận số hóa từ Agency Teams và Sale Teams
- **Giảm Sai Sót (Error Reduction):** Giảm 85% lỗi đối soát

### Key Performance Indicators (KPIs)

- **Độ Tin Cậy Hệ Thống (System Reliability):** >99.5% thời gian hoạt động hệ thống UHub trong các giai đoạn campaign
- **Độ Chính Xác Tích Hợp (Integration Accuracy):** >99% tỷ lệ đồng bộ dữ liệu UGMS-UHub thành công
- **Hoàn Thành Quy Trình (Process Completion):** <1% chu kỳ phân phối quà tặng không hoàn chỉnh
- **Sự Hài Lòng Người Dùng (User Satisfaction):** >4.2/5.0 điểm hài lòng từ khảo sát hàng quý
- **Tuân Thủ (Compliance):** 100% khả năng có sẵn dấu vết kiểm toán cho tất cả giao dịch quà tặng

## MVP Scope

### Core Features (Must Have)

- **API Tích Hợp UGMS-UHub:** Đồng bộ tự động Schemes (Mechanics của campaign), Mã quà tặng (Gift Codes), Số lượng và Phân bổ cửa hàng
  - **Lý Do (Rationale):** Yêu cầu nền tảng để loại bỏ nhập liệu Excel thủ công và đảm bảo tính nhất quán dữ liệu
- **Agency Digital Confirmation:** Giao diện di động cho Đại lý xác nhận nhận quà với khả năng tải ảnh lên
  - **Lý Do:** Giải quyết vấn đề khó khăn #2 - Kiểm soát hệ thống Quà-vào/Quà-ra tại cấp Agency
- **Sales Store Confirmation:** Biểu mẫu di động đơn giản cho Bán hàng xác nhận hàng tồn kho quà tặng tại cấp cửa hàng
  - **Lý Do:** Giải quyết vấn đề khó khăn #3 - Kích hoạt theo dõi thanh lý tại cấp cửa hàng
- **Basic UHub Gift Reporting:** Bảng điều khiển thời gian thực hiển thị việc sử dụng quà tặng, hàng tồn kho còn lại theo chiến dịch và cửa hàng
  - **Lý Do:** Thay thế báo cáo Excel bằng khả năng theo dõi tự động, thời gian thực
- **PG Integration Enhancement:** Trừ hàng tồn kho tự động khi PG quét QR để trao quà
  - **Lý Do:** Đóng vòng lặp cho theo dõi sử dụng quà tặng chính xác

### Out of Scope for MVP

- Phân tích nâng cao (Advanced Analytics) và mô hình dự đoán (Predictive Modeling) cho phân bổ quà tặng
- Hỗ trợ đa ngôn ngữ (Multi-language Support) - chỉ tiếng Việt trong MVP
- Phát triển ứng dụng di động (Mobile App Development) - chỉ giao diện di động dựa trên webapp
- Tích hợp với Nhà Cung Cấp Quà Tặng bên ngoài (External Gift Vendors) ngoài GotIt/Urbox hiện có
- Quy trình phê duyệt toàn diện (Comprehensive Approval Workflow) - chỉ phê duyệt Cấp 1 cơ bản

### MVP Success Criteria

**Functional Success:**

- Xử lý thành công một chu kỳ quản lý quà tặng hoàn chỉnh cho 1 chiến dịch thí điểm
- Không mất dữ liệu (Zero Data Loss) trong tích hợp UGMS-UHub
- 100% phạm vi xác nhận số hóa cho các địa điểm Agency và Sale thí điểm

**Technical Success:**

- Thời gian phản hồi API <2 giây cho tất cả cuộc gọi tích hợp
- Tương thích giao diện di động trên 95% thiết bị được sử dụng bởi các đội thực địa
- Không có lỗi nghiêm trọng (Zero Critical Bugs) trong triển khai MVP sản xuất

## Post-MVP Vision

### Phase 2 Features (3-6 months post-MVP)

**Advanced Analytics & Optimization:**

- Đề xuất phân bổ quà tặng dựa trên học máy (Machine Learning-based) từ historical data
- Predictive Inventory Planning với dự báo nhu cầu theo mùa
- Đối soát nâng cao với tự động hóa phân tích nguyên nhân gốc

### Long-term Vision (6-8 months)

**Tối Ưu Hóa Được Hỗ Trợ Bởi AI (AI-Powered Optimization):**

- Đề xuất phân bổ quà tặng động (Dynamic Gift Allocation) dựa trên campaign perforcemance
- Đề xuất thu hồi quà tặng tự động
- Intelligent Fraud Detection cho các bất thường trong phân phối quà tặng

**Tích Hợp Hệ Sinh Thái (Ecosystem Integration):**

- Tích hợp hành trình người mua sắm đầy đủ từ ứng dụng web UHub đến xác nhận giao quà
- Tích hợp chương trình khách hàng thân thiết (Loyalty Program) với việc kiếm và đổi quà

## Technical Considerations

### Platform Requirements

**Target Platforms:**

- **Shopper & PG:** Mobile webapp responsive design optimized cho iOS 13+ và Android 9+
- **Agency & Sales Portal:** Desktop web application compatible với Chrome 90+, Safari 14+
- **PG App Integration:** Existing PG App compatibility với iOS 12+ và Android 8+

**Performance Requirements:**

- API response time: <2 seconds cho standard operations, <5 seconds cho complex reconciliation
- Concurrent user support: 200+ users simultaneously during peak campaign periods
- Data sync latency: <30 seconds cho UGMS-UHub integration updates

### Technology Preferences

**Frontend:**

- **Angular với TypeScript** cho web interface
- **Shopper & PG:** Mobile webapp responsive design với Progressive Web App (PWA) capabilities
- **Portal cho kho:** Desktop web interface với comprehensive dashboard features

**Backend:**

- **.NET Core** cho API development với high performance
- **Azure MS SQL Server** cho transactional data với **Azure Cache** cho caching
- **RESTful API architecture** với **Azure API Management** cho gateway và documentation

**Infrastructure:**

- **Azure App Service** hoặc **Azure Serverless Functions** cho scalable hosting
- **Azure Cloud Functions** cho event-driven processing
- **Azure CDN** cho fast global content delivery

### Architecture Considerations

**Repository Structure:**

- Monorepo structure với shared components giữa UHub main app và E2E Gift Management module
- Microservices architecture cho scalability và independent deployment capability
- API gateway cho centralized authentication và rate limiting

**Service Architecture:**

- Event-driven architecture cho real-time inventory updates
- **Azure Service Bus** cho reliable UGMS-UHub data synchronization
- Webhook-based notifications cho external system integration

**Integration Requirements:**

- RSA 2048-bit digital signature cho UGMS API authentication
- OAuth 2.0 với JWT tokens cho user session management
- Audit logging với immutable transaction records
- Data encryption at rest và in transit (AES-256)

### Security/Compliance

**Data Protection:**

- GDPR-compliant personal data handling cho shopper information
- Role-based access control (RBAC) với principle of least privilege
- API rate limiting và DDoS protection
- Regular security auditing và penetration testing

## Constraints & Assumptions

### Constraints

**Budget:**

- Ngân sách phát triển được phân bổ trong ngân sách nâng cấp nền tảng UHub hiện có
- Chi phí cơ sở hạ tầng phải tận dụng thiết lập Azure hiện có để giảm thiểu chi phí vận hành bổ sung
- Chi phí tích hợp bên thứ ba (Third-party Integration) giới hạn trong các mối quan hệ nhà cung cấp hiện có (UGMS, Nhà Cung Cấp Quà Tặng)

**Timeline:**

- Triển khai MVP nhắm mục tiêu cho Q1 2026 để phù hợp với mùa chiến dịch lớn
- Phát triển không được làm gián đoạn các hoạt động UHub hiện có và các chiến dịch đang diễn ra
- Giai đoạn UAT yêu cầu tối thiểu 4 tuần với các đội Đại lý và Bán hàng thí điểm

**Resources:**

- Đội phát triển: 3 lập trình viên full-stack, 1 chuyên gia di động, 1 kỹ sư DevOps
- Sự có sẵn của đội kinh doanh: Đội BU có khả năng hạn chế trong các giai đoạn chiến dịch cao điểm (Oct-Dec, Feb-Mar)
- Agency và Sales teams availability cho UAT limited to off-peak hours

**Technical:**

- Must integrate với existing UHub codebase without major architectural changes
- UGMS system capabilities và API limitations based on current vendor contract
- Mobile compatibility requirements across diverse device ecosystem used by field teams

### Key Assumptions

- **UGMS API Readiness:** UGMS vendor provide required API endpoints
- **User Adoption:** Agency và Sales teams sẽ adopt digital confirmation process với minimal resistance
- **Network Connectivity:** Field teams có reliable internet connectivity cho real-time system updates
- **Data Quality:** Existing UGMS data quality sufficient cho automated integration without extensive cleanup
- **Change Management:** Unilever organization ready để adopt new digital processes với adequate training support

## Risks & Open Questions

### Key Risks

- **UGMS Integration Risk:** Nhà cung cấp UGMS có thể trì hoãn phát triển API hoặc cung cấp chức năng hạn chế so với yêu cầu
  - **Tác Động:** Trì hoãn toàn bộ tiến độ dự án và giảm khả năng tích hợp
- **User Adoption Risk:** Agency Teams và Sale Teams không follow-up quy trình số hóa, ưu tiên phương pháp thủ công hiện có
  - **Tác Động:** Tỷ lệ chấp nhận thấp làm giảm hiệu quả hệ thống và lợi tức đầu tư (ROI)
- **Data Migration Risk:** Dữ liệu Excel hiện có không nhất quán tạo ra thách thức trong tích hợp hệ thống và độ chính xác của historical data
  - **Tác Động:** Dữ liệu nền tảng không chính xác ảnh hưởng đến độ chính xác đối soát và niềm tin người dùng
- **Performance Risk:** Việc triển khai đồng loạt trong các campaign lớn có thể làm quá tải khả năng hệ thống
  - **Tác Động:** Hệ thống ngừng hoạt động trong thời gian kinh doanh quan trọng, làm tổn hại niềm tin shopper

### Open Questions

- **Tiến Độ API UGMS:** Khi nào nhà cung cấp UGMS có thể cam kết về đặc tả API cuối cùng và tiến độ giao hàng?
- **Change Management Approach:** Phương pháp đào tạo và giới thiệu nào hiệu quả nhất cho các đội hiện trường đa dạng trên khắp Việt Nam?
- **Historical Data Migration:** Có cần di chuyển dữ liệu chiến dịch lịch sử từ Excel, và nếu có thì phạm vi đến mức nào?
- **Integration Testing Environment:** Nhà cung cấp UGMS có cung cấp môi trường sandbox để kiểm thử tích hợp toàn diện?

### Areas Needing Further Research

- **Mobile Device Capabilities:** Kiểm tra các thiết bị được sử dụng bởi các Agency Teams và Sale Teams để đảm bảo yêu cầu tương thích
- **Network Infrastructure Assessment:** Kiểm tra thực địa độ tin cậy kết nối internet tại các store chính trên các khu vực khác nhau
- **User Journey Optimization:** Nghiên cứu người dùng chi tiết với các đội Agency Teams và Sale Teams để tối ưu hóa quy trình xác nhận
- **Security Requirements Analysis:** Xem xét các chính sách bảo mật của Unilever để đảm bảo tuân thủ yêu cầu xử lý dữ liệu

## Appendices

### A. Research Summary

**Market Research Findings:**

- Unilever Việt Nam triển khai 200-300 chiến dịch quà tặng lớn hàng năm với ngân sách trung bình 50M-500M mỗi chiến dịch
- Hệ sinh thái Key Account bao gồm 8 nhà bán lẻ lớn với hơn 500 điểm bán trên toàn quốc
- Quy trình thủ công hiện tại ước tính tốn hơn 2,000 giờ hành chính hàng năm

**Competitive Analysis:**

- P&G Việt Nam sử dụng quy trình thủ công tương tự với các vấn đề khó khăn tương đương
- Các đối thủ khu vực (Thái Lan, Malaysia) đã triển khai các giải pháp số hóa một phần với kết quả không đồng nhất
- Chưa có giải pháp E2E toàn diện đặc biệt dành cho quản lý quà tặng FMCG tại Việt Nam

**Technical Feasibility Studies:**

- Nhà cung cấp UGMS xác nhận tính khả thi phát triển API với tiến độ phát triển 2 tháng
- Kiến trúc nền tảng UHub có thể tích hợp module mới mà không cần tái cấu trúc lớn
- Kiểm tra thiết bị đội hiện trường cho thấy 85% tỷ lệ sử dụng smartphone với khả năng kỹ thuật đầy đủ

### B. Stakeholder Input

**BU Team Feedback:**

- "Cần thiết cấp bách khả năng theo dõi thời gian thực - báo cáo Excel hiện tại luôn lỗi thời khi chúng ta nhận được"
- "Đối soát thủ công mất toàn bộ đội 3-4 ngày sau mỗi chiến dịch lớn"
- "Thỏa thuận thu hồi quà phải được tích hợp vào quy trình từ ngày đầu - chúng ta mất ngân sách đáng kể do quà không sử dụng"

**Agency Team Input:**

- "Xác nhận số hóa có thể chấp nhận nếu giao diện đơn giản và không làm tăng đáng kể thời gian quy trình làm việc"
- "Khả năng tải ảnh lên cần thiết để ghi lại bất kỳ sự khác biệt hoặc hư hỏng nào"

**UTOP Admin Perspective:**

- "Quy trình thiết lập chiến dịch hiện tại phụ thuộc nhiều vào nhập dữ liệu thủ công - tự động hóa sẽ loại bỏ hầu hết các nguồn lỗi phổ biến"
- "Tích hợp với quy trình làm việc PG App hiện có rất quan trọng để duy trì tính nhất quán trải nghiệm người dùng"

### C. References

- [Tài Liệu Lịch Sử và Kiến Trúc UHub](./UHub/2025Aug-Lịch%20sử%20Uhub.md)
- [Đặc Tả Tích Hợp UGMS](./UHub/integration/Mo-Ta-Integration-UHub-vs-UGMS.md)
- [Tài Liệu Quy Trình Hiện Tại](./UHub/old-process.md)
- [Quy Trình Mới Được Đề Xuất](./UHub/new-process.md)
- [Quy Trình Vận Hành Chi Tiết](./UHub/quy-trinh.md)

## Next Steps

### Immediate Actions

1. **UGMS Vendor Engagement:** Lập lịch thảo luận kỹ thuật với đội UGMS để xác nhận thời gian phát triển API và khả năng
2. **Pilot Selection:** Xác định 2-3 địa điểm Đại lý và 5-10 cửa hàng cho chương trình thí điểm MVP
3. **Team Formation:** Tập hợp đội phát triển với sự có sẵn được xác nhận và sự phù hợp về bộ kỹ năng
4. **Requirements Finalization:** Tiến hành các phiên yêu cầu chi tiết với các bên liên quan chính để xác thực giả định

### PM Handoff

Tài liệu Tóm Tắt Dự Án (Project Brief) này cung cấp đầy đủ bối cảnh cho **E2E Physical Gift Management System**.

**Key Handoff Notes:**

- Hệ thống phải tích hợp liền mạch với hành trình người mua sắm UHub hiện có
- Phạm vi MVP tập trung vào giải quyết 4 vấn đề khó khăn cốt lõi đã xác định
- Lộ trình sau MVP có giá trị kinh doanh đáng kể và cơ hội mở rộng
- Các ràng buộc kỹ thuật yêu cầu lập kế hoạch kiến trúc cẩn thận để tránh gián đoạn các hoạt động hiện có
