# تدفقات الأحداث (Flow of Event)

## Flow of Event

## المقدمة
تدفقات الأحداث (Event-Driven Processes) للحالات الرئيسية، مرسومة بصيغة Mermaid Sequence Diagrams. جميع التدفقات تتبع بنية النظام الموثقة في `docs/project-boundaries.md` (Microservices + API Gateway + Event-Driven Architecture with RabbitMQ).

---

## Event 1: PaymentCompleted

**الناشر:** Payment Service  
**المستقبلون:** Billing Service, Customer Service, Notification Service  
**المُحفِّز:** بعد نجاح عملية دفع (UC-28)  
**اسم الحدث:** `PaymentCompleted`  
**البيانات المُرسَلة (Payload):** `{ transactionId, paymentAmount, paymentStatus, invoiceId, customerId, timestamp }`

```mermaid
sequenceDiagram
    actor Customer
    participant PaymentService as Payment Service
    participant RabbitMQ as RabbitMQ (Message Broker)
    participant BillingService as Billing Service
    participant CustomerService as Customer Service
    participant NotificationService as Notification Service

    Customer->>PaymentService: Process Payment Request
    PaymentService->>PaymentService: Simulated Payment Gateway
    PaymentService->>RabbitMQ: Publish Event: PaymentCompleted
    RabbitMQ->>BillingService: Deliver PaymentCompleted Event
    RabbitMQ->>CustomerService: Deliver PaymentCompleted Event
    RabbitMQ->>NotificationService: Deliver PaymentCompleted Event

    BillingService->>BillingService: Update Invoice Status = "Paid"
    CustomerService->>CustomerService: Update Customer Balance
    NotificationService->>NotificationService: Send Payment Confirmation (SMS/Email)
```

**تفاصيل الحدث:**
| الحقل | القيمة |
|---|---|
| Event Name | `PaymentCompleted` |
| Publisher | Payment Service |
| Message Broker | RabbitMQ |
| Subscribers | Billing Service, Customer Service, Notification Service |
| Payload | `{ transactionId, paymentAmount, paymentStatus, invoiceId, customerId, timestamp }` |

---

## Event 2: IncidentReported

**الناشر:** Incident Service  
**المستقبلون:** Support Service, Network Service  
**المُحفِّز:** بعد تسجيل عطل (UC-35)  
**اسم الحدث:** `IncidentReported`  
**البيانات المُرسَلة (Payload):** `{ incidentId, towerId, stationId, description, severity, status, reportedDate, reportedBy }`

```mermaid
sequenceDiagram
    actor Network Engineer/Customer
    participant IncidentService as Incident Service
    participant RabbitMQ as RabbitMQ (Message Broker)
    participant SupportService as Support Service
    participant NetworkService as Network Service

    Network Engineer/Customer->>IncidentService: Report Incident Request
    IncidentService->>IncidentService: Create Incident Record (Status = "Reported")
    IncidentService->>RabbitMQ: Publish Event: IncidentReported
    RabbitMQ->>SupportService: Deliver IncidentReported Event
    RabbitMQ->>NetworkService: Deliver IncidentReported Event

    SupportService->>SupportService: Create Support Ticket (if needed)
    NetworkService->>NetworkService: Assign Engineer & Prepare Resources
```

**تفاصيل الحدث:**
| الحقل | القيمة |
|---|---|
| Event Name | `IncidentReported` |
| Publisher | Incident Service |
| Message Broker | RabbitMQ |
| Subscribers | Support Service, Network Service |
| Payload | `{ incidentId, towerId, stationId, description, severity, status, reportedDate, reportedBy }` |

---

## Event 3: SubscriptionCreated

**الناشر:** Subscription Service  
**المستقبلون:** Billing Service, Notification Service  
**المُحفِّز:** بعد إنشاء اشتراك (UC-19)  
**اسم الحدث:** `SubscriptionCreated`  
**البيانات المُرسَلة (Payload):** `{ subscriptionId, customerId, simId, phoneNumber, packageId, startDate, billingCycle, status }`

```mermaid
sequenceDiagram
    actor Customer Service Employee
    participant SubscriptionService as Subscription Service
    participant RabbitMQ as RabbitMQ (Message Broker)
    participant BillingService as Billing Service
    participant NotificationService as Notification Service

    Customer Service Employee->>SubscriptionService: Create Subscription Request
    SubscriptionService->>SubscriptionService: Create Subscription Record
    SubscriptionService->>RabbitMQ: Publish Event: SubscriptionCreated
    RabbitMQ->>BillingService: Deliver SubscriptionCreated Event
    RabbitMQ->>NotificationService: Deliver SubscriptionCreated Event

    BillingService->>BillingService: Create Billing Account for Subscription
    NotificationService->>NotificationService: Send Welcome Notification (SMS/Email)
```

**تفاصيل الحدث:**
| الحقل | القيمة |
|---|---|
| Event Name | `SubscriptionCreated` |
| Publisher | Subscription Service |
| Message Broker | RabbitMQ |
| Subscribers | Billing Service, Notification Service |
| Payload | `{ subscriptionId, customerId, simId, phoneNumber, packageId, startDate, billingCycle, status }` |

---

## Event 4: SIMActivated

**الناشر:** SIM & Number Service  
**المستقبلون:** Notification Service, Subscription Service  
**المُحفِّز:** بعد تفعيل SIM (UC-11)  
**اسم الحدث:** `SIMActivated`  
**البيانات المُرسَلة (Payload):** `{ simId, iccid, msisdn, activationDate, customerId, status }`

```mermaid
sequenceDiagram
    actor Customer Service Employee
    participant SIMService as SIM & Number Service
    participant RabbitMQ as RabbitMQ (Message Broker)
    participant NotificationService as Notification Service
    participant SubscriptionService as Subscription Service

    Customer Service Employee->>SIMService: Activate SIM Request
    SIMService->>SIMService: Update SIM Status = "Activated"
    SIMService->>SIMService: Create Activation Record
    SIMService->>RabbitMQ: Publish Event: SIMActivated
    RabbitMQ->>NotificationService: Deliver SIMActivated Event
    RabbitMQ->>SubscriptionService: Deliver SIMActivated Event

    NotificationService->>NotificationService: Send SIM Activation Confirmation
    SubscriptionService->>SubscriptionService: Enable Subscription Creation (SIM Ready)
```

**تفاصيل الحدث:**
| الحقل | القيمة |
|---|---|
| Event Name | `SIMActivated` |
| Publisher | SIM & Number Service |
| Message Broker | RabbitMQ |
| Subscribers | Notification Service, Subscription Service |
| Payload | `{ simId, iccid, msisdn, activationDate, customerId, status }` |

---

## Event 5: InvoiceGenerated

**الناشر:** Billing Service  
**المستقبلون:** Notification Service  
**المُحفِّز:** بعد إنشاء فاتورة (UC-25)  
**اسم الحدث:** `InvoiceGenerated`  
**البيانات المُرسَلة (Payload):** `{ invoiceId, customerId, subscriptionId, totalAmount, items, dueDate, status, createdDate }`

```mermaid
sequenceDiagram
    actor Accountant
    participant BillingService as Billing Service
    participant RabbitMQ as RabbitMQ (Message Broker)
    participant NotificationService as Notification Service

    Accountant->>BillingService: Generate Invoice Request
    BillingService->>BillingService: Create Invoice Record & Invoice Items
    BillingService->>BillingService: Set Status = "Pending"
    BillingService->>RabbitMQ: Publish Event: InvoiceGenerated
    RabbitMQ->>NotificationService: Deliver InvoiceGenerated Event

    NotificationService->>NotificationService: Send Invoice Notification (SMS/Email)
```

**تفاصيل الحدث:**
| الحقل | القيمة |
|---|---|
| Event Name | `InvoiceGenerated` |
| Publisher | Billing Service |
| Message Broker | RabbitMQ |
| Subscribers | Notification Service |
| Payload | `{ invoiceId, customerId, subscriptionId, totalAmount, items, dueDate, status, createdDate }` |

---

## Event 6: CustomerCreated

**الناشر:** Customer Service  
**المستقبلون:** Notification Service  
**المُحفِّز:** بعد إنشاء عميل (UC-05)  
**اسم الحدث:** `CustomerCreated`  
**البيانات المُرسَلة (Payload):** `{ customerId, name, address, phone, email, customerType, registrationDate, status }`

```mermaid
sequenceDiagram
    actor Customer Service Employee
    participant CustomerService as Customer Service
    participant RabbitMQ as RabbitMQ (Message Broker)
    participant NotificationService as Notification Service

    Customer Service Employee->>CustomerService: Create Customer Request
    CustomerService->>CustomerService: Create Customer Record
    CustomerService->>RabbitMQ: Publish Event: CustomerCreated
    RabbitMQ->>NotificationService: Deliver CustomerCreated Event

    NotificationService->>NotificationService: Send Welcome Notification (SMS/Email)
```

**تفاصيل الحدث:**
| الحقل | القيمة |
|---|---|
| Event Name | `CustomerCreated` |
| Publisher | Customer Service |
| Message Broker | RabbitMQ |
| Subscribers | Notification Service |
| Payload | `{ customerId, name, address, phone, email, customerType, registrationDate, status }` |

---

## ملخص تدفقات الأحداث

| الرقم | اسم الحدث | الناشر | المستقبلون | المُحفِّز |
|---|---|---|---|---|
| 1 | PaymentCompleted | Payment Service | Billing, Customer, Notification | UC-28 |
| 2 | IncidentReported | Incident Service | Support, Network | UC-35 |
| 3 | SubscriptionCreated | Subscription Service | Billing, Notification | UC-19 |
| 4 | SIMActivated | SIM & Number Service | Notification, Subscription | UC-11 |
| 5 | InvoiceGenerated | Billing Service | Notification | UC-25 |
| 6 | CustomerCreated | Customer Service | Notification | UC-05 |

**بنية الرسائل:**
- **Message Broker:** RabbitMQ
- **نمط النشر:** Publish-Subscribe
- **ضمان التسليم:** كل حدث يُرسل إلى جميع المشتركين المسجلين
- **الاستقلالية:** كل خدمة مستقبلة تعالج الحدث بشكل مستقل دون تأثير على الناشر أو الخدمات الأخرى

---

> **تاريخ التحديث:** 2026-09-26
> **المصدر:** docs/phase-2-detailed-scenarios.md و docs/system-modules.md و docs/system-operations.md و docs/project-boundaries.md
> **عدد الأحداث:** 6
> **الصيغة:** Mermaid Sequence Diagram
> **الحالة:** Confirmed
