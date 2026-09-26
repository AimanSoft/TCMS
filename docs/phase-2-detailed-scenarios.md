# السيناريوهات التفصيلية لحالات الاستخدام

## Detailed Use Case Scenarios

## المقدمة
السيناريوهات التفصيلية للحالات الأساسية الثمانية، مستخرجة من العمليات الموثقة في `docs/system-operations.md` والكيانات الموثقة في `docs/system-data-entities.md` والجهات الفاعلة الموثقة في `docs/system-actors.md`.

---

## UC-05: Create Customer

- **Use Case ID:** UC-05
- **Use Case Name:** Create Customer
- **Primary Actor:** Customer Service Employee
- **Secondary Actors:** Super Admin, Company Admin
- **Preconditions:**
  - المستخدم مسجل الدخول إلى النظام (UC-01 تم تنفيذه)
  - العميل غير مسجل مسبقًا في النظام
  - Customer Service Employee لديه الصلاحية المناسبة
- **Main Success Scenario:**
  1. يقوم Customer Service Employee باختيار خيار "إنشاء عميل جديد"
  2. يقوم بإدخال بيانات العميل الأساسية (الاسم، العنوان، جهات الاتصال)
  3. يقوم النظام بالتحقق من صحة البيانات المدخلة
  4. يقوم النظام بإنشاء سجل عميل جديد في جدول Customers
  5. يتم تعريف Customer ID تلقائيًا
  6. يعرض النظام تأكيد إنشاء العميل بنجاح
- **Alternative Flows:**
  - **3a:** إذا كانت البيانات غير صالحة → يطلب النظام إعادة إدخال البيانات
  - **3b:** إذا كان العميل مسجل مسبقًا → يعرض النظام رسالة خطأ
- **Postconditions:**
  - يوجد سجل جديد في جدول Customers
  - يتم إنشاء Customer ID فريد
  - يمكن ربط هذا العميل بشريحة SIM أو اشتراك في المراحل اللاحقة
- **Business Rules:**
  - يجب أن يحتوي العميل على بيانات أساسية كاملة (اسم، عنوان، جهة اتصال)
  - لا يمكن إنشاء عميلين بنفس رقم الهاتف
  - البيانات المطلوبة إلزامية

---

## UC-11: Activate SIM

- **Use Case ID:** UC-11
- **Use Case Name:** Activate SIM
- **Primary Actor:** Customer Service Employee
- **Secondary Actors:** Super Admin
- **Preconditions:**
  - المستخدم مسجل الدخول إلى النظام
  - يوجد سجل SIM في النظام (تم إنشاؤه عبر UC-10)
  - SIM في حالة غير مفعّلة (غير Activated)
  - Customer Service Employee لديه الصلاحية المناسبة
- **Main Success Scenario:**
  1. يقوم Customer Service Employee باختيار SIM المطلوب تفعيله
  2. يقوم النظام بالتحقق من حالة SIM الحالية
  3. يقوم Customer Service Employee بتأكيد عملية التفعيل
  4. يقوم النظام بتغيير حالة SIM إلى "Activated"
  5. يقوم النظام بإنشاء سجل تفعيل جديد في SIM_Activations
  6. يقوم النظام بتحديث SIM_Status_History بحالة التفعيل
  7. يعرض النظام تأكيد التفعيل
- **Alternative Flows:**
  - **2a:** إذا كانت حالة SIM "Blocked" → لا يمكن تفعيلها، يعرض النظام رسالة خطأ
  - **2b:** إذا كانت حالة SIM "Activated" مسبقًا → يعرض النظام رسالة تحذير
- **Postconditions:**
  - حالة SIM تتحول إلى "Activated"
  - يوجد سجل تفعيل جديد في SIM_Activations
  - SIM_Status_History يتم تحديثه
  - يمكن ربط SIM برقم هاتف أو اشتراك
- **Business Rules:**
  - لا يمكن تفعيل SIM محظور (Blocked)
  - لا يمكن تفعيل SIM مفعّل مسبقًا
  - يجب أن يكون لدى SIM سجل إنشاء (Create SIM) قبل التفعيل

---

## UC-19: Create Subscription

- **Use Case ID:** UC-19
- **Use Case Name:** Create Subscription
- **Primary Actor:** Customer Service Employee
- **Secondary Actors:** Customer, System
- **Preconditions:**
  - المستخدم مسجل الدخول إلى النظام
  - العميل موجود في النظام (UC-05 تم تنفيذه)
  - SIM موجودة ومفعّلة (UC-11 تم تنفيذها)
  - رقم هاتف مخصص للعميل (UC-14 تم تنفيذه)
  - Customer Service Employee لديه الصلاحية المناسبة
- **Main Success Scenario:**
  1. يقوم Customer Service Employee بإنشاء اشتراك جديد
  2. يقوم النظام بالربط بين العميل وSIM والرقم
  3. يقوم Customer Service Employee باختيار الباقة المطلوبة (أو تُربط لاحقًا عبر UC-42)
  4. يقوم النظام بإنشاء سجل اشتراك جديد في جدول Subscriptions
  5. يقوم النظام بتسجيل تاريخ بدء الاشتراك
  6. يقوم النظام بتحديث حالة العميل إلى "Active Subscriber"
  7. يعرض النظام تأكيد إنشاء الاشتراك
- **Alternative Flows:**
  - **2a:** إذا لم يكن العميل موجودًا → يعرض النظام رسالة خطأ
  - **2b:** إذا لم تكن SIM مفعّلة → يعرض النظام رسالة تحذير
- **Postconditions:**
  - يوجد سجل جديد في جدول Subscriptions
  - العميل مُسجّل كمشترك نشط
  - الاشتراك مرتبط بالعميل وSIM والرقم
  - تاريخ بدء الاشتراك مسجل
- **Business Rules:**
  - يجب أن يكون العميل موجودًا في النظام قبل إنشاء اشتراك
  - يجب أن تكون SIM مفعّلة
  - يجب أن يكون الرقم مخصصًا
  - كل اشتراك مرتبط بعميل واحد فقط

---

## UC-25: Generate Invoice

- **Use Case ID:** UC-25
- **Use Case Name:** Generate Invoice
- **Primary Actor:** Accountant
- **Secondary Actors:** Customer Service Employee, System
- **Preconditions:**
  - المستخدم (Accountant) مسجل الدخول إلى النظام
  - يوجد سجلات استخدام مسجلة (UC-22/23/24 تم تنفيذها)
  - يوجد اشتراك نشط للعميل (UC-19 تم تنفيذه)
  - Accountant لديه الصلاحية المناسبة
- **Main Success Scenario:**
  1. يقوم Accountant بطلب إنشاء فاتورة لعميل معين
  2. يقوم النظام بجمع جميع سجلات الاستخدام والاشتراكات المستحقة
  3. يقوم النظام بحساب المبالغ المستحقة (UC-26)
  4. يقوم النظام بإنشاء فاتورة جديدة في جدول Invoices
  5. يقوم النظام بإنشاء بنود الفاتورة في جدول Invoice_Items
  6. يقوم النظام بتحديث حالة الفاتورة إلى "Pending"
  7. يعرض النظام الفاتورة المُنشأة
- **Alternative Flows:**
  - **2a:** إذا لم توجد سجلات استخدام → يقوم النظام بإنشاء فاتورة باشتراك فقط
  - **3a:** إذا كانت هناك فاتورة موجودة بدون دفع → يقوم النظام بتحديثها بدلًا من إنشاء جديدة
- **Postconditions:**
  - يوجد سجل جديد في جدول Invoices
  - توجد بنود فاتورة في جدول Invoice_Items
  - حالة الفاتورة تكون "Pending"
  - تاريخ الإنشاء مسجل
- **Business Rules:**
  - يتم إنشاء الفاتورة بناءً على الاستخدام الفعلي والاشتراك
  - كل فاتورة مرتبطة بعميل واحد
  - لا يمكن إنشاء فاتورة لعميل ليس لديه اشتراك نشط
  - المبالغ تُحسب تلقائيًا بناءً على البيانات المسجلة

---

## UC-28: Process Payment

- **Use Case ID:** UC-28
- **Use Case Name:** Process Payment
- **Primary Actor:** Accountant, Customer
- **Secondary Actors:** System
- **Preconditions:**
  - المستخدم مسجل الدخول إلى النظام
  - يوجد فاتورة بمستحقات (فاتورة بحالة "Pending" أو "Overdue")
  - عملية الدفع مدعومة عبر Simulated Services
  - المستخدم لديه الصلاحية المناسبة
- **Main Success Scenario:**
  1. يقوم المستخدم باختيار الفاتورة المطلوب الدفع لها
  2. يقوم النظام بعرض تفاصيل الفاتورة والمبلغ المستحق
  3. يقوم المستخدم بتأكيد عملية الدفع
  4. يقوم النظام بتنفيذ عملية الدفع عبر Simulated Payment Gateway
  5. يقوم النظام بإنشاء سجل دفع جديد في جدول Payments
  6. يقوم النظام بإنشاء سجل معاملة في جدول Transactions
  7. يقوم النظام بتحديث حالة الفاتورة إلى "Paid"
  8. يعرض النظام تأكيد الدفع
- **Alternative Flows:**
  - **3a:** إذا فشل الدفع → يقوم النظام بإبقاء حالة الفاتورة "Pending" وعرض رسالة خطأ
  - **2a:** إذا كانت الفاتورة مسددة بالفعل → يعرض النظام رسالة تحذير
- **Postconditions:**
  - يوجد سجل جديد في جدول Payments
  - يوجد سجل جديد في جدول Transactions
  - حالة الفاتورة تتحول إلى "Paid"
  - يتم تحديث رصيد العميل
- **Business Rules:**
  - الدفع يتم عبر Simulated Services فقط (لا يوجد بوابات دفع حقيقية)
  - لا يمكن الدفع لفاتورة مسددة مسبقًا
  - كل دفع مرتبط بفاتورة واحدة
  - حالة المعاملة يتم تسجيلها دائمًا

---

## UC-35: Report Incident

- **Use Case ID:** UC-35
- **Use Case Name:** Report Incident
- **Primary Actor:** Network Engineer, Customer
- **Secondary Actors:** Support Agent, System
- **Preconditions:**
  - المستخدم مسجل الدخول إلى النظام
  - البرج/المحطة موجودة في النظام (UC-31/32 تم تنفيذها)
  - المستخدم لديه الصلاحية المناسبة
- **Main Success Scenario:**
  1. يقوم المستخدم باختيار خيار "إبلاغ عن عطل"
  2. يقوم المستخدم بإدخال تفاصيل العطل (الموقع، الوصف، الشدة)
  3. يقوم النظام بالتحقق من صلاحية البيانات المدخلة
  4. يقوم النظام بإنشاء سجل عطل جديد في جدول Incidents
  5. يقوم النظام بتعيين حالة العطل إلى "Reported"
  6. يقوم النظام بتسجيل تاريخ الإبلاغ
  7. يعرض النظام تأكيد الإبلاغ ورقم العطل
- **Alternative Flows:**
  - **2a:** إذا كانت بيانات العطل غير صالحة → يطلب النظام إعادة الإدخال
  - **3a:** إذا لم تكن البرج/المحطة موجودة → يعرض النظام رسالة خطأ
- **Postconditions:**
  - يوجد سجل جديد في جدول Incidents
  - حالة العطل تكون "Reported"
  - تاريخ الإبلاغ مسجل
  - يمكن ربط العطل ببرج لاحقًا (UC-36)
- **Business Rules:**
  - يجب ربط العطل ببرج أو محطة موجودة في النظام
  - جميع حقول العطل الأساسية إلزامية
  - العطل يمكن أن يُبلّغ عنه من قبل Network Engineer أو Customer
  - حالة العطل الأولية تكون دائمًا "Reported"

---

## UC-38: Create Ticket

- **Use Case ID:** UC-38
- **Use Case Name:** Create Ticket
- **Primary Actor:** Customer
- **Secondary Actors:** Support Agent, System
- **Preconditions:**
  - العميل مسجل الدخول إلى النظام
  - العميل موجود في النظام (UC-05 تم تنفيذه)
  - العميل لديه اشتراك أو مشكلة مُبلّغ عنها
- **Main Success Scenario:**
  1. يقوم Customer باختيار خيار "إنشاء تذكرة شكوى"
  2. يقوم Customer بإدخال تفاصيل الشكوى (العنوان، الوصف، الأولوية)
  3. يقوم النظام بالتحقق من صحة البيانات
  4. يقوم النظام بإنشاء تذكرة جديدة في جدول Tickets
  5. يقوم النظام بتعيين الحالة الأولية للتذكرة إلى "Open"
  6. يقوم النظام بتعيين رقم تذكرة تلقائي
  7. يقوم النظام بتسجيل تاريخ إنشاء التذكرة
  8. يعرض النظام تأكيد إنشاء التذكرة
- **Alternative Flows:**
  - **2a:** إذا كانت بيانات الشكوى فارغة → يطلب النظام إدخال التفاصيل
  - **3a:** إذا لم يكن العميل مسجلًا → يعرض النظام رسالة خطأ
- **Postconditions:**
  - يوجد سجل جديد في جدول Tickets
  - الحالة الأولية تكون "Open"
  - رقم التذكرة مُعين تلقائيًا
  - التذكرة مرتبطة بالعميل
- **Business Rules:**
  - فقط العملاء المسجلين يمكنهم إنشاء تذاكر
  - جميع الحقول الأساسية للتذكرة إلزامية
  - التذكرة لا يمكن ربطها إلا بعميل موجود
  - الحالة الأولية دائمًا "Open"

---

## UC-42: Full Customer Lifecycle (End-to-End)

- **Use Case ID:** UC-42
- **Use Case Name:** Full Customer Lifecycle
- **Primary Actor:** Customer, Customer Service Employee
- **Secondary Actors:** System, Accountant, Network Engineer
- **Preconditions:**
  - المستخدم مسجل الدخول إلى النظام
  - لا يوجد عميل مسبق (هذا عميل جديد)
  - النظام جاهز للاستخدام (Simulated Services متاحة)
- **Main Success Scenario:**
  1. **خطوة 1 (UC-05):** يقوم Customer Service Employee بإنشاء عميل جديد
  2. **خطوة 2 (UC-10):** يقوم Customer Service Employee بإنشاء شريحة SIM جديدة
  3. **خطوة 3 (UC-11):** يقوم Customer Service Employee بتفعيل الشريحة
  4. **خطوة 4 (UC-14):** يقوم Customer Service Employee بتخصيص رقم هاتف للشريحة
  5. **خطوة 5 (UC-19):** يقوم Customer Service Employee بإنشاء اشتراك جديد للعميل
  6. **خطوة 6 (UC-15):** يقوم Company Admin بإنشاء باقة أو يختار باقة موجودة
  7. **خطوة 7 (ربط الباقة):** يتم ربط الباقة بالاشتراك
  8. **خطوة 8 (UC-25):** يقوم Accountant بإنشاء فاتورة بناءً على الاشتراك والباقة
  9. **خطوة 9 (UC-28):** يقوم Customer بدفع المبلغ عبر Simulated Payment
  10. **خطوة 10:** يقوم النظام بإرسال إشعار تأكيد (SMS/Email Mock)
- **Alternative Flows:**
  - **1a:** إذا كان العميل موجودًا → يتم تخطي خطوة الإنشاء والانتقال لتخصيص SIM
  - **9a:** إذا فشل الدفع → تبقى الحالة "Pending" ويُطلب إعادة المحاولة
- **Postconditions:**
  - العميل مُنشأ ومسجّل في النظام
  - SIM مُنشأة ومفعّلة
  - رقم هاتف مخصص
  - اشتراك نشط مرتبط بالعميل والرقم والباقة
  - فاتورة مُنشأة ومدفوعة
  - إشعار تأكيد مُرسل
  - جميع السجلات موجودة في جداول البيانات المناسبة
- **Business Rules:**
  - التدفق الإلزامي: Create Customer → Create SIM → Activate SIM → Assign Number → Create Subscription → Assign Package → Generate Invoice → Process Payment → Send Notification
  - لا يمكن تخطي أي خطوة في التدفق الأساسي
  - جميع الخدمات المستخدمة هي Simulated/Mock Services
  - لا يتضمن هذا التدفق أي شبكات أو بوابات دفع حقيقية
  - كل خطوة مرتبطة بالنتائج المتوقعة من العمليات الموثقة سابقًا

---

> **تاريخ التحديث:** 2026-09-26
> **المصدر:** docs/system-operations.md و docs/system-data-entities.md و docs/system-actors.md
> **عدد السيناريوهات:** 8
> **الحالة:** Confirmed
