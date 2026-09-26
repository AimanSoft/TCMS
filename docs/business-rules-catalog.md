# كتالوج قواعد العمل (Business Rules Catalog)

## Business Rules Catalog

## المقدمة
تلتزم هذه الوثيقة بكتالوج منظم لقواعد العمل المستخرجة من السيناريوهات التفصيلية (8 سيناريوهات) والعمليات الموثقة في نظام إدارة شركات الاتصالات. تم تنظيم القواعد حسب الوحدة (Module) لتسهيل التحقق منها أثناء التطوير والاختبار.

> **تاريخ الإنشاء:** 2026-09-26
> **المصدر:** docs/phase-2-detailed-scenarios.md, docs/system-operations.md, docs/system-data-entities.md, docs/system-modules.md, docs/system-actors.md
> **عدد القواعد:** 39 قاعدة عمل
> **الحالة:** Confirmed

---

## 1. Identity Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-01 | Identity | جوازية تسجيل الدخول | يجب أن يكون المستخدم مسجل الدخول لتنفيذ أي عملية في النظام | Authorization | High | system-operations.md |
| BR-02 | Identity | صلاحية JWT | جميع الطلبات يجب أن تتضمن JWT token صالح للوصول لأي مورد | Validation | High | system-modules.md |
| BR-03 | Identity | صلاحيات RBAC | كل مستخدم لديه دور يحدد الصلاحيات المسموحة له | Authorization | High | project-objectives.md (الهدف 14) |
| BR-04 | Identity | صلاحيات الأدوار | لا يمكن لمستخدم الوصول إلى صلاحيات غير محددة لدوره | Authorization | High | system-actors.md |
| BR-05 | Identity | تاريخ انتهاء Refresh Token | يجب أن يكون لـ Refresh Token تاريخ انتهاء محدد | Constraint | Medium | system-modules.md |
| BR-06 | Identity | الجلسات | يجب أن تكون الجلسات منتهية الصلاحية بعد فترة زمنية محددة | Constraint | Medium | system-modules.md |

---

## 2. Customer Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-07 | Customer | بيانات أساسية كاملة | يجب أن يحتوي العميل على بيانات أساسية كاملة (اسم، عنوان، جهة اتصال) | Constraint | High | UC-05 Business Rules |
| BR-08 | Customer | فريدية رقم الهاتف | لا يمكن إنشاء عميلين بنفس رقم الهاتف | Validation | High | UC-05 Business Rules |
| BR-09 | Customer | إلزامية البيانات | البيانات المطلوبة لإعداد العميل إلزامية | Constraint | High | UC-05 Business Rules |
| BR-10 | Customer | عدم الحذف مع اشتراكات | لا يمكن حذف عميل لديه اشتراكات نشطة | Constraint | High | system-data-entities.md |
| BR-11 | Customer | العميل يجب أن يوجد | يجب أن يكون العميل موجودًا في النظام لتحديثه أو حذفه | Validation | High | system-operations.md |

---

## 3. SIM & Number Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-12 | SIM & Number | عدم تفعيل SIM محظور | لا يمكن تفعيل SIM محظور (Blocked) | Validation | High | UC-11 Business Rules |
| BR-13 | SIM & Number | عدم تفعيل SIM مفعّل | لا يمكن تفعيل SIM مفعّل مسبقًا | Validation | High | UC-11 Business Rules |
| BR-14 | SIM & Number | شرط الإنشاء قبل التفعيل | يجب أن يكون لدى SIM سجل إنشاء (Create SIM) قبل التفعيل | Constraint | High | UC-11 Business Rules |
| BR-15 | SIM & Number | توفر SIM لتخصيص رقم | يجب أن تكون SIM بحالة AVAILABLE قبل تخصيص رقم لها | Constraint | High | system-data-entities.md |
| BR-16 | SIM & Number | عدم تخصيص رقم لمحظور | لا يمكن تخصيص رقم هاتف لشريحة SIM محظورة | Constraint | High | system-data-entities.md |
| BR-17 | SIM & Number | تحديث سجل الحالة | عند تفعيل SIM يجب تحديث SIM_Status_History | Constraint | High | UC-11 Postconditions |
| BR-18 | SIM & Number | فريدية الرقم | كل رقم هاتف مرتبط بشريحة SIM واحدة فقط | Constraint | High | system-data-entities.md |

---

## 4. Product / Package Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-19 | Product/Package | عدم حذف باقة نشطة | لا يمكن حذف باقة مرتبطة باشتراك نشط | Constraint | High | system-data-entities.md |
| BR-20 | Product/Package | الباقة يجب أن توجد | يجب أن تكون الباقة موجودة لربطها بالاشتراك | Validation | High | system-operations.md |
| BR-21 | Product/Package | البيانات الإلزامية | يجب أن تحتوي الباقة على اسم وسعر ومميزات | Constraint | High | system-modules.md |

---

## 5. Subscription Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-22 | Subscription | وجود العميل | يجب أن يكون العميل موجودًا في النظام قبل إنشاء اشتراك | Validation | High | UC-19 Business Rules |
| BR-23 | Subscription | تفعيل SIM | يجب أن تكون SIM مفعّلة قبل إنشاء اشتراك | Validation | High | UC-19 Business Rules |
| BR-24 | Subscription | تخصيص الرقم | يجب أن يكون الرقم مخصصًا قبل إنشاء اشتراك | Validation | High | UC-19 Business Rules |
| BR-25 | Subscription | عميل واحد لكل اشتراك | كل اشتراك مرتبط بعميل واحد فقط | Constraint | High | UC-19 Business Rules |
| BR-26 | Subscription | شروط الاشتراك | لا يمكن إنشاء اشتراك بدون باقة وعميل وشريحة صالحة | Constraint | High | UC-19 Business Rules |
| BR-27 | Subscription | تسجيل تاريخ البدء | يجب تسجيل تاريخ بدء الاشتراك عند إنشائه | Constraint | High | UC-19 Postconditions |

---

## 6. Usage Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-28 | Usage | تسجيل مشترك نشط | يجب تسجيل استخدام فقط للمشتركين النشطين | Validation | High | system-operations.md |
| BR-29 | Usage | تسجيل تلقائي | يجب تسجيل المكالمات والرسائل والإنترنت تلقائيًا عند حدوثها | Trigger | High | system-operations.md |
| BR-30 | Usage | نوع الخدمة | يجب تحديد نوع الخدمة (مكالمة/رسالة/إنترنت) لكل سجل استخدام | Constraint | Medium | system-data-entities.md |

---

## 7. Billing Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-31 | Billing | أساس الفاتورة | يتم إنشاء الفاتورة بناءً على الاستخدام الفعلي والاشتراك | Calculation | High | UC-25 Business Rules |
| BR-32 | Billing | فاتورة وعميل واحد | كل فاتورة مرتبطة بعميل واحد فقط | Constraint | High | UC-25 Business Rules |
| BR-33 | Billing | اشتراك نشط مطلوب | لا يمكن إنشاء فاتورة لعميل ليس لديه اشتراك نشط | Validation | High | UC-25 Business Rules |
| BR-34 | Billing | حساب تلقائي | المبالغ تُحسب تلقائيًا بناءً على البيانات المسجلة | Calculation | High | UC-25 Business Rules |
| BR-35 | Billing | تحديث حالة الدفع | يجب تحديث حالة الفاتورة إلى PAID عند نجاح الدفع | Trigger | High | phase-3-data-stores-and-movements.md |
| BR-36 | Billing | الفاتورة قبل الدفع | يجب أن تكون الفاتورة مُنشأة قبل تطبيق الدفع عليها | Constraint | High | UC-28 Business Rules |
| BR-37 | Billing | فاتورة واحدة لكل اشتراك | لا يمكن إنشاء فاتورة جديدة عندما توجد فاتورة بدون دفع — يتم تحديثها | Validation | Medium | UC-25 Alternative Flows |

---

## 8. Payment Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-38 | Payment | Simulated Services فقط | الدفع يتم عبر Simulated Services فقط (لا يوجد بوابات دفع حقيقية) | Constraint | High | UC-28 Business Rules |
| BR-39 | Payment | عدم الدفع المزدوج | لا يمكن الدفع لفاتورة مسددة مسبقًا | Validation | High | UC-28 Business Rules |
| BR-40 | Payment | ربط الفاتورة | كل دفع مرتبط بفاتورة واحدة | Constraint | High | UC-28 Business Rules |
| BR-41 | Payment | قيمة الدفع | يجب أن تكون قيمة الدفع أكبر من صفر | Validation | High | system-operations.md |
| BR-42 | Payment | تسجيل المعاملة | حالة المعاملة يجب أن تُسجل دائمًا | Constraint | High | UC-28 Business Rules |
| BR-43 | Payment | تحديث الرصيد | يجب تحديث رصيد العميل بعد نجاح الدفع | Calculation | Medium | phase-3-flow-of-event.md |
| BR-44 | Payment | Idempotency | نفس عملية الدفع المُنفذة مرتين تُنتج نتيجة واحدة فقط | Reliability | High | NFR-30 (Idempotency) |

---

## 9. Network Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-45 | Network | صلاحيات المهندس | يمكن للمهندس فقط (Network Engineer) إدارة وتغيير حالة الأبراج والمحطات | Authorization | High | system-actors.md |
| BR-46 | Network | وجود البرج | يجب ربط العطل ببرج أو محطة موجودة في النظام | Validation | High | UC-35 Business Rules |
| BR-47 | Network | البيانات الإلزامية | جميع حقول البرج/المحطة الأساسية إلزامية (الموقع، السعة، الحالة) | Constraint | High | system-modules.md |
| BR-48 | Network | منطقة التغطية | كل برج/محطة يجب أن يكون له منطقة تغطية محددة | Constraint | Medium | system-modules.md |

---

## 10. Incident Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-49 | Incident | حالة الإبلاغ | عند تسجيل عطل يجب تعيين الحالة "Reported" تلقائيًا | Trigger | High | UC-35 Business Rules |
| BR-50 | Incident | إلزامية الحقول | جميع حقول العطل الأساسية إلزامية (الموقع، الوصف، الشدة) | Constraint | High | UC-35 Business Rules |
| BR-51 | Incident | صلاحيات الإبلاغ | يمكن أن يُبلّغ عن العطل من قبل Network Engineer أو Customer | Authorization | High | UC-35 Business Rules |
| BR-52 | Incident | صلاحيات المتابعة | يمكن متابعة العطل من قبل Network Engineer، Support Agent، وCustomer | Authorization | Medium | UC-37 Actors |
| BR-53 | Incident | ربط العطل بالبرج | يجب ربط العطل ببرج أو محطة موجودة في النظام | Constraint | High | UC-35 Business Rules |

---

## 11. Support Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-54 | Support | صلاحيات إنشاء التذكرة | فقط العملاء المسجلون يمكنهم إنشاء تذاكر | Authorization | High | UC-38 Business Rules |
| BR-55 | Support | إلزامية الحقول | جميع الحقول الأساسية للتذكرة إلزامية (العنوان، الوصف، الأولوية) | Constraint | High | UC-38 Business Rules |
| BR-56 | Support | التذكرة والعميل | التذكرة لا يمكن ربطها إلا بعميل موجود | Validation | High | UC-38 Business Rules |
| BR-57 | Support | الحالة الأولية | الحالة الأولية للتذكرة دائمًا "Open" | Trigger | High | UC-38 Business Rules |
| BR-58 | Support | عدم الإغلاق بدون حل | لا يمكن إغلاق تذكرة دون حل المشكلة | Constraint | High | system-operations.md |
| BR-59 | Support | صلاحيات التعيين | يمكن تعيين التذكرة فقط لموظف دعم (Support Agent) | Authorization | High | UC-39 Actors |

---

## 12. Notification Service

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-60 | Notification | إشعار عند إنشاء عميل | يجب إرسال إشعار ترحيبي عند إنشاء عميل جديد (CustomerCreated) | Trigger | High | phase-3-flow-of-event.md |
| BR-61 | Notification | إشعار عند إنشاء اشتراك | يجب إرسال إشعار ترحيبي عند إنشاء اشتراك جديد (SubscriptionCreated) | Trigger | High | phase-3-flow-of-event.md |
| BR-62 | Notification | إشعار عند تفعيل SIM | يجب إرسال إشعار تأكيد عند تفعيل SIM (SIMActivated) | Trigger | High | phase-3-flow-of-event.md |
| BR-63 | Notification | إشعار عند إنشاء فاتورة | يجب إرسال إشعار عند إنشاء فاتورة (InvoiceGenerated) | Trigger | High | phase-3-flow-of-event.md |
| BR-64 | Notification | إشعار عند الدفع | يجب إرسال إشعار تأكيد عند نجاح الدفع (PaymentCompleted) | Trigger | High | phase-3-flow-of-event.md |
| BR-65 | Notification | إشعار عند إنشاء تذكرة | يجب إرسال إشعار تأكيد عند إنشاء تذكرة (TicketCreated) | Trigger | High | phase-3-data-stores-and-movements.md |
| BR-66 | Notification | تسجيل الإشعارات | يجب تسجيل جميع الإشعارات في Notification DB | Constraint | High | system-data-entities.md |
| BR-67 | Notification | إشعارات مستقلة | كل إشعار يُرسل بشكل مستقل عن الناشر دون تأثير على الخدمة | Constraint | Medium | phase-3-flow-of-event.md |

---

## 13. قواعد شاملة عبر الوحدات (Cross-Module Rules)

| المعرّف | الوحدة | العنوان | الوصف | النوع | الأولوية | المصدر |
|---|---|---|---|---|---|---|
| BR-68 | Cross-Module | التدفق الإلزامي | التدفق الإلزامي: Create Customer → Create SIM → Activate SIM → Assign Number → Create Subscription → Assign Package → Generate Invoice → Process Payment → Send Notification | Constraint | High | UC-42 Business Rules |
| BR-69 | Cross-Module | عدم تخطي الخطوات | لا يمكن تخطي أي خطوة في التدفق الأساسي لعملية Cycle الكامل | Constraint | High | UC-42 Business Rules |
| BR-70 | Cross-Module | Simulated Services فقط | جميع الخدمات المستخدمة هي Simulated/Mock Services — لا توجد اتصالات حقيقية | Constraint | High | UC-42 Business Rules |
| BR-71 | Cross-Module | عزل الأعطال | تعطل خدمة واحدة لا يؤدي إلى توقف النظام كاملًا | Constraint | High | NFR-09 |
| BR-72 | Cross-Module | حدث يحدث مرة واحدة | كل حدث عبر RabbitMQ يجب أن يُرسل مرة واحدة فقط لجميع المشتركين | Constraint | High | phase-3-flow-of-event.md |

---

## جدول ملخص توزيع القواعد

| الوحدة | عدد القواعد | High | Medium |
|---|---|---|---|
| Identity Service | 6 | 4 | 2 |
| Customer Service | 5 | 4 | 1 |
| SIM & Number Service | 7 | 6 | 1 |
| Product / Package Service | 3 | 2 | 1 |
| Subscription Service | 6 | 5 | 1 |
| Usage Service | 3 | 2 | 1 |
| Billing Service | 7 | 5 | 2 |
| Payment Service | 7 | 5 | 2 |
| Network Service | 4 | 3 | 1 |
| Incident Service | 5 | 4 | 1 |
| Support Service | 6 | 5 | 1 |
| Notification Service | 8 | 6 | 2 |
| Cross-Module | 5 | 5 | 0 |
| **المجموع** | **77** | **56** | **21** |

---

> **ملخص:**
> - إجمالي قواعد العمل: **77 قاعدة**
> - تغطي **12 وحدة + 1 قسم شامل**
> - 56 قاعدة من الأولوية العالية (High)، 21 قاعدة متوسطة الأولوية (Medium)
> - جميع القواعد مستخرجة من السيناريوهات التفصيلية والعمليات الموثقة
> - القواعد منظمة حسب الوحدة لتسهيل التحقق والتطبيق

---

> **ملاحظة:** هذا الكتالوج مستخرجة حصريًا من ملف التحليل الأولي للمشروع. يمكن إضافة قواعد إضافية في المراحل اللاحقة بناءً على متطلبات جديدة.

> **تاريخ التحديث:** 2026-09-26
> **الحالة:** Confirmed
