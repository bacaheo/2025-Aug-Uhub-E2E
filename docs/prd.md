# E2E Physical Gift Management System Brownfield Enhancement PRD

## Intro Project Analysis and Context

### Existing Project Overview

**Analysis Source:** Tài liệu project brief từ Business Analyst + IDE-based analysis

**Current Project State:**  
UHub hiện đang là nền tảng **shopper journey** cho Unilever Việt Nam bao gồm:
- Tích hợp sẵn với PG App để phân phối quà tặng
- Quy trình quản lý quà tặng thủ công bằng Excel  
- Khả năng báo cáo cơ bản
- Key Account ecosystem với 8 nhà bán lẻ lớn, hơn 500 stores

### Available Documentation Analysis

Từ tóm tắt dự án và cấu trúc file hiện tại:
- ✅ **Tech Stack Documentation** - Từ brief: Angular, .NET Core, Azure
- ✅ **Integration Documentation** - Có sẵn thông số kỹ thuật tích hợp UGMS  
- ✅ **Business Process Documentation** - So sánh quy trình hiện tại và đề xuất
- ❌ **Technical Architecture Details** - Cần phân tích sâu hơn
- ❌ **UX/UI Guidelines** - Cho mobile webapp design

### Enhancement Scope Definition

**Enhancement Type:** 
- ✅ **Integration with New Systems** (UGMS-UHub API)
- ✅ **New Feature Addition** (E2E gift tracking)
- ✅ **Major Feature Modification** (Transform manual → automated)

**Enhancement Description:**  
Tạo hệ thống tự động hóa hoàn chỉnh cho quản lý quà tặng vật lý, tích hợp UGMS với UHub để thay thế quy trình Excel thủ công bằng xác nhận số hóa (digital confirmation), theo dõi thời gian thực (real-time tracking), và đối soát tự động (automatic reconciliation).

**Impact Assessment:**
- ✅ **Major Impact** - Yêu cầu thay đổi kiến trúc, tích hợp API mới, và nhiều quy trình người dùng mới

### Goals and Background Context

**Goals:**
- Tự động hóa 80% quy trình quản lý quà tặng (tăng từ 20% lên 80%)
- Giảm 90% thời gian đối soát thủ công 
- Tạo khả năng giám sát từ đầu đến cuối (end-to-end visibility) từ trung tâm phân phối đến người tiêu dùng
- Giảm 60% lãng phí quà tặng thông qua quy trình thu hồi có hệ thống

**Background Context:**  
Unilever Việt Nam hiện tại đang vận hành 15-25 chiến dịch quà tặng mỗi năm với ngân sách từ 500K-2M USD cho mỗi chiến dịch. Quy trình thủ công hiện tại đang tạo ra 4 điểm khó khăn chính: không có sự phối hợp trong việc thu hồi quà tặng, không có kiểm soát hệ thống, không có thanh lý tại cửa hàng, và đối soát thủ công. Hệ thống E2E này sẽ tích hợp với UHub shopper journey hiện có để tạo ra chuyển đổi số hoàn chỉnh.

### Change Log

| Change | Date | Version | Description | Author |
|--------|------|---------|-------------|---------|
| Initial PRD Creation | 2025-08-29 | 1.0 | Complete brownfield PRD for E2E Gift Management System | John (PM) |

## Requirements

### Functional

1. **FR1:** Hệ thống UHub sẽ tích hợp với UGMS API để tự động đồng bộ hóa các kế hoạch chiến dịch (campaign schemes), mã quà tặng (gift codes), số lượng và phân bổ cửa hàng, thay thế hoàn toàn việc nhập dữ liệu Excel thủ công hiện tại

2. **FR2:** Người dùng Agency sẽ có giao diện di động để xác nhận số hóa việc nhận quà tặng với khả năng tải lên hình ảnh và cập nhật tồn kho tự động trong hệ thống UHub

3. **FR3:** Đội ngũ Sales sẽ có biểu mẫu di động đơn giản để xác nhận tồn kho quà tặng tại cấp độ cửa hàng với đồng bộ thời gian thực về bảng điều khiển trung tâm UHub

4. **FR4:** PG app integration sẽ được nâng cấp để tự động trừ tồn kho khi quét mã QR trong quá trình phân phối quà tặng, duy trì độ chính xác thời gian thực

5. **FR5:** UHub Gift Report dashboard sẽ thay thế báo cáo Excel với theo dõi tồn kho trực tiếp theo chiến dịch và cửa hàng, có thể truy cập bởi các đội BU

6. **FR6:** Hệ thống sẽ hỗ trợ quy trình thu hồi quà tặng có hệ thống với quy trình xác nhận số hóa thay vì phối hợp thủ công hiện tại

7. **FR7:** Automatic reconciliation engine sẽ so sánh hồ sơ UGMS ↔ Theo dõi UHub ↔ Tồn kho thực tế với báo cáo ngoại lệ và quy trình giải quyết

### Non Functional

1. **NFR1:** API response time <2 seconds cho standard operations, <5 seconds cho complex reconciliation để maintain user experience standards hiện tại của UHub

2. **NFR2:** System phải support 200+ concurrent users during peak campaign periods mà không degradation performance

3. **NFR3:** Mobile webapp phải compatible với 95% devices used by field teams (iOS 13+, Android 9+) với offline capability cho poor connectivity areas

4. **NFR4:** Data sync latency <30 seconds cho UGMS-UHub integration updates để ensure real-time accuracy

5. **NFR5:** System uptime >99.5% during campaign periods để avoid business disruption

6. **NFR6:** GDPR-compliant data handling với role-based access control và audit logging cho tất cả gift transactions

### Compatibility Requirements

1. **CR1: Existing UHub API Compatibility** - Tất cả current UHub shopper journey APIs phải remain functional without breaking changes

2. **CR2: PG App Integration Compatibility** - Enhanced PG functionality phải backward compatible với existing PG app workflows

3. **CR3: UI/UX Consistency** - New mobile interfaces phải follow existing UHub design patterns và component libraries

4. **CR4: Azure Infrastructure Compatibility** - New components phải leverage existing Azure setup để minimize additional infrastructure costs

5. **CR5: UGMS Vendor Compatibility** - Integration phải work within existing UGMS contract constraints và API limitations

## User Interface Enhancement Goals

### Integration with Existing UI

New UI elements sẽ được integrated với existing UHub design system:
- **Mobile webapp components** sẽ follow existing UHub PWA patterns và Angular component library
- **Gift management dashboard** sẽ extend current UHub admin interface với consistent navigation và styling
- **PG app enhancements** sẽ maintain existing PG workflow UX với minimal disruption, chỉ add inventory tracking capabilities

Design consistency sẽ được maintain through shared:
- Angular Material components từ existing UHub stack
- Color palette và typography standards từ current UHub branding
- Icon library và interaction patterns familiar cho users

### Modified/New Screens and Views

**Agency Mobile Interface:**
- Gift receipt confirmation screen với photo upload
- Inventory status dashboard
- Exception reporting interface

**Sales Mobile Interface:**
- Store-level gift confirmation form
- Quick inventory check screen
- Communication interface với PG teams

**BU Team Dashboard Extensions:**
- Real-time gift tracking dashboard (replacing Excel reports)
- Campaign reconciliation interface
- Gift recall management screen

**PG App Integration:**
- Enhanced QR scan confirmation với inventory deduction
- Real-time inventory sync indicators
- Exception handling workflows

### UI Consistency Requirements

1. **Visual Consistency:** All new interfaces phải match existing UHub color scheme, typography, và spacing standards

2. **Interaction Consistency:** User flows phải follow established UHub navigation patterns để minimize learning curve

3. **Mobile Optimization:** Responsive design phải work seamlessly across device spectrum used by field teams (primarily Android devices với varying screen sizes)

4. **Accessibility Standards:** Maintain existing UHub accessibility compliance cho consistent user experience

5. **Performance Standards:** UI loading times phải match current UHub performance expectations (<2s initial load)

## Technical Constraints and Integration Requirements

### Existing Technology Stack

**Languages**: TypeScript (Frontend), C# (.NET Core for Backend)  
**Frameworks**: Angular (Web Interface), .NET Core APIs, Progressive Web App capabilities  
**Database**: Azure MS SQL Server với Azure Cache for performance optimization  
**Infrastructure**: Azure App Service, Azure CDN, Azure Service Bus, Azure API Management  
**External Dependencies**: UGMS API integration, existing PG app, Key Account retailer systems

### Integration Approach

**Database Integration Strategy**: 
- Extend existing UHub database schema với new tables cho gift tracking
- Maintain referential integrity với current campaign và user management tables
- Implement audit logging tables cho compliance requirements

**API Integration Strategy**:
- RESTful API layer với Azure API Management gateway
- RSA 2048-bit authentication cho UGMS integration 
- OAuth 2.0 JWT tokens cho user session management
- Webhook architecture cho real-time UGMS-UHub synchronization

**Frontend Integration Strategy**:
- Monorepo structure với shared components giữa main UHub app và Gift Management module
- Angular lazy loading cho new gift management features
- PWA capabilities cho offline functionality tại stores với poor connectivity

**Testing Integration Strategy**:
- Unit tests integrated với existing UHub test suite
- API integration tests với UGMS sandbox environment
- End-to-end testing covering existing UHub workflows + new gift management flows

### Code Organization and Standards

**File Structure Approach**: 
- New gift management module trong existing UHub Angular structure
- Shared services layer cho UGMS integration reusable across components
- Separate mobile-optimized components cho Agency và Sales workflows

**Naming Conventions**: 
- Follow existing UHub naming patterns: PascalCase cho classes, camelCase cho methods
- Gift-specific prefixes: `Gift`, `Campaign`, `Reconciliation` cho clear identification
- API endpoints follow `/api/v1/gifts/...` pattern consistent với existing structure

**Coding Standards**: 
- Maintain existing UHub TypeScript strict mode settings
- Follow established error handling patterns với centralized logging
- Code review requirements match existing UHub development process

**Documentation Standards**:
- API documentation through existing Swagger/OpenAPI setup
- Component documentation following existing Angular documentation patterns
- Technical decision records cho major architectural choices

### Deployment and Operations

**Build Process Integration**:
- Leverage existing Azure DevOps CI/CD pipelines
- Add gift management module tests vào existing build verification
- Database migration scripts integrated với existing deployment automation

**Deployment Strategy**:
- Blue-green deployment strategy để minimize downtime during gift campaign periods
- Feature flags cho gradual rollout của new gift management capabilities
- Rollback procedures tested với existing UHub deployment practices

**Monitoring and Logging**:
- Extend existing Azure Application Insights monitoring
- Gift-specific dashboards cho real-time campaign tracking
- Alert rules cho UGMS integration failures và performance degradation

**Configuration Management**:
- Environment-specific settings through existing Azure Key Vault integration
- UGMS API endpoints và credentials managed through secure configuration
- Feature toggles cho different deployment environments

### Risk Assessment and Mitigation

**Technical Risks**:
- UGMS API development delays có thể impact toàn bộ timeline → Mitigation: Early API contract validation và mock implementations
- Concurrent user load during peak campaigns có thể overwhelm system → Mitigation: Load testing và auto-scaling configuration

**Integration Risks**:
- Breaking changes trong existing UHub functionality → Mitigation: Comprehensive regression testing và gradual feature rollout
- Data inconsistency giữa UGMS và UHub → Mitigation: Transaction-based sync với rollback capabilities

**Deployment Risks**:
- Campaign disruption during deployment → Mitigation: Blue-green deployment với health checks
- Mobile device compatibility issues → Mitigation: Extensive device testing program

**Mitigation Strategies**:
- Comprehensive testing environment mirroring production
- Gradual user adoption plan với pilot programs
- 24/7 monitoring với immediate escalation procedures

## Epic and Story Structure

### Epic Approach

**Epic Structure Decision**: Epic tổng hợp duy nhất với lý do rõ ràng

Dựa trên phân tích dự án UHub hiện có và độ phức tạp của việc cải tiến, tôi khuyến nghị **single epic approach** vì những lý do sau:

1. **Tightly Integrated Features**: Tất cả 7 giai đoạn (B1-B7) trong quy trình quản lý quà tặng phải hoạt động cùng nhau như một hệ thống - không thể tạo ra giá trị nếu chỉ triển khai một phần quy trình

2. **Shared Infrastructure**: Tích hợp API, thay đổi cơ sở dữ liệu, và các thành phần giao diện được chia sẻ qua nhiều quy trình người dùng - việc tách thành các epic riêng biệt sẽ tạo ra các phụ thuộc không cần thiết

3. **Campaign-Driven Value**: Giá trị kinh doanh chỉ được thực hiện khi toàn bộ quy trình E2E được hoàn thành - việc triển khai một phần không giải quyết được các vấn đề cốt lõi

4. **Giảm Thiểu Rủi Ro (Risk Mitigation)**: Epic duy nhất với các câu chuyện được sắp xếp cẩn thận cho phép kiểm soát tốt hơn độ phức tạp tích hợp và các kịch bản rollback

## Epic 1: E2E Physical Gift Management System Integration

**Epic Goal**: Chuyển đổi quản lý quà tặng thủ công bằng Excel thành hệ thống E2E tự động bằng cách tích hợp UGMS với UHub, cho phép quy trình xác nhận số hóa (digital confirmation workflows), theo dõi thời gian thực (real-time tracking), và đối soát tự động (automatic reconciliation) trong khi duy trì chức năng UHub hiện có và trải nghiệm người dùng.

**Integration Requirements**: Tích hợp liền mạch với hành trình khách hàng UHub hiện có, khả năng tương thích ngược với quy trình PG App hiện tại, và không gây gián đoạn cho các chiến dịch đang diễn ra trong quá trình triển khai.

### Story 1.1: UGMS-UHub API Foundation Setup

Với tư cách là **thành viên Đội BU**,  
Tôi muốn **dữ liệu chiến dịch UGMS tự động đồng bộ với hệ thống UHub**,  
để **tôi có thể loại bỏ việc nhập dữ liệu Excel thủ công và đảm bảo tính nhất quán dữ liệu trên các nền tảng**.

#### Acceptance Criteria
1. Tích hợp UGMS API xác thực thành công sử dụng chữ ký số RSA 2048-bit
2. Các kế hoạch chiến dịch, mã quà tặng, số lượng tự động đồng bộ từ UGMS đến cơ sở dữ liệu UHub
3. Dữ liệu phân bổ cửa hàng được ánh xạ chính xác với hệ thống phân cấp cửa hàng UHub hiện tại
4. Các quy tắc xác thực dữ liệu ngăn chặn dữ liệu chiến dịch không hợp lệ vào hệ thống UHub
5. Xử lý lỗi cung cấp phản hồi rõ ràng khi cuộc gọi UGMS API thất bại

#### Integration Verification
- **IV1:** Các điểm kết nối API UHub hiện có tiếp tục hoạt động mà không bị suy giảm hiệu suất
- **IV2:** Quy trình xác thực người dùng hiện tại không bị ảnh hưởng bởi tích hợp UGMS mới
- **IV3:** Hiệu suất cơ sở dữ liệu duy trì thời gian phản hồi truy vấn hiện tại với các bảng theo dõi quà tặng mới

### Story 1.2: Agency Digital Confirmation Interface

Với tư cách là **Quản lý kho Agency**,  
Tôi muốn **giao diện di động để xác nhận số hóa việc nhận quà tặng với khả năng tải lên hình ảnh**,  
để **tôi có thể thay thế xác nhận thủ công bằng giấy và cung cấp cập nhật tồn kho thời gian thực**.

#### Acceptance Criteria
1. Giao diện phản hồi di động có thể truy cập trên thiết bị iOS 13+ và Android 9+
2. Chức năng tải lên hình ảnh cho tài liệu biên nhận với tối ưu hóa nén
3. Chụp chữ ký số để xác thực xác nhận
4. Khả năng offline với đồng bộ khi kết nối được khôi phục
5. Cập nhật tồn kho tự động trong hệ thống UHub khi xác nhận

#### Integration Verification
- **IV1:** Giao diện di động tuân theo các mẫu thiết kế PWA UHub hiện có và tiêu chuẩn hiệu suất
- **IV2:** Lưu trữ hình ảnh tích hợp với hạ tầng Azure CDN hiện có
- **IV3:** Cập nhật tồn kho duy trì tính toàn vẹn tham chiếu với dữ liệu chiến dịch hiện có

### Story 1.3: Sales Store-Level Confirmation System

Với tư cách là **Đại diện Sales**,  
Tôi muốn **biểu mẫu di động đơn giản để xác nhận tồn kho quà tặng tại cấp độ cửa hàng**,  
để **tôi có thể cung cấp dữ liệu tồn kho cửa hàng thời gian thực chính xác và phối hợp hiệu quả với đội PG**.

#### Acceptance Criteria
1. Biểu mẫu di động đơn giản được tối ưu hóa cho việc nhập dữ liệu nhanh chóng
2. Hiển thị tồn kho quà tặng cụ thể của cửa hàng với phân bổ hiện tại so với thực tế
3. Giao diện giao tiếp với đội PG để phối hợp
4. Xác minh GPS để đảm bảo độ chính xác xác nhận dựa trên vị trí
5. Tích hợp với hệ thống phân cấp cửa hàng UHub hiện có

#### Integration Verification
- **IV1:** Biểu mẫu tuân theo các mẫu thiết kế di động UHub hiện có cho trải nghiệm người dùng nhất quán
- **IV2:** Tích hợp dữ liệu cửa hàng duy trì tính toàn vẹn dữ liệu địa lý UHub hiện có
- **IV3:** Hiệu suất được tối ưu hóa cho các khu vực có kết nối mạng kém

### Story 1.4: Enhanced PG App Integration với Real-time Inventory

Với tư cách là **PG (Promotional Girl)**,  
Tôi muốn **tự động trừ tồn kho khi quét mã QR trong quá trình phân phối quà tặng**,  
để **theo dõi tồn kho luôn chính xác trong thời gian thực mà không cần cập nhật thủ công**.

#### Acceptance Criteria
1. Quét mã QR tự động kích hoạt việc trừ tồn kho trong hệ thống UHub
2. Phản hồi xác nhận thời gian thực cho PG về việc trừ tồn kho thành công
3. Xử lý ngoại lệ khi tồn kho không đủ hoặc mã QR không hợp lệ
4. Khả năng xếp hàng offline với đồng bộ khi có kết nối
5. Tích hợp với quy trình ứng dụng PG hiện có với sự gián đoạn tối thiểu

#### Integration Verification
- **IV1:** Chức năng nâng cấp tương thích ngược với các tính năng ứng dụng PG hiện có
- **IV2:** Đồng bộ thời gian thực duy trì tiêu chuẩn hiệu suất của quy trình PG hiện tại
- **IV3:** Xử lý ngoại lệ không làm gián đoạn các quy trình phân phối quà tặng hiện có

### Story 1.5: Real-time Gift Dashboard Implementation

Với tư cách là **thành viên Đội BU**,  
Tôi muốn **bảng điều khiển trực tiếp thay thế báo cáo Excel với theo dõi quà tặng thời gian thực**,  
để **tôi có thể giám sát hiệu suất chiến dịch và đưa ra quyết định dựa trên dữ liệu trong các chiến dịch đang hoạt động**.

#### Acceptance Criteria
1. Bảng điều khiển hiển thị mức tồn kho thời gian thực theo chiến dịch và cửa hàng
2. Báo cáo ngoại lệ với khả năng phân tích nguyên nhân gốc
3. Chức năng xuất dữ liệu cho các yêu cầu báo cáo quy định
4. Phân tích hiệu suất so với dữ liệu chiến dịch lịch sử
5. Thiết kế phản hồi di động cho truy cập thực địa

#### Integration Verification
- **IV1:** Bảng điều khiển tích hợp với điều hướng giao diện quản trị UHub hiện có
- **IV2:** Tạo báo cáo duy trì tiêu chuẩn hiệu suất UHub hiện có
- **IV3:** Độ chính xác dữ liệu được xác minh so với các phương pháp báo cáo Excel hiện tại

### Story 1.6: Systematic Gift Recall Workflow

Với tư cách là **thành viên Đội BU**,  
Tôi muốn **quy trình thu hồi quà tặng số hóa với quy trình xác nhận**,  
để **tôi có thể thu hồi có hệ thống các quà tặng chưa sử dụng sau chiến dịch và giảm lãng phí**.

#### Acceptance Criteria
1. Quy trình khởi tạo thu hồi từ bảng điều khiển chiến dịch
2. Yêu cầu xác nhận số hóa từ cấp độ Agency và Cửa hàng
3. Theo dõi tiến trình thu hồi với cập nhật trạng thái
4. Tích hợp với các kênh giao tiếp Key Account hiện có
5. Đường kiểm toán cho tất cả các hoạt động thu hồi

#### Integration Verification
- **IV1:** Quy trình thu hồi không làm gián đoạn các chiến dịch đang diễn ra
- **IV2:** Tích hợp giao tiếp duy trì mối quan hệ stakeholder hiện có
- **IV3:** Khả năng kiểm toán đáp ứng các yêu cầu tuân thủ UHub hiện có

### Story 1.7: Automatic Reconciliation Engine Implementation

Với tư cách là **thành viên Đội BU**,  
Tôi muốn **đối soát tự động so sánh hồ sơ UGMS với theo dõi UHub và tồn kho thực tế**,  
để **thời gian đối soát sau chiến dịch giảm từ nhiều ngày xuống vài giờ với độ chính xác cao hơn**.

#### Acceptance Criteria
1. Công cụ so sánh tự động xác định sự khác biệt qua ba nguồn dữ liệu
2. Báo cáo ngoại lệ với các loại khác biệt được phân loại
3. Quy trình giải quyết các ngoại lệ đã xác định với quy trình phê duyệt
4. Theo dõi lịch sử của các cải tiến độ chính xác đối soát
5. Tích hợp với các yêu cầu báo cáo tài chính hiện có

#### Integration Verification
- **IV1:** Quy trình đối soát duy trì tính toàn vẹn dữ liệu tài chính hiện có
- **IV2:** Quy trình ngoại lệ tích hợp với các cơ chế phê duyệt UHub hiện có
- **IV3:** Tối ưu hóa hiệu suất đảm bảo đối soát hoàn thành trong giờ làm việc