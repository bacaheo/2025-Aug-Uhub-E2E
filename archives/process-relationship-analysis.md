# Phân Tích Sự Liên Quan Giữa Các Quy Trình UHub

## Danh Sách Quy Trình Cần Số Hóa

| STT | Quy trình | Trạng thái | Ghi chú |
|-----|-----------|-----------|---------|
| 1 | Quy trình đồng bộ từ UGMS → UHub | ✅ Đã có API | 6 APIs sync |
| 2 | Quy trình bổ sung quà, cùng scheme | 🔲 Cần phân tích | Phase 2 |
| 3 | Quy trình trả quà về UHub → UGMS | 🔲 Cần phân tích | Phase 2 |
| 4 | Quy trình đổi Agency (remove phân bổ → chuyển agency) | 🔲 Cần phân tích | Phase 2 |
| 5 | Quy trình Log Form / Allocation / Plan Mechanic | 🔄 Partial | ITU Log Form |
| 6 | Quy trình review thực nhận Show danh sách store phân bổ | 🔲 Cần phân tích | Phase 1 |
| 7 | Quy trình phân bổ quà cho campaign | ✅ Đã có | B2, B3 Ticket System |
| 8 | Quy trình đối soát, UHub → UGMS | ✅ Đã có | B6, B7 Multi-level Approval |
| 9 | Quy trình chuyển scheme | 🔲 Cần phân tích | Phase 2 |
| 10 | Quy trình điều chỉnh kho Agency → ticket | ✅ Đã có | B2, B3, B6 Ticket System |
| 11 | Report - Rpt vòng dời quà theo scheme | 🔲 Cần phân tích | PowerBI |

---

## 1. Phân Loại Quy Trình Theo Chức Năng

### 1.1. Quy Trình Core (Backbone) - E2E Gift Management

```
┌─────────────────────────────────────────────────────────────┐
│                    E2E GIFT MANAGEMENT                       │
└─────────────────────────────────────────────────────────────┘
                            │
            ┌───────────────┴───────────────┐
            │                               │
    ┌───────▼────────┐              ┌──────▼──────┐
    │   Setup Phase   │              │  Ops Phase   │
    └───────┬────────┘              └──────┬──────┘
            │                               │
    ┌───────▼────────┐              ┌──────▼──────┐
    │ B1. Gift       │              │ B4. Gift    │
    │     Planning   │              │     Usage   │
    └───────┬────────┘              └──────┬──────┘
            │                               │
    ┌───────▼────────┐              ┌──────▼──────┐
    │ B2. Agency WHs │              │ B5. Gift    │
    │     Delivery   │              │     Recall  │
    └───────┬────────┘              └──────┬──────┘
            │                               │
    ┌───────▼────────┐              ┌──────▼──────┐
    │ B3. Store      │              │ B6. Stock   │
    │     Delivery   │              │     Recon.  │
    └────────────────┘              └──────┬──────┘
                                            │
                                    ┌───────▼────────┐
                                    │ B7. Multi-level│
                                    │     Approval   │
                                    └────────────────┘
```

**Đặc điểm:**
- Quy trình tuần tự (Sequential)
- Bắt buộc phải hoàn thành theo thứ tự
- Không thể skip bước
- Có state transition rõ ràng

---

### 1.2. Quy Trình Supporting (Hỗ Trợ)

```
┌─────────────────────────────────────────────────────────────┐
│              SUPPORTING PROCESSES (Ad-hoc)                   │
└─────────────────────────────────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
  ┌─────▼──────┐    ┌───────▼────────┐  ┌──────▼───────┐
  │ Bổ sung    │    │ Chuyển Agency  │  │ Chuyển       │
  │ quà        │    │                │  │ Scheme       │
  └─────┬──────┘    └───────┬────────┘  └──────┬───────┘
        │                   │                   │
        │           ┌───────▼────────┐          │
        │           │ Luân chuyển    │          │
        │           │ quà giữa Store │          │
        │           └───────┬────────┘          │
        │                   │                   │
        └───────────────────┼───────────────────┘
                            │
                    ┌───────▼────────┐
                    │ Trả quà về     │
                    │ UHub → UGMS    │
                    └────────────────┘
```

**Đặc điểm:**
- Quy trình không bắt buộc
- Trigger theo nhu cầu (Ad-hoc)
- Có thể xảy ra bất kỳ lúc nào trong E2E
- Cần approval workflow riêng

---

### 1.3. Quy Trình Integration (Tích Hợp)

```
┌─────────────────────────────────────────────────────────────┐
│           INTEGRATION PROCESSES (System-to-System)           │
└─────────────────────────────────────────────────────────────┘
                            │
        ┌───────────────────┴───────────────────┐
        │                                       │
  ┌─────▼──────┐                        ┌──────▼───────┐
  │ UGMS → UHub│                        │ UHub → UGMS  │
  │ (Sync Data)│                        │ (Sync Back)  │
  └─────┬──────┘                        └──────┬───────┘
        │                                       │
        │  • API 1: Giftcode                   │  • API 6: Gift Usage
        │  • API 2: Scheme                     │  • Reconciliation Data
        │  • API 3: Customer                   │  • Adjustment Data
        │  • API 4: Xuất Kho                   │  • Evidences
        │  • API 5: Điều Chỉnh                 │
        │                                       │
        └───────────────────┬───────────────────┘
                            │
                    ┌───────▼────────┐
                    │ ITU Log Form   │
                    │ → UHub Admin   │
                    │ (Auto Email)   │
                    └────────────────┘
```

**Đặc điểm:**
- Real-time hoặc Near real-time
- Automated (không cần human intervention)
- Có authentication & validation
- Có audit trail

---

### 1.4. Quy Trình Reporting (Báo Cáo)

```
┌─────────────────────────────────────────────────────────────┐
│                   REPORTING PROCESSES                        │
└─────────────────────────────────────────────────────────────┘
                            │
        ┌───────────────────┴───────────────────┐
        │                                       │
  ┌─────▼──────┐                        ┌──────▼───────┐
  │ PowerBI    │                        │ Reports      │
  │ Dashboard  │                        │ Generation   │
  └─────┬──────┘                        └──────┬───────┘
        │                                       │
        │  • Gift Report                       │  • Rpt vòng dời quà
        │  • Campaign Performance              │  • Allocation Report
        │  • Stock Report                      │  • Reconciliation Report
        │  • Usage Report                      │  • Audit Trail Report
        │                                       │
        └───────────────────┬───────────────────┘
                            │
                    ┌───────▼────────┐
                    │ Review thực    │
                    │ nhận Show DS   │
                    │ store phân bổ  │
                    └────────────────┘
```

**Đặc điểm:**
- Read-only
- Refresh theo schedule (3 lần/ngày)
- Data từ nhiều nguồn
- Hỗ trợ decision making

---

## 2. Dependency Matrix (Ma Trận Phụ Thuộc)

### 2.1. Quy Trình Upstream Dependencies

| Quy trình | Phụ thuộc vào |
|-----------|---------------|
| **B1. Gift Planning** | - Không có (Starting point) |
| **B2. Agency WHs Delivery** | - B1. Gift Planning<br>- Quy trình Sync UGMS → UHub (API 1,2,3,4)<br>- ITU Log Form Sync |
| **B3. Store Delivery** | - B2. Agency WHs Delivery<br>- Allocation by store data |
| **B4. Gift Usage** | - B3. Store Delivery<br>- Campaign setup complete (Readiness gate)<br>- PG access & permissions |
| **B5. Gift Recall** | - B4. Gift Usage (End campaign + 5 days)<br>- Store inventory data |
| **B6. Stock Reconciliation** | - B5. Gift Recall<br>- B4. Gift Usage data<br>- B2. Initial stock data |
| **B7. Multi-level Approval** | - B6. Stock Reconciliation (Discrepancy case)<br>- Evidences from Agency |
| **Bổ sung quà** | - B1. Gift Planning (Scheme đã tồn tại)<br>- Quy trình Sync UGMS → UHub |
| **Chuyển Agency** | - B2/B3. Allocation data<br>- Quy trình điều chỉnh kho |
| **Luân chuyển quà giữa Store** | - B3. Store Delivery<br>- B4. Gift Usage (Runtime)<br>- Quy trình điều chỉnh kho |
| **Trả quà về UGMS** | - B5. Gift Recall<br>- B6. Stock Reconciliation<br>- Quy trình Sync UHub → UGMS |
| **Chuyển Scheme** | - B4. Gift Usage (Runtime)<br>- Multiple Schemes exist |
| **Điều chỉnh kho Agency** | - B2. Agency WHs Delivery<br>- Ticket System |
| **Report** | - B4. Gift Usage (Data source)<br>- B6. Reconciliation (Data source)<br>- All transactions data |
| **Review thực nhận** | - B2. Agency WHs Delivery<br>- B3. Store Delivery<br>- Allocation by store data |

---

### 2.2. Quy Trình Downstream Impact

| Quy trình | Ảnh hưởng đến |
|-----------|---------------|
| **B1. Gift Planning** | → B2, B3, B4, B5, B6, B7<br>→ All subsequent processes |
| **B2. Agency WHs Delivery** | → B3, B4<br>→ Quy trình Review thực nhận<br>→ Quy trình Điều chỉnh kho Agency |
| **B3. Store Delivery** | → B4, B5<br>→ Quy trình Luân chuyển quà giữa Store |
| **B4. Gift Usage** | → B5, B6<br>→ Quy trình Sync UHub → UGMS<br>→ Report Generation |
| **B5. Gift Recall** | → B6, B7<br>→ Quy trình Trả quà về UGMS |
| **B6. Stock Reconciliation** | → B7 (Discrepancy case)<br>→ Quy trình Sync UHub → UGMS<br>→ UGMS Stock Update |
| **B7. Multi-level Approval** | → Quy trình Sync UHub → UGMS<br>→ UGMS Stock Update with Evidences |
| **Sync UGMS → UHub** | → B2, B3<br>→ Bổ sung quà<br>→ All processes cần master data |
| **Bổ sung quà** | → B2, B3, B4<br>→ Allocation adjustment |
| **Chuyển Agency** | → B3, B4<br>→ Allocation adjustment<br>→ PG access adjustment |
| **Luân chuyển quà giữa Store** | → B4<br>→ Store inventory update<br>→ Report |
| **Trả quà về UGMS** | → UGMS Stock Update<br>→ Gift lifecycle closure |
| **Chuyển Scheme** | → B4<br>→ Campaign/Gift mapping adjustment |
| **Điều chỉnh kho Agency** | → B2, B3<br>→ Allocation adjustment<br>→ Ticket System |
| **ITU Log Form Sync** | → B1<br>→ Utop Admin Portal<br>→ Campaign setup |

---

## 3. Process Flow Diagram (Quan Hệ Tổng Thể)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              PHASE 0: SETUP                                  │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
            ┌───────────────────────┼───────────────────────┐
            │                       │                       │
    ┌───────▼────────┐      ┌───────▼────────┐    ┌───────▼────────┐
    │ Sync UGMS→UHub │      │ ITU Log Form   │    │ Utop Admin     │
    │ (API 1,2,3)    │      │ → UHub Admin   │    │ Portal Setup   │
    └───────┬────────┘      └───────┬────────┘    └───────┬────────┘
            │                       │                       │
            └───────────────────────┼───────────────────────┘
                                    │
┌───────────────────────────────────▼─────────────────────────────────────────┐
│                        PHASE 1: GIFT PLANNING (B1)                           │
│                                                                               │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐                  │
│  │ Customer     │───>│ Setup Gift   │───>│ Request      │                  │
│  │ Alignment    │    │ on UGMS      │    │ Campaign     │                  │
│  └──────────────┘    └──────────────┘    └──────┬───────┘                  │
└────────────────────────────────────────────────────┬─────────────────────────┘
                                                     │
                        ┌────────────────────────────┴─────┐
                        │                                  │
                  [Normal Flow]                     [Special Case]
                        │                                  │
┌───────────────────────▼──────────────────────┐   ┌──────▼───────────────┐
│  PHASE 2: DELIVERY TO AGENCY (B2)            │   │ Bổ sung quà         │
│                                               │   │ (cùng scheme)       │
│  ┌──────────────┐    ┌──────────────┐        │   └──────┬───────────────┘
│  │ Linfox       │───>│ Agency Check │        │          │
│  │ Delivery     │    │ Inventory    │        │          │
│  └──────────────┘    └──────┬───────┘        │          │
│                              │                │          │
│                      ┌───────▼────────┐       │   ┌──────▼───────────────┐
│                      │ Đủ hàng?       │       │   │ Điều chỉnh kho      │
│                      └───────┬────────┘       │   │ Agency → ticket     │
│                              │                │   └──────┬───────────────┘
│                  ┌───────────┴────────┐       │          │
│                  │                    │       │          │
│            [Yes]─▼            [No]────▼       │          │
│     ┌──────────────┐    ┌──────────────┐     │          │
│     │ Digital      │    │ Create       │<────┼──────────┘
│     │ Confirm      │    │ Ticket       │     │
│     └──────┬───────┘    └──────┬───────┘     │
│            │                   │              │
│            │         [BU Approval Loop]       │
│            │                   │              │
│            └───────────┬───────┘              │
└────────────────────────┼──────────────────────┘
                         │
┌────────────────────────▼──────────────────────┐
│  PHASE 3: DELIVERY TO STORES (B3)             │
│                                               │
│  ┌──────────────┐    ┌──────────────┐        │   ┌─────────────────────┐
│  │ Agency       │───>│ Sales Check  │        │   │ Chuyển Agency       │
│  │ Deliver      │    │ Inventory    │        │   │ (remove phân bổ     │
│  └──────────────┘    └──────┬───────┘        │   │  → chuyển agency)   │
│                              │                │   └─────────┬───────────┘
│                      ┌───────▼────────┐       │             │
│                      │ Đủ hàng?       │<──────┼─────────────┘
│                      └───────┬────────┘       │
│                              │                │
│                  ┌───────────┴────────┐       │
│                  │                    │       │
│            [Yes]─▼            [No]────▼       │
│     ┌──────────────┐    ┌──────────────┐     │
│     │ Sales        │    │ Create       │     │
│     │ Confirm      │    │ Ticket       │     │
│     └──────┬───────┘    └──────┬───────┘     │
│            │                   │              │
│            │         [BU Approval Loop]       │
│            │                   │              │
│            └───────────┬───────┘              │
└────────────────────────┼──────────────────────┘
                         │
                 [Campaign Readiness Gate]
                         │
┌────────────────────────▼──────────────────────┐
│  PHASE 4: GIFT USAGE (B4)                     │
│                                               │
│  ┌──────────────┐    ┌──────────────┐        │   ┌─────────────────────┐
│  │ PG Operates  │───>│ Track Gift   │────────┼──>│ PowerBI Reports:    │
│  │ Campaign     │    │ Usage        │        │   │ • Gift Report       │
│  │ (Sampling &  │    │              │        │   │ • Campaign Perf.    │
│  │  Redemption) │    └──────┬───────┘        │   └─────────────────────┘
│  └──────────────┘           │                │
│                              │                │   ┌─────────────────────┐
│                    ┌─────────▼─────────┐      │   │ Luân chuyển quà     │
│                    │ BU Trigger:       │<─────┼───│ giữa Store          │
│                    │ • Transfer Gift   │      │   └─────────────────────┘
│                    │ • Recall Gift     │      │
│                    │ • Change Agency   │      │   ┌─────────────────────┐
│                    └─────────┬─────────┘      │   │ Chuyển Scheme       │
│                              │                │   │                     │
└──────────────────────────────┼────────────────┘   └──────────┬──────────┘
                               │                               │
                               └───────────────┬───────────────┘
                                               │
┌──────────────────────────────────────────────▼─────────────────────────────┐
│  PHASE 5: GIFT RECALL (B5)                                                  │
│                                                                             │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐                 │
│  │ Agency       │───>│ Recall Gifts │───>│ Check        │                 │
│  │ Collect      │    │ from Stores  │    │ Recalled     │                 │
│  │ (5 days)     │    │              │    │ Inventory    │                 │
│  └──────────────┘    └──────────────┘    └──────┬───────┘                 │
└────────────────────────────────────────────────────┬───────────────────────┘
                                                     │
┌────────────────────────────────────────────────────▼───────────────────────┐
│  PHASE 6: STOCK RECONCILIATION (B6)                                         │
│                                                                             │
│  ┌──────────────┐    ┌──────────────┐                                      │
│  │ Agency Check │───>│ Compare:     │                                      │
│  │ vs Report    │    │ • Initial    │                                      │
│  │              │    │ • Usage      │                                      │
│  │              │    │ • Recalled   │                                      │
│  └──────────────┘    └──────┬───────┘                                      │
│                              │                                              │
│                      ┌───────▼────────┐                                     │
│                      │ Match?         │                                     │
│                      └───────┬────────┘                                     │
│                              │                                              │
│                  ┌───────────┴────────┐                                     │
│                  │                    │                                     │
│            [Yes]─▼            [No]────▼                                     │
│     ┌──────────────┐    ┌──────────────┐                                   │
│     │ Digital      │    │ Submit       │                                   │
│     │ Confirm      │    │ Ticket       │                                   │
│     └──────┬───────┘    └──────┬───────┘                                   │
│            │                   │                                            │
│            │                   │                                            │
│            │         ┌─────────▼──────────┐                                │
│            │         │ PHASE 7: MULTI-    │                                │
│            │         │ LEVEL APPROVAL (B7)│                                │
│            │         │                     │                                │
│            │         │ BU → BU Level-2    │                                │
│            │         │    → Utop Admin    │                                │
│            │         └─────────┬──────────┘                                │
│            │                   │                                            │
│            │         [Approved]│[Rejected]                                 │
│            │                   │     │                                      │
│            │                   │     └────────┐                            │
│            │                   │              │                            │
│            │                   │     ┌────────▼──────────┐                 │
│            │                   │     │ Request Agency    │                 │
│            │                   │     │ Correction        │                 │
│            │                   │     └────────┬──────────┘                 │
│            │                   │              │                            │
│            │                   │       [Loop to B6_01]                     │
│            │                   │                                            │
│            └───────────┬───────┘                                            │
└────────────────────────┼──────────────────────────────────────────────────┘
                         │
┌────────────────────────▼──────────────────────┐
│  FINAL PHASE: SYNC TO UGMS                    │
│                                               │
│  ┌──────────────┐    ┌──────────────┐        │   ┌─────────────────────┐
│  │ UHub Sync    │───>│ UGMS Update  │────────┼──>│ Trả quà về         │
│  │ Gift Usage + │    │ Stock with   │        │   │ UHub → UGMS        │
│  │ Adjustment + │    │ Evidences    │        │   │                     │
│  │ Evidences    │    │              │        │   └─────────────────────┘
│  └──────────────┘    └──────┬───────┘        │
│                              │                │   ┌─────────────────────┐
│                      ┌───────▼────────┐       │   │ Review thực nhận    │
│                      │ Gift Ready     │<──────┼───│ Show DS store       │
│                      │ for Reuse      │       │   │ phân bổ             │
│                      └────────────────┘       │   └─────────────────────┘
└───────────────────────────────────────────────┘
                         │
                    ┌────▼────┐
                    │   END   │
                    └─────────┘
```

---

## 4. Critical Dependencies (Phụ Thuộc Quan Trọng)

### 4.1. Blocking Dependencies (Chặn Hoàn Toàn)

| Quy trình bị chặn | Phụ thuộc vào | Lý do |
|-------------------|---------------|-------|
| **B2** | Sync UGMS → UHub (API 1,2,3) | Không có master data (Giftcode, Scheme, Customer) thì không tạo được Allocation |
| **B3** | B2 Agency Digital Confirm/Approved | Không có số liệu từ Agency thì không phân bổ cho Store |
| **B4** | B3 Campaign Readiness Gate | Không có quà tại Store thì PG không phát được |
| **B5** | B4 End Campaign | Phải kết thúc campaign mới recall được |
| **B6** | B5 Gift Recall Done | Không có hàng thu hồi thì không đối soát được |
| **B7** | B6 Discrepancy Ticket | Chỉ khi có sai lệch mới cần Multi-level Approval |

### 4.2. Soft Dependencies (Phụ Thuộc Mềm)

| Quy trình | Phụ thuộc vào | Có thể bypass? |
|-----------|---------------|----------------|
| **Bổ sung quà** | B2 | ✅ Yes - Có thể bổ sung trước khi delivery |
| **Chuyển Agency** | B3 | ✅ Yes - Có thể thay đổi bất kỳ lúc nào |
| **Luân chuyển quà** | B4 | ✅ Yes - Runtime operation |
| **Chuyển Scheme** | B4 | ✅ Yes - Runtime operation |
| **Điều chỉnh kho** | B2 | ✅ Yes - Ad-hoc operation |
| **Report** | B4, B6 | ❌ No - Cần data source |

---

## 5. Process Triggers (Điều Kiện Kích Hoạt)

### 5.1. Automatic Triggers (Tự Động)

| Quy trình | Trigger | Frequency |
|-----------|---------|-----------|
| **Sync UGMS → UHub** | UGMS data create/update | Real-time |
| **ITU Log Form Sync** | BU submit form | Real-time (Auto email) |
| **B5. Gift Recall** | End campaign date + 5 days | Scheduled |
| **Report Generation** | Schedule | 3 times/day |

### 5.2. Manual Triggers (Thủ Công)

| Quy trình | Trigger | Actor |
|-----------|---------|-------|
| **B1. Gift Planning** | Business decision | KA, BU |
| **B2. Agency Confirm/Ticket** | Inventory check result | Agency |
| **B3. Sales Confirm/Ticket** | Inventory check result | Sales |
| **B4. Gift Usage** | Campaign live | PG |
| **B6. Reconciliation Confirm/Ticket** | Reconciliation check | Agency |
| **B7. Multi-level Approval** | Discrepancy ticket | BU, BU Level-2 |
| **Bổ sung quà** | Gift shortage | BU |
| **Chuyển Agency** | Agency change request | BU |
| **Luân chuyển quà** | Stock imbalance | BU |
| **Trả quà về UGMS** | Gift recall complete | BU |
| **Chuyển Scheme** | Campaign adjustment | BU |
| **Điều chỉnh kho** | Inventory discrepancy | Agency, BU |

---

## 6. Data Flow Between Processes (Luồng Dữ Liệu)

### 6.1. Master Data Flow

```
UGMS (Source of Truth)
    │
    ├─[API 1: Giftcode]──────────────────────┐
    │   • gift_code                           │
    │   • gift_name                           │
    │   • gift_type                           │
    │   • uom                                 │
    │   • gift_group                          │
    │                                         │
    ├─[API 2: Scheme]────────────────────────┤
    │   • scheme_id                           │
    │   • scheme_name                         │
    │   • start_date, end_date                │
    │   • requester                           │
    │   • list_gift[]                         ├───> UHub
    │                                         │      │
    ├─[API 3: Customer]──────────────────────┤      │
    │   • customer_id                         │      │
    │   • ship_to_code                        │      │
    │   • province                            │      │
    │                                         │      │
    ├─[API 4: Xuất Kho]──────────────────────┤      │
    │   • ship_to_code                        │      │
    │   • scheme_id                           │      │
    │   • gift_list[]                         │      │
    │   • quantity                            │      │
    │                                         │      │
    └─[API 5: Điều Chỉnh Kho]────────────────┘      │
        • ship_to_code                               │
        • scheme_id                                  │
        • gift_list[]                                │
        • quantity (negative)                        │
                                                     │
ITU Log Form                                         │
    │                                               │
    └─[Auto Email]───────────────────────────────> UHub Admin
        • Scheme ID                                  │
        • Giftcode                                   │
        • Allocation by store                        │
        • Agency info                                │
        • SKUs info                                  │
                                                     │
                                                     ▼
                                            [B1, B2, B3, B4]
                                                     │
                                                     │
                                            [Gift Usage Tracking]
                                                     │
                                                     ▼
UHub ───[API 6: Gift Usage]──────────────────> UGMS
    • ship_to_code                               (Update Stock)
    • scheme_id
    • gift_list[]
    • quantity (used)
```

### 6.2. Transactional Data Flow

```
B2. Agency WHs Delivery
    │
    ├─[Digital Confirm]──────────> UHub
    │   • Allocation confirmed         │
    │   • Stock level                  │
    │                                  │
    ├─[Ticket (Discrepancy)]─────> UHub
    │   • Actual quantity                │
    │   • Discrepancy reason            │
    │                                   │
    └────────────────────────────────> B3
                                        │
                                        │
B3. Store Delivery                      │
    │                                   │
    ├─[Sales Confirm]────────────> UHub
    │   • Allocation by store confirmed │
    │                                   │
    ├─[Ticket (Discrepancy)]─────> UHub
    │   • Actual quantity                │
    │   • Discrepancy reason            │
    │                                   │
    └────────────────────────────────> B4
                                        │
                                        │
B4. Gift Usage                          │
    │                                   │
    ├─[PG Scan QR]───────────────> UHub
    │   • shopper_id                    │
    │   • gift_code                     │
    │   • quantity                      │
    │   • timestamp                     │
    │                                   │
    └─[Gift Usage Data]──────────────> Report
                                        │
                                        │
B5. Gift Recall                         │
    │                                   │
    └─[Recalled Inventory]───────────> B6
                                        │
                                        │
B6. Stock Reconciliation                │
    │                                   │
    ├─[Digital Confirm]──────────> UHub ───[API 6]───> UGMS
    │   • Reconciliation OK              │                (Normal)
    │                                   │
    ├─[Ticket (Discrepancy)]─────> UHub
    │   • Actual recalled quantity       │
    │   • Discrepancy reason            │
    │                                   │
    └────────────────────────────────> B7
                                        │
                                        │
B7. Multi-level Approval                │
    │                                   │
    ├─[BU Forward Ticket]────────> BU Level-2
    │                                   │
    ├─[BU Level-2 Approval]──────> BU ──> Utop Admin
    │                                   │
    └─[Utop Admin Confirm]───────> UHub ───[API 6]───> UGMS
        • Adjustment                       │            (With Evidences)
        • Evidences                        │
```

---

## 7. Process Priority & Sequencing (Ưu Tiên & Thứ Tự)

### 7.1. Critical Path (Đường Quan Trọng - Không Thể Delay)

```
Priority 1: E2E Core Flow (Must complete in sequence)
─────────────────────────────────────────────────────
B1 ──> B2 ──> B3 ──> B4 ──> B5 ──> B6 ──> B7 ──> END

Timeline:
• B1: T-30 days (Campaign live date - 30 days)
• B2: T-7 days (Agency ticket deadline: 168 hours)
• B3: T-2 days (Sales ticket deadline: 48 hours)
• B4: T (Campaign live)
• B5: T + Campaign duration + 5 days
• B6: T + Campaign duration + 10 days
• B7: T + Campaign duration + 15 days (if discrepancy)
```

### 7.2. Supporting Processes (Hỗ Trợ - Có Thể Song Song)

```
Priority 2: Ad-hoc Operations (Can run in parallel with main flow)
──────────────────────────────────────────────────────────────────
• Bổ sung quà (Any time during B2-B4)
• Chuyển Agency (Any time during B2-B4)
• Luân chuyển quà giữa Store (During B4)
• Chuyển Scheme (During B4)
• Điều chỉnh kho Agency (During B2-B3)
```

### 7.3. Background Processes (Nền - Continuous)

```
Priority 3: Continuous Operations
──────────────────────────────────
• Sync UGMS → UHub (Real-time)
• Report Generation (3x/day)
• ITU Log Form Sync (Real-time)
• Review thực nhận (On-demand)
```

---

## 8. Integration Points (Điểm Tích Hợp)

### 8.1. System-to-System Integration

| Integration Point | Source | Destination | Method | Frequency |
|-------------------|--------|-------------|--------|-----------|
| **Master Data Sync** | UGMS | UHub | REST API (1,2,3) | Real-time |
| **Transaction Sync** | UGMS | UHub | REST API (4,5) | Real-time |
| **Gift Usage Sync** | UHub | UGMS | REST API (6) | Real-time |
| **Log Form Sync** | ITU Log Form | UHub Admin | Auto Email | Real-time |
| **Report Data** | UHub | PowerBI | Data Export | 3x/day |

### 8.2. Human-to-System Integration

| Integration Point | Actor | System | Method | Trigger |
|-------------------|-------|--------|--------|---------|
| **Digital Confirm** | Agency | UHub | Web Portal | Manual |
| **Ticket Creation** | Agency/Sales | UHub | Web Portal | Manual |
| **Ticket Approval** | BU | UHub | Web Portal | Manual |
| **Level-2 Approval** | BU Level-2 | Email | Email Reply | Manual |
| **Utop Confirm** | Utop Admin | UHub Admin | Admin Portal | Manual |
| **PG Operations** | PG | UHub PG App | Mobile App | Manual |
| **Campaign Setup** | Utop Admin | UHub Admin | Admin Portal | Manual |

---

## 9. Error Handling & Recovery (Xử Lý Lỗi & Khôi Phục)

### 9.1. Process Rollback Scenarios

| Scenario | Affected Processes | Recovery Action |
|----------|-------------------|-----------------|
| **API Sync Failed** | B2, B3 | Retry mechanism, Manual sync fallback |
| **Agency Ticket Rejected** | B2 | Loop to B2_01 (Re-check inventory) |
| **Sales Ticket Rejected** | B3 | Loop to B3_02 (Re-check inventory) |
| **Campaign Not Ready** | B4 | Loop to B3_09 (Utop Admin fix issues) |
| **Reconciliation Discrepancy** | B6 | Trigger B7 (Multi-level Approval) |
| **Level-2 Rejection** | B7 | Loop to B6_01 (Agency re-check) |

### 9.2. Compensation Actions

| Failed Process | Compensation Action |
|----------------|---------------------|
| **Bổ sung quà Failed** | Rollback allocation, Notify BU |
| **Chuyển Agency Failed** | Restore old allocation, Notify BU |
| **Luân chuyển quà Failed** | Rollback stock movement, Notify BU |
| **Trả quà về UGMS Failed** | Keep in UHub, Manual sync required |
| **Sync UGMS Failed** | Queue for retry, Email alert to Utop Admin |

---

## 10. Business Rules & Constraints (Quy Tắc & Ràng Buộc)

### 10.1. Timeline Constraints

| Rule | Description | Exception |
|------|-------------|-----------|
| **Agency Ticket Deadline** | Must create ≤168 hours before campaign live | BU can request Utop Admin override |
| **Sales Ticket Deadline** | Must create ≤48 hours before campaign live | BU can request Utop Admin override |
| **BU Approval SLA (Agency)** | Must approve within 24 hours | Ticket expires after 24h |
| **BU Approval SLA (Sales)** | Must approve within 12 hours | Ticket expires after 12h |
| **Gift Recall Timeline** | Must complete within 5 days after campaign end | Extension requires BU approval |

### 10.2. Inventory Constraints

| Rule | Description | Validation Point |
|------|-------------|------------------|
| **No Negative Stock** | Stock cannot go below 0 | B2, B3, B4, B6 |
| **Allocation Sum Check** | Sum of store allocation = Agency total | B2, B3 |
| **Usage Limit** | Gift usage ≤ Allocated quantity | B4 |
| **Recall Expectation** | Recalled = Initial - Used | B6 |

### 10.3. Approval Rules

| Approval Type | Approver | Required For |
|---------------|----------|--------------|
| **BU Level-1** | BU | All tickets (B2, B3, B6) |
| **BU Level-2** | BU Level-2 | Reconciliation discrepancy (B7) |
| **Utop Admin** | Utop Admin | Final confirmation after Level-2 (B7) |
| **Campaign Readiness** | Utop Admin | Campaign go-live gate (B3→B4) |

---

## 11. Key Performance Indicators (KPIs)

### 11.1. Process Efficiency KPIs

| KPI | Target | Measurement |
|-----|--------|-------------|
| **Ticket Resolution Time** | < 24h (Agency), < 12h (Sales) | Time from ticket creation to approval |
| **Campaign Readiness Rate** | > 95% | % campaigns ready by planned go-live date |
| **Reconciliation Match Rate** | > 90% | % reconciliations with no discrepancy |
| **API Sync Success Rate** | > 99.9% | % successful API calls |
| **Report Generation SLA** | < 1 hour | Time from data update to report refresh |

### 11.2. Business Impact KPIs

| KPI | Target | Measurement |
|-----|--------|-------------|
| **Gift Allocation Accuracy** | > 95% | % allocations matching actual delivery |
| **Gift Usage Rate** | > 80% | (Used + Recalled) / Initial Allocation |
| **Stock Adjustment Rate** | < 10% | % campaigns requiring adjustment tickets |
| **Level-2 Approval Rate** | < 5% | % tickets escalating to Level-2 |
| **Gift Reuse Rate** | > 70% | % recalled gifts reused in next campaign |

---

## 12. Future Enhancements (Tính Năng Tương Lai)

### 12.1. Phase 2 Automation

| Enhancement | Impact Processes | Priority |
|-------------|-----------------|----------|
| **Auto Allocation Optimization** | B2, B3 | High |
| **Predictive Gift Demand** | B1, B2 | Medium |
| **Auto Store Transfer** | Luân chuyển quà | Medium |
| **Real-time Dashboard** | Report | High |
| **Mobile App for Sales** | B3 | Low |

### 12.2. Phase 2 New Processes

| New Process | Description | Dependencies |
|-------------|-------------|--------------|
| **Gift Lifecycle Management** | Track gift from creation to disposal | All processes |
| **Dynamic Scheme Adjustment** | Auto adjust scheme based on performance | B4, Report |
| **Gift Expiry Management** | Auto alert for expiring gifts | B2, B6 |
| **Cross-Scheme Gift Transfer** | Move gifts between schemes | Chuyển Scheme |

---

## Kết Luận

### Các Quy Trình Đã Số Hóa (Phase 1)
✅ **Hoàn thành:**
1. Quy trình đồng bộ UGMS → UHub (6 APIs)
2. Quy trình phân bổ quà cho campaign (B2, B3 với Ticket System)
3. Quy trình đối soát UHub → UGMS (B6, B7 với Multi-level Approval)
4. Quy trình điều chỉnh kho Agency → ticket (B2, B3, B6)

### Các Quy Trình Cần Phát Triển (Phase 2)
🔲 **Cần phân tích:**
1. Quy trình bổ sung quà, cùng scheme
2. Quy trình trả quà về UHub → UGMS
3. Quy trình đổi Agency (remove phân bổ → chuyển agency)
4. Quy trình chuyển scheme
5. Quy trình luân chuyển quà giữa Store
6. Report - Rpt vòng dời quà theo scheme

### Dependencies Quan Trọng
- **B2 phụ thuộc hoàn toàn** vào Sync UGMS → UHub
- **B4 phụ thuộc hoàn toàn** vào Campaign Readiness Gate (B3)
- **B7 chỉ kích hoạt** khi B6 có discrepancy
- **Supporting processes** có thể chạy song song với Core flow

### Recommendations
1. **Ưu tiên Phase 2**: Bổ sung quà, Luân chuyển quà (Business value cao)
2. **Automation**: Campaign Readiness Gate (Giảm manual effort)
3. **Integration**: PowerBI real-time dashboard (Tăng visibility)
4. **Mobile App**: Sales Confirm (Improve UX)
