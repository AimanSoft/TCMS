# حالات الاستخدام عالية المستوى (High-Level Use Cases)

## High-Level Use Cases

## المقدمة
قائمة حالات الاستخدام عالية المستوى مستخرجة من العمليات الموثقة في `docs/system-operations.md`. كل حالة استخدام مرتبطة بوحدة محددة ومرتبطة بجهات فاعلة (Actors) من `docs/system-actors.md`.

---

## Identity Service

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-01 | Login | Identity Service | جميع المستخدمين | تسجيل دخول المستخدم إلى النظام باستخدام بيانات الاعتماد |
| UC-02 | Logout | Identity Service | جميع المستخدمين | تسجيل خروج المستخدم من النظام وإنهاء الجلسة |
| UC-03 | Register User | Identity Service | Super Admin, Company Admin | إنشاء حساب مستخدم جديد مع تحديد الدور والصلاحيات |
| UC-04 | Refresh Token | Identity Service | جميع المستخدمين | تحديث رمز الجلسة لمنح صلاحية الدخول لفترة جديدة |

---

## Customer Service

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-05 | Create Customer | Customer Service | Customer Service Employee, Super Admin, Company Admin | إضافة عميل جديد إلى النظام مع بياناته الأساسية |
| UC-06 | Update Customer | Customer Service | Customer Service Employee, Super Admin, Company Admin | تحديث بيانات عميل موجود |
| UC-07 | Delete Customer | Customer Service | Customer Service Employee, Super Admin | حذف عميل من النظام |
| UC-08 | List Customers | Customer Service | Customer Service Employee, Super Admin, Company Admin, Branch Manager | عرض قائمة بجميع العملاء المسجلين |
| UC-09 | View Customer Details | Customer Service | Customer Service Employee, Super Admin, Company Admin, Customer | عرض التفاصيل الكاملة لعميل معين |

---

## SIM & Number Service

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-10 | Create SIM | SIM & Number Service | Customer Service Employee, Super Admin | إنشاء سجل شريحة SIM جديدة في النظام |
| UC-11 | Activate SIM | SIM & Number Service | Customer Service Employee, Super Admin | تفعيل شريحة SIM لاستخدامها |
| UC-12 | Suspend SIM | SIM & Number Service | Customer Service Employee, Super Admin | إيقاف شريحة SIM بشكل مؤقت |
| UC-13 | Block SIM | SIM & Number Service | Customer Service Employee, Super Admin | حظر شريحة SIM بالكامل |
| UC-14 | Assign Phone Number | SIM & Number Service | Customer Service Employee, Super Admin | تخصيص رقم هاتف لشريحة SIM |

---

## Product / Package Service

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-15 | Create Package | Product / Package Service | Company Admin, Super Admin | إنشاء باقة خدمات جديدة مع تحديد المميزات والأسعار |
| UC-16 | Update Package | Product / Package Service | Company Admin, Super Admin | تحديث بيانات باقة موجودة |
| UC-17 | Delete Package | Product / Package Service | Company Admin, Super Admin | حذف باقة من النظام |
| UC-18 | List Packages | Product / Package Service | Company Admin, Super Admin, Branch Manager, Customer | عرض قائمة الباقات المتاحة |

---

## Subscription Service

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-19 | Create Subscription | Subscription Service | Customer Service Employee, Customer | إنشاء اشتراك جديد لعميل مع ربط الرقم والباقة |
| UC-20 | Renew Subscription | Subscription Service | Customer Service Employee, Customer | تجديد اشتراك قائم لفترة جديدة |
| UC-21 | Cancel Subscription | Subscription Service | Customer Service Employee, Customer | إلغاء اشتراك موجود |

---

## Usage Service

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-22 | Record Call | Usage Service | النظام (تلقائي) | تسجيل تفاصيل مكالمة تمت بواسطة المشترك |
| UC-23 | Record Message | Usage Service | النظام (تلقائي) | تسجيل تفاصيل رسالة تمت بواسطة المشترك |
| UC-24 | Record Internet Usage | Usage Service | النظام (تلقائي) | تسجيل استهلاك الإنترنت للمشترك |

---

## Billing Service

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-25 | Generate Invoice | Billing Service | Accountant, Customer Service Employee | إنشاء فاتورة جديدة بناءً على الاستخدام والاشتراك |
| UC-26 | Calculate Amounts | Billing Service | Accountant, Customer Service Employee | حساب المبالغ المستحقة على المشترك |
| UC-27 | Update Payment Status | Billing Service | Accountant, Customer Service Employee | تحديث حالة الدفع للفاتورة |

---

## Payment Service

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-28 | Process Payment | Payment Service | Accountant, Customer | تنفيذ عملية دفع لمبلغ مستحق |
| UC-29 | Recharge Balance | Payment Service | Customer, Accountant | شحن رصيد المشترك |
| UC-30 | Check Transaction Status | Payment Service | Accountant, Customer | الاستعلام عن حالة معاملة مالية |

---

## Network Service

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-31 | Manage Towers | Network Service | Network Engineer, Super Admin, Company Admin | إدارة الأبراج (إضافة، تحديث، حذف) |
| UC-32 | Manage Stations | Network Service | Network Engineer, Super Admin, Company Admin | إدارة المحطات (إضافة، تحديث، حذف) |
| UC-33 | Manage Devices | Network Service | Network Engineer, Super Admin | إدارة أجهزة الشبكة |
| UC-34 | Manage Coverage Areas | Network Service | Network Engineer, Super Admin | إدارة مناطق التغطية |

---

## Incident Service

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-35 | Report Incident | Incident Service | Network Engineer, Customer | تسجيل عطل تقني جديد في النظام |
| UC-36 | Assign Incident to Tower | Incident Service | Network Engineer | ربط العطل التقني ببرج معين |
| UC-37 | Track Incident | Incident Service | Network Engineer, Support Agent, Customer | متابعة حالة العطل التقني وحلّه |

---

## Support Service

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-38 | Create Ticket | Support Service | Customer | إنشاء تذكرة شكوى جديدة |
| UC-39 | Assign Ticket | Support Service | Support Agent, Super Admin | تعيين تذكرة لموظف دعم معين |
| UC-40 | Reply to Ticket | Support Service | Support Agent | الرد على تذكرة شكوى من قبل الموظف |
| UC-41 | Transfer Ticket | Support Service | Support Agent, Super Admin | تحويل تذكرة إلى قسم آخر |

---

## End-to-End Integrated Use Case

| الرقم | اسم حالة الاستخدام | الوحدة | الجهات الفاعلة | الوصف |
|---|---|---|---|---|
| UC-42 | Full Customer Lifecycle | متعدد الوحدات | Customer, Customer Service Employee | التدفق المتكامل: إنشاء عميل → تخصيص SIM → تخصيص رقم → إنشاء اشتراك → ربط باقة → إنشاء حساب فوترة → إنشاء فاتورة → إرسال إشعار |

---

## ملخص حالات الاستخدام

| الوحدة | عدد حالات الاستخدام | نطاق الأرقام |
|---|---|---|
| Identity Service | 4 | UC-01 إلى UC-04 |
| Customer Service | 5 | UC-05 إلى UC-09 |
| SIM & Number Service | 5 | UC-10 إلى UC-14 |
| Product / Package Service | 4 | UC-15 إلى UC-18 |
| Subscription Service | 3 | UC-19 إلى UC-21 |
| Usage Service | 3 | UC-22 إلى UC-24 |
| Billing Service | 3 | UC-25 إلى UC-27 |
| Payment Service | 3 | UC-28 إلى UC-30 |
| Network Service | 4 | UC-31 إلى UC-34 |
| Incident Service | 3 | UC-35 إلى UC-37 |
| Support Service | 4 | UC-38 إلى UC-41 |
| End-to-End | 1 | UC-42 |
| **المجموع** | **42** | **UC-01 إلى UC-42** |

---

> **تاريخ التحديث:** 2026-09-26
> **المصدر:** docs/system-operations.md و docs/system-actors.md
> **عدد حالات الاستخدام:** 42
> **الحالة:** Confirmed
