# مخطط تدفق البيانات — المستوى 1 (Main Processes)

## Data Flow Diagram — Level 1 (Main Processes)

## المقدمة
مخطط تدفق البيانات للمستوى 1 (Main Processes) لنظام إدارة شركات الاتصالات، مرسوم بصيغة Mermaid Flowchart. يوضح العمليات الرئيسية الاثني عشر ومخازن البيانات والأنظمة الخارجية مع تدفقات البيانات بينها.

---

## DFD Level 1 — Main Processes

```mermaid
flowchart TB
    subgraph EA["الجهات الخارجية (External Entities)"]
        direction TB
        E_Customer["Customer"]
        E_CSE["Customer Service Employee"]
        E_Acc["Accountant"]
        E_NE["Network Engineer"]
        E_SA["Support Agent"]
        E_SAAdmin["Super Admin"]
        E_CA["Company Admin"]
        E_BM["Branch Manager"]
    end

    subgraph PROC["العمليات الرئيسية (Main Processes)"]
        direction TB
        P1["P1: Identity Management"]
        P2["P2: Customer Management"]
        P3["P3: SIM & Number Management"]
        P4["P4: Product & Package Management"]
        P5["P5: Subscription Management"]
        P6["P6: Usage Tracking"]
        P7["P7: Billing"]
        P8["P8: Payment Processing"]
        P9["P9: Network Management"]
        P10["P10: Incident Management"]
        P11["P11: Support Management"]
        P12["P12: Notification Service"]
    end

    subgraph DS["مخازن البيانات (Data Stores)"]
        direction TB
        D1["D1: Identity DB"]
        D2["D2: Customer DB"]
        D3["D3: SIM DB"]
        D4["D4: Product DB"]
        D5["D5: Subscription DB"]
        D6["D6: Usage DB"]
        D7["D7: Billing DB"]
        D8["D8: Payment DB"]
        D9["D9: Network DB"]
        D10["D10: Incident DB"]
        D11["D11: Support DB"]
        D12["D12: Notification DB"]
    end

    %% External → Processes
    E_Customer -->|"1. Request Service<br/>2. Payment<br/>3. Ticket"| P2
    E_Customer -->|"4. Incident Report"| P10
    E_Customer -->|"5. Create Ticket"| P11
    E_CSE -->|"6. Customer Data<br/>7. SIM Request<br/>8. Subscription Data"| P2
    E_CSE -->|"9. SIM Activation<br/>10. Subscription Creation"| P3
    E_CSE -->|"11. Subscription Creation"| P5
    E_Acc -->|"12. Payment Processing<br/>13. Invoice Request"| P7
    E_Acc -->|"14. Payment"| P8
    E_NE -->|"15. Incident Report<br/>16. Tower Status"| P9
    E_SA -->|"17. Ticket Response<br/>18. Status Update"| P11
    E_SAAdmin -->|"19. Management Commands"| P1
    E_CA -->|"20. Package Management<br/>21. Reports Request"| P4
    E_BM -->|"22. Reports Request"| P2

    %% Processes → External
    P2 -->|"23. Invoice<br/>24. Service Status"| E_Customer
    P2 -->|"25. Customer Details<br/>26. SIM Status"| E_CSE
    P7 -->|"27. Financial Reports<br/>28. Invoice Status"| E_Acc
    P7 -->|"29. Invoice"| E_Customer
    P9 -->|"30. Network Status<br/>31. Incident Details"| E_NE
    P11 -->|"32. Ticket Details"| E_SA
    P1 -->|"33. Reports<br/>34. Audit Logs"| E_SAAdmin
    P4 -->|"35. Reports<br/>36. System Status"| E_CA
    P2 -->|"37. Branch Operations"| E_BM

    %% Processes → Data Stores (Read/Write)
    P1 -->|"38. Auth Token<br/>39. User Roles"| D1
    P2 -->|"40. Customer Record<br/>41. Customer Update"| D2
    P3 -->|"42. SIM Record<br/>43. Activation Record"| D3
    P4 -->|"44. Package Data<br/>45. Price Data"| D4
    P5 -->|"46. Subscription Record<br/>47. Renewal"| D5
    P6 -->|"48. Usage Records"| D6
    P7 -->|"49. Invoice Record<br/>50. Invoice Items"| D7
    P8 -->|"51. Payment Record<br/>52. Transaction Record"| D8
    P9 -->|"53. Tower Data<br/>54. Device Data"| D9
    P10 -->|"55. Incident Record"| D10
    P11 -->|"56. Ticket Record<br/>57. Reply Record"| D11
    P12 -->|"58. Notification Record"| D12

    %% Data Stores → Processes
    D1 -->|"59. Auth Info"| P1
    D2 -->|"60. Customer Info"| P2
    D3 -->|"61. SIM Info<br/>62. Number Info"| P3
    D4 -->|"63. Package Info<br/>64. Feature Info"| P4
    D5 -->|"65. Subscription Info"| P5
    D6 -->|"66. Call/Message/Internet Usage"| P6
    D7 -->|"67. Invoice Data"| P7
    D8 -->|"68. Payment Data"| P8
    D9 -->|"69. Tower/Station/Device Data"| P9
    D10 -->|"70. Incident Data"| P10
    D11 -->|"71. Ticket Data"| P11
    D12 -->|"72. Notification Settings"| P12

    %% Process-to-Process (Event Driven)
    P5 -.->|"Event: SubscriptionCreated<br/>Payload: {subId, custId, simId, pkgId}| P7
    P5 -.->|"Event: SubscriptionCreated<br/>Payload: {subId, custId, email}"| P12
    P8 -.->|"Event: PaymentCompleted<br/>Payload: {txId, amount, invId}| P7
    P8 -.->|"Event: PaymentCompleted<br/>Payload: {txId, custId}"| P12
    P10 -.->|"Event: IncidentReported<br/>Payload: {incId, towerId, sev}| P11
    P10 -.->|"Event: IncidentReported<br/>Payload: {incId, towerId}| P9
    P3 -.->|"Event: SIMActivated<br/>Payload: {simId, msisdn}| P12
    P3 -.->|"Event: SIMActivated<br/>Payload: {simId, custId}| P5
    P7 -.->|"Event: InvoiceGenerated<br/>Payload: {invId, custId, amt}| P12
    P2 -.->|"Event: CustomerCreated<br/>Payload: {custId, name, email}| P12

    %% Style definitions
    style EA fill:#e1f5fe,stroke:#0288d1,stroke-width:2px
    style PROC fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style DS fill:#e8f5e9,stroke:#388e3c,stroke-width:2px
    style P1 fill:#FF9800,color:#fff
    style P2 fill:#FF9800,color:#fff
    style P3 fill:#FF9800,color:#fff
    style P4 fill:#FF9800,color:#fff
    style P5 fill:#FF9800,color:#fff
    style P6 fill:#FF9800,color:#fff
    style P7 fill:#FF9800,color:#fff
    style P8 fill:#FF9800,color:#fff
    style P9 fill:#FF9800,color:#fff
    style P10 fill:#FF9800,color:#fff
    style P11 fill:#FF9800,color:#fff
    style P12 fill:#FF9800,color:#fff
```

---

## تفصيل العمليات ومخازن البيانات

### العمليات (Processes)

| الرقم | العملية | الوصف | المخزن المرتبط |
|---|---|---|---|
| P1 | Identity Management | تسجيل الدخول، المستخدمون، الأدوار، الصلاحيات | D1: Identity DB |
| P2 | Customer Management | بيانات العملاء، العناوين، جهات الاتصال | D2: Customer DB |
| P3 | SIM & Number Management | شرائح SIM، ICCID، MSISDN، التفعيل | D3: SIM DB |
| P4 | Product & Package Management | الباقات، الأسعار، المميزات | D4: Product DB |
| P5 | Subscription Management | الاشتراكات، التجديد، الإلغاء | D5: Subscription DB |
| P6 | Usage Tracking | المكالمات، الرسائل، الإنترنت | D6: Usage DB |
| P7 | Billing | الفواتير، حساب المبالغ | D7: Billing DB |
| P8 | Payment Processing | الدفع، الشحن، المعاملات | D8: Payment DB |
| P9 | Network Management | الأبراج، المحطات، الأجهزة | D9: Network DB |
| P10 | Incident Management | الأعطال، البلاغات | D10: Incident DB |
| P11 | Support Management | الشكاوى، التذاكر، الردود | D11: Support DB |
| P12 | Notification Service | الإشعارات SMS/Email | D12: Notification DB |

### مخازن البيانات (Data Stores)

| الرقم | المخزن | البيانات المخزنة |
|---|---|---|
| D1 | Identity DB | المستخدمون، الأدوار، الصلاحيات، JWT |
| D2 | Customer DB | العملاء، العناوين، جهات الاتصال |
| D3 | SIM DB | شرائح SIM، أرقام الهواتف، الحالة |
| D4 | Product DB | الباقات، الأسعار، المميزات |
| D5 | Subscription DB | الاشتراكات، ربط العملاء |
| D6 | Usage DB | المكالمات، الرسائل، استهلاك الإنترنت |
| D7 | Billing DB | الفواتير، بنود الفواتير |
| D8 | Payment DB | المدفوعات، المعاملات المالية |
| D9 | Network DB | الأبراج، المحطات، الأجهزة |
| D10 | Incident DB | الأعطال، البلاغات |
| D11 | Support DB | التذاكر، الردود |
| D12 | Notification DB | سجلات الإشعارات |

---

## التدفقات المتكاملة المهمة

| التدفق | النوع | الوصف |
|---|---|---|
| P2 ↔ D2 | Read/Write | إدارة بيانات العملاء |
| P3 ↔ D3 | Read/Write | إدارة شرائح SIM |
| P5 ↔ D5 | Read/Write | إدارة الاشتراكات |
| P5 → P7 | Event | SubscriptionCreated → إعداد الفوترة |
| P8 → P7 | Event | PaymentCompleted → تحديث حالة الفاتورة |
| P8 → P12 | Event | PaymentCompleted → إشعار الدفع |
| P10 → P11 | Event | IncidentReported → إنشاء تذكرة دعم |
| P10 → P9 | Event | IncidentReported → تحديث حالة الشبكة |
| P3 → P12 | Event | SIMActivated → إشعار التفعيل |
| P3 → P5 | Event | SIMActivated → تفعيل الاشتراك |
| P7 → P12 | Event | InvoiceGenerated → إشعار الفاتورة |
| P2 → P12 | Event | CustomerCreated → إشعار الترحيب |

---

## ملخص المخطط

| المعيار | القيمة |
|---|---|
| عدد العمليات | 12 (P1-P12) |
| عدد مخازن البيانات | 12 (D1-D12) |
| عدد الجهات الخارجية | 8 |
| عدد التدفقات الصادرة (External → Process) | 22 |
| عدد التدفقات الواردة (Process → External) | 15 |
| عدد التدفقات (Process → Data Store) | 12 |
| عدد التدفقات (Data Store → Process) | 12 |
| عدد التدفقات المتكاملة (Event-Driven) | 12 |
| العدد الإجمالي لتدفقات البيانات | 73 |

---

> **تاريخ التحديث:** 2026-09-26
> **المصدر:** docs/system-modules.md و docs/system-operations.md و docs/system-data-entities.md و docs/phase-3-flow-of-action.md و docs/phase-3-flow-of-event.md و docs/phase-3-dfd-level-0.md
> **المستوى:** Level 1 (Main Processes)
> **الصيغة:** Mermaid Flowchart
> **الحالة:** Confirmed
