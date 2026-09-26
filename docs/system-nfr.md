# المتطلبات غير الوظيفية (NFR)

## Non-Functional Requirements

## المقدمة
تلتزم هذه الوثيقة بجميع المتطلبات غير الوظيفية المستخرجة من ملف التحليل الأولي للمشروع. تم تصنيف المتطلبات وفقًا لمعايير SKILL-14 (Requirements Engineering) وهيكل المراحل الموثقة في `docs/implement-plan.md`.

> **تاريخ الإنشاء:** 2026-09-26
> **المصدر:** docs/project-objectives.md, docs/project-scope.md, docs/system-modules.md, docs/system-operations.md
> **الحالة:** Confirmed

---

## أ) الأداء (Performance)

### NFR-01: أزمنة استجابة المصادقة
- **الفئة:** Performance
- **الوصف:** يجب أن تستغرق عمليات المصادقة (Login, Register, Refresh Token) وقتًا قصيرًا جدًا لضمان تجربة مستخدم سلسة.
- **الأولوية:** High
- **المعيار القابل للقياس:** 90% من عمليات المصادقة تُنجز خلال 500ms
- **المصدر:** docs/system-operations.md → Identity Service Operations

### NFR-02: أزمنة استجابة عمليات العملاء
- **الفئة:** Performance
- **الوصف:** يجب أن تكون عمليات العملاء (Create, Update, Delete, List, View) سريعة الاستجابة لتجنب انتظار المستخدمين.
- **الأولوية:** High
- **المعيار القابل للقياس:** 90% من عمليات العملاء تُنجز خلال 500ms
- **المصدر:** docs/system-operations.md → Customer Service Operations (5 عمليات)

### NFR-03: أزمنة استجابة عمليات SIM
- **الفئة:** Performance
- **الوصف:** يجب أن تكون عمليات SIM (Create, Activate, Suspend, Block, Assign Number) سريعة بسبب أهميتها في تدفق الاشتراك.
- **الأولوية:** High
- **المعيار القابل للقياس:** 90% من عمليات SIM تُنجز خلال 500ms
- **المصدر:** docs/system-operations.md → SIM & Number Service Operations

### NFR-04: أزمنة استجابة عمليات الفوترة
- **الفئة:** Performance
- **الوصف:** يجب أن تكون عمليات الفوترة (Generate Invoice, Calculate Amounts) سريعة حتى يتمكن المحاسب من إنشاء الفواتير بكفاءة.
- **الأولوية:** High
- **المعيار القابل للقياس:** 90% من عمليات الفوترة تُنجز خلال 500ms
- **المصدر:** docs/system-operations.md → Billing Service Operations

### NFR-05: أزمنة استجابة عمليات الدفع
- **الفئة:** Performance
- **الوصف:** يجب أن تكون عمليات الدفع (Process Payment, Recharge) سريعة ومؤكدة فورًا نظرًا لأهمية المعاملات المالية.
- **الأولوية:** High
- **المعيار القابل للقياس:** 90% من عمليات الدفع تُنجز خلال 500ms
- **المصدر:** docs/system-operations.md → Payment Service Operations

---

## ب) قابلية التوسع (Scalability)

### NFR-06: التوسع الأفقي (Horizontal Scaling)
- **الفئة:** Scalability
- **الوصف:** يجب أن يكون النظام قادرًا على إضافة خدمات جديدة (Instances) دون تغيير في البنية التحتية لاستيعاب عدد متزايد من المستخدمين.
- **الأولوية:** High
- **المعيار القابل للقياس:** يمكن إضافة Service Instance جديد خلال دقائق دون توقف النظام
- **المصدر:** docs/system-modules.md → 11 Microservices مستقلة

### NFR-07: التوسع العمودي (Vertical Scaling)
- **الفئة:** Scalability
- **الوصف:** يجب أن يكون لكل خدمة قاعدة بيانات مستقلة مما يسمح بترقية الموارد (CPU, RAM) لكل خدمة بشكل مستقل.
- **الأولوية:** High
- **المعيار القابل للقياس:** كل خدمة لها قاعدة بيانات مخصصة يمكن ترقيتها بشكل منفصل
- **المصدر:** docs/phase-3-data-stores-and-movements.md → 12 Data Store مستقل

### NFR-08: إضافة خدمة جديدة دون إعادة بناء النظام
- **الفئة:** Scalability
- **الوصف:** يجب أن يكون بإمكان إضافة وحدة/خدمة جديدة (مثل Service جديد) دون الحاجة لإعادة بناء أو تعديل الخدمات الحالية.
- **الأولوية:** High
- **المعيار القابل للقياس:** إضافة Service جديد يتطلب فقط إنشاء قاعدة بيانات جديدة + تسجيل في API Gateway
- **المصدر:** docs/project-objectives.md → الهدف 15 (System Integration) و الهدف 16 (Architecture)

---

## ج) التوفر (Availability)

### NFR-09: عزل الأعطال (Fault Isolation)
- **الفئة:** Availability
- **الوصف:** تعطل خدمة واحدة لا يؤدي إلى توقف النظام كاملًا. كل خدمة تعمل بشكل مستقل ومعزولة عن الخدمات الأخرى.
- **الأولوية:** High
- **المعيار القابل للقياس:** عند تعطل أي Service واحد، تبقى الـ 10 خدمات الأخرى تعمل بشكل طبيعي
- **المصدر:** docs/project-scope.md → Microservices Architecture، و docs/phase-3-flow-of-event.md → Event-Driven مع RabbitMQ

### NFR-10: نسبة التوفر المستهدفة
- **الفئة:** Availability
- **الوصف:** نسبة توفر النظام المستهدفة لضمان استمرارية العمليات الإدارية.
- **الأولوية:** High
- **المعيار القابل للقياس:** نسبة توفر 99.5% (أقل من 43.8 ساعة توقف سنويًا)
- **المصدر:** docs/project-objectives.md → أتمتة العمليات الإدارية

### NFR-11: آليات التعافي من الفشل (Failure Recovery)
- **الفئة:** Availability
- **الوصف:** يجب أن يحتوي النظام على آليات للتعافي التلقائي من الأعطال بما في ذلك إعادة المحاولة (Retry) وفصل الدائرة (Circuit Breaker).
- **الأولوية:** High
- **المعيار القابل للقياس:** التعافي التلقائي خلال 30 ثانية من فشل الخدمة
- **المصدر:** docs/phase-3-flow-of-event.md → RabbitMQ Message Broker يضمن التسليم

---

## د) الأمان (Security)

### NFR-12: JWT Authentication
- **الفئة:** Security
- **الوصف:** يجب استخدام JWT (JSON Web Tokens) للمصادقة على جميع المستخدمين. كل طلب API يجب أن يتضمن token صالح.
- **الأولوية:** High
- **المعيار القابل للقياس:** 100% من الطلبات تتطلب JWT token صالح
- **المصدر:** docs/system-modules.md → Identity Service (JWT، الجلسات) و docs/system-operations.md → Login, Refresh Token

### NFR-13: Password Hashing
- **الفئة:** Security
- **الوصف:** يجب تشفير كلمات المرور باستخدام bcrypt أو argon2 قبل تخزينها في قاعدة البيانات. لا يُخزن أي كلمة مرور بصيغة نصية واضحة.
- **الأولوية:** High
- **المعيار القابل للقياس:** جميع كلمات المرور مخزنة بصيغة hash (bcrypt/argon2)
- **المصدر:** docs/system-modules.md → Identity Service (تسجيل دخول، كلمات مرور)

### NFR-14: RBAC (Role-Based Access Control)
- **الفئة:** Security
- **الوصف:** يجب تطبيق نظام صلاحيات مختلف للمستخدمين بناءً على أدوارهم. كل مستخدم لديه دور محدد يحدد ما يمكنه فعله.
- **الأولوية:** High
- **المعيار القابل للقياس:** 8 أدوار محددة (Super Admin, Company Admin, Branch Manager, Customer Service Employee, Accountant, Network Engineer, Support Agent, Customer) — كل دور له صلاحيات مختلفة
- **المصدر:** docs/system-actors.md → 8 Actors و docs/project-objectives.md → الهدف 14 (RBAC)

### NFR-15: API Authorization
- **الفئة:** Security
- **الوصف:** كل نقطة نهاية API يجب أن تتطلب صلاحيات صريحة. لا يمكن لأي مستخدم الوصول إلى موارد لا تخصه.
- **الأولوية:** High
- **المعيار القابل للقياس:** 100% من الـ 42 Use Case لها صلاحيات محددة
- **المصدر:** docs/phase-2-use-cases-high-level.md → كل UC محدد بالأدوار المسموحة

### NFR-16: Input Validation
- **الفئة:** Security
- **الوصف:** يجب التحقق من صحة جميع المدخلات قبل معالجتها لمنع الهجمات (SQL Injection, XSS, إلخ).
- **الأولوية:** High
- **المعيار القابل للقياس:** جميع الطلبات تمر بالتحقق قبل المعالجة
- **المصدر:** docs/phase-2-detailed-scenarios.md → كل سيناريو يفحص صلاحية البيانات

### NFR-17: Rate Limiting
- **الفئة:** Security
- **الوصف:** يجب تحديد عدد الطلبات المسموحة لكل مستخدم/API key في فترة زمنية محددة لمنع هجمات حجب الخدمة (DDoS).
- **الأولوية:** Medium
- **المعيار القابل للقياس:** عدد الطلبات محدود لكل دقيقة لكل مستخدم
- **المصدر:** docs/system-modules.md → Identity Service (JWT, Sessions)

### NFR-18: Audit Logs
- **الفئة:** Security
- **الوصف:** يجب تسجيل جميع الأحداث الأمنية (تسجيل الدخول، تغيير الصلاحيات، العمليات المالية) في سجلات تدقيق.
- **الأولوية:** High
- **المعيار القابل للقياس:** جميع الأحداث الحرجة مسجلة في Audit Log
- **المصدر:** docs/project-scope.md → حدود النظام و docs/project-objectives.md → الهدف 8 (الدفع) و الهدف 14 (RBAC)

### NFR-19: CORS Configuration
- **الفئة:** Security
- **الوصف:** يجب تكوين CORS (Cross-Origin Resource Sharing) بشكل صحيح للسماح فقط بالنطاقات المصرح بها بالوصول إلى API.
- **الأولوية:** Medium
- **المعيار القابل للقياس:** CORS مُفعّل فقط للنطاقات المصرح بها
- **المصدر:** docs/project-scope.md → حدود النظام و docs/phase-3-dfd-level-0.md → API Gateway

### NFR-20: Secure HTTP Headers
- **الفئة:** Security
- **الوصف:** يجب تفعيل رؤوس HTTP الآمنة (Content-Security-Policy, X-Content-Type-Options, X-Frame-Options, Strict-Transport-Security).
- **الأولوية:** Medium
- **المعيار القابل للقياس:** جميع الاستجابات تحتوي على رؤوس HTTP آمنة
- **المصدر:** docs/project-scope.md → حدود النظام

### NFR-21: Refresh Tokens
- **الفئة:** Security
- **الوصف:** يجب استخدام Refresh Tokens لتمديد الجلسات دون الحاجة لإعادة تسجيل الدخول. يجب أن تكون Refresh Tokens ذات عمر محدود وآمنة.
- **الأولوية:** High
- **المعيار القابل للقياس:** Refresh Token له تاريخ انتهاء محدد ويمكن تحديثه (Refresh Token)
- **المصدر:** docs/system-modules.md → Identity Service (Refresh Token) و docs/system-operations.md → Refresh Token operation

### NFR-22: Centralized Error Handling
- **الفئة:** Security
- **الوصف:** يجب أن يحتوي النظام على آلية معالجة أخطاء موحدة لإخفاء تفاصيل الأخطاء الداخلية عن المستخدمين ومنع تسريب المعلومات الحساسة.
- **الأولوية:** Medium
- **المعيار القابل للقياس:** جميع الأخطاء تُعالج عبر Error Handler موحد
- **المصدر:** docs/project-scope.md → حدود النظام

### NFR-23: حماية بيانات العملاء والمعاملات
- **الفئة:** Security
- **الوصف:** يجب حماية جميع بيانات العملاء والمعاملات المالية من الوصول غير المصرح به. تشمل التشفير أثناء التخزين والنقل.
- **الأولوية:** High
- **المعيار القابل للقياس:** جميع بيانات العملاء والمعاملات محمية بالتشفير
- **المصدر:** docs/project-objectives.md → إدارة بيانات العملاء (الهدف 2)، إدارة الدفع (الهدف 8)

---

## هـ) قابلية الصيانة (Maintainability)

### NFR-24: استقلالية الخدمات
- **الفئة:** Maintainability
- **الوصف:** كل خدمة مستقلة قابلة للتطوير والصيانة دون التأثير على الخدمات الأخرى. قاعدة بيانات واحدة لكل خدمة (Database per Service).
- **الأولوية:** High
- **المعيار القابل للقياس:** 11 خدمة مستقلة، لكل منها قاعدة بيانات مخصصة
- **المصدر:** docs/system-modules.md → 11 Microservices و docs/phase-3-data-stores-and-movements.md → 12 Data Store مستقل

### NFR-25: كود نظيف ومنظم
- **الفئة:** Maintainability
- **الوصف:** يجب أن يتبع الكود معايير تنظيم واضحة مع استخدام أنماط تصميم مناسبة وتسمية موحدة.
- **الأولوية:** Medium
- **المعيار القابل للقياس:** كود منظم حسب الأنماط المناسبة لكل خدمة
- **المصدر:** docs/project-scope.md → محاكاة إدارية و docs/implement-plan.md → 7 مراحل تنفيذ

### NFR-26: توثيق كافٍ
- **الفئة:** Maintainability
- **الوصف:** يجب أن يكون كل خدمة موثقة بشكل كافٍ يشمل: وصف الوظيفة، نقاط النهاية (API)، هيكل قاعدة البيانات، والعمليات المتكاملة.
- **الأولوية:** High
- **المعيار القابل للقياس:** 11 خدمة موثقة بالكامل في docs/
- **المصدر:** docs/system-modules.md و docs/phase-3-data-stores-and-movements.md و docs/phase-3-flow-of-action.md

### NFR-27: اختبارات آلية
- **الفئة:** Maintainability
- **الوصف:** يجب وجود اختبارات آلية لكل خدمة تشمل: اختبارات الوحدة (Unit Tests) واختبارات التكامل (Integration Tests).
- **الأولوية:** High
- **المعيار القابل للقياس:** اختبارات آلية موجودة لكل الخدمات الـ 11
- **المصدر:** docs/implement-plan.md → Phase 7 (Testing, Audit & Final Review)

---

## و) الموثوقية (Reliability)

### NFR-28: عدم فقدان بيانات العمليات المالية
- **الفئة:** Reliability
- **الوصف:** يجب عدم فقدان أي بيانات للعمليات المالية (Payments, Invoices, Transactions). كل عملية يجب أن تُحفظ بشكل دائم وآمن.
- **الأولوية:** High
- **المعيار القابل للقياس:** 0% فقدان بيانات مالية — جميع العمليات محفوظة في قاعدة البيانات
- **المصدر:** docs/project-objectives.md → الهدف 7 (الفواتير) و الهدف 8 (الدفع) و docs/system-data-entities.md → Payments, Transactions

### NFR-29: ضمان تنفيذ الأحداث (Event Reliability)
- **الفئة:** Reliability
- **الوصف:** يجب ضمان وصول جميع الأحداث عبر RabbitMQ إلى جميع المشتركين المطلوبين. لا يجب فقدان أي حدث.
- **الأولوية:** High
- **المعيار القابل للقياس:** 100% من الأحداث المنشورة تصل إلى جميع المشتركين المسجلين
- **المصدر:** docs/phase-3-flow-of-event.md → 6 أحداث رسمية + 4 أحداث إضافية عبر RabbitMQ

### NFR-30: Idempotency في العمليات المالية
- **الفئة:** Reliability
- **الوصف:** يجب أن تكون عمليات الدفع والمعاملات المالية Idempotent — أي أن تنفيذ نفس العملية مرتين لا يؤدي إلى تغيير النتيجة أو إنشاء سجل مكرر.
- **الأولوية:** High
- **المعيار القابل للقياس:** نفس عملية الدفع المُنفذة مرتين تُنتج نتيجة واحدة فقط
- **المصدر:** docs/phase-2-detailed-scenarios.md → UC-28 Process Payment (يمنع الدفع لفاتورة مسددة مسبقًا)

---

## ز) قابلية الملاحظة (Observability)

### NFR-31: Logs (سجلات)
- **الفئة:** Observability
- **الوصف:** يجب أن يُنتج كل خدمة سجلات مفصلة تشمل: المعلومات، التحذيرات، والأخطاء. السجلات يجب أن تكون منظمة وقابلة للبحث.
- **الأولوية:** High
- **المعيار القابل للقياس:** جميع الخدمات الـ 11 تُنتج Logs منظمة
- **المصدر:** docs/phase-3-data-stores-and-movements.md → 12 Data Store و 13 API Flow

### NFR-32: Errors (تتبع الأخطاء)
- **الفئة:** Observability
- **الوصف:** يجب أن يتم تتبع جميع الأخطاء وتسجيلها مع معلومات كافية لتسهيل تشخيص المشكلات (Error Context, Stack Trace, Timestamp).
- **الأولوية:** High
- **المعيار القابل للقياس:** جميع الأخطاء مسجلة مع السياق الكامل
- **المصدر:** docs/phase-2-detailed-scenarios.md → كل سيناريو يحدد Alternative Flows و Exception Flows

### NFR-33: Events (تتبع الأحداث)
- **الفئة:** Observability
- **الوصف:** يجب أن يتم تتبع جميع الأحداث عبر النظام — من النشر إلى التسليم إلى المعالجة. كل حدث يجب أن يكون قابلاً للمراقبة.
- **الأولوية:** High
- **المعيار القابل للقياس:** 10 أحداث (6 رسمية + 4 إضافية) كلها قابلة للتتبع
- **المصدر:** docs/phase-3-flow-of-event.md → 6 Events و docs/phase-3-data-stores-and-movements.md → 10 Events

### NFR-34: Monitoring و Metrics (مراقبة وأداء)
- **الفئة:** Observability
- **الوصف:** يجب توفير لوحة مراقبة (Dashboard) تعرض مقاييس أداء النظام الأساسية: معدل الطلبات، أزمنة الاستجابة، معدل الأخطاء، حالة كل خدمة.
- **الأولوية:** High
- **المعيار القابل للقياس:** Dashboard يعرض جميع المقاييس الأساسية في وقت حقيقي
- **المصدر:** docs/project-objectives.md → الهدف 13 (لوحات تحكم وتقارير)

---

## جدول ملخص المتطلبات غير الوظيفية

| المعرّف | الفئة | الوصف المختصر | الأولوية | المعيار القابل للقياس |
|---|---|---|---|---|
| NFR-01 | Performance | أزمنة استجابة المصادقة | High | 90% خلال 500ms |
| NFR-02 | Performance | أزمنة استجابة عمليات العملاء | High | 90% خلال 500ms |
| NFR-03 | Performance | أزمنة استجابة عمليات SIM | High | 90% خلال 500ms |
| NFR-04 | Performance | أزمنة استجابة عمليات الفوترة | High | 90% خلال 500ms |
| NFR-05 | Performance | أزمنة استجابة عمليات الدفع | High | 90% خلال 500ms |
| NFR-06 | Scalability | التوسع الأفقي | High | إضافة Instance خلال دقائق |
| NFR-07 | Scalability | التوسع العمودي | High | قاعدة بيانات مخصصة لكل خدمة |
| NFR-08 | Scalability | إضافة خدمة جديدة دون إعادة بناء | High | Service جديد = DB جديدة + API Gateway |
| NFR-09 | Availability | عزل الأعطال | High | 10 خدمات تعمل عند تعطل 1 |
| NFR-10 | Availability | نسبة التوفر المستهدفة | High | 99.5% |
| NFR-11 | Availability | آليات التعافي من الفشل | High | تعافي تلقائي خلال 30 ثانية |
| NFR-12 | Security | JWT Authentication | High | 100% من الطلبات تتطلب JWT |
| NFR-13 | Security | Password Hashing | High | bcrypt/argon2 لجميع كلمات المرور |
| NFR-14 | Security | RBAC | High | 8 أدوار بصلاحيات مختلفة |
| NFR-15 | Security | API Authorization | High | 100% من الـ 42 UC لها صلاحيات |
| NFR-16 | Security | Input Validation | High | جميع الطلبات تمر بالتحقق |
| NFR-17 | Security | Rate Limiting | Medium | عدد الطلبات محدود لكل دقيقة |
| NFR-18 | Security | Audit Logs | High | جميع الأحداث الحرجة مسجلة |
| NFR-19 | Security | CORS Configuration | Medium | فقط النطاقات المصرح بها |
| NFR-20 | Security | Secure HTTP Headers | Medium | جميع الاستجابات تحتوي على رؤوس آمنة |
| NFR-21 | Security | Refresh Tokens | High | عمر محدد وآمن |
| NFR-22 | Security | Centralized Error Handling | Medium | Error Handler موحد |
| NFR-23 | Security | حماية بيانات العملاء والمعاملات | High | تشفير أثناء التخزين والنقل |
| NFR-24 | Maintainability | استقلالية الخدمات | High | 11 خدمة، 12 DB مستقلة |
| NFR-25 | Maintainability | كود نظيف ومنظم | Medium | معايير تنظيم واضحة |
| NFR-26 | Maintainability | توثيق كافٍ | High | 11 خدمة موثقة |
| NFR-27 | Maintainability | اختبارات آلية | High | اختبارات لكل الخدمات |
| NFR-28 | Reliability | عدم فقدان بيانات مالية | High | 0% فقدان بيانات |
| NFR-29 | Reliability | ضمان تنفيذ الأحداث | High | 100% وصول الأحداث |
| NFR-30 | Reliability | Idempotency مالي | High | تنفيذ مكرر = نتيجة واحدة |
| NFR-31 | Observability | Logs | High | جميع الخدمات تُنتج Logs |
| NFR-32 | Observability | Errors | High | جميع الأخطاء مسجلة مع السياق |
| NFR-33 | Observability | Events | High | 10 أحداث قابلة للتتبع |
| NFR-34 | Observability | Monitoring و Metrics | High | Dashboard في وقت حقيقي |

### إحصائيات عامة

| الفئة | عدد المتطلبات | High | Medium | Low |
|---|---|---|---|---|
| Performance | 5 | 5 | 0 | 0 |
| Scalability | 3 | 3 | 0 | 0 |
| Availability | 3 | 3 | 0 | 0 |
| Security | 12 | 9 | 3 | 0 |
| Maintainability | 4 | 3 | 1 | 0 |
| Reliability | 3 | 3 | 0 | 0 |
| Observability | 4 | 4 | 0 | 0 |
| **المجموع** | **34** | **30** | **4** | **0** |

---

> **ملخص:**
> - إجمالي المتطلبات غير الوظيفية: **34 NFR**
> - تغطي **7 فئات**: Performance, Scalability, Availability, Security, Maintainability, Reliability, Observability
> - 30 متطلبًا من الأولوية العالية (High)، 4 متطلبات متوسطة الأولوية (Medium)
> - جميع المتطلبات مستخرجة من ملف التحليل الأولي والوثائق الموثقة في المشروع
> - لا يوجد أي متطلب منخفض الأولوية (Low)

---

> **ملاحظة:** هذه الوثيقة مستخرجة حصريًا من ملف التحليل الأولي للمشروع. المتطلبات القابلة للقياس هي مقترحات مبنية على معايير SKILL-14 والمواد الموثقة.

> **تاريخ التحديث:** 2026-09-26
> **الحالة:** Confirmed
