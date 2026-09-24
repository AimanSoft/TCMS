# مخطط تدفق الأحداث (Flow of Events)

## نظرة عامة

يوضح هذا المستند كيف تحدث الأحداث وتنتشر عبر النظام باستخدام **RabbitMQ** في بنية Event-Driven Architecture.

## الأحداث الرئيسية

### 1. `PaymentCompletedEvent`

هذا هو الحدث الأهم في النظام، يتم إطلاقه عند اكتمال معالجة الدفع.

#### مخطط Mermaid.js

```mermaid
sequenceDiagram
    participant Client
    participant APIGateway
    participant PaymentService
    participant RabbitMQ
    participant ReportService
    participant NotificationService
    participant ApprovalService

    Client->>APIGateway: POST /payments/process
    APIGateway->>PaymentService: Process Payment
    PaymentService->>PaymentGateway: Charge Customer
    PaymentGateway-->>PaymentService: Payment Success
    PaymentService->>RabbitMQ: Publish PaymentCompletedEvent
    RabbitMQ-->>ReportService: Consume Event
    ReportService->>PostgreSQL: Update Financial Records
    RabbitMQ-->>NotificationService: Consume Event
    NotificationService->>Client: Send Confirmation
    RabbitMQ-->>ApprovalService: Consume Event
    ApprovalService->>PostgreSQL: Update Request Status
    ApprovalService->>RabbitMQ: Publish ApprovalGrantedEvent
```

```mermaid
flowchart TD
    A[Payment Request] --> B[Payment Service]
    B --> C{Payment Successful?}
    C -->|Yes| D[Publish PaymentCompletedEvent]
    C -->|No| E[Publish PaymentFailedEvent]
    D --> F[RabbitMQ Exchange: payment_events]
    F --> G[Report Service]
    F --> H[Notification Service]
    F --> I[Approval Service]
    G --> J[Update Financial Reports]
    H --> K[Send User Notification]
    I --> L[Update Request Status]
    L --> M[Publish ApprovalGrantedEvent]
```

### 2. `RequestSubmittedEvent`

```mermaid
sequenceDiagram
    participant Client
    participant RequestService
    participant RabbitMQ
    participant ApprovalService
    participant NotificationService

    Client->>RequestService: Submit Travel Request
    RequestService->>PostgreSQL: Save Request
    RequestService->>RabbitMQ: Publish RequestSubmittedEvent
    RabbitMQ->>ApprovalService: Consume Event
    ApprovalService->>ApprovalService: Create Approval Task
    ApprovalService->>NotificationService: Notify Manager
```

```mermaid
flowchart LR
    A[Client Submits Request] --> B[Request Service]
    B --> C[Save to Database]
    C --> D[Publish RequestSubmittedEvent]
    D --> E[RabbitMQ]
    E --> F[Approval Service]
    E --> G[Audit Service]
    F --> H[Create Approval Workflow]
    H --> I[Notify Manager]
```

### 3. `ApprovalGrantedEvent`

```mermaid
sequenceDiagram
    participant Manager
    participant ApprovalService
    participant RabbitMQ
    participant RequestService
    participant NotificationService

    Manager->>ApprovalService: Approve Request
    ApprovalService->>PostgreSQL: Update Status
    ApprovalService->>RabbitMQ: Publish ApprovalGrantedEvent
    RabbitMQ->>RequestService: Consume Event
    RequestService->>RequestService: Enable Next Steps
    RabbitMQ->>NotificationService: Notify Employee
```

```mermaid
flowchart TB
    A[Manager Approves] --> B[Approval Service]
    B --> C{All Approvals Complete?}
    C -->|Yes| D[Publish ApprovalGrantedEvent]
    C -->|No| E[Wait for More Approvals]
    D --> F[RabbitMQ Exchange]
    F --> G[Request Service]
    F --> H[Notification Service]
    G --> I[Enable Payment Processing]
    H --> J[Notify Employee]
```

## بنية RabbitMQ

### Exchanges
- `payment_events`: نوع direct للأحداث المالية
- `request_events`: نوع topic لأحداث الطلبات
- `notification_events`: نوع fanout للإشعارات

### Queues
- `payment.completed.queue`: لمعالجة المدفوعات المكتملة
- `request.submitted.queue`: لمعالجة الطلبات الجديدة
- `approval.granted.queue`: لمعالجة الموافقات

### Bindings
```
payment_events ----→ payment.completed.queue
request_events ----→ request.submitted.queue
approval_events ----→ approval.granted.queue
```

## معالجة الأخطاء

### Dead Letter Queue (DLQ)
```mermaid
flowchart LR
    A[Main Queue] -->|Failed| B[Retry Queue]
    B -->|Failed| C[Dead Letter Queue]
    C --> D[Alert System]
    D --> E[Manual Intervention]
```

### سياسات إعادة المحاولة
- أول إعادة محاولة: بعد 5 ثوانٍ
- ثاني إعادة محاولة: بعد 30 ثانية
- ثالث إعادة محاولة: بعد 5 دقائق
- بعد الفشل: إرسال إلى DLQ

## مراقبة الأحداث

- تتبع عدد الأحداث المنشورة والمستهلكة
- قياس زمن الاستجابة لكل خدمة
- تنبيهات عند تجاوز الحدود
- سجلات مفصلة لتصحيح الأخطاء
