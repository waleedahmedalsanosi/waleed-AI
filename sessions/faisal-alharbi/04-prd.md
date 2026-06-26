# Product Requirements Document

**Company:** Munaqis (مناقص)
**Product:** Munaqis — Contractor Qualification SaaS
**Version:** 1.0
**Date:** 2026-06-26
**Authors:** Waleed Al-Sanosi (PM), Dreamy

---

## 1. Product Vision

**One-line vision:** Munaqis enables Saudi SME contractors to win more government and corporate tenders by organizing their qualification data once and generating professional tender files on demand.

**Problem statement:** Small and medium contractors in Saudi Arabia (5–50 employees) routinely lose tenders not because they lack the capacity to deliver the work, but because their qualification files are incomplete, inconsistently formatted, or submitted past deadline. The Etimad and Monafasat government procurement portals are designed for large enterprise contractors — SMEs have no dedicated tooling and rely on spreadsheets, paper files, and personal connections. Each new tender requires re-assembling the same documents from scratch, a process that costs hours and is prone to errors that disqualify otherwise-capable bidders.

**Success metric (v1):** At least 10 paying contractors (at any tier) actively using the platform to generate qualification files within 60 days of launch.

---

## 2. Users

### Primary User
- **Who:** Owner or bid coordinator at a Saudi SME contracting company (5–50 employees) in construction, facilities management, or subcontracting. Located in Riyadh or Jeddah.
- **Pain:** Re-assembles qualification documents from scratch for every tender; frequently submits incomplete files; loses track of deadlines; no organized record of past projects.
- **Job to be done:** Submit a professional, complete tender qualification packet without spending days gathering and reformatting documents every time.

### Secondary Users
- **Accountant / admin staff at the same company** — may be delegated data entry tasks (document uploads, project history logging) but is not the primary buyer or decision-maker.

---

## 3. Scope

### In Scope (v1 MVP)

1. **Contractor Company Profile** — Structured data entry form capturing the core company record: commercial registration number, IBAN, VAT registration, company classification tier (as assigned by Ministry of Commerce), specializations, and license expiry dates. Stored as the master data record the platform generates from.

2. **Document Vault** — Secure upload and storage of supporting qualification documents: CR certificate, VAT certificate, GOSI certificate, Zakat certificate, professional licenses, ISO certifications. Each document tagged with expiry date and auto-flagged when within 30 days of expiry.

3. **Project History Log** — Structured entry for past projects: client name, contract value, project type, start/end date, contact reference. Searchable and filterable. Used as the source for "similar project experience" sections of qualification files.

4. **Qualification File Generator** — Takes a tender's specific requirements (entered manually by the user or uploaded as a PDF checklist) and assembles a qualification packet from the company profile, document vault, and project history. Outputs as a structured PDF ready for submission.

5. **Tender Deadline Tracker** — Calendar view of active tender deadlines and document expiry dates. Email or SMS reminder at 7 days and 1 day before each deadline.

6. **Multi-tier Subscription & Billing** — SAR 500/month (Small tier: up to 20 employees, up to 5 active tenders tracked simultaneously) and SAR 1,500/month (Medium tier: up to 50 employees, unlimited active tenders). Billing via Saudi payment gateway (Moyasar or HyperPay).

### Out of Scope (v1)

1. **Government portal API integration (Etimad, Monafasat)** — API access from these portals is unconfirmed as of intake. v1 is a manual-input, manual-submission model. Auto-submission to portals is Phase 2 contingent on API feasibility confirmation.

2. **AI-based tender recommendation engine** — Matching contractors to open tenders based on profile and past projects. Deferred to Phase 2; requires a live tender data feed and ML infrastructure.

3. **Mobile apps (iOS / Android)** — Web-first in v1. Mobile app development deferred until web product achieves product-market fit.

4. **Gulf market expansion (Kuwait, UAE)** — Each country has distinct portal requirements and regulatory frameworks. KSA first. Expansion post-PMF.

5. **Multi-user / team accounts** — In v1, one account per company (single login). Role-based access for multiple staff members is a Phase 2 feature.

6. **Automated tender discovery and alerts** — Scraping or monitoring government portals for new matching tenders. Deferred pending portal API policy clarity.

---

## 4. User Stories

**Contractor Company Profile**
> As a bid coordinator, I want to enter my company's registration, classification, and license data once so that I never have to re-enter this information for a new tender.
> Acceptance criteria:
> - User can enter and save all required fields (CR number, VAT, GOSI, Zakat, MoC classification tier, specializations)
> - Saved profile persists across sessions
> - User can edit any field and the update is reflected in all future generated files
> - Expiry date fields (licenses, certificates) display a warning badge when within 30 days of expiry

**Document Vault**
> As a bid coordinator, I want to upload my qualification documents and have them stored with expiry dates so that I always know what's current and can attach the right version to any tender.
> Acceptance criteria:
> - User can upload PDF files up to 20MB per document
> - Each uploaded document has a required "document type" tag and optional expiry date
> - Dashboard shows a count of expired and expiring-soon documents
> - Documents can be previewed in-browser before download

**Project History Log**
> As a bid coordinator, I want to log our completed projects so that I can quickly pull a list of relevant experience when a tender asks for "similar projects."
> Acceptance criteria:
> - User can add a project with: client name, contract value (SAR), project type, start date, end date, reference contact (name + phone)
> - Projects are searchable by type, client, and date range
> - Project list can be exported to PDF as a formatted "Project Experience" table

**Qualification File Generator**
> As a bid coordinator, I want to describe what a tender requires and have the platform assemble the right documents for me so that I submit a complete, professional packet without manually collating files each time.
> Acceptance criteria:
> - User can create a new "Tender" with a name, deadline, and a checklist of required items (manually entered or uploaded as PDF)
> - System maps each checklist item to the closest available asset (profile data, vault document, or project history entry)
> - User sees a "readiness view" showing which required items are satisfied and which are missing
> - User can generate a single PDF export containing all satisfied items in a standardized cover + tabs format
> - Generated PDF file name includes company name and tender name

**Tender Deadline Tracker**
> As a bid coordinator, I want to see all my active tender deadlines in one place so that I never miss a submission date.
> Acceptance criteria:
> - Calendar and list views of upcoming tender deadlines
> - User can add/edit/delete tenders from the tracker
> - Automated reminder sent by email at 7 days and 1 day before deadline (user-configurable)
> - Overdue tenders shown in red; upcoming tenders within 7 days shown in amber

**Multi-tier Subscription & Billing**
> As a company owner, I want to subscribe to the appropriate plan and manage my billing so that I have uninterrupted access to the platform.
> Acceptance criteria:
> - Two tier options displayed clearly at signup and upgrade screens
> - Payment processed via Moyasar or HyperPay (KSA-local gateway)
> - User receives a tax invoice after each successful payment (required for KSA VAT compliance)
> - Subscription can be cancelled from the account settings page with immediate effect (access ends at period end)
> - Trial period: 14-day free trial with no credit card required

---

## 5. Technical Requirements

**Platform:** Web (browser-based SaaS, responsive design for desktop + tablet)

**Architecture notes:**
- Arabic RTL layout is required from day one across all UI screens — not a post-launch localization pass
- PDF generation must support Arabic text rendering (Arabic fonts embedded in output PDFs)
- All user data scoped to single company account in v1 (no cross-company data)
- File storage: contractor-uploaded documents (vault) and generated PDFs; assume average 50MB per company account at launch

**Integrations required:**
- **Payment gateway:** Moyasar (preferred, Saudi-native, supports mada/Visa/Mastercard) or HyperPay as fallback
- **Email/SMS notifications:** Twilio (SMS) or SendGrid (email) for deadline reminders
- **PDF generation library:** Puppeteer or equivalent that supports Arabic RTL + custom fonts

**Performance requirements:**
- Qualification file PDF generation: < 10 seconds for a standard 10-document packet
- Page load time: < 3 seconds on a 10Mbps connection
- Uptime: 99.5% monthly (standard for SaaS at this stage)

**Security / compliance:**
- PDPL (Saudi Personal Data Protection Law) — contractor data is commercial, not strictly personal, but company contact data (names, phone numbers) is in scope; privacy policy and data processing terms required
- Data residency: hosting in KSA or regional AWS (Bahrain) preferred to support enterprise customers who require local data storage
- Authentication: email + password with OTP confirmation; session timeout after 30 minutes of inactivity

---

## 6. Design Requirements

**Language support:** Arabic (RTL) primary + English bilingual. All UI labels, error messages, and generated PDF content must be available in both languages. Language toggle in user settings. Arabic is the default.

**Platform conventions:** Web conventions. No native mobile design patterns in v1.

**Accessibility:** WCAG 2.1 AA minimum for all user-facing screens.

**Brand:** Munaqis brand (مناقص) — name and logo to be provided by founder. Dreamy to design the full UI system based on brand assets. If founder has no assets at design kickoff, Dreamy will propose a design direction for founder approval.

---

## 7. Timeline & Milestones

| Milestone | Target Date | Owner |
|-----------|-------------|-------|
| API feasibility confirmed (Etimad/Monafasat) | Week 1 | Founder + Dreamy |
| Architecture sign-off & tech stack finalized | +2 weeks | Dreamy |
| Design complete (all screens, Figma, founder approved) | +4 weeks | Dreamy |
| Core profile + document vault feature-complete | +8 weeks | Dreamy |
| Qualification file generator + tracker feature-complete | +16 weeks | Dreamy |
| Billing integration + subscription flows complete | +20 weeks | Dreamy |
| MVP feature-complete (code freeze) | +22 weeks | Dreamy |
| Beta testing with 3–5 pilot contractors | +24 weeks | Founder + Dreamy |
| Bug fixes and feedback incorporation | +26 weeks | Dreamy |
| v1 public launch | +28 weeks (~7 months) | Founder + Dreamy |

*Timeline based on a 2-person Dreamy team (1 frontend + 1 backend). Adjust per actual team capacity at engagement start.*

---

## 8. Open Questions

1. **Government portal API access** — Do Etimad and Monafasat offer any public or private API access for third-party applications? Faisal to investigate with MoF/MoH contacts; Dreamy to attempt developer portal discovery in parallel. Answer required before finalizing v1 feature set. *(Owner: Founder + Dreamy, Due: Week 1)*

2. **Qualification file format requirements** — Are tender qualification file formats standardized across Saudi government entities, or does each ministry/authority have a different format? Faisal to provide 3–5 sample tender qualification checklists from real tenders. *(Owner: Founder, Due: Week 1)*

3. **Payment gateway selection** — Moyasar vs HyperPay: does Faisal have an existing merchant account with either, or does this need to be set up from scratch? Saudi merchant account requires a registered entity — confirms the incorporation blocker. *(Owner: Founder, Due: Week 2)*

4. **Data residency requirement** — Do the target customers (SME contractors) or their clients (government entities they bid to) have any data residency requirements that would prevent hosting on Bahrain-region AWS? *(Owner: Founder, Due: Week 2)*

5. **Branding assets** — Does Faisal have an existing Munaqis logo and brand identity, or does Dreamy design from scratch? *(Owner: Founder, Due: Design kickoff)*

---

## Arabic Summary / ملخص المتطلبات

<div dir="rtl">

## وثيقة متطلبات المنتج — ملخص تنفيذي

**الشركة:** مناقص
**المنتج:** منصة مناقص لتأهيل المقاولين
**التاريخ:** 26/06/2026
**أعدّها:** وليد الأحمد السنوسي، Dreamy

---

### رؤية المنتج

تُمكّن منصة **مناقص** المقاولين الصغار والمتوسطين في المملكة العربية السعودية من الفوز بعدد أكبر من المناقصات الحكومية والخاصة، وذلك عبر إدخال بيانات الشركة مرةً واحدةً فقط وتوليد ملفات التأهيل اللازمة لكل مناقصة بصورة تلقائية عند الطلب.

**المشكلة:** يخسر المقاولون الصغار والمتوسطون (5–50 موظفاً) فرصاً تجارية كثيرة، ليس لعدم قدرتهم على التنفيذ، بل لأن ملفات تأهيلهم تكون ناقصةً أو غير منظمة أو مُقدَّمةً بعد انقضاء المهلة. لا توجد في السوق السعودي أدوات مخصصة للمقاول الصغير تُساعده على الاستجابة لمتطلبات بوابات الاعتماد الحكومية كـ«اعتماد» و«منافسات».

---

### النطاق الوظيفي — الإصدار الأول (MVP)

**المميزات المتضمنة في الإصدار الأول:**

١. **ملف الشركة الموحّد** — إدخال بيانات الشركة مرةً واحدةً (السجل التجاري، رقم التسجيل الضريبي، تصنيف وزارة التجارة، التخصصات، تواريخ انتهاء الرخص).

٢. **مستودع المستندات** — رفع وتخزين وثائق التأهيل (شهادة السجل التجاري، شهادة الزكاة، شهادة الغوسي، شهادة ضريبة القيمة المضافة، الشهادات المهنية)، مع تنبيه تلقائي قبل انتهاء صلاحية كل مستند بـ30 يوماً.

٣. **سجل المشاريع** — توثيق المشاريع المنجزة (اسم العميل، قيمة العقد، نوع المشروع، التواريخ، جهة الاتصال المرجعية) مع إمكانية البحث والتصفية والتصدير.

٤. **مولّد ملفات التأهيل** — إدخال متطلبات مناقصة محددة، ثم يجمع النظام تلقائياً الوثائق والبيانات المناسبة من ملف الشركة ومستودع المستندات وسجل المشاريع، وتصدير ملف PDF جاهز للتقديم.

٥. **متتبّع مواعيد المناقصات** — عرض تقويمي لمواعيد تقديم المناقصات وانتهاء المستندات، مع تذكيرات تلقائية بالبريد الإلكتروني أو الرسائل القصيرة.

٦. **نظام الاشتراك والفواتير** — خطتان: 500 ريال/شهر (للشركات الصغيرة) و1,500 ريال/شهر (للشركات المتوسطة)، مع فترة تجريبية مجانية 14 يوماً وإصدار فاتورة ضريبية بعد كل دفعة.

**المميزات المرجأة إلى إصدارات لاحقة:**

١. التكامل مع بوابات حكومية (اعتماد، منافسات) — الربط عبر واجهات برمجية مباشرة.
٢. محرّك توصية المناقصات المدعوم بالذكاء الاصطناعي.
٣. تطبيقات الجوال (iOS / Android).
٤. التوسع خارج المملكة (الكويت، الإمارات).
٥. حسابات متعددة المستخدمين داخل الشركة الواحدة.

---

### الجدول الزمني التقديري

| المرحلة | الهدف الزمني | المسؤول |
|---------|-------------|---------|
| تأكيد إمكانية الربط بالبوابات الحكومية | الأسبوع الأول | المؤسس + Dreamy |
| اعتماد هيكل التقنية والمعمارية | + أسبوعان | Dreamy |
| اكتمال التصميم واعتماده من المؤسس | + 4 أسابيع | Dreamy |
| اكتمال النموذج الأولي (MVP) بالكامل | + 22 أسبوعاً | Dreamy |
| الاختبار التجريبي مع أولى شركات المقاولين | + 24 أسبوعاً | المؤسس + Dreamy |
| الإطلاق الرسمي للإصدار الأول | + 28 أسبوعاً (7 أشهر) | المؤسس + Dreamy |

---

### الأسئلة المفتوحة

١. هل تتيح بوابتا «اعتماد» و«منافسات» واجهات برمجية للطرف الثالث؟ (المسؤول: المؤسس + Dreamy — الأسبوع الأول)
٢. هل نماذج ملفات التأهيل موحّدة عبر الجهات الحكومية، أم تختلف من جهة إلى أخرى؟ (المسؤول: المؤسس — الأسبوع الأول)
٣. هل لدى المؤسس حساب تاجر مع موسر أو HyperPay؟ (المسؤول: المؤسس — الأسبوع الثاني)
٤. هل توجد اشتراطات لحفظ البيانات داخل المملكة؟ (المسؤول: المؤسس — الأسبوع الثاني)
٥. هل تتوفر هوية بصرية جاهزة لمناقص، أم يصمّمها Dreamy من الصفر؟ (المسؤول: المؤسس — بداية مرحلة التصميم)

</div>

---

*PRD v1.0 — subject to revision based on technical discovery and founder feedback.*
