# 🪪 TCMS — Master ID Cross-Reference Card

> Telecommunications Company Management System
> **الهدف:** ربط جميع المعرفات (IDs) ببعضها البعض وبجداول قاعدة البيانات — مرجع موحد لـ Phase 5 (Architecture) و Phase 6 (Implementation).

---

## الجدول 1: Master ID Registry (سجل المعرفات الموحد)

| ID Type | Prefix | Range | Count | Source File |
|---|---|---|---|---|
| Objectives | OBJ | OBJ-01 → OBJ-16 | 16 | `docs/project-objectives.md` |
| Actors | ACT | ACT-01 → ACT-08 | 8 | `docs/system-actors.md` |
| Modules | M | M1 → M12 | 12 | `docs/system-modules.md` |
| Use Cases | UC | UC-01 → UC-42 | 42 | `docs/phase-2-use-cases-high-level.md` |
| User Stories | US | US-01 → US-65 | 65 | `docs/user-stories.md` |
| Business Rules | BR | BR-01 → BR-77 | 77 | `docs/business-rules-catalog.md` |
| NFRs | NFR | NFR-01 → NFR-34 | 34 | `docs/system-nfr.md` |
| Data Stores | D | D1 → D12 | 12 | `docs/phase-3-data-stores-and-movements.md` |
| Events | EVT | EVT-01 → EVT-10 | 10 | `docs/phase-3-data-stores-and-movements.md` |
| Direct Flows (API) | FLOW | FLOW-01 → FLOW-13 | 13 | `docs/phase-3-data-stores-and-movements.md` |
| End-to-End UC | UC-E2E | UC-42 | 1 | `docs/phase-2-use-cases-high-level.md` |

### Actor → ACT ID Mapping

| ACT ID | Actor Name | Actor Type |
|---|---|---|
| ACT-01 | Super Admin | Internal |
| ACT-02 | Company Admin | Internal |
| ACT-03 | Branch Manager | Internal |
| ACT-04 | Customer Service Employee | Internal |
| ACT-05 | Accountant | Internal |
| ACT-06 | Network Engineer | Internal |
| ACT-07 | Support Agent | Internal |
| ACT-08 | Customer | External |

### Module → M ID Mapping

| M ID | Module Name | Database |
|---|---|---|
| M1 | Identity Service | Identity DB |
| M2 | Customer Service | Customer DB |
| M3 | SIM & Number Service | SIM DB |
| M4 | Product / Package Service | Product DB |
| M5 | Subscription Service | Subscription DB |
| M6 | Usage Service | Usage DB |
| M7 | Billing Service | Billing DB |
| M8 | Payment Service | Payment DB |
| M9 | Network Service | Network DB |
| M10 | Incident Service | Incident DB |
| M11 | Support Service | Support DB |
| M12 | Notification Service | Notification DB |

---

## الجدول 2: Modules → Database Tables Mapping

| Module ID | Module Name | Database | Main Tables | Access Pattern |
|---|---|---|---|---|
| M1 | Identity Service | Identity DB | `users`, `roles`, `permissions`, `user_roles`, `role_permissions`, `refresh_tokens` | Read/Write, High Frequency |
| M2 | Customer Service | Customer DB | `customers`, `addresses`, `contacts`, `customer_documents` | Read/Write, High Consistency |
| M3 | SIM & Number Service | SIM DB | `sims`, `phone_numbers`, `sim_activations`, `sim_status_history` | Read/Write, High Frequency |
| M4 | Product / Package Service | Product DB | `packages`, `package_features`, `package_prices`, `services` | Read Heavy, Write Light |
| M5 | Subscription Service | Subscription DB | `subscriptions` | Read/Write, High Consistency |
| M6 | Usage Service | Usage DB | `usage_records` | Write Heavy, Read for Billing |
| M7 | Billing Service | Billing DB | `invoices`, `invoice_items` | Read/Write, High Consistency |
| M8 | Payment Service | Payment DB | `payments`, `transactions` | Read/Write, High Security |
| M9 | Network Service | Network DB | `towers`, `stations`, `devices`, `coverage_areas` | Read/Write, Medium Performance |
| M10 | Incident Service | Incident DB | `incidents` | Read/Write |
| M11 | Support Service | Support DB | `tickets`, `ticket_replies` | Read/Write |
| M12 | Notification Service | Notification DB | `notifications` | Write Heavy, Read for Review |

---

## الجدول 3: Use Cases → Modules → Database Tables

| UC ID | Use Case Name | Module (M) | DB Tables (Read) | DB Tables (Write) |
|---|---|---|---|---|
| UC-01 | Login | M1 | `users`, `roles`, `permissions` | `refresh_tokens` |
| UC-02 | Logout | M1 | `users` | `refresh_tokens` (invalidate) |
| UC-03 | Register User | M1 | — | `users`, `roles`, `user_roles`, `role_permissions` |
| UC-04 | Refresh Token | M1 | `users`, `refresh_tokens` | `refresh_tokens` |
| UC-05 | Create Customer | M2 | — | `customers`, `addresses`, `contacts` |
| UC-06 | Update Customer | M2 | `customers`, `addresses`, `contacts` | `customers`, `addresses`, `contacts` |
| UC-07 | Delete Customer | M2 | `customers` | `customers`, `addresses`, `contacts` |
| UC-08 | List Customers | M2 | `customers` | — |
| UC-09 | View Customer Details | M2 | `customers`, `addresses`, `contacts` | — |
| UC-10 | Create SIM | M3 | `phone_numbers` | `sims` |
| UC-11 | Activate SIM | M3 | `sims`, `phone_numbers` | `sims`, `sim_activations`, `sim_status_history` |
| UC-12 | Suspend SIM | M3 | `sims` | `sims`, `sim_status_history` |
| UC-13 | Block SIM | M3 | `sims` | `sims`, `sim_status_history` |
| UC-14 | Assign Phone Number | M3 | `phone_numbers`, `sims` | `sims`, `phone_numbers` |
| UC-15 | Create Package | M4 | — | `packages`, `package_features`, `package_prices`, `services` |
| UC-16 | Update Package | M4 | `packages` | `packages`, `package_features`, `package_prices` |
| UC-17 | Delete Package | M4 | `packages` | `packages` |
| UC-18 | List Packages | M4 | `packages` | — |
| UC-19 | Create Subscription | M5 | `customers`, `sims`, `packages` | `subscriptions` |
| UC-20 | Renew Subscription | M5 | `subscriptions`, `packages` | `subscriptions` |
| UC-21 | Cancel Subscription | M5 | `subscriptions` | `subscriptions` |
| UC-22 | Record Call | M6 | — | `usage_records` |
| UC-23 | Record Message | M6 | — | `usage_records` |
| UC-24 | Record Internet Usage | M6 | — | `usage_records` |
| UC-25 | Generate Invoice | M7 | `subscriptions`, `usage_records` | `invoices`, `invoice_items` |
| UC-26 | Calculate Amounts | M7 | `usage_records`, `subscriptions`, `packages` | `invoices`, `invoice_items` |
| UC-27 | Update Payment Status | M7 | `invoices` | `invoices` |
| UC-28 | Process Payment | M8 | `invoices` | `payments`, `transactions`, `invoices` |
| UC-29 | Recharge Balance | M8 | `customers` | `payments`, `transactions` |
| UC-30 | Check Transaction Status | M8 | `transactions` | — |
| UC-31 | Manage Towers | M9 | `towers`, `stations`, `devices`, `coverage_areas` | `towers`, `stations`, `devices`, `coverage_areas` |
| UC-32 | Manage Stations | M9 | `towers`, `stations` | `stations` |
| UC-33 | Manage Devices | M9 | `towers`, `stations`, `devices` | `devices` |
| UC-34 | Manage Coverage Areas | M9 | `towers`, `coverage_areas` | `coverage_areas` |
| UC-35 | Report Incident | M10 | `towers`, `stations`, `devices` | `incidents` |
| UC-36 | Assign Incident to Tower | M10 | `incidents`, `towers` | `incidents` |
| UC-37 | Track Incident | M10 | `incidents`, `towers`, `stations` | — |
| UC-38 | Create Ticket | M11 | `customers` | `tickets` |
| UC-39 | Assign Ticket | M11 | `tickets` | `tickets` |
| UC-40 | Reply to Ticket | M11 | `tickets` | `ticket_replies` |
| UC-41 | Transfer Ticket | M11 | `tickets` | `tickets` |
| UC-42 | Full Customer Lifecycle | M1–M12 | جميع الجداول | جميع الجداول |

---

## الجدول 4: Business Rules → Database Constraints

| BR ID | Rule Category | Module (M) | DB Table | Constraint Type |
|---|---|---|---|---|
| BR-01 | Unique National ID | M2 | `customers` | UNIQUE (`national_id`) |
| BR-02 | Unique Phone Number | M3 | `phone_numbers` | UNIQUE (`msisdn`) |
| BR-03 | SIM Status Validation | M3 | `sims` | CHECK (`status IN ('AVAILABLE', 'ACTIVE', 'SUSPENDED', 'BLOCKED')`) |
| BR-04 | SIM-Phone Binding | M3 | `sims`, `phone_numbers` | FK (`sim_id`) + CHECK (one-to-one) |
| BR-05 | Subscription State Machine | M5 | `subscriptions` | CHECK (`status IN ('ACTIVE', 'RENEWED', 'CANCELLED', 'EXPIRED')`) |
| BR-06 | Package Price Validation | M4 | `package_prices` | CHECK (`price > 0`) |
| BR-07 | Customer Required Fields | M2 | `customers` | NOT NULL (`name`, `national_id`, `phone`) |
| BR-08 | Customer Email Format | M2 | `customers` | CHECK (`email LIKE '%_@_%._%'`) |
| BR-09 | Address Required for Customer | M2 | `addresses` | NOT NULL (`street`, `city`) |
| BR-10 | Customer Status | M2 | `customers` | CHECK (`status IN ('ACTIVE', 'INACTIVE', 'SUSPENDED')`) |
| BR-11 | Customer Type | M2 | `customers` | CHECK (`customer_type IN ('INDIVIDUAL', 'CORPORATE')`) |
| BR-12 | SIM Activation Flow | M3 | `sim_activations` | NOT NULL (`sim_id`, `activation_date`, `activated_by`) |
| BR-13 | SIM Activation Status | M3 | `sim_activations` | CHECK (`status IN ('PENDING', 'COMPLETED', 'FAILED')`) |
| BR-14 | Phone Number Assignment | M3 | `phone_numbers` | FK (`customer_id`), CHECK (one active assignment) |
| BR-15 | SIM Block Validation | M3 | `sims` | CHECK (`status = 'BLOCKED'` → no reassignment) |
| BR-16 | Phone Number Status | M3 | `phone_numbers` | CHECK (`status IN ('AVAILABLE', 'ASSIGNED', 'BLOCKED')`) |
| BR-17 | SIM ICCID Uniqueness | M3 | `sims` | UNIQUE (`iccid`) |
| BR-18 | Phone Number Format | M3 | `phone_numbers` | CHECK (`msisdn ~ '^\d{10,12}$'`) |
| BR-19 | Package Name Uniqueness | M4 | `packages` | UNIQUE (`name`) |
| BR-20 | Package Validity Period | M4 | `packages` | CHECK (`validity_days > 0`) |
| BR-21 | Package Feature Count | M4 | `package_features` | CHECK (`feature_count >= 1`) |
| BR-22 | Subscription Uniqueness | M5 | `subscriptions` | UNIQUE (`customer_id`, `sim_id`) — one active subscription per SIM |
| BR-23 | Subscription Start Date | M5 | `subscriptions` | CHECK (`start_date <= end_date`) |
| BR-24 | Subscription Auto-Renew | M5 | `subscriptions` | CHECK (`auto_renew IN (true, false)`) |
| BR-25 | Subscription State Transition | M5 | `subscriptions` | State Machine: ACTIVE → RENEWED, ACTIVE → CANCELLED |
| BR-26 | Billing Cycle Validation | M5 | `subscriptions` | CHECK (`billing_cycle IN ('MONTHLY', 'QUARTERLY', 'YEARLY')`) |
| BR-27 | Package Not Expired | M5 | `packages`, `subscriptions` | CHECK (`start_date < package.expiry_date`) |
| BR-28 | Usage Record Required | M6 | `usage_records` | NOT NULL (`customer_id`, `usage_type`, `amount`, `timestamp`) |
| BR-29 | Usage Type Validation | M6 | `usage_records` | CHECK (`usage_type IN ('CALL', 'SMS', 'INTERNET')`) |
| BR-30 | Usage Amount Positive | M6 | `usage_records` | CHECK (`amount > 0`) |
| BR-31 | Invoice Number Uniqueness | M7 | `invoices` | UNIQUE (`invoice_number`) |
| BR-32 | Invoice Total Calculation | M7 | `invoices`, `invoice_items` | CHECK (`total_amount = SUM(invoice_items.amount)`) |
| BR-33 | Invoice Status | M7 | `invoices` | CHECK (`status IN ('PENDING', 'PAID', 'OVERDUE', 'CANCELLED')`) |
| BR-34 | Invoice Due Date | M7 | `invoices` | CHECK (`due_date > created_date`) |
| BR-35 | Invoice Item Price | M7 | `invoice_items` | CHECK (`price >= 0`) |
| BR-36 | Invoice Item Quantity | M7 | `invoice_items` | CHECK (`quantity > 0`) |
| BR-37 | Invoice Linked to Subscription | M7 | `invoices`, `subscriptions` | FK (`subscription_id`) — must exist active or recent |
| BR-38 | Payment Amount Validation | M8 | `payments` | CHECK (`amount > 0`) |
| BR-39 | Payment Method | M8 | `payments` | CHECK (`method IN ('CASH', 'BANK_TRANSFER', 'CREDIT_CARD', 'MOBILE_WALLET')`) |
| BR-40 | Payment Status | M8 | `payments` | CHECK (`status IN ('PENDING', 'COMPLETED', 'FAILED', 'REFUNDED')`) |
| BR-41 | Transaction Reference Uniqueness | M8 | `transactions` | UNIQUE (`reference_id`) |
| BR-42 | Payment-Invoice Link | M8 | `payments`, `invoices` | FK (`invoice_id`) — payment must link to existing invoice |
| BR-43 | Payment Idempotency | M8 | `payments` | UNIQUE (`reference_id`) — duplicate payments rejected |
| BR-44 | Transaction Amount Match | M8 | `payments`, `transactions` | CHECK (`payment.amount = transaction.amount`) |
| BR-45 | Tower Location Required | M9 | `towers` | NOT NULL (`latitude`, `longitude`, `region`) |
| BR-46 | Incident Severity Level | M10 | `incidents` | CHECK (`severity IN ('LOW', 'MEDIUM', 'HIGH', 'CRITICAL')`) |
| BR-47 | Incident Status State Machine | M10 | `incidents` | CHECK (`status IN ('REPORTED', 'ASSIGNED', 'IN_PROGRESS', 'RESOLVED', 'CLOSED')`) |
| BR-48 | Incident Linked to Tower | M10 | `incidents` | FK (`tower_id`) — must exist |
| BR-49 | Incident Resolution Time | M10 | `incidents` | CHECK (`resolved_date >= reported_date`) |
| BR-50 | Incident Severity Assignment | M10 | `incidents` | CRITICAL/HIGH must assign to senior engineer |
| BR-51 | Incident Device Link | M10 | `incidents`, `devices` | FK (`device_id`), optional |
| BR-52 | Ticket Number Uniqueness | M11 | `tickets` | UNIQUE (`ticket_number`) |
| BR-53 | Ticket Priority | M11 | `tickets` | CHECK (`priority IN ('LOW', 'MEDIUM', 'HIGH', 'URGENT')`) |
| BR-54 | Ticket Status | M11 | `tickets` | CHECK (`status IN ('OPEN', 'ASSIGNED', 'IN_PROGRESS', 'RESOLVED', 'CLOSED')`) |
| BR-55 | Ticket Assigned To | M11 | `tickets` | FK (`assigned_to`), must be Support Agent (ACT-07) |
| BR-56 | Ticket Reply Required | M11 | `ticket_replies` | NOT NULL (`ticket_id`, `reply_text`, `replied_by`, `timestamp`) |
| BR-57 | Ticket Transfer Validation | M11 | `tickets` | CHECK (`transferred_to` is a valid department) |
| BR-58 | Ticket Customer Link | M11 | `tickets` | FK (`customer_id`) — must exist |
| BR-59 | Notification Type | M12 | `notifications` | CHECK (`type IN ('SMS', 'EMAIL')`) |
| BR-60 | Notification Status | M12 | `notifications` | CHECK (`status IN ('SENT', 'FAILED', 'PENDING')`) |
| BR-61 | Notification Recipient | M12 | `notifications` | FK (`customer_id` or `user_id`) |
| BR-62 | User Role Assignment | M1 | `users`, `roles`, `user_roles` | Each user must have exactly one role |
| BR-63 | Permission Consistency | M1 | `role_permissions` | FK (`role_id`) must exist, FK (`permission_id`) must exist |
| BR-64 | JWT Expiration | M1 | `users`, `refresh_tokens` | CHECK (`expires_at > current_timestamp`) |
| BR-65 | Password Hashing | M1 | `users` | CHECK (`password_hash != plain_text_password`) |
| BR-66 | User Login Attempt Limit | M1 | `users` | CHECK (`failed_attempts < 5`) |
| BR-67 | Role Hierarchy | M1 | `roles` | Super Admin > Company Admin > Branch Manager > Employee |
| BR-68 | Full Lifecycle Data Integrity | M1–M12 | All | FK constraints must be satisfied across all services |
| BR-69 | Event Consistency | M1–M12 | All | Every event must have a corresponding state change |
| BR-70 | API Gateway Rate Limit | M1 | `users` | Rate limit per user per minute |
| BR-71 | RBAC Access Control | M1 | All | Every UC must have at least one role allowed |
| BR-72 | Audit Log Requirement | M1, M8 | All | Every write operation must be logged |
| BR-73 | Customer Created Event | M2 | `customers` | On insert → publish `CustomerCreated` event |
| BR-74 | Payment Completed Event | M8 | `payments` | On status='COMPLETED' → publish `PaymentCompleted` event |
| BR-75 | Invoice Generated Event | M7 | `invoices` | On insert → publish `InvoiceGenerated` event |
| BR-76 | SIM Activated Event | M3 | `sim_activations` | On status='COMPLETED' → publish `SIMActivated` event |
| BR-77 | Notification on Event | M12 | `notifications` | On every event → insert notification record |

---

## الجدول 5: Data Entities → Database Tables → IDs Stored

| Entity Group | Database | Table | Primary Key | Foreign Keys | Related IDs (Master) |
|---|---|---|---|---|---|
| Users (Identity) | Identity DB | `users` | `user_id` (UUID) | `role_id` | ACT-01 → ACT-08 (all actors have a user account) |
| Roles (Identity) | Identity DB | `roles` | `role_id` (UUID) | — | Role names map to ACT-01→ACT-08 permissions |
| Permissions (Identity) | Identity DB | `permissions` | `permission_id` (UUID) | — | UC-01 to UC-42 → permission codes |
| User_Roles (Identity) | Identity DB | `user_roles` | `user_role_id` (UUID) | `user_id`, `role_id` | Links ACT-IDs to user accounts |
| Refresh_Tokens (Identity) | Identity DB | `refresh_tokens` | `token_id` (UUID) | `user_id` | UC-04 (Refresh Token) |
| Customers | Customer DB | `customers` | `customer_id` (UUID) | — | UC-05 to UC-09, ACT-04, ACT-08 |
| Addresses | Customer DB | `addresses` | `address_id` (UUID) | `customer_id` | UC-05 (Create Customer) |
| Contacts | Customer DB | `contacts` | `contact_id` (UUID) | `customer_id` | UC-05 (Create Customer) |
| Customer_Documents | Customer DB | `customer_documents` | `doc_id` (UUID) | `customer_id` | UC-05 (Create Customer) |
| SIMs | SIM DB | `sims` | `sim_id` (UUID) | `customer_id`, `phone_number_id` | UC-10 to UC-14, ACT-04 |
| Phone_Numbers | SIM DB | `phone_numbers` | `phone_number_id` (UUID) | `customer_id` | UC-14 (Assign Phone Number) |
| SIM_Activations | SIM DB | `sim_activations` | `activation_id` (UUID) | `sim_id`, `user_id` | UC-11 (Activate SIM) |
| SIM_Status_History | SIM DB | `sim_status_history` | `history_id` (UUID) | `sim_id` | UC-11, UC-12, UC-13 |
| Packages | Product DB | `packages` | `package_id` (UUID) | — | UC-15 to UC-18, ACT-02 |
| Package_Features | Product DB | `package_features` | `feature_id` (UUID) | `package_id` | UC-15, UC-16 |
| Package_Prices | Product DB | `package_prices` | `price_id` (UUID) | `package_id` | UC-15, UC-16 |
| Services (Extra) | Product DB | `services` | `service_id` (UUID) | `package_id` | UC-15, UC-16 |
| Subscriptions | Subscription DB | `subscriptions` | `subscription_id` (UUID) | `customer_id`, `sim_id`, `package_id` | UC-19 to UC-21, ACT-04, ACT-08 |
| Usage_Records | Usage DB | `usage_records` | `usage_id` (UUID) | `customer_id`, `subscription_id` | UC-22 to UC-24 |
| Invoices | Billing DB | `invoices` | `invoice_id` (UUID) | `subscription_id`, `customer_id` | UC-25 to UC-27, ACT-05 |
| Invoice_Items | Billing DB | `invoice_items` | `item_id` (UUID) | `invoice_id` | UC-25, UC-26 |
| Payments | Payment DB | `payments` | `payment_id` (UUID) | `invoice_id`, `customer_id` | UC-28 to UC-30, ACT-05, ACT-08 |
| Transactions | Payment DB | `transactions` | `transaction_id` (UUID) | `payment_id`, `invoice_id` | UC-28 to UC-30 |
| Towers | Network DB | `towers` | `tower_id` (UUID) | — | UC-31 to UC-34, ACT-06 |
| Stations | Network DB | `stations` | `station_id` (UUID) | `tower_id` | UC-31, UC-32, ACT-06 |
| Devices | Network DB | `devices` | `device_id` (UUID) | `station_id` | UC-33, ACT-06 |
| Coverage_Areas | Network DB | `coverage_areas` | `area_id` (UUID) | `tower_id` | UC-34, ACT-06 |
| Incidents | Incident DB | `incidents` | `incident_id` (UUID) | `tower_id`, `station_id`, `device_id` | UC-35 to UC-37, ACT-06 |
| Tickets | Support DB | `tickets` | `ticket_id` (UUID) | `customer_id`, `assigned_to` (user_id) | UC-38 to UC-41, ACT-07, ACT-08 |
| Ticket_Replyies | Support DB | `ticket_replies` | `reply_id` (UUID) | `ticket_id`, `replied_by` (user_id) | UC-40, ACT-07 |
| Notifications | Notification DB | `notifications` | `notification_id` (UUID) | `customer_id` or `user_id` | EVT-01 to EVT-10, all event triggers |

---

## الجدول 6: End-to-End ID Flow (تتبع المعرفات عبر قاعدة البيانات)

| Step | Action | Source Table | Target Table | Key Used | Event Published |
|---|---|---|---|---|---|
| 1 | **Create Customer** | — | `customers` | `customer_id` (new UUID) | `CustomerCreated` (EVT-06) |
| 2 | **Create SIM** | `phone_numbers` | `sims` | `sim_id` (new UUID), `phone_number_id` | — |
| 3 | **Assign Phone Number** | `phone_numbers` | `sims` | `phone_number_id`, `sim_id` | — |
| 4 | **Activate SIM** | `sims` | `sim_activations`, `sim_status_history` | `sim_id`, `activation_id` | `SIMActivated` (EVT-04) |
| 5 | **Create Subscription** | `customers`, `sims`, `packages` | `subscriptions` | `subscription_id` (new UUID), `customer_id`, `sim_id`, `package_id` | `SubscriptionCreated` (EVT-03) |
| 6 | **Generate Invoice** | `subscriptions`, `usage_records` | `invoices`, `invoice_items` | `invoice_id` (new UUID), `subscription_id` | `InvoiceGenerated` (EVT-05) |
| 7 | **Process Payment** | `invoices` | `payments`, `transactions`, `invoices` (update) | `payment_id`, `transaction_id`, `invoice_id` | `PaymentCompleted` (EVT-01) |
| 8 | **Send Notification** | `notifications` | `notifications` | `notification_id` (new UUID), `customer_id` | — |
| 9 | **Record Usage** | — | `usage_records` | `usage_id` (new UUID), `customer_id`, `subscription_id` | — |
| 10 | **Report Incident** | `towers`, `stations` | `incidents` | `incident_id` (new UUID), `tower_id` | `IncidentReported` (EVT-02) |
| 11 | **Create Ticket** | `customers` | `tickets` | `ticket_id` (new UUID), `customer_id` | `TicketCreated` (EVT-07) |
| 12 | **Assign Ticket** | `tickets` | `tickets` (update) | `ticket_id`, `assigned_to` (user_id) | `TicketAssigned` (EVT-08) |
| 13 | **Resolve Incident** | `incidents` | `incidents` (update) | `incident_id`, `resolution` | `IncidentResolved` (EVT-09) |
| 14 | **Renew Subscription** | `subscriptions` | `subscriptions` (update) | `subscription_id` | `SubscriptionRenewed` (EVT-10) |

---

## الجدول 7: Events → Publishers → Subscribers → Payload

| EVT ID | Event Name | Publisher (M) | Subscribers (M) | Key Fields in Payload |
|---|---|---|---|---|
| EVT-01 | `PaymentCompleted` | M8 | M7, M2, M12 | `transactionId`, `amount`, `invoiceId`, `customerId` |
| EVT-02 | `IncidentReported` | M10 | M11, M9 | `incidentId`, `towerId`, `stationId`, `severity` |
| EVT-03 | `SubscriptionCreated` | M5 | M7, M12 | `subscriptionId`, `customerId`, `simId`, `packageId` |
| EVT-04 | `SIMActivated` | M3 | M12, M5 | `simId`, `iccid`, `msisdn`, `customerId` |
| EVT-05 | `InvoiceGenerated` | M7 | M12 | `invoiceId`, `customerId`, `totalAmount` |
| EVT-06 | `CustomerCreated` | M2 | M12 | `customerId`, `name`, `phone`, `email` |
| EVT-07 | `TicketCreated` | M11 | M12 | `ticketId`, `customerId`, `subject`, `priority` |
| EVT-08 | `TicketAssigned` | M11 | M12 | `ticketId`, `assignedAgent`, `status` |
| EVT-09 | `IncidentResolved` | M9 | M11, M10 | `incidentId`, `resolution`, `resolvedDate` |
| EVT-10 | `SubscriptionRenewed` | M5 | M7, M12 | `subscriptionId`, `newEndDate`, `amount` |

---

## الجدول 8: Objectives → Actors → Use Cases → Modules (الكامل)

| OBJ ID | Objective | Actors (ACT) | Use Cases (UC) | Modules (M) |
|---|---|---|---|---|
| OBJ-01 | Automate Telecom Operations | All 8 | UC-01 to UC-42 | M1–M12 |
| OBJ-02 | Customer Data Management | ACT-04, ACT-01, ACT-02, ACT-08 | UC-05 to UC-09 | M2 |
| OBJ-03 | Phone Numbers & SIM | ACT-04, ACT-01 | UC-10 to UC-14 | M3 |
| OBJ-04 | Packages & Services | ACT-02, ACT-01 | UC-15 to UC-18 | M4 |
| OBJ-05 | Subscriptions & Renewal | ACT-04, ACT-08 | UC-19 to UC-21 | M5 |
| OBJ-06 | Usage Recording | System | UC-22 to UC-24 | M6 |
| OBJ-07 | Invoices & Billing | ACT-05, ACT-04 | UC-25 to UC-27 | M7 |
| OBJ-08 | Payments & Recharge | ACT-05, ACT-08 | UC-28 to UC-30 | M8 |
| OBJ-09 | Network Towers & Stations | ACT-06, ACT-01, ACT-02 | UC-31 to UC-34 | M9 |
| OBJ-10 | Incidents Tracking | ACT-06, ACT-08 | UC-35 to UC-37 | M10 |
| OBJ-11 | Support Tickets | ACT-07, ACT-08 | UC-38 to UC-41 | M11 |
| OBJ-12 | Notifications | All (as receivers) | Events: EVT-01 to EVT-10 | M12 |
| OBJ-13 | Dashboards & Reports | All | UC-08, UC-18, UC-37 (Reports) | M1–M12 |
| OBJ-14 | RBAC | ACT-01 to ACT-07 | UC-01 to UC-09 (all Auth UCs) | M1 |
| OBJ-15 | System Integration | All | UC-42, All Events | M1–M12 |
| OBJ-16 | Software Architecture | All | UC-42, DFD L0/L1, Flows | M1–M12 |

---

## How to Use This Card

### للمطور (Backend Developer)

1. **ابدأ بالجدول 3 (Use Cases → Modules → DB Tables)** — ابحث عن الـ Use Case الذي تريد تنفيذه.
2. **حدد الجداول التي ستحتاج قراءتها وكتابتها.**
3. **اعرف المعرفات (IDs) من الجدول 1** — مثلاً UC-19 يابعة لـ M5 (Subscription Service).
4. **اعرف الـ Foreign Keys من الجدول 5** — مثلاً `subscriptions` لها FK على `customer_id`, `sim_id`, `package_id`.
5. **تحقق من قواعد العمل من الجدول 4** — مثلاً BR-22: كل SIM يمكن أن يكون له اشتراك واحد فعال فقط.
6. **ابحث عن الأحداث المنشورة من الجدول 7** — مثلاً عند إنشاء اشتراك → `SubscriptionCreated` event يُنشر.

### لمهندس قواعد البيانات (DB Engineer)

1. **ابدأ بالجدول 2 (Modules → Database Tables)** — أنشئ 12 قاعدة بيانات منفصلة.
2. **استخدم الجدول 5 (Data Entities → DB Tables → IDs)** لتحديد Primary Keys و Foreign Keys.
3. **طبّق قيود الجدول 4 (Business Rules → DB Constraints)** — UNIQUE, CHECK, FK.
4. **اتبع الجدول 6 (End-to-End ID Flow)** لترتيب إنشاء الجداول والعلاقات.
5. **تأكد من الـ Event-Driven Architecture** باستخدام الجدول 7.

### للمختبر (QA/Tester)

1. **استخدم الجدول 1** لتحديد نطاق الاختبار (كل IDs).
2. **استخدم الجدول 3** لكتابة Test Cases لكل Use Case.
3. **استخدم الجدول 4** لكتابة Negative Test Cases لكل قاعدة عمل.
4. **استخدم الجدول 6** لكتابة End-to-End Test Scenarios.
5. **استخدم الجدول 5** لتحديد البيانات المطلوبة لكل اختبار.
6. **تحقق من الـ Events** من الجدول 7 — تأكد من نشر الأحداث الصحيحة.

---

> **ملاحظة:** هذه البطاقة هي المرجع الأساسي لربط جميع عناصر المشروع. يُنصح بنسخها واستخدامها كمرجع في Phase 5 (Architecture) و Phase 6 (Implementation).
>
> **الحالة:** Confirmed
> **آخر تحديث:** 2026-09-26
> **الإصدار:** 1.0
