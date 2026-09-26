# تدفقات الإجراء (Flow of Action)

## Flow of Action

## المقدمة
تدفقات الإجراء للحالات الأساسية الثمانية، مرسومة بصيغة Mermaid Sequence Diagrams. جميع التدفقات مبنية على السيناريوهات التفصيلية الموثقة في `docs/phase-2-detailed-scenarios.md` وبنية النظام الموثقة في `docs/project-boundaries.md` (Microservices + API Gateway + Event-Driven Architecture).

---

## UC-05: Create Customer

```mermaid
sequenceDiagram
    actor Customer Service Employee
    participant APIGateway as API Gateway
    participant IdentityService as Identity Service
    participant CustomerService as Customer Service
    participant CustomerDB as Customers Database

    Customer Service Employee->>APIGateway: POST /customers (Create Customer)
    APIGateway->>IdentityService: Validate JWT Token
    IdentityService-->>APIGateway: Token Valid + Permissions
    APIGateway->>CustomerService: Create Customer Request
    CustomerService->>CustomerDB: INSERT Customer Record
    CustomerDB-->>CustomerService: Customer ID Generated
    CustomerService-->>APIGateway: Success Response (Customer ID)
    APIGateway-->>Customer Service Employee: Confirm Customer Created
```

---

## UC-11: Activate SIM

```mermaid
sequenceDiagram
    actor Customer Service Employee
    participant APIGateway as API Gateway
    participant IdentityService as Identity Service
    participant SIMService as SIM & Number Service
    participant SIMDB as SIMs Database
    participant ActivationDB as SIM_Activations Table
    participant StatusHistoryDB as SIM_Status_History Table

    Customer Service Employee->>APIGateway: POST /sim/{simId}/activate
    APIGateway->>IdentityService: Validate JWT Token
    IdentityService-->>APIGateway: Token Valid + Permissions
    APIGateway->>SIMService: Activate SIM Request
    SIMService->>SIMDB: GET SIM Status
    SIMDB-->>SIMService: SIM Status (Not Activated)
    SIMService->>SIMDB: UPDATE SIM Status = "Activated"
    SIMService->>ActivationDB: INSERT Activation Record
    SIMService->>StatusHistoryDB: INSERT Status History Record
    SIMService-->>APIGateway: Success Response
    APIGateway-->>Customer Service Employee: Confirm SIM Activated
```

---

## UC-19: Create Subscription

```mermaid
sequenceDiagram
    actor Customer Service Employee
    participant APIGateway as API Gateway
    participant IdentityService as Identity Service
    participant SubscriptionService as Subscription Service
    participant CustomerDB as Customers Database
    participant SIMDB as SIMs Database
    participant SubscriptionDB as Subscriptions Table

    Customer Service Employee->>APIGateway: POST /subscriptions (Create Subscription)
    APIGateway->>IdentityService: Validate JWT Token
    IdentityService-->>APIGateway: Token Valid + Permissions
    APIGateway->>SubscriptionService: Create Subscription Request
    SubscriptionService->>CustomerDB: Verify Customer Exists
    CustomerDB-->>SubscriptionService: Customer Found
    SubscriptionService->>SIMDB: Verify SIM Activated
    SIMDB-->>SubscriptionService: SIM Status = Activated
    SubscriptionService->>SubscriptionDB: INSERT Subscription Record
    SubscriptionService->>SubscriptionDB: Update Customer Status = "Active Subscriber"
    SubscriptionDB-->>SubscriptionService: Subscription ID Generated
    SubscriptionService-->>APIGateway: Success Response (Subscription ID)
    APIGateway-->>Customer Service Employee: Confirm Subscription Created
```

---

## UC-25: Generate Invoice

```mermaid
sequenceDiagram
    actor Accountant
    participant APIGateway as API Gateway
    participant IdentityService as Identity Service
    participant BillingService as Billing Service
    participant UsageService as Usage Service
    participant InvoiceDB as Invoices Table
    participant InvoiceItemsDB as Invoice_Items Table
    participant SubscriptionDB as Subscriptions Table

    Accountant->>APIGateway: POST /invoices (Generate Invoice)
    APIGateway->>IdentityService: Validate JWT Token
    IdentityService-->>APIGateway: Token Valid + Permissions
    APIGateway->>BillingService: Generate Invoice Request
    BillingService->>SubscriptionDB: Get Active Subscription
    SubscriptionDB-->>BillingService: Subscription Details
    BillingService->>UsageService: Request Usage Records
    UsageService-->>BillingService: Call, Message, Internet Usage Records
    BillingService->>BillingService: Calculate Amounts
    BillingService->>InvoiceDB: INSERT Invoice Record
    BillingService->>InvoiceItemsDB: INSERT Invoice Items
    BillingService->>InvoiceDB: Set Invoice Status = "Pending"
    InvoiceDB-->>BillingService: Invoice ID Generated
    BillingService-->>APIGateway: Success Response (Invoice ID)
    APIGateway-->>Accountant: Return Generated Invoice
```

---

## UC-28: Process Payment

```mermaid
sequenceDiagram
    actor Customer/Accountant
    participant APIGateway as API Gateway
    participant IdentityService as Identity Service
    participant BillingService as Billing Service
    participant PaymentService as Payment Service
    participant PaymentDB as Payments Table
    participant TransactionDB as Transactions Table
    participant InvoiceDB as Invoices Table

    Customer/Accountant->>APIGateway: POST /payments (Process Payment)
    APIGateway->>IdentityService: Validate JWT Token
    IdentityService-->>APIGateway: Token Valid + Permissions
    APIGateway->>BillingService: Get Invoice Details
    BillingService->>InvoiceDB: Get Invoice by ID
    InvoiceDB-->>BillingService: Invoice Details (Amount Pending)
    BillingService-->>APIGateway: Invoice Information
    APIGateway->>PaymentService: Process Payment Request
    PaymentService->>PaymentService: Simulated Payment Gateway
    PaymentService->>PaymentDB: INSERT Payment Record
    PaymentService->>TransactionDB: INSERT Transaction Record
    PaymentService->>InvoiceDB: UPDATE Invoice Status = "Paid"
    PaymentService->>PaymentDB: UPDATE Balance
    PaymentService-->>APIGateway: Payment Success
    APIGateway-->>Customer/Accountant: Confirm Payment Completed
```

---

## UC-35: Report Incident

```mermaid
sequenceDiagram
    actor Network Engineer/Customer
    participant APIGateway as API Gateway
    participant IdentityService as Identity Service
    participant IncidentService as Incident Service
    participant IncidentDB as Incidents Table
    participant NetworkDB as Network Database (Towers)

    Network Engineer/Customer->>APIGateway: POST /incidents (Report Incident)
    APIGateway->>IdentityService: Validate JWT Token
    IdentityService-->>APIGateway: Token Valid + Permissions
    APIGateway->>IncidentService: Report Incident Request
    IncidentService->>NetworkDB: Verify Tower/Station Exists
    NetworkDB-->>IncidentService: Tower/Station Found
    IncidentService->>IncidentDB: INSERT Incident Record
    IncidentService->>IncidentDB: Set Incident Status = "Reported"
    IncidentService->>IncidentDB: Set Report Date
    IncidentDB-->>IncidentService: Incident ID Generated
    IncidentService-->>APIGateway: Success Response (Incident ID)
    APIGateway-->>Network Engineer/Customer: Confirm Incident Reported
```

---

## UC-38: Create Ticket

```mermaid
sequenceDiagram
    actor Customer
    participant APIGateway as API Gateway
    participant IdentityService as Identity Service
    participant SupportService as Support Service
    participant CustomerDB as Customers Database
    participant TicketDB as Tickets Table

    Customer->>APIGateway: POST /tickets (Create Ticket)
    APIGateway->>IdentityService: Validate JWT Token
    IdentityService-->>APIGateway: Token Valid + Permissions
    APIGateway->>SupportService: Create Ticket Request
    SupportService->>CustomerDB: Verify Customer Exists
    CustomerDB-->>SupportService: Customer Found
    SupportService->>TicketDB: INSERT Ticket Record
    SupportService->>TicketDB: Set Ticket Status = "Open"
    SupportService->>TicketDB: Generate Ticket Number
    SupportService->>TicketDB: Set Created Date
    TicketDB-->>SupportService: Ticket ID Generated
    SupportService-->>APIGateway: Success Response (Ticket ID)
    APIGateway-->>Customer: Confirm Ticket Created
```

---

## UC-42: Full Customer Lifecycle (End-to-End)

```mermaid
sequenceDiagram
    actor Customer
    participant APIGateway as API Gateway
    participant IdentityService as Identity Service
    participant CustomerService as Customer Service
    participant CustomerDB as Customers Database
    participant SIMService as SIM & Number Service
    participant SIMDB as SIMs Database
    participant SubscriptionService as Subscription Service
    participant SubscriptionDB as Subscriptions Table
    participant PackageService as Product/Package Service
    participant BillingService as Billing Service
    participant InvoiceDB as Invoices Table
    participant PaymentService as Payment Service
    participant PaymentDB as Payments Table
    participant NotificationService as Notification Service

    Customer->>APIGateway: Initiate Full Customer Lifecycle
    APIGateway->>IdentityService: Validate JWT Token
    IdentityService-->>APIGateway: Token Valid

    Note over Customer,APIGateway: Step 1: Create Customer
    APIGateway->>CustomerService: Create Customer
    CustomerService->>CustomerDB: INSERT Customer Record
    CustomerDB-->>CustomerService: Customer ID

    Note over Customer,APIGateway: Step 2: Create SIM
    CustomerService->>APIGateway: Request SIM Creation
    APIGateway->>SIMService: Create SIM
    SIMService->>SIMDB: INSERT SIM Record
    SIMDB-->>SIMService: SIM ID

    Note over Customer,APIGateway: Step 3: Activate SIM
    SIMService->>SIMDB: UPDATE SIM Status = "Activated"
    SIMService->>SIMDB: INSERT Activation Record

    Note over Customer,APIGateway: Step 4: Assign Phone Number
    SIMService->>SIMDB: UPDATE SIM with Phone Number

    Note over Customer,APIGateway: Step 5: Create Subscription
    APIGateway->>SubscriptionService: Create Subscription
    SubscriptionService->>CustomerDB: Verify Customer
    SubscriptionService->>SIMDB: Verify SIM Activated
    SubscriptionService->>SubscriptionDB: INSERT Subscription Record
    SubscriptionDB-->>SubscriptionService: Subscription ID

    Note over Customer,APIGateway: Step 6: Assign Package
    SubscriptionService->>PackageService: Assign Package
    PackageService->>SubscriptionDB: UPDATE Subscription with Package ID

    Note over Customer,APIGateway: Step 7: Generate Invoice
    SubscriptionService->>APIGateway: Trigger Invoice Generation
    APIGateway->>BillingService: Generate Invoice
    BillingService->>SubscriptionDB: Get Subscription Details
    BillingService->>BillingService: Calculate Amounts
    BillingService->>InvoiceDB: INSERT Invoice Record
    InvoiceDB-->>BillingService: Invoice ID
    BillingService->>InvoiceDB: Set Status = "Pending"

    Note over Customer,APIGateway: Step 8: Process Payment
    BillingService->>APIGateway: Create Payment Request
    APIGateway->>PaymentService: Process Payment
    PaymentService->>PaymentService: Simulated Payment Gateway
    PaymentService->>PaymentDB: INSERT Payment Record
    PaymentService->>TransactionDB: INSERT Transaction Record
    PaymentService->>InvoiceDB: UPDATE Status = "Paid"

    Note over Customer,APIGateway: Step 9: Send Notification
    BillingService->>NotificationService: Send Confirmation
    NotificationService->>NotificationService: SMS/Email Mock
    NotificationService-->>APIGateway: Notification Sent

    APIGateway-->>Customer: Full Lifecycle Complete — All Records Created
```

---

## ملخص تدفقات الإجراء

| الرقم | حالة الاستخدام | الخدمات المشاركة | قواعد البيانات |
|---|---|---|---|
| UC-05 | Create Customer | Identity, Customer | Customers DB |
| UC-11 | Activate SIM | Identity, SIM & Number | SIMs, SIM_Activations, SIM_Status_History |
| UC-19 | Create Subscription | Identity, Subscription | Customers, SIMs, Subscriptions |
| UC-25 | Generate Invoice | Identity, Billing, Usage | Invoices, Invoice_Items, Subscriptions |
| UC-28 | Process Payment | Identity, Billing, Payment | Payments, Transactions, Invoices |
| UC-35 | Report Incident | Identity, Incident | Incidents, Network (Towers) |
| UC-38 | Create Ticket | Identity, Support | Tickets, Customers |
| UC-42 | Full Customer Lifecycle | Identity, Customer, SIM, Subscription, Package, Billing, Payment, Notification | جميع قواعد البيانات |

---

> **تاريخ التحديث:** 2026-09-26
> **المصدر:** docs/phase-2-detailed-scenarios.md و docs/system-operations.md و docs/project-boundaries.md
> **عدد تدفقات الإجراء:** 8
> **الصيغة:** Mermaid Sequence Diagram
> **الحالة:** Confirmed
