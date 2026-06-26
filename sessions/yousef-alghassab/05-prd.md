# Product Requirements Document

**Company:** أمين (Amin)
**Product:** أمين — Healthcare Review & Recommendation Platform
**Version:** 1.0
**Date:** 2026-06-26
**Authors:** Waleed Al-Sanosi (PM), Dreamy

---

## 1. Product Vision

**One-line vision:** أمين enables Saudi patients to find trusted healthcare providers by surfacing verified, AI-filtered reviews — and gives clinics and hospitals the demand intelligence to grow their practice.

**Problem statement:** Patients in Saudi Arabia have no structured, credible source for choosing doctors or healthcare facilities. They rely on personal networks, which are limited and inconsistent. Existing generic review platforms (Google Maps, social media) lack healthcare-specific verification, are legally constrained by Saudi defamation law for naming individual physicians, and provide no useful data to healthcare providers. The result is a misalignment of supply and demand in a sector where trust is the primary purchase driver.

**Success metric (v1):** 500 verified patient reviews across 50+ listed facilities within 90 days of launch, with at least 10 healthcare providers on a paid analytics subscription.

---

## 2. Users

### Primary User — Patient / Healthcare Seeker

- **Who:** Adult Saudi residents seeking doctors, clinics, or healthcare facilities for themselves or their families
- **Pain:** Cannot find a trustworthy, structured way to evaluate and compare healthcare providers; relies on WhatsApp group recommendations or calling around
- **Job to be done:** Find the right doctor or facility for a specific condition, based on real experiences from people with similar situations

### Secondary User — Healthcare Provider (Clinic / Hospital / Facility)

- **Who:** Hospitals, polyclinics, specialist clinics, physiotherapy centres, wellness facilities operating in Saudi Arabia
- **Pain:** No structured feedback mechanism; no visibility into what patients search for, what competitors offer, or how to demonstrate quality
- **Job to be done:** Attract patients, manage reputation, and access demand analytics to support business decisions

### Tertiary User — Platform Admin

- **Who:** Dreamy / founder operations team
- **Pain:** Needs to maintain platform quality, verify providers, moderate reviews, and manage the coded doctor profile system
- **Job to be done:** Ensure review integrity, prevent abuse, onboard and verify healthcare providers

---

## 3. Scope

### In Scope (v1 MVP)

1. **Patient search and browse** — Search for healthcare providers by specialty, facility type, and location; browse results with rating summaries and review excerpts
2. **Coded doctor profiles** — Doctor profiles referenced by a unique code within their facility; patient can request the doctor's name privately within the app to avoid public defamation risk
3. **Review submission with verification** — Post-visit review flow with verification layer using at least one of: appointment linkage, location signal (geolocation), or manual confirmation
4. **AI review filtering** — Automated screening of submitted reviews to remove non-objective, abusive, or fabricated content before publication
5. **Post-visit review reminder** — Automated notification to patients after a verified visit, prompting a review at the right moment
6. **Provider profile and dashboard** — Healthcare providers can create a verified profile and access a basic analytics dashboard: review counts, specialty demand trends, patient volume indicators
7. **Admin moderation panel** — Internal tool for reviewing flagged content, verifying provider documents, managing doctor codes, and overseeing the provider onboarding queue
8. **User authentication** — Patient and provider registration/login; provider identity verification flow with document upload

### Out of Scope (v1)

1. **Direct appointment booking** — Complex integration with clinic scheduling systems; deferred to Phase 2 after provider adoption is established
2. **In-platform recruitment / job listings** — Future revenue stream; requires separate legal and operational framework
3. **Advanced provider subscription tiers** — Freemium launch for providers; paid tier design deferred until v1 adoption data is available
4. **Cross-border GCC expansion** — Saudi Arabia only in v1; expansion requires separate regulatory and localisation work
5. **Public doctor naming** — Legal framework for publicly naming individual physicians is not validated; coded profile approach used in v1
6. **Delivery service / transport integration** — One of the verification mechanisms discussed; deferred — not core to MVP verification flow

---

## 4. User Stories

**Patient Search and Browse**
> As a patient, I want to search for orthopaedic clinics near me and see verified ratings, so that I can shortlist options before calling to book.
> Acceptance criteria:
> - Search returns results filtered by specialty and within a specified radius
> - Each result shows overall rating, number of reviews, facility type, and location
> - Results load within 3 seconds on a 4G connection

**Coded Doctor Profiles**
> As a patient, I want to identify the specific doctor I saw by a code and request their name through the app, so that I can review them without the platform publicly exposing their identity.
> Acceptance criteria:
> - Each doctor within a facility has a unique alphanumeric code visible on their profile
> - Patient can submit a name request; name is returned in-app within 24 hours pending admin verification
> - Doctor's full name is never displayed publicly in the patient-facing browse flow

**Review Submission with Verification**
> As a patient who visited a clinic, I want to submit a review of my experience, so that future patients can benefit from my insight and the facility gets actionable feedback.
> Acceptance criteria:
> - Review flow is accessible only after at least one verification signal is present (location, appointment link, or manual confirmation)
> - Review form captures: overall rating, experience categories (reception, wait time, doctor communication, outcome), and free-text comment
> - Submitted review enters AI moderation queue before going live

**AI Review Filtering**
> As a platform admin, I want submitted reviews to be screened automatically for abusive, fabricated, or non-objective content, so that only high-quality reviews are published.
> Acceptance criteria:
> - AI model flags reviews containing profanity, personal attacks, or patterns inconsistent with genuine patient experience
> - Flagged reviews are held for admin review; non-flagged reviews publish within 30 minutes of submission
> - Admin can override AI decision in either direction

**Post-Visit Review Reminder**
> As a platform, I want to send a review prompt to patients after a verified facility visit, so that review collection happens at the moment of highest recall and motivation.
> Acceptance criteria:
> - Reminder is triggered within 24–48 hours of a confirmed visit signal
> - Reminder is sent via push notification with a direct deep link to the review form
> - Patient can snooze or permanently opt out of reminders

**Provider Profile and Dashboard**
> As a clinic manager, I want to see how patients are finding and rating my facility, so that I can identify improvement areas and demonstrate quality to new patients.
> Acceptance criteria:
> - Dashboard shows: total reviews, average rating, specialty search volume for the facility's categories, and month-on-month trends
> - Provider can respond to reviews (response visible on the review page)
> - Dashboard is accessible on web browser (mobile-responsive)

**Admin Moderation Panel**
> As an admin, I want to manage all review moderation, provider verification, and doctor code assignments in a single internal tool, so that platform quality is maintainable at scale.
> Acceptance criteria:
> - Queue view shows all pending reviews with AI flag status and review text
> - Provider onboarding queue shows submitted documents with approve/reject controls
> - Doctor code assignment interface allows admin to create, link, and deactivate codes per facility

**User Authentication**
> As a new user, I want to create an account and log in securely, so that my review history and preferences are saved and my identity is verifiable.
> Acceptance criteria:
> - Patient registration via phone number (OTP) + optional email
> - Provider registration requires facility name, CR number, and document upload; goes to admin review queue
> - Forgot password and session management follow standard security practices

---

## 5. Technical Requirements

**Platform:** Mobile-first (iOS + Android via React Native); provider dashboard web-accessible (responsive web)
**Architecture notes:**
- Multi-role auth system (patient, provider, admin) with role-based access control
- Coded doctor profile system requires a mapping layer between public codes and private identity records — privacy-sensitive, admin-only access
- AI review filtering: NLP model (Arabic + English); can begin with a rules-based layer + LLM call for borderline cases
- Review verification: modular verification service — appointment link, geolocation signal, manual confirmation — designed so additional signals can be added without rearchitecting

**Integrations required:**
- Saudi payment gateway (e.g., Moyasar or HyperPay) — for future provider subscriptions; API integration in v1, activation in v2
- Push notification service (Firebase Cloud Messaging)
- Geolocation (device GPS)
- SMS/OTP provider for patient authentication (e.g., Unifonic or similar KSA provider)

**Performance requirements:** Search results < 3 seconds on 4G; review submission < 2 seconds; admin queue supports up to 500 concurrent reviews

**Security / compliance:**
- Saudi Personal Data Protection Law (PDPL) compliance required — data residency in KSA or compliant cloud region
- Coded doctor profile data is classified as sensitive — encrypted at rest, admin-only access
- Provider documents (CR, licences) stored with access logging

---

## 6. Design Requirements

**Language support:** Arabic (RTL) primary; English secondary — all user-facing text bilingual from v1 launch
**Platform conventions:** iOS HIG for iOS, Material Design 3 for Android; web dashboard follows standard responsive web patterns
**Accessibility:** WCAG 2.1 AA minimum
**Brand:** TBD — founder has not shared branding assets; Dreamy to propose visual identity as part of design phase

---

## 7. Timeline & Milestones

| Milestone | Target Date | Owner |
|-----------|-------------|-------|
| Design & architecture complete | Week 4 from start | Dreamy |
| Core patient flows (search, browse, review) | Month 3 | Dreamy |
| Provider dashboard + admin panel | Month 5 | Dreamy |
| AI filtering integration + verification layer | Month 6 | Dreamy |
| Beta testing with 3–5 pilot facilities | Month 7 | Founder + Dreamy |
| v1 public launch | Month 9 | Founder + Dreamy |

*Timeline based on 2-person Dreamy team. Conditional on founder delivering: pilot facility introductions by Month 4, branding assets by Month 2.*

---

## 8. Open Questions

1. **Doctor naming legal framework** (Founder + Legal): Has the coded-profile approach been reviewed by a Saudi legal advisor? Can doctor names be disclosed in-app privately, or must all disclosure be mediated through admin?
2. **Appointment data source** (Founder): Is there an existing appointment booking system at pilot facilities Dreamy can integrate with for verification, or is location/manual the primary signal at launch?
3. **Government platform registration** (Founder + Dreamy): Which government platform requires quarterly renewal (1,500–2,000 SAR)? What is the registration category — is this a healthcare information platform or something else?
4. **Provider onboarding process** (Founder): What documents will Dreamy require to verify a healthcare facility? Who manages the verification — Dreamy operations or a contracted third party?
5. **Branding** (Founder): Does the أمين brand have any existing visual assets, or is this a greenfield design engagement?
6. **Co-founder clarity** (Founder): Who is Yousef's partner, what is their role, and what is the equity structure of the current entity?

---

## Arabic Summary / ملخص المتطلبات

<div dir="rtl">

## وثيقة متطلبات المنتج — ملخص تنفيذي

**الشركة:** أمين
**المنتج:** منصة أمين للمراجعات والتوصيات الصحية
**التاريخ:** 26/06/2026
**أعدّها:** وليد الأحمد السنوسي، مدير المنتج — Dreamy

---

### رؤية المنتج

تُتيح منصة **أمين** للمرضى في المملكة العربية السعودية الوصول إلى مزودي الرعاية الصحية المناسبين، من خلال تقييمات حقيقية موثّقة ومُصفّاة بالذكاء الاصطناعي. وفي الوقت ذاته، تمنح العيادات والمستشفيات بيانات تحليلية دقيقة تُساعدها على تحسين خدماتها وتوسيع نطاق وصولها.

المشكلة الجوهرية هي غياب مصدر موثوق ومنظَّم يُمكِّن المريض من اختيار الطبيب أو المنشأة الصحية بناءً على تجارب حقيقية. المنصات العامة الحالية لا تأخذ في الحسبان التحديات القانونية المرتبطة بتقييم الأطباء في السياق السعودي، ولا توفّر أي قيمة مضافة لمقدمي الخدمة.

---

### النطاق الوظيفي — الإصدار الأول

**المميزات المتضمنة في الإصدار الأول (MVP):**

1. **البحث والاستعراض للمريض** — البحث عن مزودي الخدمة الصحية حسب التخصص والنوع والموقع
2. **ملفات الأطباء المُرمَّزة** — كود فريد لكل طبيب داخل منشأته مع إمكانية طلب الاسم بشكل خاص داخل التطبيق
3. **تقديم المراجعات مع التحقق** — آلية توثيق المراجعات عبر ربط المواعيد أو الموقع الجغرافي أو التأكيد اليدوي
4. **تصفية المراجعات بالذكاء الاصطناعي** — فلترة المحتوى غير الموضوعي أو المسيء تلقائياً قبل النشر
5. **تذكير بعد الزيارة** — إشعار تلقائي للمريض بعد الزيارة المؤكَّدة لدعوته لكتابة مراجعته
6. **ملف وإحصائيات مقدم الخدمة** — لوحة بيانات للمنشأة تعرض المراجعات وحجم الطلب والتخصصات الأكثر بحثاً
7. **لوحة إدارة المنصة** — أداة داخلية للإشراف على المراجعات، والتحقق من مقدمي الخدمة، وإدارة رموز الأطباء
8. **نظام تسجيل الدخول والتحقق من الهوية** — تسجيل للمرضى عبر رقم الجوال، ولمقدمي الخدمة عبر وثائق رسمية

**المميزات المرجأة إلى إصدارات لاحقة:**

1. الحجز المباشر داخل التطبيق — يستلزم تكاملاً مع أنظمة جدولة المواعيد في المنشآت
2. التوظيف والبحث عن الكوادر الصحية — يتطلب إطاراً قانونياً وتشغيلياً منفصلاً
3. الاشتراكات المدفوعة المتقدمة لمقدمي الخدمة — تُطلق بعد اكتساب قاعدة مستخدمين كافية
4. التوسع خارج المملكة العربية السعودية — يستلزم توطيناً وامتثالاً تنظيمياً لكل سوق

---

### الجدول الزمني التقديري

| المرحلة | الموعد المستهدف | المسؤول |
|---------|----------------|---------|
| اكتمال التصميم والبنية التقنية | الأسبوع الرابع | Dreamy |
| رحلات المريض الأساسية (البحث، التصفح، المراجعة) | الشهر الثالث | Dreamy |
| لوحة مقدم الخدمة + لوحة الإدارة | الشهر الخامس | Dreamy |
| تكامل الذكاء الاصطناعي وطبقة التحقق | الشهر السادس | Dreamy |
| الاختبار التجريبي مع 3–5 منشآت | الشهر السابع | المؤسس + Dreamy |
| الإطلاق الرسمي للإصدار الأول | الشهر التاسع | المؤسس + Dreamy |

---

### الأسئلة المفتوحة

1. هل تمت مراجعة آلية الرموز مع مستشار قانوني متخصص في قطاع الصحة السعودي؟
2. هل تتوفر بيانات مواعيد من المنشآت التجريبية لاستخدامها في التحقق من الزيارات؟
3. ما المنصة الحكومية التي تستلزم تجديداً كل ثلاثة أشهر وما طبيعة التسجيل المطلوب؟
4. ما متطلبات التحقق من هوية مقدمي الخدمة عند التسجيل في المنصة؟
5. هل يمتلك مشروع أمين هوية بصرية محددة، أم يُوكَل تصميمها إلى Dreamy؟

</div>

---

*PRD v1.0 — subject to revision based on technical discovery and founder feedback.*
