# Phân Tích Vòng Đời Gift Code Theo Scheme

## Tổng Quan

Gift Code (quà vật lý) có vòng đời phức tạp từ lúc được tạo trong UGMS cho đến khi hết hiệu lực hoặc bị loại bỏ. Vòng đời này được quản lý chặt chẽ qua các state transitions với approval workflow đa cấp.

---

## 1. State Diagram - Gift Code Lifecycle

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         GIFT CODE LIFECYCLE                                  │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   CREATED    │ (UGMS)
    │  in Scheme A │
    └──────┬───────┘
           │
           │ [Sync API 1,2]
           ▼
    ┌──────────────┐
    │  REGISTERED  │ (UHub Master Data)
    │  in Scheme A │
    └──────┬───────┘
           │
           │ [B1. Gift Planning]
           ▼
    ┌──────────────┐
    │   PLANNED    │ (Allocation Created)
    │  Campaign 1  │
    └──────┬───────┘
           │
           │ [B2. Agency Delivery]
           ▼
    ┌──────────────┐
    │ AT_AGENCY_WH │ (Agency Warehouse)
    │  Scheme A    │
    └──────┬───────┘
           │
           │ [B3. Store Delivery]
           ▼
    ┌──────────────┐
    │  AT_STORE    │ (Store Inventory)
    │  Campaign 1  │
    └──────┬───────┘
           │
           ├──────────────────────────────┬──────────────────────────┐
           │                              │                          │
           │ [B4. Gift Usage]             │ [B4. Not Used]          │ [B4. Damaged at Store]
           ▼                              ▼                          ▼
    ┌──────────────┐              ┌──────────────┐          ┌──────────────┐
    │   DELIVERED  │              │   UNUSED     │          │   DAMAGED    │
    │  to Shopper  │              │  (Surplus)   │          │  at Store    │
    └──────┬───────┘              └──────┬───────┘          └──────┬───────┘
           │                              │                          │
           │ [B5. Gift Recall]            │ [B5. Recall to Agency]  │ [B5. Recall to Agency]
           │ (Not applicable)             ▼                          ▼
           │                      ┌──────────────┐          ┌──────────────┐
           │                      │  RECALLED    │          │  RECALLED    │
           │                      │  to Agency   │          │ (Damaged)    │
           │                      └──────┬───────┘          └──────┬───────┘
           │                              │                          │
           │                              │ [B6. Reconciliation]     │ [B6. Reconciliation]
           │                              ▼                          ▼
           │                      ┌──────────────┐          ┌──────────────┐
           │                      │ RECONCILED   │          │ DISCREPANCY  │
           │                      │   (Match)    │          │  (Damaged)   │
           │                      └──────┬───────┘          └──────┬───────┘
           │                              │                          │
           │                              │                          │ [B7. BU Approval]
           │                              │                          ▼
           │                              │                  ┌──────────────┐
           │                              │                  │  WRITE_OFF   │
           │                              │                  │  (Approved)  │
           │                              │                  └──────┬───────┘
           │                              │                          │
           │                              │                          │
           │                              │                          ▼
           │                              │                  ┌──────────────┐
           │                              │                  │  DISPOSED    │
           │                              │                  │  (End Life)  │
           │                              │                  └──────────────┘
           │                              │
           │                              │
           ▼                              ▼
    ┌──────────────┐              ┌──────────────┐
    │   CONSUMED   │              │  AVAILABLE   │
    │  (End Life)  │              │  FOR_REUSE   │
    └──────────────┘              └──────┬───────┘
                                          │
                        ┌─────────────────┼─────────────────┐
                        │                 │                 │
                        │                 │                 │
         [Reuse Same    │    [Transfer to │     [Damaged    │
          Scheme]       │     New Scheme] │      in WHs]    │
                        │                 │                 │
                        ▼                 ▼                 ▼
                 ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
                 │   PLANNED    │  │ TRANSFERRED  │  │   DAMAGED    │
                 │  Campaign 2  │  │  to Scheme B │  │  at Agency   │
                 │  (Same A)    │  │              │  │              │
                 └──────┬───────┘  └──────┬───────┘  └──────┬───────┘
                        │                 │                 │
                        │                 │                 │ [BU Level-2]
                        │                 │                 │  Approval
                        │                 │                 ▼
                        │                 │          ┌──────────────┐
                        │                 │          │  WRITE_OFF   │
                        │                 │          │  (Approved)  │
                        │                 │          └──────┬───────┘
                        │                 │                 │
                        │                 │                 ▼
                        │                 │          ┌──────────────┐
                        │                 │          │  DISPOSED    │
                        │                 │          │  (End Life)  │
                        │                 │          └──────────────┘
                        │                 │
                        │                 │ [New Lifecycle]
                        │                 │  in Scheme B
                        │                 ▼
                        │          ┌──────────────┐
                        │          │  PLANNED     │
                        │          │  Campaign X  │
                        │          │  (Scheme B)  │
                        │          └──────┬───────┘
                        │                 │
                        │                 │
                        └─────────────────┴────────> [Loop to AT_AGENCY_WH]
                                                      (New Campaign Cycle)
```

---

## 2. State Definitions (Định Nghĩa Trạng Thái)

### 2.1. Master Data States (Trạng Thái Dữ Liệu Gốc)

| State | Description | System | Duration |
|-------|-------------|--------|----------|
| **CREATED** | Gift Code vừa được tạo trong UGMS | UGMS | Permanent |
| **REGISTERED** | Đã sync sang UHub, sẵn sàng sử dụng | UHub Master | Permanent |

### 2.2. Planning States (Trạng Thái Lập Kế Hoạch)

| State | Description | System | Duration |
|-------|-------------|--------|----------|
| **PLANNED** | Đã được phân bổ cho 1 Campaign cụ thể | UHub Campaign | Until Campaign End |

### 2.3. Inventory States (Trạng Thái Tồn Kho)

| State | Description | Location | Owner |
|-------|-------------|----------|-------|
| **AT_AGENCY_WH** | Đang ở kho Agency | Agency Warehouse | Agency |
| **AT_STORE** | Đang ở Store/Cửa hàng | Store | Sales/PG |

### 2.4. Usage States (Trạng Thái Sử Dụng)

| State | Description | Location | Reversible? |
|-------|-------------|----------|-------------|
| **DELIVERED** | Đã phát cho Shopper | Shopper | ❌ No - End Life |
| **UNUSED** | Chưa phát, còn nguyên vẹn | Store/Agency | ✅ Yes - Reusable |
| **DAMAGED** | Bị hư hỏng | Store/Agency | ❌ No - Need Write-off |

### 2.5. Reconciliation States (Trạng Thái Đối Soát)

| State | Description | Trigger | Next Action |
|-------|-------------|---------|-------------|
| **RECALLED** | Đã thu hồi về Agency | B5. Gift Recall | B6. Reconciliation |
| **RECONCILED** | Đối soát khớp, không có vấn đề | B6. Match | Available for Reuse |
| **DISCREPANCY** | Đối soát không khớp (thiếu/hư) | B6. No Match | B7. Multi-level Approval |

### 2.6. Reuse States (Trạng Thái Tái Sử Dụng)

| State | Description | Scheme | Lifecycle |
|-------|-------------|--------|-----------|
| **AVAILABLE_FOR_REUSE** | Sẵn sàng tái sử dụng | Same Scheme | Continue |
| **TRANSFERRED** | Chuyển sang Scheme khác | New Scheme | New Lifecycle |

### 2.7. Terminal States (Trạng Thái Kết Thúc)

| State | Description | Reason | Reversible? |
|-------|-------------|--------|-------------|
| **CONSUMED** | Đã phát cho Shopper | Normal Usage | ❌ No |
| **WRITE_OFF** | Xóa khỏi inventory | Damaged/Lost | ❌ No |
| **DISPOSED** | Hủy bỏ hoàn toàn | End of Life | ❌ No |

---

## 3. State Transitions (Chuyển Đổi Trạng Thái)

### 3.1. Normal Flow (Luồng Bình Thường - Happy Path)

```
CREATED (UGMS)
    ↓ [API Sync]
REGISTERED (UHub)
    ↓ [B1. Planning]
PLANNED (Campaign 1, Scheme A)
    ↓ [B2. Agency Delivery]
AT_AGENCY_WH
    ↓ [B3. Store Delivery]
AT_STORE
    ↓ [B4. PG Scan QR + Shopper Receive]
DELIVERED
    ↓ [Shopper Confirms]
CONSUMED (End Life)
```

**Timeline**: T-30 days → T → T+Campaign Duration

**Actors**: UGMS, UHub, BU, Agency, Sales, PG, Shopper

**Success Rate**: ~80% (theo target Gift Usage Rate)

---

### 3.2. Reuse Flow - Same Scheme (Luồng Tái Sử Dụng - Cùng Scheme)

```
CREATED (UGMS, Scheme A)
    ↓ [API Sync]
REGISTERED (UHub, Scheme A)
    ↓ [B1. Planning]
PLANNED (Campaign 1, Scheme A)
    ↓ [B2 → B3 → B4]
AT_STORE
    ↓ [B4. Not Used]
UNUSED (Surplus at Store)
    ↓ [B5. Recall to Agency]
RECALLED
    ↓ [B6. Reconciliation - Match]
RECONCILED
    ↓ [Agency Confirms OK]
AVAILABLE_FOR_REUSE (Scheme A)
    ↓ [B1. Planning for Campaign 2]
PLANNED (Campaign 2, Scheme A)
    ↓ [Loop to B2 → B3 → B4]
AT_AGENCY_WH → AT_STORE → DELIVERED/UNUSED
```

**Timeline**: Campaign 1 End + 10 days → Campaign 2 Start

**Reuse Rate Target**: >70%

**Note**: Gift Code vẫn thuộc Scheme A, chỉ đổi Campaign

---

### 3.3. Transfer Flow - New Scheme (Luồng Chuyển Đổi - Scheme Mới)

```
AVAILABLE_FOR_REUSE (Scheme A)
    ↓ [BU Decision: Transfer to Scheme B]
TRANSFERRED (Scheme B)
    ↓ [Update Scheme Mapping in UHub]
REGISTERED (Scheme B) [New Lifecycle Starts]
    ↓ [B1. Planning]
PLANNED (Campaign X, Scheme B)
    ↓ [Loop to B2 → B3 → B4]
AT_AGENCY_WH → AT_STORE → DELIVERED/UNUSED
```

**Trigger**: BU quyết định chuyển scheme (quy trình "Chuyển Scheme")

**Approval Required**: BU Level-1

**Impact**: 
- Gift Code lifecycle reset
- Scheme ID updated in UHub
- Allocation rules change
- Tracking starts from 0

**Use Case**:
- Campaign Scheme A kết thúc sớm, còn quà thừa
- Scheme B đang thiếu quà cùng loại
- BU approve chuyển để tối ưu inventory

---

### 3.4. Damage at Store Flow (Luồng Hư Hỏng Tại Store)

```
AT_STORE (Campaign 1)
    ↓ [B4. Damaged during Campaign]
DAMAGED (at Store)
    ↓ [B5. Recall to Agency]
RECALLED (Damaged)
    ↓ [B6. Reconciliation]
DISCREPANCY (Quantity mismatch due to damage)
    ↓ [Agency Submit Ticket with Evidence]
    ↓ [B7. BU Review]
    ↓ [BU Forward to BU Level-2]
    ↓ [BU Level-2 Review Evidence]
    ↓ [BU Level-2 Approve Write-off]
WRITE_OFF (Approved by BU Level-2)
    ↓ [Utop Admin Confirm]
    ↓ [Sync to UGMS]
DISPOSED (End Life)
```

**Timeline**: B6 + 5 days (cho Multi-level Approval)

**Actors**: Agency, BU, BU Level-2, Utop Admin

**Evidences Required**:
- Photos of damaged gifts
- Incident report from Store/Agency
- Sales confirmation (if applicable)
- Store manager signature

**Approval Chain**: Agency → BU → BU Level-2 → Utop Admin → UGMS

---

### 3.5. Damage at Agency WH Flow (Luồng Hư Hỏng Tại Kho Agency)

```
AVAILABLE_FOR_REUSE (at Agency WH)
    ↓ [Agency discovers damage during storage]
DAMAGED (at Agency WH)
    ↓ [Agency Submit Ticket via UHub]
    ↓ [Auto Email to BU]
    ↓ [BU Review Evidence]
    ↓ [BU Forward to BU Level-2]
    ↓ [BU Level-2 Review & Approve]
WRITE_OFF (Approved by BU Level-2)
    ↓ [Utop Admin Confirm]
    ↓ [Sync to UGMS]
DISPOSED (End Life)
```

**Difference from Store Damage**:
- Không cần B5 (Recall) vì đã ở Agency
- Không cần B6 (Reconciliation) vì chưa phát
- Trực tiếp vào Ticket workflow

**Common Causes**:
- Water damage during storage
- Expiry date passed
- Physical damage during handling
- Pest damage

---

### 3.6. Lost/Theft Flow (Luồng Mất/Bị Trộm)

```
AT_STORE / AT_AGENCY_WH
    ↓ [Discovery of loss/theft]
    ↓ [Sales/Agency Report to BU]
DISCREPANCY (Inventory shortage)
    ↓ [B6. Reconciliation detects missing quantity]
    ↓ [Agency Submit Ticket]
    ↓ [Evidence: Police report, CCTV footage, Witness statement]
    ↓ [B7. BU Review]
    ↓ [BU Forward to BU Level-2]
    ↓ [BU Level-2 Review Evidence + Police Report]
    ↓ [BU Level-2 Approve Write-off]
WRITE_OFF (Approved by BU Level-2)
    ↓ [Utop Admin Confirm]
    ↓ [Sync to UGMS]
DISPOSED (End Life)
```

**High Priority**: Cần police report để BU Level-2 approve

**Investigation Required**:
- CCTV footage review
- Witness statements
- Police report (for theft)
- Insurance claim (if applicable)

---

### 3.7. Expiry Flow (Luồng Hết Hạn)

```
AVAILABLE_FOR_REUSE (at Agency WH)
    ↓ [System Auto-check Expiry Date]
    ↓ [Alert: Expiry Warning 30 days before]
    ↓ [Alert: Expiry Warning 7 days before]
    ↓ [Expiry Date Reached]
DAMAGED (Expired)
    ↓ [Auto-generate Ticket]
    ↓ [BU Review]
    ↓ [BU Forward to BU Level-2]
    ↓ [BU Level-2 Auto-approve (if within SLA)]
WRITE_OFF (Auto-approved)
    ↓ [Utop Admin Confirm]
    ↓ [Sync to UGMS]
DISPOSED (End Life)
```

**Automation**: 
- Auto-generate ticket khi hết hạn
- Auto-alert 30 days, 7 days trước
- BU Level-2 có thể setup auto-approval rule cho expiry cases

**Prevention**:
- FIFO (First In First Out) policy
- Expiry date tracking in UHub
- Regular inventory rotation

---

## 4. State Transition Matrix (Ma Trận Chuyển Đổi)

### 4.1. Allowed Transitions (Chuyển Đổi Được Phép)

| From State | To State | Trigger | Actor | Approval Required |
|------------|----------|---------|-------|-------------------|
| CREATED | REGISTERED | API Sync | System | ❌ No |
| REGISTERED | PLANNED | B1. Planning | BU | ❌ No |
| PLANNED | AT_AGENCY_WH | B2. Delivery | Agency | ✅ Yes (Digital Confirm) |
| AT_AGENCY_WH | AT_STORE | B3. Delivery | Sales | ✅ Yes (Sales Confirm) |
| AT_STORE | DELIVERED | B4. Usage | PG + Shopper | ❌ No (Auto) |
| AT_STORE | UNUSED | B4. Not Used | N/A | ❌ No |
| AT_STORE | DAMAGED | B4. Incident | Sales/PG | ❌ No (Report) |
| UNUSED | RECALLED | B5. Recall | Agency | ❌ No |
| DAMAGED | RECALLED | B5. Recall | Agency | ❌ No |
| RECALLED | RECONCILED | B6. Match | Agency | ✅ Yes (Digital Confirm) |
| RECALLED | DISCREPANCY | B6. No Match | Agency | ❌ No (Auto-detect) |
| RECONCILED | AVAILABLE_FOR_REUSE | B6. Complete | System | ❌ No |
| DISCREPANCY | WRITE_OFF | B7. Approval | BU Level-2 | ✅ Yes (Multi-level) |
| AVAILABLE_FOR_REUSE | PLANNED | B1. Reuse | BU | ❌ No |
| AVAILABLE_FOR_REUSE | TRANSFERRED | Transfer Scheme | BU | ✅ Yes (BU Level-1) |
| AVAILABLE_FOR_REUSE | DAMAGED | Inspection | Agency | ❌ No (Report) |
| TRANSFERRED | REGISTERED | System Update | System | ❌ No |
| DELIVERED | CONSUMED | Auto | System | ❌ No |
| WRITE_OFF | DISPOSED | Utop Confirm | Utop Admin | ✅ Yes |

### 4.2. Forbidden Transitions (Chuyển Đổi Bị Cấm)

| From State | To State | Reason |
|------------|----------|--------|
| CONSUMED | Any | End state - irreversible |
| DISPOSED | Any | End state - irreversible |
| DELIVERED | RECALLED | Cannot recall after delivered to Shopper |
| DAMAGED | AVAILABLE_FOR_REUSE | Damaged gifts cannot be reused |
| WRITE_OFF | AVAILABLE_FOR_REUSE | Written-off gifts cannot be reused |
| PLANNED | CREATED | Cannot reverse to creation state |

---

## 5. Actors & Responsibilities (Vai Trò & Trách Nhiệm)

### 5.1. System Actors

| Actor | Responsibilities | States Managed |
|-------|-----------------|----------------|
| **UGMS** | Create Gift Code, Sync to UHub, Update stock | CREATED, REGISTERED |
| **UHub** | Manage lifecycle, Track state transitions, Sync back to UGMS | REGISTERED → DISPOSED |
| **ITU Log Form** | Sync campaign info to UHub Admin | PLANNED |

### 5.2. Human Actors

| Actor | Responsibilities | States Managed |
|-------|-----------------|----------------|
| **BU** | Plan campaigns, Approve tickets (Level-1), Trigger operations | PLANNED, TRANSFERRED |
| **BU Level-2** | Approve write-offs (Level-2), Review evidences | WRITE_OFF |
| **Agency** | Receive/deliver gifts, Report damages, Submit tickets, Reconcile inventory | AT_AGENCY_WH, RECALLED, RECONCILED, DISCREPANCY |
| **Sales** | Receive gifts at store, Report damages | AT_STORE, DAMAGED |
| **PG** | Scan QR, Deliver gifts to Shoppers | DELIVERED |
| **Shopper** | Receive gifts | CONSUMED |
| **Utop Admin** | Confirm tickets, Final approval, Sync to UGMS | WRITE_OFF, DISPOSED |

---

## 6. Business Rules for State Transitions

### 6.1. Inventory Rules

| Rule | Description | Enforcement |
|------|-------------|-------------|
| **No Negative Stock** | Stock cannot go below 0 at any state | System validation |
| **FIFO Policy** | First In First Out for reuse | Agency manual process |
| **Expiry Check** | Cannot use expired gifts | System validation |
| **Damage Reporting** | Must report damage within 24 hours | SLA enforcement |

### 6.2. Approval Rules

| State Transition | Approval Level | SLA | Evidence Required |
|------------------|---------------|-----|-------------------|
| AT_AGENCY_WH | BU Level-1 | 24h | Digital Confirm or Ticket |
| AT_STORE | BU Level-1 | 12h | Digital Confirm or Ticket |
| WRITE_OFF | BU Level-2 | 48h | Photos, Reports, Police report (if theft) |
| TRANSFERRED | BU Level-1 | 24h | Transfer justification |

### 6.3. Reuse Rules

| Condition | Action | Notes |
|-----------|--------|-------|
| **Gift in good condition** | Can reuse in same or different campaign | Priority: Same Scheme > Transfer Scheme |
| **Gift expired** | Must write-off | Auto-alert 30 days before |
| **Gift damaged** | Must write-off with evidence | BU Level-2 approval required |
| **Gift delivered to Shopper** | End life, cannot reuse | Final state |

---

## 7. Metrics & KPIs

### 7.1. Lifecycle Metrics

| Metric | Formula | Target | Notes |
|--------|---------|--------|-------|
| **Gift Usage Rate** | (Delivered + Damaged) / Initial Allocation | >80% | Excludes reused gifts |
| **Gift Reuse Rate** | Reused / Total Recalled | >70% | Measures efficiency |
| **Gift Write-off Rate** | Write-off / Total Allocated | <5% | Measures waste |
| **Average Lifecycle Duration** | End Date - Created Date | <180 days | Faster is better |
| **Transfer Success Rate** | Transferred & Used / Total Transferred | >90% | Measures planning accuracy |

### 7.2. Operational Metrics

| Metric | Formula | Target | Notes |
|--------|---------|--------|-------|
| **Reconciliation Match Rate** | Reconciled / Total Recalled | >90% | Measures inventory accuracy |
| **Damage Rate at Store** | Damaged at Store / AT_STORE | <3% | Measures handling quality |
| **Damage Rate at Agency** | Damaged at Agency / AT_AGENCY_WH | <2% | Measures storage quality |
| **Theft/Loss Rate** | Lost / Total Allocated | <1% | Security metric |
| **Expiry Waste Rate** | Expired / Total Available | <1% | Measures planning efficiency |

---

## 8. Edge Cases & Special Scenarios

### 8.1. Campaign Cancelled Mid-Flight

**Scenario**: Campaign bị cancel khi đang live (B4 stage)

**Impact States**: AT_STORE, DELIVERED, UNUSED

**Actions**:
1. Ngừng phát quà mới (DELIVERED)
2. Gifts đã phát (DELIVERED) → CONSUMED (không thu hồi)
3. Gifts chưa phát (UNUSED) → immediate B5 Recall
4. Skip B6 Reconciliation delay, run immediately
5. Fast-track to AVAILABLE_FOR_REUSE

**Approval**: BU Level-1 (Fast-track)

---

### 8.2. Scheme Terminated

**Scenario**: Scheme bị terminate, tất cả campaigns dừng

**Impact States**: PLANNED, AT_AGENCY_WH, AT_STORE, AVAILABLE_FOR_REUSE

**Actions**:
1. PLANNED → Cancel allocation
2. AT_AGENCY_WH/AT_STORE → immediate B5 Recall
3. AVAILABLE_FOR_REUSE → Force TRANSFERRED to new Scheme
4. Write-off if cannot transfer

**Approval**: BU Level-2 (High-impact decision)

---

### 8.3. Multi-Scheme Gift (Same Gift Code across Schemes)

**Scenario**: Gift Code GFT001 được dùng trong cả Scheme A và Scheme B

**Challenge**: Tracking lifecycle across schemes

**Solution**:
- Lifecycle tracking theo `{gift_code, scheme_id}` compound key
- State independent per scheme
- Transfer flow creates new lifecycle record
- UGMS maintains unified inventory view

---

### 8.4. Partial Delivery/Recall

**Scenario**: Chỉ 1 phần gift được deliver/recall, phần còn lại ở state khác

**Example**: 
- Initial: 100 gifts AT_STORE
- Delivered: 60 gifts → DELIVERED
- Damaged: 10 gifts → DAMAGED
- Unused: 30 gifts → UNUSED

**Tracking**:
- Quantity tracking per state
- Sum(all states) = Initial Allocation
- State transitions handle quantities, not individual items
- Reconciliation checks quantity sum

---

### 8.5. Gift Code Consolidation

**Scenario**: Merge nhiều small quantities thành 1 batch lớn để dễ quản lý

**Use Case**:
- Store A: 10 gifts unused
- Store B: 15 gifts unused
- Store C: 20 gifts unused
- Total: 45 gifts → Consolidate to 1 Agency WH

**Process**:
1. B5 Recall from all stores
2. Consolidate at Agency WH
3. B6 Reconciliation
4. New PLANNED allocation for next campaign

**Benefit**: Reduce storage complexity, easier tracking

---

## 9. Data Model for Lifecycle Tracking

### 9.1. Gift_Code_Lifecycle Table

```sql
CREATE TABLE Gift_Code_Lifecycle (
    lifecycle_id UUID PRIMARY KEY,
    gift_code VARCHAR(50) NOT NULL,
    scheme_id VARCHAR(50) NOT NULL,
    campaign_id VARCHAR(50),
    
    -- State tracking
    current_state VARCHAR(50) NOT NULL,
    previous_state VARCHAR(50),
    state_changed_at TIMESTAMP NOT NULL,
    
    -- Quantity tracking
    quantity INT NOT NULL DEFAULT 1,
    
    -- Location tracking
    current_location VARCHAR(100), -- Agency WH, Store Code, Shopper ID
    location_type VARCHAR(20), -- AGENCY, STORE, SHOPPER, DISPOSED
    
    -- Metadata
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL,
    created_by VARCHAR(50),
    updated_by VARCHAR(50),
    
    -- Audit trail
    transition_reason TEXT,
    evidence_urls TEXT[], -- Photos, documents
    approval_chain TEXT[], -- JSON array of approvers
    
    -- Scheme transfer tracking
    original_scheme_id VARCHAR(50),
    transfer_count INT DEFAULT 0,
    
    CONSTRAINT fk_gift_code FOREIGN KEY (gift_code) REFERENCES Gift_Codes(code),
    CONSTRAINT fk_scheme FOREIGN KEY (scheme_id) REFERENCES Schemes(scheme_id),
    CONSTRAINT fk_campaign FOREIGN KEY (campaign_id) REFERENCES Campaigns(campaign_id)
);

-- Index for fast state queries
CREATE INDEX idx_gift_lifecycle_state ON Gift_Code_Lifecycle(gift_code, scheme_id, current_state);
CREATE INDEX idx_gift_lifecycle_location ON Gift_Code_Lifecycle(current_location, location_type);
```

### 9.2. State_Transition_Log Table

```sql
CREATE TABLE State_Transition_Log (
    transition_id UUID PRIMARY KEY,
    lifecycle_id UUID NOT NULL,
    
    -- Transition details
    from_state VARCHAR(50) NOT NULL,
    to_state VARCHAR(50) NOT NULL,
    transition_at TIMESTAMP NOT NULL,
    
    -- Trigger
    trigger_type VARCHAR(50), -- API_SYNC, MANUAL, AUTO, SYSTEM
    trigger_by VARCHAR(50), -- Actor username
    trigger_process VARCHAR(50), -- B1, B2, B3, etc.
    
    -- Metadata
    reason TEXT,
    evidence_urls TEXT[],
    approval_id UUID, -- Link to Approval_Log if applicable
    
    CONSTRAINT fk_lifecycle FOREIGN KEY (lifecycle_id) REFERENCES Gift_Code_Lifecycle(lifecycle_id)
);

-- Index for audit trail
CREATE INDEX idx_transition_log_lifecycle ON State_Transition_Log(lifecycle_id, transition_at);
```

### 9.3. Write_Off_Log Table

```sql
CREATE TABLE Write_Off_Log (
    writeoff_id UUID PRIMARY KEY,
    lifecycle_id UUID NOT NULL,
    
    -- Write-off details
    writeoff_reason VARCHAR(100) NOT NULL, -- DAMAGED, LOST, EXPIRED, THEFT
    writeoff_at TIMESTAMP NOT NULL,
    
    -- Quantities
    quantity INT NOT NULL,
    estimated_value DECIMAL(10,2),
    
    -- Evidence
    evidence_urls TEXT[] NOT NULL,
    incident_report TEXT,
    police_report_number VARCHAR(50), -- If theft
    
    -- Approval chain
    bu_approver VARCHAR(50),
    bu_approved_at TIMESTAMP,
    bu_level2_approver VARCHAR(50),
    bu_level2_approved_at TIMESTAMP,
    utop_admin_approver VARCHAR(50),
    utop_admin_approved_at TIMESTAMP,
    
    -- Sync to UGMS
    synced_to_ugms BOOLEAN DEFAULT FALSE,
    synced_at TIMESTAMP,
    
    CONSTRAINT fk_lifecycle FOREIGN KEY (lifecycle_id) REFERENCES Gift_Code_Lifecycle(lifecycle_id)
);
```

---

## 10. Process Integration Points

### 10.1. B1. Gift Planning

**Impact on Lifecycle**:
- State: REGISTERED → PLANNED
- Creates `campaign_id` link
- Allocates quantity per store

**Data Updated**:
```json
{
  "current_state": "PLANNED",
  "campaign_id": "CAMP001",
  "allocation": [
    {"store_code": "ST001", "quantity": 50},
    {"store_code": "ST002", "quantity": 30}
  ]
}
```

---

### 10.2. B2. Agency WHs Delivery

**Impact on Lifecycle**:
- State: PLANNED → AT_AGENCY_WH
- Updates `current_location` to Agency WH code
- Records Agency digital confirm or ticket

**Ticket Scenario**:
```json
{
  "current_state": "DISCREPANCY",
  "planned_quantity": 100,
  "actual_quantity": 95,
  "discrepancy_reason": "5 units damaged during transport",
  "ticket_id": "TKT_AGY_001",
  "evidence_urls": ["photo1.jpg", "photo2.jpg"]
}
```

**After BU Approval**:
- 95 units: AT_AGENCY_WH
- 5 units: WRITE_OFF (immediate)

---

### 10.3. B3. Store Delivery

**Impact on Lifecycle**:
- State: AT_AGENCY_WH → AT_STORE
- Updates `current_location` to Store code
- Records Sales digital confirm or ticket

**Ticket Scenario** (similar to B2)

---

### 10.4. B4. Gift Usage

**Impact on Lifecycle**:
- State: AT_STORE → DELIVERED / UNUSED / DAMAGED
- Updates quantity distribution across states
- Tracks individual delivery events

**Usage Tracking**:
```json
{
  "gift_code": "GFT001",
  "scheme_id": "SCHM001",
  "campaign_id": "CAMP001",
  "store_code": "ST001",
  "initial_quantity": 50,
  "state_breakdown": {
    "DELIVERED": 35,
    "UNUSED": 12,
    "DAMAGED": 3
  }
}
```

---

### 10.5. B5. Gift Recall

**Impact on Lifecycle**:
- State: UNUSED/DAMAGED → RECALLED
- Updates `current_location` back to Agency WH
- Prepares for B6 Reconciliation

---

### 10.6. B6. Stock Reconciliation

**Impact on Lifecycle**:
- State: RECALLED → RECONCILED / DISCREPANCY
- Validates quantity match
- Triggers B7 if discrepancy

**Reconciliation Logic**:
```
Expected Recalled = Initial - Delivered
Actual Recalled = (from Agency count)

IF Actual = Expected THEN
    State = RECONCILED
    → AVAILABLE_FOR_REUSE
ELSE
    State = DISCREPANCY
    → Trigger B7 Multi-level Approval
END IF
```

---

### 10.7. B7. Multi-level Approval

**Impact on Lifecycle**:
- State: DISCREPANCY → WRITE_OFF (if approved)
- State: DISCREPANCY → RECALLED (if rejected, loop to B6)
- Records approval chain

**Approval Chain**:
```json
{
  "ticket_id": "TKT_RECON_001",
  "approval_chain": [
    {"level": 1, "approver": "BU_User1", "approved_at": "2025-01-15T10:00:00Z", "status": "APPROVED"},
    {"level": 2, "approver": "BU_Level2_User1", "approved_at": "2025-01-16T14:30:00Z", "status": "APPROVED"},
    {"level": 3, "approver": "UtopAdmin1", "approved_at": "2025-01-17T09:00:00Z", "status": "APPROVED"}
  ]
}
```

---

## 11. Reporting & Analytics

### 11.1. Lifecycle Dashboard (PowerBI)

**KPIs to Track**:
1. **Current State Distribution**
   - Pie chart: % gifts in each state
   - Breakdown by Scheme, Campaign

2. **Lifecycle Duration**
   - Histogram: Time from CREATED to CONSUMED
   - Average by Scheme

3. **Reuse Rate Trend**
   - Line chart over time
   - Target line at 70%

4. **Write-off Analysis**
   - Bar chart by reason (Damaged, Lost, Expired, Theft)
   - Trend over time

5. **Scheme Transfer Analysis**
   - Sankey diagram: Flow between schemes
   - Transfer success rate

### 11.2. Alerts & Notifications

| Alert Type | Trigger | Recipient | Action |
|------------|---------|-----------|--------|
| **Expiry Warning** | 30 days before expiry | BU, Agency | Plan usage or transfer |
| **Low Reuse Rate** | Reuse rate < 50% for 2 consecutive campaigns | BU Level-2 | Investigate root cause |
| **High Write-off Rate** | Write-off > 10% in a campaign | BU Level-2 | Review process |
| **Stuck in State** | Gift in same state > 60 days | Utop Admin | Investigate & resolve |
| **Theft Detected** | Loss rate > 2% at a location | BU, Security | Investigate |

---

## 12. Recommendations

### 12.1. Phase 1 Enhancements (Immediate)

1. **Implement State Machine in UHub**
   - Add `Gift_Code_Lifecycle` table
   - Add `State_Transition_Log` table
   - Enforce state transition rules

2. **Add Lifecycle Tracking UI**
   - Agency Portal: View gifts in AT_AGENCY_WH, AVAILABLE_FOR_REUSE
   - Sales Portal: View gifts in AT_STORE
   - BU Portal: View all states, trigger operations

3. **Auto-Alert for Expiry**
   - Background job checks expiry dates
   - Auto-email 30 days, 7 days before
   - Auto-generate write-off ticket on expiry

### 12.2. Phase 2 Enhancements (Future)

1. **Predictive Analytics**
   - ML model to predict reuse likelihood
   - Optimize scheme transfer decisions
   - Reduce write-off rate

2. **Blockchain for Audit Trail**
   - Immutable state transition log
   - Enhanced transparency for BU Level-2 approval
   - Compliance with audit requirements

3. **Mobile App for Agency**
   - Scan barcode/QR to update state
   - Real-time inventory sync
   - Offline mode support

4. **Auto-Reallocation Algorithm**
   - Auto-suggest optimal reuse campaigns
   - Auto-suggest scheme transfers
   - Minimize waste, maximize usage rate

---

## 13. Conclusion

Gift Code lifecycle trong UHub là một state machine phức tạp với:

- **12 core states** (CREATED → DISPOSED)
- **7 terminal states** (CONSUMED, DISPOSED, WRITE_OFF)
- **20+ state transitions** với approval workflows
- **6 actors** (System + 5 human roles)
- **Multiple flows**: Normal, Reuse, Transfer, Damage, Loss, Expiry

**Key Success Factors**:
1. ✅ Chặt chẽ tracking state transitions
2. ✅ Evidences bắt buộc cho write-off
3. ✅ Multi-level approval cho high-impact decisions
4. ✅ Auto-alerts cho expiry và anomalies
5. ✅ Reuse optimization để giảm waste

**Target Metrics**:
- Gift Usage Rate: >80%
- Gift Reuse Rate: >70%
- Write-off Rate: <5%
- Reconciliation Match Rate: >90%

---

**Tài liệu tham khảo**:
- E2E Gift Management Flow (B1-B7)
- API Sync UGMS ↔ UHub
- Multi-level Approval Workflow
- Ticket System Design
