# 🪪 TCMS — Project Identity Card

> Telecommunications Company Management System

## 1. معلومات أساسية
- **اسم المشروع:** نظام إدارة شركات الاتصالات
- **الاسم الإنجليزي:** Telecommunications Company Management System (TCMS)
- **المسار:** C:\TCMS
- **المؤلف:** أيمن سعيد أحمد سيف
- **الإشراف:** د. صلاح السياني
- **المادة:** تكامل وعمارة الأنظمة
- **الجامعة:** جامعة تعز
- **السنة:** 1447 هـ — 2026 م

## 2. الرؤية والهدف
- **فكرة المشروع:** نظام إدارة متكامل لشركات الاتصالات يهدف إلى تنظيم وإدارة العمليات الداخلية والخدمات المقدمة للعملاء، بما في ذلك إدارة الاشتراكات، الفواتير، العملاء، والبنية التحتية للشبكة.
- **الهدف الرئيسي:** تطوير نظام برمجي متكامل يتيح إدارة شركات الاتصالات بكفاءة من خلال توحيد العمليات الإدارية والفنية في منصة واحدة، مع ضمان دقة البيانات وسهولة الوصول إليها.
- **النطاق:** محاكاة إدارية (Administrative Simulation) لشركة اتصالات وهمية. جميع البيانات والعمليات Mock Data. لا يتصل بشبكات أو بوابات دفع أو أبراج حقيقية.

## 3. المعمارية التقنية
- **النمط:** Microservices + API Gateway + Event-Driven Architecture (RabbitMQ)
- **البيئة:** OpenCode / Docker / PostgreSQL / RabbitMQ
- **الواجهة:** واجهة ويب إدارية + لوحة تحكم
- **Security:** JWT + RBAC + bcrypt/argon2

## 4. الإحصائيات الرئيسية
| العنصر | العدد |
|---|---|
| الجهات الفاعلة (Actors) | 8 |
| الوحدات (Modules) | 11 (+ Notification = 12) |
| حالات الاستخدام (Use Cases) | 42 |
| قصص المستخدم (User Stories) | 65 (US-01 إلى US-57) |
| قواعد العمل (Business Rules) | 77 (BR-01 إلى BR-77) |
| المتطلبات غير الوظيفية (NFRs) | 34 (NFR-01 إلى NFR-34) |
| مخازن البيانات (Data Stores) | 12 |
| الأحداث (Events) | 10 |
| ملفات الوثائق (Docs) | 24 |

## 5. حالة المراحل (Phases Status)
| المرحلة | الحالة | المهام |
|---|---|---|
| Phase 1 — Project Planning & Initial Analysis | ✅ Completed | 11/11 |
| Phase 2 — Use Case Analysis | ✅ Completed | 6/6 |
| Phase 3 — System Flows & Data Flow | ✅ Completed | 8/8 |
| Phase 4 — Website Structure & UI/UX | 🔄 In Progress | 0/13 |
| Phase 5 — System Architecture & Integration | ⬜ Not Started | — |
| Phase 6 — Implementation | ⬜ Not Started | — |
| Phase 7 — Testing, Audit & Final Review | ⬜ Not Started | — |

## 6. الملفات الرئيسية (Key Files)
| الملف | الوصف |
|---|---|
| docs/implement-plan.md | خطة التنفيذ الرئيسية |
| docs/project-idea.md | فكرة المشروع |
| docs/project-objectives.md | الأهداف الرئيسية (16 هدف) |
| docs/project-scope.md | نطاق المشروع |
| docs/project-boundaries.md | حدود المشروع التقنية والوظيفية |
| docs/system-actors.md | الجهات الفاعلة (8 Actors) |
| docs/system-modules.md | الوحدات الرئيسية (11 Modules) |
| docs/system-operations.md | العمليات الأولية |
| docs/system-data-entities.md | كيانات البيانات (12 مجموعة) |
| docs/system-nfr.md | المتطلبات غير الوظيفية (34 NFR) |
| docs/business-rules-catalog.md | كتالوج قواعد العمل (77 Rule) |
| docs/traceability-matrix.md | مصفوفة التتبع (5 مصفوفات) |
| docs/user-stories.md | قصص المستخدم (65 Story) |
| docs/skills-compliance-review.md | مراجعة توافق المهارات |
| docs/phase-2-actors-review.md | مراجعة الجهات الفاعلة Phase 2 |
| docs/phase-2-detailed-scenarios.md | السيناريوهات التفصيلية Phase 2 |
| docs/phase-2-use-case-diagrams.md | مخططات حالات الاستخدام Phase 2 |
| docs/phase-2-use-cases-high-level.md | حالات الاستخدام عالية المستوى Phase 2 |
| docs/phase-3-use-cases-review.md | مراجعة حالات الاستخدام Phase 3 |
| docs/phase-3-flow-of-action.md | تدفقات العمليات Phase 3 (8 مخططات) |
| docs/phase-3-flow-of-event.md | تدفقات الأحداث Phase 3 (6 مخططات) |
| docs/phase-3-dfd-level-0.md | مخطط السياق DFD Level 0 |
| docs/phase-3-dfd-level-1.md | المعمليات الرئيسية DFD Level 1 |
| docs/phase-3-data-stores-and-movements.md | مخازن البيانات والتدفقات Phase 3 |

## 7. المهارات المطبقة (Skills Applied)
- SKILL-02: SDLC — ✅ متوافق
- SKILL-07: Architecture — (لـ Phase 5)
- SKILL-13: HCI & UI/UX — (لـ Phase 4 الحالي)
- SKILL-14: Requirements Engineering — ✅ متوافق
- SKILL-01: Project Management — ✅ متوافق
- SKILL-05: Quality Assurance — (لـ Phase 7)
- SKILL-06: Security — (لـ Phase 5-6)
- SKILL-10: Agile Methodologies — (متوافق)

## 8. معرفات المشروع (IDs)
- **Objectives:** OBJ-01 إلى OBJ-16
- **Actors:** 8 معرفات (Super Admin, Company Admin, Branch Manager, Customer Service Employee, Accountant, Network Engineer, Support Agent, Customer)
- **Modules:** M1 إلى M12 (M12 = Notification)
- **Use Cases:** UC-01 إلى UC-42
- **User Stories:** US-01 إلى US-57 (65 قصة)
- **Business Rules:** BR-01 إلى BR-77
- **NFRs:** NFR-01 إلى NFR-34

## 9. هيكل الوثائق
```
C:\TCMS\
├── docs\                          (24 ملف وثائقي)
│   ├── project-idea.md
│   ├── project-objectives.md
│   ├── project-scope.md
│   ├── project-boundaries.md
│   ├── system-actors.md
│   ├── system-modules.md
│   ├── system-operations.md
│   ├── system-data-entities.md
│   ├── system-nfr.md
│   ├── business-rules-catalog.md
│   ├── traceability-matrix.md
│   ├── user-stories.md
│   ├── skills-compliance-review.md
│   ├── implement-plan.md
│   ├── phase-2-*.md (4 files)
│   └── phase-3-*.md (6 files)
├── phases\
│   ├── phase-1\todo.md            (11/11 ✅)
│   ├── phase-2\todo.md            (6/6 ✅)
│   ├── phase-3\todo.md            (8/8 ✅)
│   └── phase-4\todo.md            (0/13 🔄)
├── skills\                        (14 مهارة)
│   └── 01_...14_*\SKILL.md
├── docs\PROJECT-CARD.md           ← هذه البطاقة
├── package.json, package-lock.json
└── .github\ISSUE_TEMPLATE\        (4 قوالب)
```

## 10. روابط سريعة (للأدوات الأخرى)
- **Repository:** C:\TCMS
- **Documentation Root:** C:\TCMS\docs\
- **Phases Root:** C:\TCMS\phases\
- **Skills Root:** C:\TCMS\skills\

## 11. تعليمات للربط مع أدوات AI أخرى
هذه البطاقة مصممة ليتم نسخها إلى أي أداة AI (Cursor، Claude، ChatGPT، GitHub Copilot، إلخ) لتوفير سياق كامل عن المشروع في أقل عدد من الرموز.

عند ربط المشروع بأداة أخرى:
1. انسخ محتوى هذه البطاقة.
2. الصقها في بداية المحادثة.
3. أضف سؤالك أو طلبك.
4. الأداة ستفهم المشروع فورًا.

---
*آخر تحديث: 2026-09-26*
*الإصدار: 1.0*
