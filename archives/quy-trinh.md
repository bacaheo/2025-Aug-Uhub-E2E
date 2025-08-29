# Quy Trình End-to-End Gift Management - UHub System

**Tác giả:** Kết quả Brainstorming Session với Business Analyst Mary
**Ngày tạo:** 2025-08-28
**Nguồn:** Kết hợp phân tích từ old-process.md, new-process.md, và lịch sử UHub

---

## Executive Summary

### Session Topic & Goals
- **Chủ đề**: Quy trình End-to-End Gift Management cho hệ thống UHub
- **Mục tiêu**: Khám phá tất cả khía cạnh từ planning đến delivery
- **Kỹ thuật sử dụng**: Role Playing - phân tích từ góc nhìn 4 stakeholder chính

### Key Insights Generated
- **Total ideas**: 12+ insights quan trọng từ 4 stakeholder
- **Themes chính**: Historical Analytics, Proactive Management, Real-time Monitoring, Automated Reporting
- **Pattern nhận diện**: Chuyển từ Reactive → Proactive trong Gift Management

---

## Role Playing Insights

### 🛍️ Shopper (Người Tiêu Dùng)
**Core Concern**: "Có đủ quà để phát tại store"
- **Insight chính**: Phân bổ quà và điều phối quà giữa các store trong quá trình chạy campaign
- **Impact**: Không có quà → PG không thể triển khai campaign → Shopper experience bị ảnh hưởng

### 👩‍💼 PG (Promoter Girl) 
**Solutions cho "hết quà giữa chừng"**:
- **Immediate Actions**: Hẹn shopper quay lại nhận quà sau hoặc ship sau cho shopper
- **Proactive Measures**: PG nên raise quà gần hết từ sớm để phân bổ/điều phối bổ sung
- **System Requirements**: Hệ thống cần alert daily cho BT/BU khi quà gần hết ở 1 store bất kỳ

### 📊 BT/BU (Brand Team/Business Unit)
**Smart Planning Strategy**:
- **Historical Analytics**: Plan quà theo lịch sử chạy các campaign trước đó
- **Performance Insights**: Biết store nào nhiều lượt đổi, store nào ít
- **Proactive Action**: Khi phát hiện quà gần hết → action ngay để điều phối chuyển quà từ store có số mỗi/ngày ít
- **Goal**: Hạn chế xảy ra tình trạng hết quà

### 🏢 UTOP Admin (System Administrator)
**Technical Implementation**:
- **Daily Automation**: Tạo daily email thông báo cho BT/BU số lượng quà đã dùng/còn lại theo store/campaign
- **System Integration**: Kết nối UGMS, UHub, Store systems để real-time tracking
- **Monitoring & Alerting**: Automated warning system cho proactive management

---

## Quy Trình End-to-End Gift Management (7 Giai Đoạn)

### B1. Gift Planning (Lập Kế Hoạch Quà - Enhanced với Historical Analytics)

#### 1.1 Customer Alignment với Gift Recall Alignment (KA)
- **Stakeholder**: Key Account (KA) + Unilever
- **Cải tiến**: Bổ sung Gift Recall Alignment từ đầu
- **Quy trình**:
  - Thỏa thuận về campaigns (Standard Schemes)
  - **MỚI**: Thống nhất quy trình thu hồi quà rõ ràng
  - Điều kiện và thời gian thu hồi quà thừa

#### 1.2 UGMS Setup với Historical Analytics (BU)
- **Stakeholder**: Brand Team (BT)
- **Quy trình cải tiến**:
  - Thiết lập volume quà trong UGMS
  - **MỚI**: Phân tích lịch sử campaign trước đó
  - **MỚI**: Xác định store performance (nhiều/ít lượt đổi)
  - **MỚI**: Predictive planning dựa trên historical data

#### 1.3 UGMS-UHub Integration (BU)
- **Tính năng**: Tự động đồng bộ UGMS → UHub
- **Data sync**: Scheme, Gift Code, số lượng, phân bổ
- **Benefit**: Thay thế Excel thủ công, real-time update

---

### B2. Gift Delivery to Agency WHs (Giao Quà Đến Kho Agency)

#### 2.1 Linfox Delivery (DC)
- **Stakeholder**: Distribution Center (DC)
- **Quy trình**: Vận chuyển từ kho chính → kho Agency (không đổi)

#### 2.2 UHub Store Allocation với Smart Distribution (BU)
- **Cải tiến**: UHub tự động phân bổ dựa trên historical performance
- **Logic**: Store có lượt đổi cao → allocation nhiều hơn
- **Benefit**: Giảm risk hết quà tại store hot

#### 2.3 Agency Confirm on UHub (Agency)
- **Quy trình digital**: Agency confirm trên UHub thay vì thủ công
- **Auto-update**: Inventory tracking tự động
- **Pain Point solved**: System control Gift-in tại Agency WHs

---

### B3. Gift Delivery to Stores (Giao Quà Đến Store)

#### 3.1 Deliver to Store (Agency)
- **Quy trình**: Agency phân phối quà đến Store

#### 3.2 Sales Confirm on UHub (Sales)
- **Tính năng mới**: Sales confirm nhận hàng trên UHub
- **Auto-update**: Inventory tại store level
- **Pain Point solved**: Digital liquidation Gift-in/Gift-out tại Store

---

### B4. Gift Usage với Real-time Monitoring (Sử Dụng Quà Tặng)

#### 4.1 UHub Gift Report với Daily Alerts (BU)
- **Real-time tracking**: Số lượng đã phát/còn lại theo store
- **MỚI**: Daily email alerts khi quà gần hết
- **Recipients**: BT/BU để action kịp thời
- **Format**: Store-level breakdown theo campaign

#### 4.2 UTOP Setup Campaign (BU → UTOP Admin)
- **Điều kiện**: Campaign chỉ ready khi inventory confirmed
- **Integration**: Đồng bộ inventory status với campaign activation

#### 4.3 PG Support với Early Warning (Agency)
- **PG Activities**: 
  - Hướng dẫn shopper tham gia CTKM
  - Scan QR, phát quà vật lý/sampling
- **MỚI**: PG raise alert khi quà gần hết
- **Real-time update**: PG confirm → instant inventory update

---

### B5. Gift Recall/Redistribution (Thu Hồi & Phân Bổ Lại)

#### 5.1 Proactive Redistribution (BU)
- **Trigger**: Daily alerts + historical analytics
- **Action**: Điều phối từ store ít sang store nhiều
- **Timeline**: Action ngay khi detect shortage risk
- **Method**: Transfer quà giữa stores trong cùng region

#### 5.2 Gift Recall với Alignment (Agency)
- **Improvement**: Có Gift Recall Alignment từ B1
- **Process**: Thu hồi quà thừa về Agency WHs theo plan

---

### B6. Stock Management với Automated Reconciliation (Quản Lý Kho)

#### 6.1 UHub Reconciliation (Agency)
- **Process**: Agency reconciliation trên UHub
- **Auto-compare**: Hệ thống so sánh actual vs tracked data
- **Discrepancy handling**: Báo cáo và giải quyết khác biệt

#### 6.2 BU Review & Confirm (BU)
- **Process**: BU review reconciliation từ Agency
- **Action**: Approve hoặc yêu cầu điều chỉnh
- **Finalization**: Confirm final inventory status

#### 6.3 Automated Daily Reporting (UTOP Admin)
- **Deliverable**: Daily email thông báo BT/BU
- **Content**: Số lượng quà đã dùng/còn lại theo store/campaign
- **Automation**: System-generated, no manual effort
- **Pain Point solved**: Thay thế Manual Reconciliation Excel x UHUB

---

### B7. Stock Optimization với Approval Workflow (Tối Ưu Kho)

#### 7.1 Ticket System với Level 2 Approval (BU)
- **Functionality**: Raise ticket cho stock changes
- **Types**: Stock reduction, Gift recall, Re-allocation
- **Approval**: Level 2 management approval required
- **Integration**: Auto-sync UHUB ↔ UGMS

#### 7.2 Continuous Optimization (BU + UTOP Admin)
- **Data-driven decisions**: Dựa trên daily reports + historical analytics
- **Proactive adjustments**: Prevent stock-out scenarios
- **Performance monitoring**: KPIs theo store/campaign

---

## Idea Categorization

### Immediate Opportunities (Ready to Implement)
1. **Daily Email Alerts**: Tự động thông báo BT/BU khi quà gần hết
2. **Historical Analytics**: Sử dụng data campaign cũ cho planning
3. **PG Early Warning**: PG raise alert proactive khi stock low
4. **Smart Allocation**: Phân bổ dựa trên performance của store

### Future Innovations (Requires Development)
1. **Predictive Analytics**: AI/ML cho demand forecasting
2. **Real-time Redistribution**: Auto-transfer quà giữa stores
3. **Mobile Integration**: PG app với inventory visibility
4. **Customer Communication**: Auto-notify shopper về gift availability

### Moonshots (Transformative Concepts)
1. **Dynamic Campaigns**: Tự động điều chỉnh mechanics dựa trên inventory
2. **Regional Hubs**: Smart distribution centers với AI optimization
3. **Blockchain Tracking**: Immutable gift journey từ kho đến shopper

### Insights & Learnings
1. **Proactive > Reactive**: Early warning prevents stock-out
2. **Historical Data = Gold**: Past performance predicts future needs
3. **Real-time Visibility**: Key cho decision making kịp thời
4. **Stakeholder Alignment**: Mỗi role có concerns khác nhau nhưng cùng mục tiêu

---

## Action Planning

### Top 3 Priority Ideas
1. **Daily Automated Reporting System** 
   - **Rationale**: Foundation cho proactive management
   - **Next steps**: Define report format, recipients, automation logic
   - **Timeline**: 4-6 weeks development

2. **Historical Analytics Planning**
   - **Rationale**: Optimize allocation từ đầu campaign
   - **Next steps**: Data analysis của campaigns cũ, build predictive model
   - **Timeline**: 6-8 weeks analysis + implementation

3. **Early Warning Alert System**
   - **Rationale**: Prevent stock-out scenarios
   - **Next steps**: Define thresholds, alert mechanisms, response workflows
   - **Timeline**: 2-4 weeks for basic alerting

### Resources Needed
- **Technical**: Database analyst, system integration developer
- **Business**: Historical campaign data, BU/BT stakeholder time
- **Operational**: PG training cho new alerting process

---

## Pain Points Solved

| Old Pain Point | New Solution |
|---|---|
| **No Gift Recall Alignment** | ✅ Gift Recall Alignment trong Customer Alignment |
| **No System Control at Agency WHs** | ✅ Agency Confirm on UHub với real-time tracking |
| **No Liquidation at Store Level** | ✅ Sales Confirm on UHub + inventory auto-update |
| **Manual Reconciliation** | ✅ UHub Reconciliation + Daily Automated Reporting |
| **Reactive Stock Management** | ✅ Proactive Alerts + Historical Analytics Planning |

---

## Reflection & Follow-up

### What Worked Well
- **Role Playing technique**: Hiệu quả cho multi-stakeholder perspective
- **Combining documentation**: Old/new process + brainstorming insights
- **Progressive depth**: Từ concern → solution → implementation

### Areas for Further Exploration
1. **Integration complexity**: UGMS ↔ UHub ↔ Store systems
2. **Change management**: Training stakeholders cho new processes  
3. **Performance metrics**: KPIs để measure success của new process

### Recommended Follow-up
1. **Technical deep-dive session**: Architecture cho integration
2. **Stakeholder workshops**: Validate insights với actual users
3. **Pilot program**: Test new process với subset of stores/campaigns

### Questions for Future Sessions
1. Làm sao measure effectiveness của proactive vs reactive approach?
2. Integration timeline realistic với existing systems là gì?
3. Change management strategy để adopt new processes?

---

## Stakeholder Responsibilities Matrix

| Giai Đoạn | KA | BU/BT | DC | Agency | Sales | UTOP Admin | PG |
|---|---|---|---|---|---|---|---|
| **B1. Planning** | Alignment | Analytics + UGMS | - | - | - | Integration | - |
| **B2. Agency Delivery** | - | Allocation | Linfox | Confirm UHub | - | System Support | - |
| **B3. Store Delivery** | - | - | - | Deliver | Confirm UHub | - | - |
| **B4. Usage** | - | Review Reports | - | - | - | Daily Emails | Alert + Support |
| **B5. Recall/Redistribution** | - | Action Plans | - | Execute | - | Coordination | Feedback |
| **B6. Reconciliation** | - | Review & Approve | - | UHub Input | - | Automated Reports | - |
| **B7. Optimization** | - | Tickets & Approval | - | - | - | System Updates | Performance Data |

---

*Document generated from Role Playing brainstorming session focusing on End-to-End Gift Management for UHub System. All insights derived from stakeholder perspective analysis combined with existing process documentation.*