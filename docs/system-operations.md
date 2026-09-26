# العمليات الأولية للنظام

## System Operations

## المقدمة
قائمة العمليات الرئيسية للنظام على مستوى عالٍ، مستخرجة من التحليل الأولي للمشروع. كل عملية مرتبطة بوحدة محددة أو تمثل عمليات متكاملة عبر وحدات متعددة.

---

## 1. Identity Service Operations

| العملية | الوصف |
|---|---|
| تسجيل الدخول (Login) | تسجيل دخول المستخدم إلى النظام |
| تسجيل الخروج (Logout) | تسجيل خروج المستخدم من النظام |
| تسجيل مستخدم جديد (Register) | إنشاء حساب مستخدم جديد |
| تحديث رمز الدخول (Refresh Token) | تحديث رمز الجلسة |

---

## 2. Customer Service Operations

| العملية | الوصف |
|---|---|
| إنشاء عميل (Create Customer) | إضافة عميل جديد إلى النظام |
| تحديث بيانات عميل (Update Customer) | تحديث بيانات عميل موجود |
| حذف عميل (Delete Customer) | حذف عميل من النظام |
| عرض قائمة العملاء (List Customers) | عرض قائمة بجميع العملاء |
| عرض تفاصيل عميل (Get Customer Details) | عرض التفاصيل الكاملة لعميل معين |

---

## 3. SIM & Number Service Operations

| العملية | الوصف |
|---|---|
| إنشاء شريحة SIM (Create SIM) | إنشاء سجل شريحة SIM جديدة |
| تفعيل شريحة (Activate SIM) | تفعيل شريحة SIM |
| إيقاف شريحة مؤقتًا (Suspend SIM) | إيقاف شريحة SIM بشكل مؤقت |
| حظر شريحة (Block SIM) | حظر شريحة SIM بالكامل |
| تخصيص رقم هاتف (Assign Number) | تخصيص رقم هاتف لشريحة SIM |

---

## 4. Product / Package Service Operations

| العملية | الوصف |
|---|---|
| إنشاء باقة (Create Package) | إنشاء باقة خدمات جديدة |
| تحديث باقة (Update Package) | تحديث بيانات باقة موجودة |
| حذف باقة (Delete Package) | حذف باقة من النظام |
| عرض الباقات (List Packages) | عرض قائمة الباقات المتاحة |

---

## 5. Subscription Service Operations

| العملية | الوصف |
|---|---|
| إنشاء اشتراك (Create Subscription) | إنشاء اشتراك جديد لعميل |
| تجديد اشتراك (Renew Subscription) | تجديد اشتراك قائم |
| إلغاء اشتراك (Cancel Subscription) | إلغاء اشتراك |

---

## 6. Usage Service Operations

| العملية | الوصف |
|---|---|
| تسجيل مكالمة (Record Call) | تسجيل تفاصيل مكالمة |
| تسجيل رسالة (Record Message) | تسجيل تفاصيل رسالة |
| تسجيل استهلاك إنترنت (Record Internet Usage) | تسجيل استهلاك الإنترنت |

---

## 7. Billing Service Operations

| العملية | الوصف |
|---|---|
| إنشاء فاتورة (Generate Invoice) | إنشاء فاتورة جديدة |
| حساب المبالغ (Calculate Amounts) | حساب المبالغ المستحقة |
| تحديث حالة الدفع (Update Payment Status) | تحديث حالة الدفع للفاتورة |

---

## 8. Payment Service Operations

| العملية | الوصف |
|---|---|
| تنفيذ عملية دفع (Process Payment) | تنفيذ عملية دفع |
| شحن رصيد (Recharge) | شحن رصيد المشترك |
| الاستعلام عن حالة المعاملة (Check Transaction Status) | الاستعلام عن حالة معاملة |

---

## 9. Network Service Operations

| العملية | الوصف |
|---|---|
| إدارة الأبراج (Manage Towers) | إضافة/تحديث/حذف الأبراج |
| إدارة المحطات (Manage Stations) | إضافة/تحديث/حذف المحطات |
| إدارة الأجهزة (Manage Devices) | إدارة أجهزة الشبكة |
| إدارة مناطق التغطية (Manage Coverage Areas) | إدارة مناطق التغطية |

---

## 10. Incident Service Operations

| العملية | الوصف |
|---|---|
| تسجيل عطل (Report Incident) | تسجيل عطل تقني جديد |
| ربط العطل بالبرج (Assign Incident to Tower) | ربط العطل ببرج معين |
| متابعة العطل (Track Incident) | متابعة حالة العطل |

---

## 11. Support Service Operations

| العملية | الوصف |
|---|---|
| إنشاء تذكرة (Create Ticket) | إنشاء تذكرة شكوى جديدة |
| تعيين تذكرة (Assign Ticket) | تعيين تذكرة لموظف معين |
| الرد على تذكرة (Reply to Ticket) | الرد على تذكرة |
| تحويل تذكرة (Transfer Ticket) | تحويل تذكرة لقسم آخر |

---

## 12. العمليات المتكاملة (End-to-End)

تُمثل العمليات التالية تدفقًا متكاملًا عبر وحدات متعددة:

| الخطوة | العملية | الوحدة المصدر |
|---|---|---|
| 1 | إنشاء عميل (Create Customer) | Customer Service |
| 2 | تخصيص SIM (Assign SIM) | SIM & Number Service |
| 3 | تخصيص رقم (Assign Number) | SIM & Number Service |
| 4 | إنشاء اشتراك (Create Subscription) | Subscription Service |
| 5 | ربط الباقة (Assign Package) | Product / Package Service |
| 6 | إنشاء حساب فوترة (Create Billing Account) | Billing Service |
| 7 | إنشاء فاتورة (Generate Invoice) | Billing Service |
| 8 | إرسال إشعار (Send Notification) | Notification (SMS/Email Mock) |

---

> **تاريخ التحديث:** 2026-09-26
> **المصدر:** ملف التحليل الأولي للمشروع
> **الحالة:** Confirmed
