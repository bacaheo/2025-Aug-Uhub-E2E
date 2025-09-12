# SCOPE OF WORK – PHASE 1
## Gift Management System × UHub Integration

*Phiên bản v1.0 - Ngày cập nhật: 2024-09-10*

---

## Tổng Quan Dự Án

**Mục tiêu chính**: Tự động hóa và số hóa toàn bộ quy trình Gift Management E2E, giải quyết các pain points chính của quy trình thủ công hiện tại.

**Giải quyết Pain Points**:
1. **PP #1**: No Gift Recall Alignment - Không thể thu hồi quà sau campaign
2. **PP #2**: No System Control Gift-in/Gift-out at Agency WHs - Thiếu kiểm soát hệ thống tại kho Agency  
3. **PP #3**: No liquidation for Gift-in/Gift out at Stores level - Không có thanh lý tự động tại Store
4. **PP #4**: Manual Reconciliation Post-campaign - Đối soát thủ công sau campaign

---

## Chi Tiết Scope of Work

| No. | Function / Module | Pain Points Solved | Detailed Scope of Work |
|-----|-------------------|-------------------|------------------------|
| **0** | **Foundation & Core Infrastructure** | All (Enabler) | **Thiết lập nền tảng hạ tầng:**<br>• Azure App Service environment setup (Dev/Staging/Prod)<br>• CI/CD pipeline configuration với Azure DevOps + automated testing<br>• Database schema design với PostgreSQL 13 + audit trail tables<br>• Authentication framework (.NET 8 Core + OAuth 2.0 + JWT)<br>• RBAC implementation cho 6 roles: KA, BU, Agency, Sales, Utop Admin, BU Level-2<br>• Health check endpoints và system monitoring (Application Insights)<br>• GDPR/PDPA compliant consent management<br>• Global error handling và logging framework |
| **1** | **UGMS → UHub API Integration** | PP #2, #3, #4 | **Tự động hóa đồng bộ dữ liệu từ UGMS:**<br>• REST API endpoint secured bằng RSA 2048-bit + JWT authentication<br>• Data entities sync: GiftCode, Scheme, Customer, Ship_to, Quantity<br>• API client library với retry logic và comprehensive error handling<br>• Data mapping & validation layer giữa UGMS và UHub models<br>• Webhook endpoint để nhận real-time UGMS notifications<br>• Idempotency keys để tránh duplicate processing<br>• Swagger documentation + Postman collection cho testing<br>• Mock UGMS server cho UAT environment<br>• Unit tests coverage ≥ 90% |
| **2** | **ITU Log Form Processing & Campaign Intake** | PP #4 | **Tự động hóa xử lý Campaign Setup requests:**<br>• Microsoft Graph API integration để monitor ITU Log Form inbox<br>• AI/Regex parser để extract: Scheme ID, GiftCode ID, Agency ID, Allocation CSV<br>• Validation rules cho mandatory fields và business logic<br>• Raw email + parsed JSON storage cho audit trail<br>• Auto-create Campaign Draft trong UHub Admin Portal<br>• Automatic notification đến Utop Admin và BU owner<br>• Error handling và notification back to BU nếu parsing fails<br>• Template management cho các loại email formats khác nhau |
| **3A** | **Agency Warehouse Digital Confirmation System** | PP #2 | **Số hóa quy trình nhận hàng tại kho Agency:**<br>• Web UI cho Agency xem incoming Gift Vol từ UGMS sync<br>• Digital selection và confirmation của Allocation-by-Store<br>• Generate Digital GRN (Goods Received Note) với timestamps<br>• SLA enforcement: Auto-lock confirmation 168h trước go-live<br>• Integration với Enhanced Ticket System cho discrepancies<br>• Mobile-responsive design cho warehouse operations<br>• Barcode/QR code scanning capability (future-ready)<br>• Bulk import functionality cho large allocations |
| **3B** | **Store-level Digital Confirmation System (MVP)** | PP #3 | **Số hóa quy trình nhận hàng tại Store:**<br>• Mobile-first web UI cho Store/Sales staff<br>• Digital confirmation theo Allocation by store<br>• SLA enforcement: Auto-lock 48h trước Campaign Live<br>• Ticket integration cho stock discrepancies<br>• CSV bulk upload cho retail chains >100 stores<br>• Offline-capable PWA design<br>• Simple inventory adjustment workflow<br>• Integration với PG App cho campaign readiness |
| **4** | **Enhanced Ticket & SLA Management System** | PP #2, #3, #4 | **Workflow tự động với SLA tracking:**<br>• Unified Ticket entity: NEW → BU_REVIEW → BU_APPROVED/REJECTED → CLOSED<br>• Configurable SLA timers (Agency 24h, Store 12h) với auto-expiry<br>• Evidence upload capability (photos, documents)<br>• Comment threads và collaboration features<br>• Role-based views và permissions<br>• Email/SMS reminders every 4h until resolution<br>• Escalation matrix integration<br>• Dashboard analytics cho ticket performance<br>• REST API cho future mobile integration |
| **5** | **BU Level-2 Email Approval Workflow** | PP #4 | **Governance workflow cho stock adjustments:**<br>• Auto-trigger L2 approval cho stock reduction/recall/re-allocation<br>• Tokenized email templates với Approve/Reject signed URLs<br>• Single-use tokens với 72h TTL<br>• Evidence attachment và review capability<br>• Rejection workflow với mandatory reason fields<br>• Approval chain logging và audit trail<br>• Integration với audit system cho compliance<br>• Email template customization theo business requirements |
| **6** | **Recall Alignment & Auto-Reconciliation Service** | PP #1, #4 | **Tự động hóa quy trình reconciliation:**<br>• Scheduled jobs computing "Expected Leftover" = Initial In – Gift Out<br>• Agency portal để nhập Actual Recall numbers + condition codes<br>• Auto-reconciliation logic: delta = 0 → Auto-confirm & sync UGMS<br>• Auto-create Discrepancy Tickets cho delta ≠ 0<br>• L2 approval integration cho discrepancy resolution<br>• Bi-directional UGMS sync với evidence links<br>• Inventory adjustment API calls back to UGMS<br>• Comprehensive reporting và analytics |
| **7** | **System-wide Audit Trail & Evidence Repository** | PP #2, #3, #4 | **Complete compliance và traceability:**<br>• Audit tables capturing all CRUD + workflow events<br>• User activity tracking (ID, role, IP, device, timestamp)<br>• Evidence storage trên Azure Blob với versioning<br>• Immutability locks (30-day retention minimum)<br>• Correlation-ID propagation across microservices<br>• Admin UI cho audit trail search và investigation<br>• Compliance reports cho internal/external audits<br>• Data retention policy implementation |
| **8** | **Notification & Alert Management Hub** | PP #2, #3, #4 | **Centralized communication system:**<br>• Email templates cho: ITU errors, SLA warnings, Ticket updates, L2 approvals<br>• SMS/Zalo ZNS integration hooks<br>• Configurable recipients theo Scheme ID<br>• Template customization và multilingual support<br>• Delivery confirmation và bounce handling<br>• Silent retry mechanisms<br>• Notification preferences per user role<br>• Real-time push notifications cho web app |
| **9** | **PowerBI Embedded Dashboards & Reporting** | PP #4 | **Real-time insights và performance tracking:**<br>• **Gift Stock Report** (NEW): Drill-down từ Scheme → Agency → Store<br>• **Campaign Performance Report**: Redemption rates, unique shoppers, cost per gift<br>• Datasets: Gift Setup, Digital Confirmations, Ticket Status, Actual vs Planned<br>• Refresh schedule: 3 times daily (8h, 16h, 24h)<br>• Row-level security (RLS) tied to BU/Agency permissions<br>• Export capabilities (PDF, Excel, CSV)<br>• Mobile-optimized dashboard views<br>• Custom KPI alerts và thresholds |
| **10** | **Security, Compliance & Quality Assurance** | All | **Enterprise-grade security implementation:**<br>• Complete role matrix review với Unilever ITU team<br>• Static code analysis (SonarQube) + dependency CVE scanning<br>• OWASP Top-10 penetration testing + remediation<br>• Security audit và compliance certification<br>• Disaster Recovery procedures + DB PITR validation (≤ 5min data loss)<br>• Performance testing (load/stress) cho peak campaign periods<br>• UAT coordination và sign-off procedures<br>• Production deployment và go-live support |

---

## Technical Specifications & Standards

### Architecture Requirements
- **.NET 8 Core** với Clean Architecture pattern
- **PostgreSQL 13** với partitioning cho high-volume data
- **Azure App Service** với auto-scaling capabilities
- **REST APIs** với OpenAPI 3.0 specification
- **JWT-based authentication** với refresh token rotation
- **Response time SLA**: ≤ 300ms (p95), ≤ 500ms (p99)

### Integration Standards
- **UGMS API**: RSA-2048 encryption + OAuth 2.0
- **Email Processing**: Microsoft Graph API
- **File Storage**: Azure Blob Storage với CDN
- **Monitoring**: Application Insights + custom metrics
- **Language Support**: Vietnamese (primary) + English

---

## Phase 1 Boundaries & Exclusions

### **IN SCOPE - Phase 1**
✅ Digital confirmation workflows (Agency + Store)  
✅ Enhanced ticket system với SLA automation  
✅ UGMS integration cho gift data sync  
✅ Level-2 approval workflows  
✅ Basic PowerBI reporting  
✅ Email notification system  
✅ Audit trail implementation  

### **OUT OF SCOPE - Phase 1**
❌ Physical logistics execution (delivery vẫn offline)  
❌ Dynamic gift transfer between stores (B4_03/B4_04/B4_05)  
❌ Mobile native apps cho PG/Agency  
❌ Bi-directional UGMS ↔ UHub campaign creation  
❌ Advanced AI/ML analytics  
❌ Third-party integrations (ngoài UGMS)  
❌ Advanced workflow automation (sẽ có trong Phase 2)  

---

## Success Criteria & Acceptance

### **Business Success Metrics**
- **Gift Recall Alignment**: 100% campaigns có recall plan từ đầu
- **Digital Confirmation**: 95% Agency + Store sử dụng digital thay vì Excel
- **Ticket Resolution**: Average resolution time <24h Agency, <12h Store
- **Reconciliation Accuracy**: 98% auto-reconciliation success rate
- **User Adoption**: 90% user satisfaction score sau 3 tháng go-live

### **Technical Acceptance Criteria**
- **API Performance**: 99.9% uptime, <300ms response time
- **Data Accuracy**: 100% sync accuracy giữa UGMS và UHub
- **Security**: Pass penetration testing với zero high-severity issues
- **Scalability**: Support 10,000 concurrent users, 1M gifts per campaign
- **Audit Compliance**: 100% transaction traceability

---

## Project Timeline & Milestones

| Phase | Duration | Key Deliverables |
|-------|----------|------------------|
| **Setup & Planning** | 2 weeks | Environment setup, detailed requirements |
| **Core Development** | 8 weeks | Functions 0-3 implementation |
| **Integration Phase** | 4 weeks | Functions 4-6 + UGMS integration |
| **Advanced Features** | 4 weeks | Functions 7-9 implementation |
| **Testing & Security** | 3 weeks | Function 10 + UAT |
| **Go-live Support** | 1 week | Production deployment + support |
| **Total Duration** | **22 weeks** | **Complete Phase 1 delivery** |

---

*Tài liệu này được tạo dựa trên phân tích chi tiết quy trình Gift Management hiện tại và các yêu cầu đã được định nghĩa trong các tài liệu thiết kế hệ thống.*
