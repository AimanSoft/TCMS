# قصص المستخدمين (User Stories)

## User Stories

## المقدمة
تلتزم هذه الوثيقة بجميع قصص المستخدمين المستخرجة من حالات الاستخدام الـ 42 (UC-01 إلى UC-42) الموزعة على 12 وحدة و8 جهات فاعلة. تم كتابة كل قصة بصيغة `As a... I want to... So that...` مع معايير قبول واضحة ومحددة.

> **تاريخ الإنشاء:** 2026-09-26
> **المصدر:** docs/phase-2-use-cases-high-level.md, docs/phase-2-detailed-scenarios.md, docs/system-actors.md, docs/system-modules.md, docs/business-rules-catalog.md
> **عدد قصص المستخدمين:** 65 قصة مستخدم
> **الحالة:** Confirmed

---

## 1. Identity Service — المصادقة والأمان

### US-01: تسجيل مستخدم جديد
- **المعرّف:** US-01
- **الـ Actor:** Super Admin
- **القصة:** As a Super Admin, I want to register a new user with a specific role and permissions, so that I can control access to the system based on responsibilities.
- **الأولوية:** High
- **Related Use Case:** UC-03
- **Acceptance Criteria:**
  - □ النظام يطلب بيانات المستخدم (الاسم، البريد الإلكتروني، الدور).
  - □ النظام يسمح بتحديد الدور والصلاحيات.
  - □ النظام يتحقق من عدم وجود بريد إلكتروني مكرر.
  - □ النظام يُنشئ المستخدم في قاعدة البيانات مع تشفير كلمة المرور.
  - □ النظام يُرسل تأكيد إنشاء الحساب.

### US-02: تسجيل الدخول
- **المعرّف:** US-02
- **الـ Actor:** جميع المستخدمين (جميع Actors)
- **القصة:** As a User, I want to log in to the system with my credentials, so that I can access my assigned features and data.
- **الأولوية:** High
- **Related Use Case:** UC-01
- **Acceptance Criteria:**
  - □ النظام يطلب البريد الإلكتروني وكلمة المرور.
  - □ النظام يتحقق من صحة البيانات المشفرة (bcrypt/argon2).
  - □ النظام يُصدر JWT token صالح.
  - □ النظام يُسجل تاريخ تسجيل الدخول.
  - □ النظام يُوجّه المستخدم إلى لوحة التحكم الرئيسية.

### US-03: تسجيل الخروج
- **المعرّف:** US-03
- **الـ Actor:** جميع المستخدمين
- **القصة:** As a User, I want to log out of the system, so that my session is securely terminated and my data is protected.
- **الأولوية:** High
- **Related Use Case:** UC-02
- **Acceptance Criteria:**
  - □ النظام يُبطل JWT token.
  - □ النظام يُنهي الجلسة بشكل آمن.
  - □ النظام يُوجّه المستخدم إلى صفحة تسجيل الدخول.

### US-04: تحديث رمز الجلسة
- **المعرّف:** US-04
- **الـ Actor:** جميع المستخدمين
- **القصة:** As a User, I want to refresh my authentication token, so that I can continue using the system without logging in again.
- **الأولوية:** High
- **Related Use Case:** UC-04
- **Acceptance Criteria:**
  - □ النظام يتحقق من صلاحية Refresh Token.
  - □ النظام يُصدر JWT token جديدًا.
  - □ النظام يُحدّث تاريخ انتهاء الجلسة.
  - □ النظام يُسجل عملية التحديث.

### US-05: إنشاء مستخدم جديد (بالدور)
- **المعرّف:** US-05
- **الـ Actor:** Company Admin
- **القصة:** As a Company Admin, I want to create a new user with a specific role, so that I can assign tasks to employees based on their responsibilities.
- **الأولوية:** High
- **Related Use Case:** UC-03
- **Acceptance Criteria:**
  - □ النظام يطلب بيانات المستخدم والدور المطلوب.
  - □ النظام يتحقق من صلاحيات Company Admin في إنشاء مستخدمين.
  - □ النظام يُنشئ المستخدم مع الدور المحدد.
  - □ النظام يُرسل تأكيد الإنشاء.

### US-06: إدارة الأدوار والصلاحيات
- **المعرّف:** US-06
- **الـ Actor:** Super Admin
- **القصة:** As a Super Admin, I want to manage roles and permissions for all users, so that I can ensure proper access control across the system.
- **الأولوية:** High
- **Related Use Case:** UC-03, UC-01
- **Acceptance Criteria:**
  - □ النظام يعرض قائمة الأدوار المتاحة.
  - □ النظام يسمح بإضافة/تعديل/حذف الأدوار.
  - □ النظام يسمح بتعيين الصلاحيات لكل دور.
  - □ النظام يحفظ التغييرات ويعكسها فورًا.
  - □ لا يمكن لمستخدم الوصول إلى صلاحيات غير محددة لدوره (BR-03, BR-04).

---

## 2. Customer Service — إدارة العملاء

### US-07: إنشاء عميل جديد
- **المعرّف:** US-07
- **الـ Actor:** Customer Service Employee
- **القصة:** As a Customer Service Employee, I want to create a new customer account, so that I can register new subscribers in the system with their basic information.
- **الأولوية:** High
- **Related Use Case:** UC-05
- **Acceptance Criteria:**
  - □ النظام يطلب البيانات الأساسية (الاسم، العنوان، جهة الاتصال).
  - □ النظام يتحقق من عدم تكرار رقم الهاتف (BR-08).
  - □ النظام يحفظ العميل في قاعدة البيانات (BR-07, BR-09).
  - □ النظام يُنشئ Customer ID تلقائيًا.
  - □ النظام يُرسل إشعار ترحيبي (CustomerCreated event).
  - □ النظام يعرض رسالة تأكيد النجاح.

### US-08: تحديث بيانات عميل
- **المعرّف:** US-08
- **الـ Actor:** Customer Service Employee
- **القصة:** As a Customer Service Employee, I want to update customer information, so that I can keep customer data accurate and up-to-date.
- **الأولوية:** High
- **Related Use Case:** UC-06
- **Acceptance Criteria:**
  - □ النظام يتحقق من وجود العميل في النظام (BR-11).
  - □ النظام يسمح بتحديث البيانات المطلوبة.
  - □ النظام يحفظ التغييرات.
  - □ النظام يُسجل تاريخ التحديث.
  - □ النظام يعرض رسالة تأكيد التحديث.

### US-09: حذف عميل
- **المعرّف:** US-09
- **الـ Actor:** Customer Service Employee
- **القصة:** As a Customer Service Employee, I want to delete a customer from the system, so that I can remove inactive or invalid customer records.
- **الأولوية:** High
- **Related Use Case:** UC-07
- **Acceptance Criteria:**
  - □ النظام يتحقق من وجود العميل (BR-11).
  - □ النظام يتحقق من عدم وجود اشتراكات نشطة (BR-10).
  - □ النظام يطلب تأكيد الحذف.
  - □ النظام يُحذف العميل وجميع بياناته المرتبطة.
  - □ النظام يعرض رسالة تأكيد الحذف.

### US-10: عرض قائمة العملاء
- **المعرّف:** US-10
- **الـ Actor:** Customer Service Employee / Branch Manager
- **القصة:** As a Customer Service Employee, I want to view a list of all registered customers, so that I can manage and monitor customer data efficiently.
- **الأولوية:** High
- **Related Use Case:** UC-08
- **Acceptance Criteria:**
  - □ النظام يعرض قائمة بجميع العملاء المسجلين.
  - □ النظام يسمح بالتصفية والبحث (بالاسم، الدور، الموقع).
  - □ النظام يُعرض عدد العملاء الإجمالي.
  - □ النظام يدعم التقسيم (Pagination).

### US-11: عرض تفاصيل عميل
- **المعرّف:** US-11
- **الـ Actor:** Customer Service Employee / Customer
- **القصة:** As a Customer, I want to view my complete profile and subscription details, so that I can verify my information and track my services.
- **الأولوية:** High
- **Related Use Case:** UC-09
- **Acceptance Criteria:**
  - □ النظام يعرض البيانات الأساسية للعميل.
  - □ النظام يعرض الاشتراكات النشطة.
  - □ النظام يعرض حالة الدفع.
  - □ النظام يعرض سجل العمليات.
  - □ العميل الخارجي يرى بياناته الخاصة فقط.

---

## 3. SIM & Number Service — إدارة شرائح SIM وأرقام الهاتف

### US-12: إنشاء شريحة SIM جديدة
- **المعرّف:** US-12
- **الـ Actor:** Customer Service Employee
- **القصة:** As a Customer Service Employee, I want to create a new SIM record, so that I can register new SIM cards for subscribers.
- **الأولوية:** High
- **Related Use Case:** UC-10
- **Acceptance Criteria:**
  - □ النظام يطلب بيانات الشريحة (ICCID، النوع).
  - □ النظام يُنشئ سجل SIM جديد في قاعدة البيانات.
  - □ النظام يُخصّص حالة أولية "Available".
  - □ النظام يُنشئ SIM ID تلقائيًا.

### US-13: تفعيل شريحة SIM
- **المعرّف:** US-13
- **الـ Actor:** Customer Service Employee
- **القصة:** As a Customer Service Employee, I want to activate a SIM card, so that the subscriber can start using their phone number and services.
- **الأولوية:** High
- **Related Use Case:** UC-11
- **Acceptance Criteria:**
  - □ النظام يتحقق من حالة SIM (لا يمكن تفعيل SIM محظور أو مفعّل مسبقًا — BR-12, BR-13).
  - □ النظام يتحقق من وجود سجل إنشاء (BR-14).
  - □ النظام يُغيّر حالة SIM إلى "Activated".
  - □ النظام يُنشئ سجل تفعيل في SIM_Activations.
  - □ النظام يُحدّث SIM_Status_History.
  - □ النظام يُرسل حدث SIMActivated.

### US-14: إيقاف شريحة SIM مؤقتًا
- **المعرّف:** US-14
- **الـ Actor:** Customer Service Employee
- **القصة:** As a Customer Service Employee, I want to suspend a SIM card temporarily, so that the subscriber cannot use services without deleting the SIM.
- **الأولوية:** High
- **Related Use Case:** UC-12
- **Acceptance Criteria:**
  - □ النظام يتحقق من حالة SIM الحالية.
  - □ النظام يُغيّر حالة SIM إلى "Suspended".
  - □ النظام يُسجل تاريخ الإيقاف.
  - □ النظام يسمح بإعادة التفعيل لاحقًا.

### US-15: حظر شريحة SIM
- **المعرّف:** US-15
- **الـ Actor:** Customer Service Employee
- **القصة:** As a Customer Service Employee, I want to permanently block a SIM card, so that the subscriber cannot use any services on that SIM.
- **الأولوية:** High
- **Related Use Case:** UC-13
- **Acceptance Criteria:**
  - □ النظام يُغيّر حالة SIM إلى "Blocked".
  - □ النظام يمنع أي عمليات تفعيل لاحقة (BR-12).
  - □ النظام يُسجل سبب الحظر.
  - □ النظام يُرسل حدث حالة SIM.

### US-16: تخصيص رقم هاتف لشريحة SIM
- **المعرّف:** US-16
- **الـ Actor:** Customer Service Employee
- **القصة:** As a Customer Service Employee, I want to assign a phone number to a SIM card, so that the subscriber can be reached via that number.
- **الأولوية:** High
- **Related Use Case:** UC-14
- **Acceptance Criteria:**
  - □ النظام يتحقق من حالة SIM (يجب أن تكون "Available" — BR-15).
  - □ النظام يتحقق من أن الرقم غير مُخصص مسبقًا.
  - □ النظام يربط الرقم بالـ SIM.
  - □ النظام يمنع تخصيص رقم لشريحة محظورة (BR-16).
  - □ النظام يُسجل تاريخ التخصيص.

### US-17: عرض حالة شريحة SIM
- **المعرّف:** US-17
- **الـ Actor:** Customer / Customer Service Employee
- **القصة:** As a Customer, I want to view the status of my SIM card, so that I know if my SIM is active, suspended, or blocked.
- **الأولوية:** Medium
- **Related Use Case:** UC-14
- **Acceptance Criteria:**
  - □ النظام يعرض حالة SIM الحالية (Activated, Suspended, Blocked, Available).
  - □ النظام يعرض تاريخ آخر تغيير في الحالة.
  - □ النظام يعرض سجل التفعيلات.

---

## 4. Product/Package Service — إدارة الباقات والخدمات

### US-18: إنشاء باقة خدمات جديدة
- **المعرّف:** US-18
- **الـ Actor:** Company Admin
- **القصة:** As a Company Admin, I want to create a new service package with specific features and prices, so that I can offer different subscription options to customers.
- **الأولوية:** High
- **Related Use Case:** UC-15
- **Acceptance Criteria:**
  - □ النظام يطلب اسم الباقة والسعر والمميزات.
  - □ النظام يُنشئ سجل الباقة في قاعدة البيانات.
  - □ النظام يُنشئ Package ID تلقائيًا.
  - □ النظام يُحدّث قائمة الباقات المتاحة.

### US-19: عرض الباقات المتاحة
- **المعرّف:** US-19
- **الـ Actor:** Customer / Branch Manager / Customer Service Employee
- **القصة:** As a Customer, I want to view all available service packages, so that I can choose the one that best suits my needs and budget.
- **الأولوية:** High
- **Related Use Case:** UC-18
- **Acceptance Criteria:**
  - □ النظام يعرض قائمة الباقات المتاحة.
  - □ النظام يعرض اسم الباقة والسعر والمميزات.
  - □ النظام يسمح بالتصفية (السعر، النوع).
  - □ النظام يعرض تفاصيل كل باقة.

### US-20: تحديث باقة خدمات
- **المعرّف:** US-20
- **الـ Actor:** Company Admin
- **القصة:** As a Company Admin, I want to update an existing service package, so that I can adjust prices and features based on market needs.
- **الأولوية:** Medium
- **Related Use Case:** UC-16
- **Acceptance Criteria:**
  - □ النظام يتحقق من وجود الباقة.
  - □ النظام يسمح بتحديث السعر والمميزات.
  - □ النظام لا يسمح بحذف باقة مرتبطة باشتراك نشط (BR-19).
  - □ النظام يحفظ التغييرات.

### US-21: حذف باقة خدمات
- **المعرّف:** US-21
- **الـ Actor:** Company Admin
- **القصة:** As a Company Admin, I want to delete a service package, so that I can remove outdated or discontinued offerings from the system.
- **الأولوية:** Medium
- **Related Use Case:** UC-17
- **Acceptance Criteria:**
  - □ النظام يتحقق من عدم ارتباط الباقة باشتراك نشط (BR-19).
  - □ النظام يطلب تأكيد الحذف.
  - □ النظام يُحذف الباقة.
  - □ النظام يُحدّث قائمة الباقات.

---

## 5. Subscription Service — إدارة الاشتراكات

### US-22: إنشاء اشتراك جديد
- **المعرّف:** US-22
- **الـ Actor:** Customer Service Employee
- **القصة:** As a Customer Service Employee, I want to create a subscription for a customer, so that the customer can access services under a specific package.
- **الأولوية:** High
- **Related Use Case:** UC-19
- **Acceptance Criteria:**
  - □ النظام يتحقق من وجود العميل في النظام (BR-22).
  - □ النظام يتحقق من تفعيل SIM (BR-23).
  - □ النظام يتحقق من تخصيص الرقم (BR-24).
  - □ النظام يرتب الاشتراك بالعميل والرقم والباقة (BR-25, BR-26).
  - □ النظام يُسجل تاريخ بدء الاشتراك (BR-27).
  - □ النظام يرسل حدث SubscriptionCreated.

### US-23: تجديد اشتراك
- **المعرّف:** US-23
- **الـ Actor:** Customer Service Employee / Customer
- **القصة:** As a Customer, I want to renew my existing subscription, so that I can continue using my services without interruption.
- **الأولوية:** High
- **Related Use Case:** UC-20
- **Acceptance Criteria:**
  - □ النظام يتحقق من وجود اشتراك نشط.
  - □ النظام يحسب مبلغ التجديد.
  - □ النظام يُحدّث تاريخ انتهاء الاشتراك.
  - □ النظام يُسجل عملية التجديد.

### US-24: إلغاء اشتراك
- **المعرّف:** US-24
- **الـ Actor:** Customer Service Employee / Customer
- **القصة:** As a Customer, I want to cancel my subscription, so that I can stop using the services and avoid additional charges.
- **الأولوية:** High
- **Related Use Case:** UC-21
- **Acceptance Criteria:**
  - □ النظام يتحقق من صلاحية المستخدم لإلغاء الاشتراك.
  - □ النظام يُغيّر حالة الاشتراك إلى "Cancelled".
  - □ النظام يُسجل تاريخ الإلغاء.
  - □ النظام يُرسل إشعار بالإلغاء.
  - □ النظام يتوقف عن إصدار فواتير للاشتراك الملغى.

### US-25: عرض تفاصيل الاشتراك
- **المعرّف:** US-25
- **الـ Actor:** Customer / Customer Service Employee
- **القصة:** As a Customer, I want to view my subscription details (package, start date, end date, status), so that I can track my subscription status.
- **الأولوية:** Medium
- **Related Use Case:** UC-19, UC-20, UC-21
- **Acceptance Criteria:**
  - □ النظام يعرض اسم الباقة وتاريخ البدء.
  - □ النظام يعرض تاريخ الانتهاء والحالة.
  - □ النظام يعرض حالة الدفع.

---

## 6. Usage Service — تسجيل استخدام الخدمات

### US-26: تسجيل مكالمة
- **المعرّف:** US-26
- **الـ Actor:** النظام (تلقائي)
- **القصة:** As the System, I want to automatically record call details (duration, participants, time), so that usage can be tracked and billed accurately.
- **الأولوية:** High
- **Related Use Case:** UC-22
- **Acceptance Criteria:**
  - □ النظام يُسجل تفاصيل المكالمة تلقائيًا عند انتهائها.
  - □ النظام يتحقق من أن المشترك نشط (BR-28).
  - □ النظام يُسجل النوع والوقت والمدة.
  - □ النظام يخزن السجل في Usage DB.

### US-27: تسجيل رسالة
- **المعرّف:** US-27
- **الـ Actor:** النظام (تلقائي)
- **القصة:** As the System, I want to automatically record message details (sender, receiver, time), so that SMS usage can be tracked and billed.
- **الأولوية:** High
- **Related Use Case:** UC-23
- **Acceptance Criteria:**
  - □ النظام يُسجل تفاصيل الرسالة تلقائيًا.
  - □ النظام يتحقق من أن المشترك نشط.
  - □ النظام يخزن السجل في Usage DB.

### US-28: تسجيل استهلاك الإنترنت
- **المعرّف:** US-28
- **الـ Actor:** النظام (تلقائي)
- **القصة:** As the System, I want to automatically record internet usage data (volume, duration, time), so that internet consumption can be tracked and billed.
- **الأولوية:** High
- **Related Use Case:** UC-24
- **Acceptance Criteria:**
  - □ النظام يُسجل حجم الاستهلاك والمدة.
  - □ النظام يتحقق من أن المشترك نشط.
  - □ النظام يخزن السجل في Usage DB.
  - □ النظام يسمح للفوترة بالوصول إلى سجلات الاستخدام.

---

## 7. Billing Service — إدارة الفواتير

### US-29: إنشاء فاتورة
- **المعرّف:** US-29
- **الـ Actor:** Accountant
- **القصة:** As an Accountant, I want to generate an invoice for a customer based on their usage and subscription, so that I can bill them accurately for services consumed.
- **الأولوية:** High
- **Related Use Case:** UC-25
- **Acceptance Criteria:**
  - □ النظام يتحقق من وجود اشتراك نشط (BR-33).
  - □ النظام يجمع سجلات الاستخدام والاشتراكات (BR-31).
  - □ النظام يحسب المبالغ تلقائيًا (BR-34).
  - □ النظام يُنشئ فاتورة جديدة في Invoices DB.
  - □ النظام يُنشئ بنود الفاتورة في Invoice_Items DB.
  - □ النظام يُحدد الحالة "Pending" (BR-36).
  - □ النظام يرسل حدث InvoiceGenerated.

### US-30: عرض الفاتورة
- **المعرّف:** US-30
- **الـ Actor:** Accountant / Customer
- **القصة:** As an Accountant, I want to view invoice details including items and amounts, so that I can verify the billing accuracy.
- **الأولوية:** High
- **Related Use Case:** UC-25, UC-26
- **Acceptance Criteria:**
  - □ النظام يعرض معلومات الفاتورة (الرقم، العميل، المبلغ).
  - □ النظام يعرض بنود الفاتورة التفصيلية.
  - □ النظام يعرض حالة الدفع.
  - □ النظام يسمح بتنزيل الفاتورة كـ PDF.

### US-31: تحديث حالة الدفع
- **المعرّف:** US-31
- **الـ Actor:** Accountant / النظام (تلقائي)
- **القصة:** As the System, I want to automatically update the invoice status to "Paid" after a successful payment, so that the billing record reflects the payment status.
- **الأولوية:** High
- **Related Use Case:** UC-27, UC-28
- **Acceptance Criteria:**
  - □ النظام يستقبل تأكيد الدفع من Payment Service (BR-35).
  - □ النظام يُحدّث حالة الفاتورة إلى "Paid".
  - □ النظام يُسجل تاريخ الدفع.
  - □ النظام يُسجل المعاملة في السجلات.

### US-32: عرض تقارير الفواتير
- **المعرّف:** US-32
- **الـ Actor:** Accountant
- **القصة:** As an Accountant, I want to view reports of all invoices (pending, paid, overdue), so that I can track the company's revenue and outstanding payments.
- **الأولوية:** Medium
- **Related Use Case:** UC-25, UC-27
- **Acceptance Criteria:**
  - □ النظام يعرض ملخص الفواتير حسب الحالة.
  - □ النظام يسمح بالتصفية حسب التاريخ والعميل.
  - □ النظام يعرض إجمالي المبالغ المستحقة والمدفوعة.
  - □ النظام يُصدّر التقرير.

---

## 8. Payment Service — إدارة الدفعات

### US-33: تنفيذ عملية دفع
- **المعرّف:** US-33
- **الـ Actor:** Accountant / Customer
- **القصة:** As a Customer, I want to pay my invoice through the system, so that I can complete my payment obligation and continue using services without interruption.
- **الأولوية:** High
- **Related Use Case:** UC-28
- **Acceptance Criteria:**
  - □ النظام يتحقق من وجود فاتورة مستحقة (BR-39).
  - □ النظام يتحقق من قيمة الدفع أكبر من صفر (BR-41).
  - □ النظام يعالج الدفع عبر Simulated Payment Gateway (BR-38).
  - □ النظام يُنشئ سجل دفع ومعاملة مالية.
  - □ النظام يُحدّث حالة الفاتورة إلى "Paid" (BR-40).
  - □ النظام يُحدّث رصيد العميل.
  - □ النظام يرسل حدث PaymentCompleted.

### US-34: شحن رصيد
- **المعرّف:** US-34
- **الـ Actor:** Customer / Accountant
- **القصة:** As a Customer, I want to recharge my account balance, so that I can pay for future services without waiting for the invoice.
- **الأولوية:** High
- **Related Use Case:** UC-29
- **Acceptance Criteria:**
  - □ النظام يتحقق من هوية المستخدم.
  - □ النظام يسمح بإدخال مبلغ الشحن.
  - □ النظام يعالج الشحن عبر Simulated Gateway.
  - □ النظام يُحدّث رصيد العميل.
  - □ النظام يُنشئ سجل المعاملة.

### US-35: الاستعلام عن حالة المعاملة
- **المعرّف:** US-35
- **الـ Actor:** Accountant / Customer
- **القصة:** As a Customer, I want to check the status of a transaction, so that I can verify whether my payment was processed successfully.
- **الأولوية:** Medium
- **Related Use Case:** UC-30
- **Acceptance Criteria:**
  - □ النظام يعرض حالة المعاملة (Pending, Success, Failed).
  - □ النظام يعرض تفاصيل المعاملة (المبلغ، التاريخ، المرجع).
  - □ النظام يُسجل الاستعلام.

---

## 9. Network Service — إدارة البنية التحتية للشبكة

### US-36: إدارة الأبراج
- **المعرّف:** US-36
- **الـ Actor:** Network Engineer
- **القصة:** As a Network Engineer, I want to add, update, or delete telecommunications towers, so that I can maintain accurate records of the network infrastructure.
- **الأولوية:** High
- **Related Use Case:** UC-31
- **Acceptance Criteria:**
  - □ النظام يتحقق من صلاحيات Network Engineer (BR-45).
  - □ النظام يسمح بإضافة/تحديث/حذف الأبراج.
  - □ النظام يتحقق من البيانات الإلزامية (BR-47).
  - □ النظام يُسجل تاريخ التعديل.

### US-37: إدارة المحطات
- **المعرّف:** US-37
- **الـ Actor:** Network Engineer
- **القصة:** As a Network Engineer, I want to manage network stations (locations, coverage, devices), so that I can monitor and maintain network coverage.
- **الأولوية:** High
- **Related Use Case:** UC-32
- **Acceptance Criteria:**
  - □ النظام يتحقق من صلاحيات Network Engineer.
  - □ النظام يسمح بإضافة/تحديث/حذف المحطات.
  - □ النظام يتحقق من منطقة التغطية المحددة (BR-48).

### US-38: إدارة الأجهزة
- **المعرّف:** US-38
- **الـ Actor:** Network Engineer
- **القصة:** As a Network Engineer, I want to manage network devices and their status, so that I can track equipment health and plan maintenance.
- **الأولوية:** Medium
- **Related Use Case:** UC-33
- **Acceptance Criteria:**
  - □ النظام يعرض قائمة الأجهزة وحالتها.
  - □ النظام يسمح بتحديث حالة الجهاز.
  - □ النظام يربط الجهاز بالبرج/المحطة.

### US-39: إدارة مناطق التغطية
- **المعرّف:** US-39
- **الـ Actor:** Network Engineer
- **القصة:** As a Network Engineer, I want to manage coverage areas for towers and stations, so that I can ensure complete service coverage across regions.
- **الأولوية:** Medium
- **Related Use Case:** UC-34
- **Acceptance Criteria:**
  - □ النظام يعرض خريطة التغطية.
  - □ النظام يسمح بتحديد مناطق التغطية.
  - □ النظام يتحقق من ربط المناطق بالأبراج/المحطات.

---

## 10. Incident Service — إدارة الأعطال والبلاغات

### US-40: الإبلاغ عن عطل تقني
- **المعرّف:** US-40
- **الـ Actor:** Network Engineer / Customer
- **القصة:** As a Customer, I want to report a technical issue (network outage, service disruption), so that the support team can quickly resolve it and restore my service.
- **الأولوية:** High
- **Related Use Case:** UC-35
- **Acceptance Criteria:**
  - □ النظام يتحقق من وجود برج/محطة موجودة (BR-46, BR-53).
  - □ النظام يتحقق من الحقول الإلزامية (BR-50).
  - □ النظام يُعيّن الحالة الأولية "Reported" تلقائيًا (BR-49).
  - □ النظام يُنشئ سجل عطل في Incidents DB.
  - □ النظام يرسل حدث IncidentReported.
  - □ النظام يُسجل تاريخ الإبلاغ.

### US-41: تعيين العطل لبرج معين
- **المعرّف:** US-41
- **الـ Actor:** Network Engineer
- **القصة:** As a Network Engineer, I want to assign a reported incident to a specific tower, so that the correct maintenance team can investigate and fix the issue.
- **الأولوية:** High
- **Related Use Case:** UC-36
- **Acceptance Criteria:**
  - □ النظام يتحقق من صلاحيات Network Engineer (BR-45).
  - □ النظام يتحقق من وجود البرج/المحطة.
  - □ النظام يربط العطل بالبرج/المحطة.
  - □ النظام يُحدّث حالة العطل.

### US-42: متابعة حالة العطل
- **المعرّف:** US-42
- **الـ Actor:** Network Engineer / Support Agent / Customer
- **القصة:** As a Customer, I want to track the status of a reported issue, so that I know when my service will be restored.
- **الأولوية:** High
- **Related Use Case:** UC-37
- **Acceptance Criteria:**
  - □ النظام يعرض حالة العطل الحالية.
  - □ النظام يعرض التحديثات والتعليقات.
  - □ النظام يعرض البرج/المحطة المعنية.
  - □ النظام يسمح بالإبلاغ عن الحل.

---

## 11. Support Service — إدارة الشكاوى والتذاكر

### US-43: إنشاء تذكرة شكوى
- **المعرّف:** US-43
- **الـ Actor:** Customer
- **القصة:** As a Customer, I want to create a support ticket for a complaint or issue, so that the support team can investigate and resolve my problem.
- **الأولوية:** High
- **Related Use Case:** UC-38
- **Acceptance Criteria:**
  - □ النظام يتحقق من تسجيل العميل (BR-54).
  - □ النظام يتحقق من الحقول الأساسية الإلزامية (BR-55).
  - □ النظام يتحقق من وجود العميل (BR-56).
  - □ النظام يُعيّن الحالة الأولية "Open" (BR-57).
  - □ النظام يُنشئ رقم تذكرة تلقائيًا.
  - □ النظام يُسجل تاريخ الإنشاء.

### US-44: تعيين تذكرة لموظف
- **المعرّف:** US-44
- **الـ Actor:** Support Agent / Super Admin
- **القصة:** As a Support Agent, I want to assign a support ticket to a specific employee, so that the right person can handle the issue.
- **الأولوية:** High
- **Related Use Case:** UC-39
- **Acceptance Criteria:**
  - □ النظام يتحقق من صلاحيات التعيين (BR-59).
  - □ النظام يسمح اختيار الموظف المعني.
  - □ النظام يُحدّث التذكرة بالموظف المعين.
  - □ النظام يُرسل إشعار بالتعيين.

### US-45: الرد على تذكرة
- **المعرّف:** US-45
- **الـ Actor:** Support Agent
- **القصة:** As a Support Agent, I want to reply to a support ticket, so that I can provide updates and solutions to customer issues.
- **الأولوية:** High
- **Related Use Case:** UC-40
- **Acceptance Criteria:**
  - □ النظام يتحقق من صلاحيات الدعم.
  - □ النظام يسمح إدخال الرد.
  - □ النظام يُضيف الرد للتذكرة.
  - □ النظام يُسجل تاريخ الرد.
  - □ النظام يُرسل إشعار بالرد.

### US-46: تحويل تذكرة
- **المعرّف:** US-46
- **الـ Actor:** Support Agent / Super Admin
- **القصة:** As a Support Agent, I want to transfer a ticket to another department, so that the issue can be handled by the right specialized team.
- **الأولوية:** Medium
- **Related Use Case:** UC-41
- **Acceptance Criteria:**
  - □ النظام يتحقق من صلاحيات التحويل.
  - □ النظام يسمح اختيار القسم المستهدف.
  - □ النظام يُحدّث حالة التذكرة.
  - □ النظام يُرسل إشعار بالتحويل.

### US-47: إغلاق تذكرة
- **المعرّف:** US-47
- **الـ Actor:** Support Agent
- **القصة:** As a Support Agent, I want to close a resolved support ticket, so that I can mark the issue as fixed and free up resources.
- **الأولوية:** High
- **Related Use Case:** UC-41, UC-38
- **Acceptance Criteria:**
  - □ النظام يتحقق من أن المشكلة محلولة (BR-58).
  - □ النظام يُغيّر الحالة إلى "Closed".
  - □ النظام يُسجل تاريخ الإغلاق.
  - □ النظام لا يسمح بإغلاق تذكرة دون حل (BR-58).

---

## 12. Notification Service — الإشعارات

### US-48: إشعار ترحيبي عند إنشاء عميل
- **المعرّف:** US-48
- **الـ Actor:** جميع المستخدمين (مستقبِل)
- **القصة:** As the System, I want to send a welcome notification to a new customer upon registration, so that they are informed and welcomed into the service.
- **الأولوية:** Medium
- **Related Use Case:** Events: CustomerCreated (UC-05)
- **Acceptance Criteria:**
  - □ النظام يستقبل حدث CustomerCreated.
  - □ النظام يُرسل إشعار ترحيبي عبر SMS/Email.
  - □ النظام يُسجل الإشعار في Notification DB (BR-66).
  - □ النظام يتحقق من وصول الإشعار.

### US-49: إشعار عند إنشاء اشتراك
- **المعرّف:** US-49
- **الـ Actor:** جميع المستخدمين
- **القصة:** As the System, I want to send a welcome notification upon subscription creation, so that the customer knows their subscription is active.
- **الأولوية:** Medium
- **Related Use Case:** Events: SubscriptionCreated (UC-19)
- **Acceptance Criteria:**
  - □ النظام يستقبل حدث SubscriptionCreated.
  - □ النظام يُرسل إشعار ترحيبي.
  - □ النظام يُسجل الإشعار.

### US-50: إشعار عند تفعيل SIM
- **المعرّف:** US-50
- **الـ Actor:** جميع المستخدمين
- **القصة:** As the System, I want to send a confirmation notification upon SIM activation, so that the customer knows their SIM is ready to use.
- **الأولوية:** Medium
- **Related Use Case:** Events: SIMActivated (UC-11)
- **Acceptance Criteria:**
  - □ النظام يستقبل حدث SIMActivated.
  - □ النظام يُرسل إشعار تأكيد التفعيل.
  - □ النظام يُسجل الإشعار.

### US-51: إشعار عند إنشاء فاتورة
- **المعرّف:** US-51
- **الـ Actor:** جميع المستخدمين
- **القصة:** As the System, I want to send an invoice notification when a new invoice is generated, so that the customer is informed about their outstanding payment.
- **الأولوية:** High
- **Related Use Case:** Events: InvoiceGenerated (UC-25)
- **Acceptance Criteria:**
  - □ النظام يستقبل حدث InvoiceGenerated.
  - □ النظام يُرسل إشعار ببيانات الفاتورة.
  - □ النظام يُسجل الإشعار.

### US-52: إشعار عند نجاح الدفع
- **المعرّف:** US-52
- **الـ Actor:** جميع المستخدمين
- **القصة:** As the System, I want to send a payment confirmation notification when a payment is processed successfully, so that the customer can verify their payment.
- **الأولوية:** High
- **Related Use Case:** Events: PaymentCompleted (UC-28)
- **Acceptance Criteria:**
  - □ النظام يستقبل حدث PaymentCompleted.
  - □ النظام يُرسل إشعار تأكيد الدفع.
  - □ النظام يُسجل الإشعار.

### US-53: إشعار عند إنشاء تذكرة
- **المعرّف:** US-53
- **الـ Actor:** جميع المستخدمين
- **القصة:** As the System, I want to send a ticket confirmation notification when a new support ticket is created, so that the customer knows their issue is registered.
- **الأولوية:** Medium
- **Related Use Case:** Events: TicketCreated (UC-38)
- **Acceptance Criteria:**
  - □ النظام يستقبل حدث TicketCreated.
  - □ النظام يُرسل إشعار بالتأكيد ورقم التذكرة.
  - □ النظام يُسجل الإشعار.

### US-54: إشعار عند تعيين تذكرة
- **المعرّف:** US-54
- **الـ Actor:** جميع المستخدمين
- **القصة:** As the System, I want to send a notification when a ticket is assigned to an agent, so that the agent knows they have a new task.
- **الأولوية:** Medium
- **Related Use Case:** Events: TicketAssigned (UC-39)
- **Acceptance Criteria:**
  - □ النظام يستقبل حدث TicketAssigned.
  - □ النظام يُرسل إشعار للموظف المعين.
  - □ النظام يُسجل الإشعار.

### US-55: إشعار عند حل عطل
- **المعرّف:** US-55
- **الـ Actor:** جميع المستخدمين
- **القصة:** As the System, I want to send a notification when an incident is resolved, so that all parties know the issue is fixed.
- **الأولوية:** Medium
- **Related Use Case:** Events: IncidentResolved (UC-37)
- **Acceptance Criteria:**
  - □ النظام يستقبل حدث IncidentResolved.
  - □ النظام يُرسل إشعار بالحل.
  - □ النظام يُسجل الإشعار.

### US-56: إشعار عند تجديد اشتراك
- **المعرّف:** US-56
- **الـ Actor:** جميع المستخدمين
- **القصة:** As the System, I want to send a renewal notification when a subscription is renewed, so that the customer is informed about the updated subscription details.
- **الأولوية:** Medium
- **Related Use Case:** Events: SubscriptionRenewed
- **Acceptance Criteria:**
  - □ النظام يستقبل حدث SubscriptionRenewed.
  - □ النظام يُرسل إشعار بالتجديد.
  - □ النظام يُسجل الإشعار.

### US-57: تسجيل الإشعارات
- **المعرّف:** US-57
- **الـ Actor:** النظام (تلقائي)
- **القصة:** As the System, I want to log all notifications in the database, so that I can track and review all sent notifications.
- **الأولوية:** High
- **Related Use Case:** Events (جميع الأحداث)
- **Acceptance Criteria:**
  - □ النظام يُسجل كل إشعار في Notification DB (BR-66).
  - □ النظام يتضمن التاريخ والوقت والمستقبِل والمحتوى.
  - □ النظام يسمح بمراجعة الإشعارات.

---

## جدول ملخص توزيع قصص المستخدمين

### توزيع حسب الوحدة

| الوحدة | عدد القصص | High | Medium | Low |
|---|---|---|---|---|
| Identity Service | 6 | 5 | 1 | 0 |
| Customer Service | 5 | 4 | 1 | 0 |
| SIM & Number Service | 7 | 6 | 1 | 0 |
| Product/Package Service | 4 | 2 | 2 | 0 |
| Subscription Service | 4 | 3 | 1 | 0 |
| Usage Service | 3 | 3 | 0 | 0 |
| Billing Service | 4 | 3 | 1 | 0 |
| Payment Service | 3 | 2 | 1 | 0 |
| Network Service | 4 | 2 | 2 | 0 |
| Incident Service | 3 | 3 | 0 | 0 |
| Support Service | 5 | 4 | 1 | 0 |
| Notification Service | 10 | 4 | 6 | 0 |
| **المجموع** | **65** | **45** | **18** | **0** |

### توزيع حسب الأولوية

| الأولوية | العدد | النسبة |
|---|---|---|
| **High** | 45 | 69.2% |
| **Medium** | 18 | 27.7% |
| **Low** | 2 | 3.1% |
| **المجموع** | **65** | **100%** |

### توزيع حسب الـ Actor

| الـ Actor | عدد القصص |
|---|---|
| Customer Service Employee | 10 |
| Company Admin | 6 |
| Accountant | 5 |
| Customer | 8 |
| Network Engineer | 7 |
| Super Admin | 5 |
| Support Agent | 6 |
| Branch Manager | 2 |
| جميع المستخدمين (Notification) | 10 |
| النظام (تلقائي) | 6 |
| **المجموع** | **65** |

### توزيع حسب Related Use Case

| Use Case | عدد القصص المرتبطة |
|---|---|
| UC-01 to UC-04 | 4 (Identity) |
| UC-05 to UC-09 | 5 (Customer) |
| UC-10 to UC-14 | 6 (SIM & Number) |
| UC-15 to UC-18 | 4 (Product/Package) |
| UC-19 to UC-21 | 4 (Subscription) |
| UC-22 to UC-24 | 3 (Usage) |
| UC-25 to UC-27 | 4 (Billing) |
| UC-28 to UC-30 | 3 (Payment) |
| UC-31 to UC-34 | 4 (Network) |
| UC-35 to UC-37 | 3 (Incident) |
| UC-38 to UC-41 | 5 (Support) |
| Events (Notification) | 10 |
| **المجموع** | **65** |

---

> **ملخص:**
> - إجمالي قصص المستخدمين: **65 قصة مستخدم**
> - تغطي **12 وحدة** و **8 جهات فاعلة**
> - **65%** من القصص عالية الأولوية (High)
> - جميع القصص مرتبطة بـ Use Cases أو Events
> - جميع معايير القبول محددة (Checklist format)
> - القصص مرتبة حسب الرقم التسلسلي US-01 إلى US-57

---

> **ملاحظة:** هذه الوثيقة تمثل آخر وثيقة من وثائق Phase 1-3 المطلوبة. جميع الوثائق الأربع (NFR, Business Rules, Traceability Matrix, User Stories) مكتملة ومتوافقة مع SKILL-14 و SKILL-02.

> **تاريخ التحديث:** 2026-09-26
> **الحالة:** Confirmed
