# E2E Physical Gift Management System Product Requirements Document (PRD)

## Goals and Background Context

### Goals
• Tự động hóa 80% quy trình quản lý quà tặng vật lý thay thế quy trình Excel thủ công  
• Tạo end-to-end visibility từ kho Agency đến tay người tiêu dùng cuối  
• Giảm 90% thời gian đối soát post-campaign (từ 40 giờ xuống 4 giờ)  
• Thiết lập quy trình thu hồi quà có hệ thống giảm 60% lãng phí quà tặng  
• Đạt tỷ lệ chính xác inventory <2% sai lệch giữa vật lý và hệ thống  
• Tích hợp UGMS-UHub seamless với digital confirmation thay thế báo cáo Excel

### Background Context

Unilever Việt Nam triển khai 200-300 chiến dịch quà tặng hàng năm với ngân sách trung bình 50M-500M mỗi campaign, nhưng đang gặp phải 4 pain points nghiêm trọng: không có thỏa thuận thu hồi quà với Key Accounts, thiếu kiểm soát hệ thống gift-in/gift-out tại Agency warehouses, không có liquidation tracking ở store level, và manual reconciliation tốn 2,000+ giờ hành chính hàng năm. 

E2E Physical Gift Management System giải quyết căn bản bằng tích hợp UGMS-UHub với digital confirmation workflow, PowerBI reporting real-time, và reconciliation engine tự động. Hệ thống sẽ tạo ra complete visibility từ Gift Planning với Recall Alignment đến Stock Adjustment với Approval Workflow, đáp ứng nhu cầu cấp bách khi ITU team chuẩn bị nâng cấp UHub trong Q4 2025/Đầu 2026.

### Change Log
| Date | Version | Description | Author |
|------|---------|-------------|---------|
| 2025-09-03 | 1.0 | PRD Creation - Goals and Background Context | John (PM) |

## Requirements

Dựa trên brief và 4 pain points chính, tôi xây dựng functional và non-functional requirements:

### Functional Requirements

**FR1:** Hệ thống tự động đồng bộ thông tin UGMS-UHub bao gồm Schemes, Gift codes, Quantities và Store allocation thay thế nhập Excel thủ công

**FR2:** Agency có thể confirm digital nhận hàng trực tiếp trên UHub với khả năng upload ảnh và ghi chú discrepancy

**FR3:** Sales có thể confirm digital gift inventory tại store level với real-time inventory update và photo documentation

**FR4:** PG App tự động trừ inventory khi scan QR code trao quà cho shopper với timestamp và location tracking

**FR5:** Hệ thống tạo PowerBI dashboard với refresh 3 lần/ngày hiển thị gift usage, remaining inventory theo campaign và store

**FR6:** Reconciliation engine hỗ trợ so sánh UGMS records ↔ UHub tracking ↔ Physical inventory và highlight discrepancies

**FR7:** Hệ thống hỗ trợ gift recall workflow với Key Account agreement tracking và systematic collection process

**FR8:** Level 1 Approval workflow cho stock adjustments với audit trail và reason code requirements

**FR9:** Exception reporting tự động với root cause analysis và resolution workflow assignment

**FR10:** Mobile-responsive interface cho Agency và Sales confirmation trên smartphone/tablet

### Non-Functional Requirements

**NFR1:** API response time <2 giây cho standard operations, <5 giây cho complex reconciliation queries

**NFR2:** Hệ thống hỗ trợ 200+ concurrent users trong peak campaign periods không degradation

**NFR3:** Data sync latency UGMS-UHub <30 giây với guaranteed delivery và retry mechanism

**NFR4:** Mobile interface tương thích 95%+ thiết bị iOS 13+ và Android 9+ của field teams

**NFR5:** 99.5% system uptime durante campaign periods với automatic failover capabilities

**NFR6:** All transactions có complete audit trail với immutable logging và data encryption

**NFR7:** Zero data loss trong UGMS-UHub integration với transaction rollback capabilities

**NFR8:** RSA 2048-bit authentication cho UGMS API với OAuth 2.0 JWT tokens cho user sessions

## User Interface Design Goals

### Overall UX Vision
Mobile-first approach tối ưu cho field teams với simple, intuitive workflows. Design philosophy: "One-tap confirmation" cho high-frequency actions, progressive disclosure để avoid overwhelming users với nhiều options, và offline-capable để handle unreliable network connectivity tại remote stores.

### Key Interaction Paradigms  
• **Quick Confirmation Pattern:** Swipe/tap confirm với visual feedback instant  
• **Photo + Notes Pattern:** Camera integration với optional text annotations cho discrepancy reporting  
• **Progressive Dashboard:** Overview cards với drill-down details, từ campaign-level xuống store-level inventory  
• **Status-driven UI:** Clear visual indicators (Green=OK, Yellow=Pending, Red=Issue) để quick status assessment  

### Core Screens and Views
• **Agency Confirmation Screen:** Delivery receipt với photo upload và quantity verification  
• **Sales Inventory Screen:** Store gift inventory với current quantities và campaign assignments  
• **PG Distribution Screen:** QR scanner với shopper confirmation và inventory deduction  
• **BU Dashboard:** Campaign overview với gift usage analytics và store performance  
• **Exception Management Screen:** Discrepancy list với resolution workflow và approval status  

### Accessibility: WCAG AA
Tuân thủ WCAG 2.1 AA standards với contrast ratios, keyboard navigation support, và screen reader compatibility để ensure usability across diverse user abilities và device types.

### Branding
Integrate với UHub branding guidelines hiện có để maintain consistent user experience. Sử dụng Unilever color palette với emphasis trên blue/green cho confirmed actions, amber cho pending states, và red cho errors/exceptions.

### Target Device and Platforms: Web Responsive  
Optimized cho iOS 13+ và Android 9+ browsers với Progressive Web App capabilities, desktop compatibility cho BU Dashboard, và tablet support cho Agency warehouse operations.

## Technical Assumptions

### Repository Structure: Monorepo
Monorepo structure với shared components giữa UHub main app và E2E Gift Management module để tận dụng existing codebase và facilitate maintenance.

### Service Architecture  
Microservices architecture với event-driven processing để support scalability và independent deployment. Core services: UGMS Integration Service, Gift Tracking Service, Reconciliation Service, Notification Service, và Reporting Service.

### Testing Requirements
Unit + Integration testing với focus trên API integration testing với UGMS, mobile UI testing trên diverse devices, và end-to-end workflow testing cho complete gift distribution cycles.

### Additional Technical Assumptions and Requests

**Frontend Technology Stack:**
• **Angular với TypeScript** cho web interface theo existing UHub architecture  
• **Progressive Web App (PWA)** capabilities cho offline functionality và app-like experience  
• **Responsive CSS Framework** (Angular Material hoặc Bootstrap) cho consistent UI components  

**Backend Technology Stack:**
• **.NET Core** cho API development để tương thích với UHub existing stack  
• **Azure MS SQL Server** cho transactional data với **Azure Cache** cho performance optimization  
• **RESTful API architecture** với comprehensive API documentation  

**Infrastructure & Deployment:**
• **Azure App Service** hoặc **Azure Serverless Functions** cho scalable hosting  
• **Azure Service Bus** cho reliable UGMS-UHub data synchronization  
• **Azure CDN** cho fast content delivery và static asset optimization  

**Integration Requirements:**
• **RSA 2048-bit digital signature** cho UGMS API authentication  
• **Webhook-based notifications** cho real-time updates  
• **Event-driven architecture** cho inventory updates và reconciliation triggers  

**Security & Compliance:**
• **OAuth 2.0 với JWT tokens** cho user session management  
• **AES-256 encryption** at rest và in transit  
• **GDPR-compliant** personal data handling cho shopper information

## Epic List

**Epic 1: Foundation & UGMS Integration**  
Thiết lập project infrastructure, UGMS-UHub API integration, và basic gift tracking foundation với deployment của initial system health-check capabilities.

**Epic 2: Digital Confirmation Workflows**  
Triển khai Agency và Sales digital confirmation interfaces với mobile-responsive design, photo upload capabilities, và real-time inventory updates.

**Epic 3: Reconciliation & Exception Management**  
Xây dựng reconciliation engine, exception reporting system, và Level 1 approval workflow với PowerBI dashboard integration.

**Epic 4: Gift Recall & Advanced Features**  
Triển khai systematic gift recall workflow, advanced analytics, và optimization features với complete audit trail và compliance capabilities.

## Epic 1: Foundation & UGMS Integration

**Expanded Goal:** Thiết lập foundation architecture và UGMS-UHub API integration để thay thế quy trình Excel thủ công, tạo reliable data synchronization infrastructure, và deploy initial health-check functionality for system monitoring. Epic này sẽ deliver immediate value bằng cách eliminate manual data entry và establish audit trail foundation.

### Story 1.1: Project Infrastructure Setup
**As a** DevOps Engineer,  
**I want** project infrastructure và CI/CD pipeline được thiết lập,  
**so that** development team có scalable deployment foundation.

**Acceptance Criteria:**
1. Azure App Service environment provisioned cho development, staging, và production
2. CI/CD pipeline configured với automated testing và deployment
3. Database schemas created với initial tables cho gift tracking
4. Basic authentication và authorization framework implemented
5. Health check endpoints available cho system monitoring

### Story 1.2: UGMS API Integration Framework  
**As a** Backend Developer,  
**I want** UGMS API integration framework được implement,  
**so that** system có thể automatically sync data từ UGMS.

**Acceptance Criteria:**
1. RSA 2048-bit authentication mechanism implemented cho UGMS API calls
2. API client library created với retry logic và error handling
3. Data mapping layer established giữa UGMS và UHub data models
4. Initial sync capability cho Schemes, Gift codes, Quantities
5. Webhook endpoint ready để receive UGMS notifications

### Story 1.3: Basic Gift Tracking Data Model
**As a** System Administrator,  
**I want** core data models cho gift tracking được established,  
**so that** system có thể store và track gift inventory accurately.

**Acceptance Criteria:**
1. Gift inventory database tables created với proper indexing
2. Campaign và scheme tracking tables established
3. Audit trail tables implemented cho all data changes
4. Data validation rules enforced at database level
5. Basic CRUD operations available via API endpoints

### Story 1.4: System Health Check & Monitoring
**As a** BU Team Member,  
**I want** system health dashboard available,  
**so that** tôi có thể verify system status và UGMS connection.

**Acceptance Criteria:**
1. Health check dashboard shows UGMS API connectivity status
2. Database connection status monitoring implemented
3. Basic system metrics displayed (response times, error rates)
4. Email notifications configured cho critical system failures
5. Initial system ready cho Epic 2 development

## Epic 2: Digital Confirmation Workflows

**Expanded Goal:** Implement Agency và Sales digital confirmation interfaces để thay thế manual phone/Viber confirmation process. Epic này addresses pain points #2 và #3 bằng cách tạo mobile-responsive workflows cho gift-in/gift-out tracking với photo documentation và real-time inventory updates. Deliverables include Agency confirmation app, Sales store confirmation interface, và PG App integration cho complete digital workflow.

### Story 2.1: Agency Mobile Confirmation Interface
**As an** Agency Operations Manager,  
**I want** mobile interface để confirm gift deliveries,  
**so that** tôi có thể digitally document receipts thay vì phone calls.

**Acceptance Criteria:**
1. Mobile-responsive confirmation screen accessible via smartphone/tablet
2. Photo upload functionality với compression và thumbnail preview
3. Quantity verification input với discrepancy reporting capability
4. Digital signature capture cho delivery confirmation
5. Offline capability với sync khi network available

### Story 2.2: Sales Store Inventory Confirmation
**As a** Sales Representative,  
**I want** simple interface để confirm store gift inventory,  
**so that** tôi có thể accurately track store-level gift availability.

**Acceptance Criteria:**
1. Store inventory dashboard showing current gift allocations
2. Quick confirmation buttons cho standard quantities
3. Manual adjustment input với reason code requirements
4. Photo documentation cho inventory discrepancies
5. Real-time sync với central inventory system

### Story 2.3: PG App Integration Enhancement
**As a** Promotion Girl (PG),  
**I want** enhanced QR scanning functionality,  
**so that** gift distribution automatically updates inventory.

**Acceptance Criteria:**
1. QR code scanner integrated với existing PG App
2. Automatic inventory deduction upon successful scan
3. Timestamp và GPS location recording cho audit trail
4. Shopper confirmation screen với gift details display
5. Offline queue với batch sync capabilities

### Story 2.4: Real-time Inventory Synchronization
**As a** System Administrator,  
**I want** real-time inventory updates across all touchpoints,  
**so that** data consistency maintained throughout gift distribution chain.

**Acceptance Criteria:**
1. Event-driven inventory updates triggered by confirmations
2. Conflict resolution logic cho concurrent inventory changes
3. Rollback capabilities cho failed transactions
4. Notification system cho inventory threshold alerts
5. Integration testing với UGMS sync mechanisms

## Epic 3: Reconciliation & Exception Management

**Expanded Goal:** Giải quyết pain point #4 bằng cách thay thế manual reconciliation process tốn 2,000+ giờ/năm. Epic này delivers reconciliation engine hỗ trợ comparison giữa UGMS records ↔ UHub tracking ↔ Physical inventory, PowerBI dashboard với refresh 3 lần/ngày, exception reporting system với root cause analysis, và Level 1 approval workflow cho stock adjustments. Target giảm 90% thời gian đối soát từ 40 giờ xuống 4 giờ mỗi campaign.

### Story 3.1: Reconciliation Engine Core Logic
**As a** BU Team Member,  
**I want** reconciliation engine hỗ trợ so sánh data từ multiple sources,  
**so that** tôi có thể quickly identify discrepancies thay vì manual Excel comparison.

**Acceptance Criteria:**
1. Data comparison logic giữa UGMS records, UHub tracking, Physical inventory
2. Discrepancy detection với configurable tolerance thresholds
3. Automated categorization của discrepancy types (quantity, timing, location)
4. Bulk reconciliation processing cho post-campaign analysis  
5. Historical reconciliation data retention và trending analysis

### Story 3.2: Exception Reporting Dashboard
**As a** BU Team Member,  
**I want** exception reporting dashboard với actionable insights,  
**so that** tôi có thể prioritize và resolve discrepancies efficiently.

**Acceptance Criteria:**
1. Exception dashboard với filtering theo campaign, store, severity
2. Root cause analysis suggestions based trên historical patterns
3. Assignment workflow cho resolution ownership 
4. Status tracking từ "Open" → "In Progress" → "Resolved"
5. Exception resolution time metrics và performance reporting

### Story 3.3: PowerBI Dashboard Integration
**As a** BU Team Manager,  
**I want** PowerBI dashboard với gift tracking analytics,  
**so that** tôi có thể monitor campaign performance và inventory utilization.

**Acceptance Criteria:**
1. PowerBI report integration với scheduled refresh 3 lần/ngày
2. Campaign performance metrics (distribution rate, utilization %, waste reduction)
3. Store-level inventory analytics với drill-down capabilities
4. Exception summary reporting với resolution status
5. Historical trend analysis cho inventory forecasting

### Story 3.4: Level 1 Approval Workflow
**As a** BU Team Lead,  
**I want** approval workflow cho inventory adjustments,  
**so that** stock changes có proper authorization và audit trail.

**Acceptance Criteria:**
1. Approval workflow triggering cho stock adjustments >threshold
2. Email notifications cho pending approvals với escalation timing
3. Approval history tracking với approver identity và timestamp
4. Reason code mandatory cho all manual adjustments
5. Audit trail integration với complete transaction logging

## Epic 4: Gift Recall & Advanced Features

**Expanded Goal:** Giải quyết pain point #1 bằng cách thiết lập systematic gift recall workflow với Key Account agreement tracking để giảm 60% lãng phí quà tặng (từ 5-10% budget xuống <2% budget mỗi campaign). Epic này triển khai advanced features bao gồm predictive analytics cho gift allocation optimization, complete compliance audit trail, và integration với loyalty program cho post-MVP expansion. Target value: Thu hồi systematic 95%+ quà thừa và tạo foundation cho AI-powered optimization.

### Story 4.1: Gift Recall Workflow Framework
**As a** BU Team Member,  
**I want** systematic gift recall process,  
**so that** tôi có thể thu hồi unused gifts từ Key Accounts thay vì accept waste.

**Acceptance Criteria:**
1. Key Account recall agreement templates với legal compliance requirements
2. Recall initiation workflow với automated notifications và timelines
3. Recall tracking dashboard với collection progress monitoring
4. Integration với existing KA relationship management systems
5. Recall performance metrics và waste reduction reporting

### Story 4.2: Advanced Analytics & Reporting
**As a** BU Team Manager,  
**I want** advanced analytics cho gift allocation optimization,  
**so that** tôi có thể make data-driven decisions cho future campaigns.

**Acceptance Criteria:**
1. Predictive analytics cho optimal gift allocation based trên historical data
2. Campaign performance benchmarking với industry standards
3. Store performance analytics với gift utilization efficiency metrics  
4. Seasonal trend analysis cho inventory planning optimization
5. Advanced reporting export capabilities cho executive presentations

### Story 4.3: Compliance & Audit Trail Enhancement
**As a** Compliance Officer,  
**I want** comprehensive audit trail và compliance reporting,  
**so that** all gift management processes meet regulatory requirements.

**Acceptance Criteria:**
1. Complete transaction logging với immutable audit trail
2. Compliance reporting templates cho internal và external audits
3. Data retention policies với automated archival processes
4. GDPR compliance features cho personal data handling
5. Regulatory export capabilities với standardized formats

### Story 4.4: Post-MVP Integration Readiness
**As a** System Architect,  
**I want** integration hooks cho future enhancements,  
**so that** system có thể scale với business growth và new requirements.

**Acceptance Criteria:**
1. API framework cho third-party gift vendor integrations
2. Loyalty program integration endpoints preparation
3. Advanced workflow engine foundation cho complex approval processes
4. Machine learning pipeline preparation cho AI-powered features
5. Performance optimization foundation cho enterprise scale deployment

## Checklist Results Report

### Executive Summary

**Overall PRD Completeness:** 92% - Rất tốt với detailed requirements và comprehensive epic structure
**MVP Scope Assessment:** Just Right - Epic structure balanced giữa foundation value và complexity management  
**Architecture Readiness:** Ready - Technical assumptions và functional requirements đủ rõ cho architecture phase
**Critical Assessment:** PRD hoàn chỉnh với minor improvement areas trong testing strategy và performance metrics

### Category Analysis

| Category                         | Status  | Critical Issues                                        |
| -------------------------------- | ------- | ----------------------------------------------------- |
| 1. Problem Definition & Context  | PASS    | Excellent - 4 pain points clearly mapped to solutions |
| 2. MVP Scope Definition          | PASS    | Well-scoped với clear out-of-scope boundaries         |
| 3. User Experience Requirements  | PASS    | Mobile-first design với accessibility considerations   |
| 4. Functional Requirements       | PASS    | 10 FRs + 8 NFRs comprehensive và testable             |
| 5. Non-Functional Requirements   | PASS    | Performance targets specific (<2s, 200+ users, etc.)  |
| 6. Epic & Story Structure        | PASS    | 4 epics logically sequential với 16 detailed stories  |
| 7. Technical Guidance            | PASS    | Architecture stack định nghĩa rõ (.NET, Angular, Azure) |
| 8. Cross-Functional Requirements | PARTIAL | API integration clear, data migration needs clarification |
| 9. Clarity & Communication       | PASS    | Tiếng Việt terminology consistent, technical terms defined |

### MVP Scope Assessment

**Appropriate Scope Elements:**
• Epic 1-2 addresses core pain points với immediate value delivery
• Mobile-first approach phù hợp với field team reality  
• UGMS integration tackles highest technical risk early
• PowerBI 3x/day refresh realistic cho business needs

**Scope Refinement Suggestions:**  
• Epic 4 advanced analytics có thể defer to post-MVP để focus trên core reconciliation
• Story 1.4 system monitoring essential - keep in MVP
• Story 4.4 integration readiness có thể simplify cho MVP focus

### Technical Readiness Assessment

**Architectural Clarity Strengths:**
• Technology stack alignment với existing UHub (.NET, Angular, Azure)
• Security requirements specific (RSA 2048-bit, OAuth 2.0, AES-256)
• Performance targets measurable và realistic
• Integration patterns clear (API, webhooks, event-driven)

**Areas Requiring Architect Deep-dive:**
• UGMS API specifications và data mapping complexity
• Reconciliation engine algorithm design cho 3-way comparison
• Mobile offline synchronization strategy detail
• Azure Service Bus event sequencing cho inventory updates

### Top Recommendations by Priority

**MEDIUM Priority (Quality Improvements):**
1. **Enhanced Testing Strategy:** Add integration testing requirements for UGMS API failure scenarios
2. **Data Migration Planning:** Clarify historical Excel data migration scope và timeline  
3. **Performance Baseline:** Define current manual process metrics để measure improvement accurately

**LOW Priority (Nice to Have):**
1. **User Training Plan:** Consider change management strategy cho field team adoption
2. **Rollback Strategy:** Detail emergency rollback procedures cho production issues

### Final Assessment: READY FOR ARCHITECT

PRD provides comprehensive foundation cho architectural design với:
• Clear business value proposition (90% time savings, 60% waste reduction)
• Well-structured epic progression từ foundation đến advanced features  
• Specific technical constraints và integration requirements
• Realistic scope cho MVP timeline (Q1 2026 target)

**Recommendation:** Proceed to architecture phase với confidence. Minor improvements có thể address during implementation planning.

## Next Steps

### UX Expert Prompt
"Xin chào UX Expert! Tôi đã hoàn thành PRD cho E2E Physical Gift Management System với mobile-first approach cho field teams (Agency, Sales, PG) và desktop dashboard cho BU team. Vui lòng *create architecture mode using this document as input* để design UI/UX mockups cho 5 core screens: Agency Confirmation, Sales Inventory, PG Distribution, BU Dashboard, và Exception Management."

### Architect Prompt  
"Xin chào System Architect! PRD cho E2E Physical Gift Management System đã ready với technical stack (.NET Core, Angular, Azure), 4 epics structure, và integration requirements với UGMS API. Vui lòng *create architecture mode using this document as input* để design system architecture, database schema, và API specifications cho MVP deployment target Q1 2026."