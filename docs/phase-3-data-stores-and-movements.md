# مخازن البيانات وحركة البيانات بين الخدمات

## Data Stores and Data Movements Between Services

## المقدمة
وثيقة تفصيلية لمخازن البيانات وحركة البيانات بين خدمات نظام إدارة شركات الاتصالات، مبنية على المخرجات الموثقة في `docs/system-data-entities.md` و `docs/phase-3-flow-of-action.md` و `docs/phase-3-flow-of-event.md`.

---

## الجزء الأول — مخازن البيانات (Data Stores)

### الجدول التفصيلي لمخازن البيانات

| # | الخدمة | قاعدة البيانات | الجداول الرئيسية | الوصف |
|---|---|---|---|---|
| 1 | Identity Service | Identity DB | users, roles, permissions, user_roles, role_permissions, refresh_tokens | بيانات المستخدمين، الأدوار، الصلاحيات، رموز الجلسات |
| 2 | Customer Service | Customer DB | customers, addresses, contacts, customer_documents | بيانات العملاء الأساسية، العناوين، جهات الاتصال، المستندات |
| 3 | SIM & Number Service | SIM DB | sims, phone_numbers, sim_activations, sim_status_history | سجلات شرائح SIM، أرقام الهواتف، سجلات التفعيل، تاريخ الحالة |
| 4 | Product / Package Service | Product DB | packages, package_features, package_prices, services | الباقات، المميزات، الأسعار، الخدمات الإضافية |
| 5 | Subscription Service | Subscription DB | subscriptions | سجلات الاشتراكات، ربط العميل بالرقم والباقة |
| 6 | Usage Service | Usage DB | usage_records (calls, messages, internet) | سجلات المكالمات، الرسائل، استهلاك الإنترنت |
| 7 | Billing Service | Billing DB | invoices, invoice_items | الفواتير، بنود الفاتورة |
| 8 | Payment Service | Payment DB | payments, transactions | عمليات الدفع، المعاملات المالية |
| 9 | Network Service | Network DB | towers, stations, devices, coverage_areas | الأبراج، المحطات، الأجهزة، مناطق التغطية |
| 10 | Incident Service | Incident DB | incidents | سجلات الأعطال والبلاغات |
| 11 | Support Service | Support DB | tickets, ticket_replies | تذاكر الدعم، ردود التذاكر |
| 12 | Notification Service | Notification DB | notifications | سجلات الإشعارات (SMS/Email) |

### وصف مفصل لكل مخزن بيانات

#### 1. Identity DB
- **الغرض:** إدارة المصادقة والتفويض لجميع المستخدمين
- **الجداول الرئيسية:**
  - `users`: بيانات المستخدمين (اسم، بريد إلكتروني، كلمة مرور مشفرة، الدور)
  - `roles`: الأدوار المختلفة (Super Admin, Company Admin, Branch Manager, إلخ)
  - `permissions`: الصلاحيات المرتبطة بكل دور
  - `user_roles`: ربط المستخدمين بالأدوار
  - `role_permissions`: ربط الأدوار بالصلاحيات
  - `refresh_tokens`: رموز تحديث الجلسات
- **نوع الوصول:** قراءة وكتابة متكررة، متطلب أداء عالي

#### 2. Customer DB
- **الغرض:** تخزين وإدارة بيانات العملاء الأساسية
- **الجداول الرئيسية:**
  - `customers`: البيانات الأساسية للعميل (الاسم، الهوية، الحالة)
  - `addresses`: عناوين العملاء
  - `contacts`: جهات الاتصال
  - `customer_documents`: المستندات المطلوبة للعميل
- **نوع الوصول:** قراءة وكتابة، متطلب اتساق عالي

#### 3. SIM DB
- **الغرض:** إدارة شرائح SIM وأرقام الهواتف
- **الجداول الرئيسية:**
  - `sims`: سجلات شرائح SIM (ICCID، الحالة، التاريخ)
  - `phone_numbers`: أرقام الهواتف (MSISDN، الحالة، العميل المرتبط)
  - `sim_activations`: سجلات تفعيل SIM
  - `sim_status_history`: تاريخ تغيرات حالة SIM
- **نوع الوصول:** قراءة وكتابة، متطلب أداء عالي

#### 4. Product DB
- **الغرض:** إدارة الباقات والخدمات وأسعارها
- **الجداول الرئيسية:**
  - `packages`: الباقات المتاحة (الاسم، الوصف، النوع)
  - `package_features`: مميزات كل باقة
  - `package_prices`: أسعار الباقات
  - `services`: الخدمات الإضافية المتاحة
- **نوع الوصول:** قراءة متكررة، كتابة أقل

#### 5. Subscription DB
- **الغرض:** إدارة اشتراكات المشتركين
- **الجداول الرئيسية:**
  - `subscriptions`: سجلات الاشتراكات (العميل، الرقم، الباقة، تاريخ البدء، الحالة، التجديد)
- **نوع الوصول:** قراءة وكتابة، متطلب اتساق عالي

#### 6. Usage DB
- **الغرض:** تسجيل استخدام خدمات الاتصالات
- **الجداول الرئيسية:**
  - `usage_records`: سجلات المكالمات والرسائل واستهلاك الإنترنت (نوع الخدمة، المدة، المشترك، التاريخ)
- **نوع الوصول:** كتابة متكررة، قراءة لإنشاء الفواتير

#### 7. Billing DB
- **الغرض:** إدارة الفواتير وبنودها
- **الجداول الرئيسية:**
  - `invoices`: الفواتير (رقم الفاتورة، العميل، المبلغ الإجمالي، الحالة، تاريخ الاستحقاق)
  - `invoice_items`: بنود الفاتورة (الوصف، الكمية، السعر، الإجمالي)
- **نوع الوصول:** قراءة وكتابة، متطلب اتساق عالي

#### 8. Payment DB
- **الغرض:** إدارة عمليات الدفع والمعاملات
- **الجداول الرئيسية:**
  - `payments`: عمليات الدفع (رقم العملية، المبلغ، الطريقة، الحالة)
  - `transactions`: المعاملات المالية التفصيلية
- **نوع الوصول:** قراءة وكتابة، متطلب أمان عالي

#### 9. Network DB
- **الغرض:** إدارة الأبراج والمحطات والأجهزة
- **الجداول الرئيسية:**
  - `towers`: الأبراج (الموقع، السعة، الحالة)
  - `stations`: المحطات
  - `devices`: الأجهزة (النوع، الحالة، البرج المرتبط)
  - `coverage_areas`: مناطق التغطية
- **نوع الوصول:** قراءة وكتابة، متطلب أداء متوسط

#### 10. Incident DB
- **الغرض:** تسجيل ومتابعة الأعطال والبلاغات
- **الجداول الرئيسية:**
  - `incidents`: سجلات الأعطال (الرقم، الوصف، الشدة، الحالة، البرج المرتبط، تاريخ الإبلاغ)
- **نوع الوصول:** قراءة وكتابة

#### 11. Support DB
- **الغرض:** إدارة شكاوى العملاء والتذاكر
- **الجداول الرئيسية:**
  - `tickets`: التذاكر (الرقم، العميل، العنوان، الوصف، الأولوية، الحالة)
  - `ticket_replies`: ردود التذاكر (التذكرة، الرد، الموظف، التاريخ)
- **نوع الوصول:** قراءة وكتابة

#### 12. Notification DB
- **الغرض:** سجلات الإشعارات المرسلة
- **الجداول الرئيسية:**
  - `notifications`: الإشعارات (النوع، المستقبل، المحتوى، الحالة، التاريخ)
- **نوع الوصول:** كتابة متكررة، قراءة للمراجعة

---

## الجزء الثاني — حركة البيانات (Data Movements)

### أ) حركة البيانات عبر API المباشر (Synchronous)

| # | الخدمة المصدر | الخدمة المستهدفة | البيانات المُبادلة | الغرض |
|---|---|---|---|---|
| 1 | Identity Service | جميع الخدمات | Auth Token, User Roles, Permissions | التحقق من هوية المستخدم والصلاحيات |
| 2 | Customer Service | Subscription Service | Customer ID, Customer Details | التحقق من وجود العميل عند إنشاء اشتراك |
| 3 | Customer Service | Billing Service | Customer ID, Customer Info | إنشاء فاتورة مرتبطة بالعميل |
| 4 | SIM & Number Service | Subscription Service | SIM ID, SIM Status, Phone Number | التحقق من تفعيل SIM عند إنشاء اشتراك |
| 5 | Product/Package Service | Subscription Service | Package ID, Package Details, Price | ربط الباقة بالاشتراك |
| 6 | Usage Service | Billing Service | Call Records, Message Records, Internet Usage | حساب المبالغ المستحقة |
| 7 | Subscription Service | Billing Service | Subscription Details, Billing Cycle | إنشاء حساب فوترة |
| 8 | Billing Service | Payment Service | Invoice ID, Amount, Due Date | تنفيذ عملية الدفع |
| 9 | Payment Service | Billing Service | Payment Confirmation, Transaction ID | تحديث حالة الفاتورة |
| 10 | Billing Service | Invoice DB (داخلي) | Invoice Data, Invoice Items | إنشاء سجل الفاتورة |
| 11 | Incident Service | Network Service | Incident ID, Tower ID, Description | ربط العطل بالبرج المعني |
| 12 | Identity Service | Customer Service | User Permissions | التحقق من صلاحية إنشاء/تعديل عميل |
| 13 | Network Service | Incident Service | Tower/Station Status | التحقق من وجود البرج عند الإبلاغ |

### ب) حركة البيانات عبر الأحداث (Asynchronous via RabbitMQ)

| # | اسم الحدث | الناشر | المشتركون | البيانات المُرسَلة (Payload) | الغرض |
|---|---|---|---|---|---|
| 1 | `PaymentCompleted` | Payment Service | Billing Service, Customer Service, Notification Service | `{ transactionId, amount, paymentStatus, invoiceId, customerId, timestamp }` | تحديث حالة الفاتورة، تحديث الرصيد، إرسال إشعار الدفع |
| 2 | `IncidentReported` | Incident Service | Support Service, Network Service | `{ incidentId, towerId, stationId, description, severity, status, reportedDate, reportedBy }` | إنشاء تذكرة دعم، تخصيص مهندس شبكة |
| 3 | `SubscriptionCreated` | Subscription Service | Billing Service, Notification Service | `{ subscriptionId, customerId, simId, phoneNumber, packageId, startDate, billingCycle, status }` | إنشاء حساب فوترة، إرسال إشعار ترحيبي |
| 4 | `SIMActivated` | SIM & Number Service | Notification Service, Subscription Service | `{ simId, iccid, msisdn, activationDate, customerId, status }` | إرسال إشعار التفعيل، تفعيل إمكانية الاشتراك |
| 5 | `InvoiceGenerated` | Billing Service | Notification Service | `{ invoiceId, customerId, subscriptionId, totalAmount, items, dueDate, status, createdDate }` | إرسال إشعار الفاتورة للمشترك |
| 6 | `CustomerCreated` | Customer Service | Notification Service | `{ customerId, name, address, phone, email, customerType, registrationDate, status }` | إرسال إشعار ترحيبي للعميل الجديد |
| 7 | `TicketCreated` | Support Service | Notification Service | `{ ticketId, customerId, subject, priority, status, createdDate }` | إرسال إشعار تأكيد التذكرة |
| 8 | `TicketAssigned` | Support Service | Notification Service | `{ ticketId, assignedAgent, status }` | إشعار بتعيين التذكرة لموظف |
| 9 | `IncidentResolved` | Network Service | Support Service, Incident Service | `{ incidentId, resolution, resolvedDate, status }` | تحديث حالة العطل، إغلاق التذكرة |
| 10 | `SubscriptionRenewed` | Subscription Service | Billing Service, Notification Service | `{ subscriptionId, newEndDate, amount, status }` | تحديث الفوترة، إرسال إشعار التجديد |

---

## ملخص التكامل بين الخدمات

### لماذا تم اختيار Event-Driven Architecture لبعض التدفقات؟

تم اختيار Event-Driven Architecture (MQ) للتدفقات التالية لأنها تتطلب:
1. **فصل الخدمات (Decoupling):** حدث مثل `PaymentCompleted` يؤثر على 3 خدمات مختلفة (Billing, Customer, Notification). لو استُخدم API مباشر، لكانت كل خدمة تحتاج دعوة منفصلة، مما يزيد الترابط ويقلل المرونة.
2. **الموثوقية (Reliability):** RabbitMQ يضمن وصول الحدث لجميع المشتركين حتى لو كانت إحدى الخدمات غير متاحة مؤقتًا.
3. **قابلية التوسع (Scalability):** يمكن إضافة مشتركين جدد لحدث دون تعديل الناشر.
4. **التسلسل الزمني (Ordering):** الأحداث مثل `InvoiceGenerated` يجب أن تُرسل فقط بعد إتمام `Billing`.

### لماذا تم اختيار API المباشر لتدفقات أخرى؟

تم اختيار API المباشر (Synchronous) للتدفقات التالية لأنها تتطلب:
1. **الاستجابة الفورية (Real-time Response):** مثل تسجيل الدخول (Identity Service) — يجب الحصول على نتيجة فورية قبل المتابعة.
2. **الاتساق الصارم (Strong Consistency):** مثل إنشاء اشتراك — يجب التحقق من العميل وSIM قبل المتابعة، وهذا يتطلب استجابة فورية.
3. **الاعتماد المتبادل المباشر (Direct Dependency):** مثل استدعاء Usage Service من Billing — يجب حساب المبالغ فورًا لإصدار الفاتورة.
4. **بساطة التنفيذ:** في الحالات التي لا تتطلب فصلًا بين الخدمات، فإن API المباشر أبسط وأسرع.

### كيف يخدم ذلك متطلبات مادة "تكامل وعمارة الأنظمة"؟

1. **نمط التكامل المختلط (Hybrid Integration):** يجمع بين نمطين مختلفين من التكامل (Synchronous و Asynchronous) بناءً على طبيعة كل عملية، مما يُظهر فهمًا عميقًا لمبادئ عمارة الأنظمة.
2. **المبدأ الأساسي (Separation of Concerns):** كل خدمة مسؤولة عن بياناتها الخاصة (Database per Service)، مما يضمن فصل المهام ويزيد من قابلية الصيانة.
3. **مبدأ المرونة (Flexibility):** Event-Driven يوفر مرونة أعلى في إضافة خدمات جديدة دون تعديل الخدمات الحالية.
4. **مبدأ الموثوقية (Reliability):** RabbitMQ يضمن عدم فقدان الأحداث حتى مع وجود أخطاء مؤقتة في الخدمات المستقبلة.
5. **مبدأ الأداء (Performance):** API المباشر يوفر أداءً أفضل للتدفقات الحرجة، بينما MQ يعالج التدفقات غير الحرجة بكفاءة أعلى.
6. **التوافق مع المعمارية المُوثقة:** التدفقات تتوافق تمامًا مع بنية النظام الموثقة: Microservices + API Gateway + Event-Driven Architecture.

---

> **تاريخ التحديث:** 2026-09-26
> **المصدر:** docs/system-data-entities.md و docs/phase-3-flow-of-action.md و docs/phase-3-flow-of-event.md و docs/phase-3-dfd-level-1.md
> **عدد مخازن البيانات:** 12
> **عدد تدفقات API المباشرة:** 13
> **عدد تدفقات الأحداث (Events):** 10
> **الحالة:** Confirmed
