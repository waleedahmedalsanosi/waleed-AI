# Client Review Package

**Company:** أمين (Amin)
**Prepared by:** Waleed Al-Sanosi, PM — Dreamy
**Date:** 2026-06-26
**Review type:** Pre-development scope confirmation

---

## Overview

This document summarizes what Dreamy proposes to build for أمين as part of our technical co-founding partnership. Please review each section carefully and confirm or raise questions before we begin development.

---

## What We're Building

### Product Summary

أمين is a mobile app and web platform that helps patients in Saudi Arabia find the right doctor or healthcare facility based on verified reviews from real patients — and gives clinics and hospitals a dashboard to understand how patients are finding them and what they're looking for. Reviews are screened by AI to ensure quality, and doctor profiles use a privacy-protecting code system so patients can identify who they saw without exposing individual doctors to public naming risks.

### Who It's For

**Primary user (patients):** Anyone in Saudi Arabia looking for a doctor, clinic, physiotherapy centre, or wellness facility — they search by specialty or location, read verified reviews, and leave their own after visiting.

**Secondary user (healthcare providers):** Clinics, hospitals, and facilities that want to be listed, manage their profile, respond to reviews, and access analytics about patient demand in their specialty.

---

## Scope: What's Included in v1

1. **Search and browse** — Patients can search for healthcare providers by specialty (e.g., orthopaedics, paediatrics) and location, and see results with ratings and review summaries
2. **Coded doctor profiles** — Each doctor within a facility has a unique code; patients can request the doctor's name privately through the app without it being shown publicly
3. **Verified reviews** — Patients submit reviews after a visit, verified through location, appointment records, or manual confirmation — so only real patients can review
4. **Smart review filtering** — AI automatically checks reviews for inappropriate content before they appear on the platform
5. **Post-visit reminders** — The app sends a notification after a verified visit inviting the patient to leave a review at the right moment
6. **Provider dashboard** — Healthcare facilities see how many people searched for them, how many reviews they have, and what specialties are most in demand in their area
7. **Admin control panel** — Dreamy's team can manage all reviews, verify new facilities, and handle doctor code assignments through an internal tool
8. **Registration and login** — Patients register with their phone number; facilities register with business documents for verification

**Expected timeline:**
- Week 4: All designs approved and technical architecture finalised
- Month 3: Patient search, browse, and doctor profiles live in testing
- Month 6: All features complete
- Month 7: Beta testing with 3–5 partner facilities
- Month 9: Public launch on App Store, Google Play, and web

---

## Scope: What's NOT Included in v1

1. **Direct appointment booking** — Integrating with clinic scheduling systems is complex and requires individual partnerships with each facility; we'll evaluate this for v2 after launch
2. **Job listings / healthcare recruitment** — A future revenue stream, but requires separate legal setup; deferred
3. **Paid subscriptions for providers** — The platform will be free for providers in v1 to drive adoption; paid tiers will be introduced in v2 based on what providers actually value
4. **Expansion outside Saudi Arabia** — v1 is Saudi Arabia only; each Gulf market requires separate regulatory and localisation work
5. **Publicly displaying doctor names** — Until the legal framework is fully validated, doctors are identified by code; the in-app private name request flow handles patient needs

---

## What Dreamy Will Deliver

| Deliverable | Description | When |
|-------------|-------------|------|
| Design & architecture | All app screens designed, technical plan documented, everything approved by you | Week 4 |
| Patient flows | Search, browse, doctor profiles, and registration working in testing | Month 3 |
| Coded doctor system | Doctor code management and private name request flow | Month 3 |
| Reviews & verification | Review submission, location/appointment verification, AI filtering | Month 6 |
| Post-visit reminders | Automated notifications after confirmed visits | Month 6 |
| Provider dashboard | Facility profile, review management, analytics | Month 5 |
| Admin panel | Internal moderation and provider verification tool | Month 5 |
| Beta support | Help onboard 3–5 pilot facilities and fix issues during testing | Month 7 |
| v1 launch | Live product on App Store, Google Play, and web | Month 9 |

---

## What We Need from You

Before development begins, we need you to confirm or provide:

- [ ] **Legal framework for coded profiles** — Have you confirmed with a Saudi legal advisor that the coded doctor profile system and private in-app name disclosure are legally defensible? We need this answer before building the doctor profile system
- [ ] **Appointment data access** — Can you connect us with 1–2 pilot facilities that have appointment systems we could link to for review verification? Or will we rely on geolocation and manual confirmation at launch?
- [ ] **Government platform details** — Which platform requires quarterly renewal (1,500–2,000 SAR), and what registration category will أمين fall under? We need this resolved before launch planning
- [ ] **Branding assets** — Do you have a logo, colour palette, or brand guidelines for أمين, or should Dreamy design the visual identity? (Needed by Week 2)
- [ ] **Co-founder confirmation** — Please confirm the identity and role of your co-founder before we sign the partnership agreement
- [ ] **Company incorporation** — The partnership requires a new legal entity; please confirm incorporation timeline
- [ ] **Beta facilities** — We need introductions to at least 3 healthcare facilities willing to pilot أمين by Month 4 so we can test with real data

---

## Equity Terms Reminder

As discussed, Dreamy will receive **30%** equity in أمين in exchange for the services described in this document.

*Please confirm you've reviewed the full Scope of Work document and are aligned on these terms before we proceed.*

---

## Arabic Summary / ملخص للمراجعة

<div dir="rtl">

## ملخص حزمة المراجعة للعميل

**الشركة:** أمين
**أعدّها:** وليد الأحمد السنوسي، Dreamy
**التاريخ:** 26/06/2026

---

### ما الذي سنبنيه؟

**أمين** منصة رقمية (تطبيق جوال وموقع ويب) تُمكّن المرضى في المملكة العربية السعودية من البحث عن الأطباء والمنشآت الصحية المناسبة بناءً على تقييمات حقيقية موثّقة — مع نظام حماية خصوصية الأطباء عبر الرموز، وتصفية ذكية للمحتوى بالذكاء الاصطناعي. وفي الوقت ذاته، تمنح المنشآت الصحية لوحة بيانات تحليلية لفهم سلوك المرضى والطلب على تخصصاتها.

### المميزات المشمولة في الإصدار الأول

1. البحث والتصفح — حسب التخصص والموقع الجغرافي مع ملخص التقييمات
2. ملفات الأطباء المُرمَّزة — كود فريد لكل طبيب مع طلب الاسم بشكل خاص داخل التطبيق
3. تقديم المراجعات مع التحقق — ربط الزيارات الحقيقية بالمراجعات
4. التصفية الذكية للمراجعات بالذكاء الاصطناعي
5. التذكير التلقائي بعد الزيارة لدعوة المريض لتقييم تجربته
6. لوحة بيانات مقدم الخدمة — المراجعات، الإحصائيات، الطلب على التخصصات
7. لوحة إدارة المنصة — الإشراف الداخلي والتحقق من المنشآت
8. التسجيل والمصادقة لجميع الأطراف

### المميزات المرجأة

1. الحجز المباشر داخل التطبيق (مرحلة لاحقة)
2. التوظيف والبحث عن الكوادر الصحية (مرحلة لاحقة)
3. الاشتراكات المدفوعة لمقدمي الخدمة (تُطلَق بعد الإطلاق وجمع البيانات)
4. التوسع إلى دول الخليج الأخرى (بعد تثبيت السوق السعودية)

### ما نحتاجه منكم

- [ ] تأكيد سلامة نظام الرموز من الناحية القانونية السعودية
- [ ] التواصل مع منشآت صحية للاختبار التجريبي (3 على الأقل بحلول الشهر الرابع)
- [ ] تفاصيل المنصة الحكومية وتكاليف التجديد الدورية
- [ ] أصول الهوية البصرية لأمين (أو التأكيد على أن Dreamy ستتولى التصميم)
- [ ] تأكيد هوية الشريك ودوره في المشروع
- [ ] إتمام تأسيس الكيان القانوني الجديد

### شروط الأسهم

ستحصل Dreamy على نسبة **30٪** من أسهم شركة أمين مقابل الخدمات المتفق عليها في هذه الوثيقة.

---

*يُرجى مراجعة هذه الوثيقة والتواصل معنا بأي استفسارات قبل بدء التطوير.*

</div>

---

**To confirm:** Please reply with "Confirmed" or raise any questions.
*This document does not replace the formal Scope of Work agreement.*
